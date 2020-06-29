# 软件I2C的实现原理及方法


------

# 一、STM32的I2C特性及架构
如果我们直接控制STM32的两个GPIO引脚，分别用作SCL及SDA，按照上述信号的时序要求，直接像控制LED灯那样控制引脚的输出(若是接收数据时则读取SDA电平)，就可以实现I2C通讯。同样，假如我们按照USART的要求去控制引脚，也能实现USART通讯。所以只要遵守协议，就是标准的通讯，不管您如何实现它，不管是ST生产的控制器还是ATMEL生产的存储器，都能按通讯标准交互。由于直接控制GPIO引脚电平产生通讯时序时，需要由CPU控制每个时刻的引脚状态，所以称之为“软件模拟协议”方式。相对地，还有“硬件协议”方式，STM32的I2C片上外设专门负责实现I2C通讯协议，只要配置好该外设，它就会自动根据协议要求产生通讯信号，收发数据并缓存起来，CPU只要检测该外设的状态和访问数据寄存器，就能完成数据收发。这种由硬件外设处理I2C协议的方式减轻了CPU的工作，且使软件设计更加简单。
## （一）STM32的I2C外设简介
STM32的I2C外设可用作通讯的主机及从机，支持100Kbit/s和400Kbit/s的速率，支持7位、10位设备地址，支持DMA数据传输，并具有数据校验功能。它的I2C外设还支持SMBus2.0协议，SMBus协议与I2C类似，主要应用于笔记本电脑的电池管理中，感兴趣的读者可参考《SMBus20》文档了解。
## （二）STM32的I2C架构剖析
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629102939739.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 1.通讯引脚
I2C的所有硬件架构都是根据图中左侧SCL线和SDA线展开的(其中的SMBA线用于SMBUS的警告信号，I2C通讯没有使用)。STM32芯片有多个I2C外设，它们的I2C通讯信号引出到不同的GPIO引脚上，使用时必须配置到这些指定的引脚。
| 引脚 | I2C1              | I2C2 |
| ---- | ----------------- | ---- |
| SCL  | PB5 / PB8(重映射) | PB10 |
| SDA  | PB6 / PB9(重映射) | PB11 |

### 2.时钟控制逻辑
SCL线的时钟信号，由I2C接口根据时钟控制寄存器(CCR)控制，控制的参数主要为时钟频率。配置I2C的CCR寄存器可修改通讯速率相关的参数：

 - 可选择I2C通讯的“标准/快速”模式，这两个模式分别I2C对应100/400Kbit/s的通讯速率。


 - 在快速模式下可选择SCL时钟的占空比，可选Tlow/Thigh=2或Tlow/Thigh=16/9模式，我们知道I2C协议在SCL高电平时对SDA信号采样，SCL低电平时SDA准备下一个数据，修改SCL的高低电平比会影响数据采样，但其实这两个模式的比例差别并不大，若不是要求非常严格，这里随便选就可以了。

- CCR寄存器中还有一个12位的配置因子CCR，它与I2C外设的输入时钟源共同作用，产生SCL时钟，STM32的I2C外设都挂载在APB1总线上，使用APB1的时钟源PCLK1，SCL信号线的输出时钟公式如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020062910342723.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)计算结果得出CCR为30，向该寄存器位写入此值则可以控制IIC的通讯速率为400KHz，其实即使配置出来的SCL时钟不完全等于标准的400KHz，IIC通讯的正确性也不会受到影响，因为所有数据通讯都是由SCL协调的，只要它的时钟频率不远高于标准即可。

### 3.数据控制逻辑
I2C的SDA信号主要连接到数据移位寄存器上，数据移位寄存器的数据来源及目标是数据寄存器(DR)、地址寄存器(OAR)、PEC寄存器以及SDA数据线。当向外发送数据的时候，数据移位寄存器以“数据寄存器”为数据源，把数据一位一位地通过SDA信号线发送出去；当从外部接收数据的时候，数据移位寄存器把SDA信号线采样到的数据一位一位地存储到“数据寄存器”中。若使能了数据校验，接收到的数据会经过PCE计算器运算，运算结果存储在“PEC寄存器”中。当STM32的I2C工作在从机模式的时候，接收到设备地址信号时，数据移位寄存器会把接收到的地址与STM32的自身的“I2C地址寄存器”的值作比较，以便响应主机的寻址。STM32的自身I2C地址可通过修改“自身地址寄存器”修改，支持同时使用两个I2C设备地址，两个地址分别存储在OAR1和OAR2中。

### 4.整体控制逻辑
整体控制逻辑负责协调整个I2C外设，控制逻辑的工作模式根据我们配置的“控制寄存器(CR1/CR2)”的参数而改变。在外设工作时，控制逻辑会根据外设的工作状态修改“状态寄存器(SR1和SR2)”，我们只要读取这些寄存器相关的寄存器位，就可以了解I2C的工作状态。除此之外，控制逻辑还根据要求，负责控制产生I2C中断信号、DMA请求及各种I2C的通讯信号(起始、停止、响应信号等)


# 二、通讯过程
使用I2C外设通讯时，在通讯的不同阶段它会对“状态寄存器(SR1及SR2)”的不同数据位写入参数，我们通过读取这些寄存器标志来了解通讯状态。

### 1.主发送器
图中的是“主发送器”流程，即作为I2C通讯的主机端时，向外发送数据时的过程。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629103805652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)主发送器发送流程及事件说明如下：

 1. 控制产生起始信号(S)，当发生起始信号后，它产生事件“EV5”，并会对SR1寄存器的“SB”位置1，表示起始信号已经发送；
 2. 紧接着发送设备地址并等待应答信号，若有从机应答，则产生事件“EV6”及“EV8”，这时SR1寄存器的“ADDR”位及“TXE”位被置1，ADDR
    为1表示地址已经发送，TXE为1表示数据寄存器为空；
 3. 以上步骤正常执行并对ADDR位清零后，我们往I2C的“数据寄存器DR”写入要发送的数据，这时TXE位会被重置0，表示数据寄存器非空，I2C外设通过SDA信号线一位位把数据发送出去后，又会产生“EV8”事件，即TXE位被置1，重复这个过程，就可以发送多个字节数据了；
 4. 当我们发送数据完成后，控制I2C设备产生一个停止信号(P)，这个时候会产生EV8_2事件，SR1的TXE位及BTF位都被置1，表示通讯结束。

假如我们使能了I2C中断，以上所有事件产生时，都会产生I2C中断信号，进入同一个中断服务函数，到I2C中断服务程序后，再通过检查寄存器位来判断是哪一个事件。

### 2.主接收器
再来分析主接收器过程，即作为I2C通讯的主机端时，从外部接收数据的过程，见图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629104005590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
主接收器接收流程及事件说明如下：

 1. 同主发送流程，起始信号(S)是由主机端产生的，控制发生起始信号后，它产生事件“EV5”，并会对SR1寄存器的“SB”位置1，表示起始信号已经发送；
 2. 紧接着发送设备地址并等待应答信号，若有从机应答，则产生事件“EV6”这时SR1寄存器的“ADDR”位被置1，表示地址已经发送。
 3. 从机端接收到地址后，开始向主机端发送数据。当主机接收到这些数据后，会产生“EV7”事件，SR1寄存器的RXNE被置1，表示接收数据寄存器非空，我们读取该寄存器后，可对数据寄存器清空，以便接收下一次数据。此时我们可以控制I2C发送应答信号(ACK)或非应答信号(NACK)，若应答，则重复以上步骤接收数据，若非应答，则停止传输；
 4. 发送非应答信号后，产生停止信号(P)，结束传输。

在发送和接收过程中，有的事件不只是标志了我们上面提到的状态位，还可能同时标志主机状态之类的状态位，而且读了之后还需要清除标志位，比较复杂。我们可使用STM32标准库函数来直接检测这些事件的复合标志，降低编程难度。

# 三、代码实现
I2C.h
```c
#ifndef __MYIIC_H
#define __MYIIC_H
#include "sys.h"


//IO方向设置
 
#define SDA_IN()  {GPIOB->CRL&=0X0FFFFFFF;GPIOB->CRL|=(u32)8<<28;}
#define SDA_OUT() {GPIOB->CRL&=0X0FFFFFFF;GPIOB->CRL|=(u32)3<<28;}

//IO操作函数	 
#define IIC_SCL    PBout(6) //SCL
#define IIC_SDA    PBout(7) //SDA	 
#define READ_SDA   PBin(7)  //输入SDA 

//IIC所有操作函数
void IIC_Init(void);                //初始化IIC的IO口				 
void IIC_Start(void);				//发送IIC开始信号
void IIC_Stop(void);	  			//发送IIC停止信号
void IIC_Send_Byte(u8 txd);			//IIC发送一个字节
u8 IIC_Read_Byte(unsigned char ack);//IIC读取一个字节
u8 IIC_Wait_Ack(void); 				//IIC等待ACK信号
void IIC_Ack(void);					//IIC发送ACK信号
void IIC_NAck(void);				//IIC不发送ACK信号

void IIC_Write_One_Byte(u8 daddr,u8 addr,u8 data);
u8 IIC_Read_One_Byte(u8 daddr,u8 addr);	  
#endif

```

I2C.c

```c
#include "myiic.h"
#include "delay.h"
//初始化IIC
void IIC_Init(void)
{					     
	GPIO_InitTypeDef GPIO_InitStructure;
	RCC_APB2PeriphClockCmd(	RCC_APB2Periph_GPIOB, ENABLE );	//使能GPIOB时钟
	   
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_6|GPIO_Pin_7;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP ;   //推挽输出
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);
	GPIO_SetBits(GPIOB,GPIO_Pin_6|GPIO_Pin_7); 	//PB6,PB7 输出高
}
//产生IIC起始信号
void IIC_Start(void)
{
	SDA_OUT();     //sda线输出
	IIC_SDA=1;	  	  
	IIC_SCL=1;
	delay_us(4);
 	IIC_SDA=0;//START:when CLK is high,DATA change form high to low 
	delay_us(4);
	IIC_SCL=0;//钳住I2C总线，准备发送或接收数据 
}	  
//产生IIC停止信号
void IIC_Stop(void)
{
	SDA_OUT();//sda线输出
	IIC_SCL=0;
	IIC_SDA=0;//STOP:when CLK is high DATA change form low to high
 	delay_us(4);
	IIC_SCL=1; 
	IIC_SDA=1;//发送I2C总线结束信号
	delay_us(4);							   	
}
//等待应答信号到来
//返回值：1，接收应答失败
//        0，接收应答成功
u8 IIC_Wait_Ack(void)
{
	u8 ucErrTime=0;
	SDA_IN();      //SDA设置为输入  
	IIC_SDA=1;delay_us(1);	   
	IIC_SCL=1;delay_us(1);	 
	while(READ_SDA)
	{
		ucErrTime++;
		if(ucErrTime>250)
		{
			IIC_Stop();
			return 1;
		}
	}
	IIC_SCL=0;//时钟输出0 	   
	return 0;  
} 
//产生ACK应答
void IIC_Ack(void)
{
	IIC_SCL=0;
	SDA_OUT();
	IIC_SDA=0;
	delay_us(2);
	IIC_SCL=1;
	delay_us(2);
	IIC_SCL=0;
}
//不产生ACK应答		    
void IIC_NAck(void)
{
	IIC_SCL=0;
	SDA_OUT();
	IIC_SDA=1;
	delay_us(2);
	IIC_SCL=1;
	delay_us(2);
	IIC_SCL=0;
}					 				     
//IIC发送一个字节
//返回从机有无应答
//1，有应答
//0，无应答			  
void IIC_Send_Byte(u8 txd)
{                        
    u8 t;   
	SDA_OUT(); 	    
    IIC_SCL=0;//拉低时钟开始数据传输
    for(t=0;t<8;t++)
    {              
        //IIC_SDA=(txd&0x80)>>7;
		if((txd&0x80)>>7)
			IIC_SDA=1;
		else
			IIC_SDA=0;
		txd<<=1; 	  
		delay_us(2);   //对TEA5767这三个延时都是必须的
		IIC_SCL=1;
		delay_us(2); 
		IIC_SCL=0;	
		delay_us(2);
    }	 
} 	    
//读1个字节，ack=1时，发送ACK，ack=0，发送nACK   
u8 IIC_Read_Byte(unsigned char ack)
{
	unsigned char i,receive=0;
	SDA_IN();//SDA设置为输入
    for(i=0;i<8;i++ )
	{
        IIC_SCL=0; 
        delay_us(2);
		IIC_SCL=1;
        receive<<=1;
        if(READ_SDA)receive++;   
		delay_us(1); 
    }					 
    if (!ack)
        IIC_NAck();//发送nACK
    else
        IIC_Ack(); //发送ACK   
    return receive;
}
```

------



[^参考教程]: 

野火STM32系列教程

正点原子STM32系列教程

洋桃电子STM32系列教程

[^参考资料]: 

《STM32F10X-中文参考手册》

《I2C总线协议》


