# 硬件I2C的实现


------

## 



在前面两篇文章中已经讲过了I2C的原理，这篇文章主要讲硬件I2C的实现。

------



## I2C初始化结构体详解
跟其它外设一样，STM32标准库提供了I2C初始化结构体及初始化函数来配置I2C外设。初始化结构体及函数定义在库文件“stm32f10x_i2c.h”及“stm32f10x_i2c.c”中，编程时我们可以结合这两个文件内的注释使用或参考库帮助文档。

```c
typedef struct{
	uint32_tI2C_ClockSpeed; /*!< 设置SCL时钟频率，此值要低于400000*/
	uint16_tI2C_Mode;      /*!< 指定工作模式，可选I2C模式及SMBUS模式*/
	uint16_tI2C_DutyCycle; /*指定时钟占空比，可选low/high = 2:1及16:9模式*/
	uint16_tI2C_OwnAddress1;     /*!< 指定自身的I2C设备地址*/
	uint16_tI2C_Ack;                 /*!< 使能或关闭响应(一般都要使能) */
	uint16_tI2C_AcknowledgedAddress;/*!< 指定地址的长度，可为7位及10位*/
} I2C_InitTypeDef;
```
这些结构体成员说明如下，其中括号内的文字是对应参数在STM32标准库中定义的宏：

  **（1）I2C_ClockSpeed**

本成员设置的是I2C的传输速率，在调用初始化函数时，函数会根据我们输入的数值经过运算后把时钟因子写入到I2C的时钟控制寄存器CCR。而我们写入的这个参数值不得高于400KHz。实际上由于CCR寄存器不能写入小数类型的时钟因子，影响到SCL的实际频率可能会低于本成员设置的参数值，这时除了通讯稍慢一点以外，不会对I2C的标准通讯造成其它影响。

 **（2）I2C_Mode**

本成员是选择I2C的使用方式，有I2C模式(I2C_Mode_I2C)和SMBus主、从模式(I2C_Mode_SMBusHost、I2C_Mode_SMBusDevice ) 。I2C不需要在此处区分主从模式，直接设置I2C_Mode_I2C即可。
**（3）I2C_DutyCycle**
本成员设置的是I2C的SCL线时钟的占空比。该配置有两个选择，分别为低电平时间比高电平时间为2：1  ( I2C_DutyCycle_2)和16：9  (I2C_DutyCycle_16_9)。其实这两个模式的比例差别并不大，一般要求都不会如此严格，这里随便选就可以。
**（4）I2C_OwnAddress1**
本成员配置的是STM32的I2C设备自己的地址，每个连接到I2C总线上的设备都要有一 个 自 己 的 地 址 ， 作 为 主 机 也 不 例 外 。地 址 可 设 置 为7位或10位(受 下 面I2C_AcknowledgeAddress成员决定)，只要该地址是I2C总线上唯一的即可。

STM32的I2C外设可同时使用两个地址，即同时对两个地址作出响应，这个结构成员I2C_OwnAddress1配置的是默认的、OAR1寄存器存储的地址，若需要设置第二个地址寄存器OAR2，可使用I2C_OwnAddress2Config函数来配置，OAR2不支持10位地址，只有7位。
**(5)I2C_Ack_Enable**
本成员是关于I2C应答设置，设置为使能则可以发送响应信号。本实验配置为允许应答(I2C_Ack_Enable)，这是绝大多数遵循I2C标准的设备的通讯要求，改为禁止应答(I2C_Ack_Disable)往往会导致通讯错误。

**(6)I2C_AcknowledgeAddress**
本成员选择I2C的寻址模式是7位还是10位地址。这需要根据实际连接到I2C总线上设备的地址进行选择，这个成员的配置也影响到I2C_OwnAddress1成员，只有这里设置成10位模式时，I2C_OwnAddress1才支持10位地址。

配置完这些结构体成员值，调用库函数I2C_Init即可把结构体的配置写入到寄存器中。


## 代码实现
### I2C.h
```c
#ifndef __I2C_H
#define __I2C_H	 
#include "sys.h"

#define I2CPORT		GPIOB	//定义IO接口
#define I2C_SCL		GPIO_Pin_6	//定义IO接口
#define I2C_SDA		GPIO_Pin_7	//定义IO接口

#define HostAddress	0xC0	//总线主机的器件地址
#define BusSpeed	200000	//总线速度（不高于400000）


void I2C_Configuration(void);
void I2C_SAND_BUFFER(u8 SlaveAddr, u8 WriteAddr, u8* pBuffer, u16 NumByteToWrite);
void I2C_SAND_BYTE(u8 SlaveAddr,u8 writeAddr,u8 pBuffer);
void I2C_READ_BUFFER(u8 SlaveAddr,u8 readAddr,u8* pBuffer,u16 NumByteToRead);
u8 I2C_READ_BYTE(u8 SlaveAddr,u8 readAddr);
		 				    
#endif

```
### I2C.c

```c
#include "i2c.h"


void I2C_GPIO_Init(void){ //I2C接口初始化
	GPIO_InitTypeDef  GPIO_InitStructure; 	
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA|RCC_APB2Periph_GPIOB|RCC_APB2Periph_GPIOC,ENABLE);       
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_I2C1, ENABLE); //启动I2C功能 
    GPIO_InitStructure.GPIO_Pin = I2C_SCL | I2C_SDA; //选择端口号                      
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD; //选择IO接口工作方式       
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz; //设置IO接口速度（2/10/50MHz）    
	GPIO_Init(I2CPORT, &GPIO_InitStructure);
}

void I2C_Configuration(void){ //I2C初始化
	I2C_InitTypeDef  I2C_InitStructure;
	I2C_GPIO_Init(); //先设置GPIO接口的状态
	I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;//设置为I2C模式
	I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
	I2C_InitStructure.I2C_OwnAddress1 = HostAddress; //主机地址（从机不得用此地址）
	I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;//允许应答
	I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit; //7位地址模式
	I2C_InitStructure.I2C_ClockSpeed = BusSpeed; //总线速度设置 	
	I2C_Init(I2C1,&I2C_InitStructure);
	I2C_Cmd(I2C1,ENABLE);//开启I2C					
}

void I2C_SAND_BUFFER(u8 SlaveAddr,u8 WriteAddr,u8* pBuffer,u16 NumByteToWrite){ //I2C发送数据串（器件地址，内部地址，寄存器，数量）
	I2C_GenerateSTART(I2C1,ENABLE);//产生起始位
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT)); //清除EV5
	I2C_Send7bitAddress(I2C1,SlaveAddr,I2C_Direction_Transmitter);//发送器件地址
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));//清除EV6
	I2C_SendData(I2C1,WriteAddr); //内部功能地址
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED));//移位寄存器非空，数据寄存器已空，产生EV8，发送数据到DR既清除该事件
	while(NumByteToWrite--){ //循环发送数据	
		I2C_SendData(I2C1,*pBuffer); //发送数据
		pBuffer++; //数据指针移位
		while (!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED));//清除EV8
	}
	I2C_GenerateSTOP(I2C1,ENABLE);//产生停止信号
}
void I2C_SAND_BYTE(u8 SlaveAddr,u8 writeAddr,u8 pBuffer){ //I2C发送一个字节（从地址，内部地址，内容）
	I2C_GenerateSTART(I2C1,ENABLE); //发送开始信号
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT)); //等待完成	
	I2C_Send7bitAddress(I2C1,SlaveAddr, I2C_Direction_Transmitter); //发送从器件地址及状态（写入）
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED)); //等待完成	
	I2C_SendData(I2C1,writeAddr); //发送从器件内部寄存器地址
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED)); //等待完成	
	I2C_SendData(I2C1,pBuffer); //发送要写入的内容
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED)); //等待完成	
	I2C_GenerateSTOP(I2C1,ENABLE); //发送结束信号
}
void I2C_READ_BUFFER(u8 SlaveAddr,u8 readAddr,u8* pBuffer,u16 NumByteToRead){ //I2C读取数据串（器件地址，寄存器，内部地址，数量）
	while(I2C_GetFlagStatus(I2C1,I2C_FLAG_BUSY));
	I2C_GenerateSTART(I2C1,ENABLE);//开启信号
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT));	//清除 EV5
	I2C_Send7bitAddress(I2C1,SlaveAddr, I2C_Direction_Transmitter); //写入器件地址
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));//清除 EV6
	I2C_Cmd(I2C1,ENABLE);
	I2C_SendData(I2C1,readAddr); //发送读的地址
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED)); //清除 EV8
	I2C_GenerateSTART(I2C1,ENABLE); //开启信号
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT)); //清除 EV5
	I2C_Send7bitAddress(I2C1,SlaveAddr,I2C_Direction_Receiver); //将器件地址传出，主机为读
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED)); //清除EV6
	while(NumByteToRead){
		if(NumByteToRead == 1){ //只剩下最后一个数据时进入 if 语句
			I2C_AcknowledgeConfig(I2C1,DISABLE); //最后有一个数据时关闭应答位
			I2C_GenerateSTOP(I2C1,ENABLE);	//最后一个数据时使能停止位
		}
		if(I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_RECEIVED)){ //读取数据
			*pBuffer = I2C_ReceiveData(I2C1);//调用库函数将数据取出到 pBuffer
			pBuffer++; //指针移位
			NumByteToRead--; //字节数减 1 
		}
	}
	I2C_AcknowledgeConfig(I2C1,ENABLE);
}
u8 I2C_READ_BYTE(u8 SlaveAddr,u8 readAddr){ //I2C读取一个字节
	u8 a;
	while(I2C_GetFlagStatus(I2C1,I2C_FLAG_BUSY));
	I2C_GenerateSTART(I2C1,ENABLE);
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT));
	I2C_Send7bitAddress(I2C1,SlaveAddr, I2C_Direction_Transmitter); 
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED));
	I2C_Cmd(I2C1,ENABLE);
	I2C_SendData(I2C1,readAddr);
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_BYTE_TRANSMITTED));
	I2C_GenerateSTART(I2C1,ENABLE);
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_MODE_SELECT));
	I2C_Send7bitAddress(I2C1,SlaveAddr, I2C_Direction_Receiver);
	while(!I2C_CheckEvent(I2C1,I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED));
	I2C_AcknowledgeConfig(I2C1,DISABLE); //最后有一个数据时关闭应答位
	I2C_GenerateSTOP(I2C1,ENABLE);	//最后一个数据时使能停止位
	a = I2C_ReceiveData(I2C1);
	return a;
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


