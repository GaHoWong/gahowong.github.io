# STM8s_PWM的实现


### 实验目的
在串口STM8s_UART串口的实现的基础上，实现下面的功能。

1.	使用Timer，在LED引脚(PB5或者PA3)产生一个100Hz的PWM信号，即周期为0.01秒
2.	该PWM信号的占空比由全局变量MyPWM控制，范围从1%~99%
### 实验内容与原理
1. 定时器实际上就是一个计数器，每个时钟周期，计数器的值自动加1

2. 定时器的时钟频率设置：

   Timer1的时钟频率 = fCK_PSC / (PSCR[15:0]+1)
   Timer2/4的时钟频率 = fCK_PSC / 2PSCR[3:0]
   其中：fCK_PSC = fMASTER，PSCR为寄存器

3. 将定时器设置为比较(compare)模式，即可产生PWM信号，把定时器的值和ARR(auto reload register)寄存器的值比较，如果相等就发生中断。

4. PWM信号的占空比 = (高电平时间 / 周期) * 100%
   例如：周期为0.01秒时，如果高电平时间为0.0001秒(0.1毫秒)，占空比就为1%

![](https://img-blog.csdnimg.cn/20200528181328502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

现在，我想要设置TIM2_CH1输出PWM波形

#### 一、	配置TIM2预分频寄存器（PSCR）
![pscr](https://img-blog.csdnimg.cn/20200528181452879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

PSC [2:0]：分频器的值分频器的值除分频时钟CK_PSC计数器时钟频率  `fCK_CNT =fCK_PSC / 2(PSC[2:0])` 

PSC中包含了每次更新事件(包含当通过TIM4_EGR寄存器的UG产生的更新事件)需要加载到实际分频寄存器的值这就意味着为了使新的分频器值启用，必须产生一次更新事件。

`TIM2_PSCR=0x00;` //分频值=0 16M

设置预分频的值为16M那么计数周期为1/16us，也就是说计数器会每隔1/16us计数一次。

如果`TIM2_PSCR = 0x07`，即`fCK_CNT=fCK_PSC/2^7=125Khz。`

#### 二、配置自动装载寄存器（ARR）
![arr](https://img-blog.csdnimg.cn/20200528181626405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![arr](https://img-blog.csdnimg.cn/20200528181730868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

当自动重装载寄存器=0时，计数器处于阻塞状态，也就是不计数状态，因为当`CNTR=ARR`时，CNTR就会清零，所以配置时自动重装载寄存器应该大于0。

另外自动重装载寄存器的值就是PWM波形的周期，比如`ARR=0X0100`，PWM的周期为0x0100*1/16=16us,PWM的周期就是16us。

实验要求是产生一个100Hz的PWM信号，即周期为0.01秒。

因为125Khz/1.25Khz = 100hz

所以自动重装载寄存器ARR的值应当为(1250-1) = 1249= 0x04e2

```c
  TIM2_ARRH = 0x04;  
  TIM2_ARRL = 0xe2;
```
#### 三、配置捕获/比较使能寄存器1(TIMx_CCER1)
![ccer](https://img-blog.csdnimg.cn/20200528181825544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![ccr](https://img-blog.csdnimg.cn/20200528181846397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

这个寄存器的配置可以选通相应的Tim2通道。

bit5，bit4为ch2配置，bit1,bit0为ch1配置

如果当前OC1为输出通道，则

bit1:OC1低电平有效

bit0:OC1信号被输出到当前引脚上

```c
TIM2_CCER1 |= 0x03;  //开启－ OC1信号输出到对应的输出引脚。OC1低电平有效
```
#### 四、配置捕获/比较模式寄存器1(TIMx_CCMR1)
![ccmr](https://img-blog.csdnimg.cn/20200528181951925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![ccmr](https://img-blog.csdnimg.cn/20200528182010215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

01101000 = 0x68，即选择PWM模式一，开启TIMx_CCR1寄存器的预装载功能，CC1通道被配置为输出。

```c
TIM2_CCMR1 |= 0x68;  //开启预装载功能，PWM模式1。
```
#### 五、配置捕获/比较寄存器1 (TIMx_CCR1)
![ccr](https://img-blog.csdnimg.cn/20200528182133517.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![ccr](https://img-blog.csdnimg.cn/20200528182148281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

这个寄存器决定着PWM的占空比。

CCR/ARR=PWM的占空比， PWM的周期的占空比=(高电平时间 / 周期) * 100%

#### 六、中断使能寄存器（TIMx_IER）
![ier](https://img-blog.csdnimg.cn/20200528182234695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

中断使能寄存器，如果需要中断可以在此设置。

bit1:CC1E 捕获/比较1中断使能

0：CC1 中断不使能

1：CC1中断使能

```c
TIM2_IER = 0x00; //不使能 
```
#### 七、控制寄存器（CR）
![cr](https://img-blog.csdnimg.cn/20200528182346998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

Bit 0 CEN：使能计数器

0：禁止计数器；

1：使能计数器。

```bash
TIM2_CR1 = 0x01;     //使能计数器
```

#### 实现代码

```c
#include <iostm8s103f3.h>
#include <intrinsics.h>
#include <stdio.h>

#define UART1_FLAG_TXE  (unsigned char)0x80  // 数据发送寄存器 空标志
#define UART1_FLAG_RXNE (unsigned char)0x20  // 数据接收寄存器 非空标志


typedef   signed char     s8;
typedef   signed short    s16;
typedef   signed long     s32;
typedef unsigned char     u8;
typedef unsigned short    u16;
typedef unsigned long     u32;

u8 TxWPtr=0, TxRPtr=0;  //发送接收标志
u8 UART_RxBuffer[32];   //接收缓存
u8 UART_TxBuffer[64];   //发送缓存
u8 USART1_RX_STA = 0;   //接收状态标记，最高两位是状态标志位，其余低位是数据位
int MyPWM;

void UART_init(){
  //设置波特率为115200
  UART1_BRR2 = 0x0B;  
  UART1_BRR1 = 0x08; 
  
  UART1_CR1 = 0x00;  //使能uart，确定一个起始位，8个数据位
  UART1_CR2 = 0x2c;  //1010 1100发送接收中断、发送、接收->使能
  UART1_CR3 = 0x00;  //一位停止位
}
//发送一个字节
void UART1_SendByte(u8 data)
{
  UART_TxBuffer[TxWPtr]=data;
  TxWPtr++;
  if(TxWPtr > 63)//数组下标溢出复位
  {
    TxWPtr = 0;
    TxRPtr = 0;
  }
  UART1_CR2 |= 0x80;
}

//发生字符串
void UART1_SendString(u8 Str[])
{
  u8 i = 0;
  while(Str[i])
    UART1_SendByte(Str[i++]);
}
//发送一个整数
void uart_SendInt(int x)
{
  static const char dec[] = "0123456789";
  unsigned int div_val = 10000;

  if (x < 0){
    x = - x;
    UART1_SendByte('-');
  }
  while (div_val > 1 && div_val > x)
    div_val /= 10;
  do{
    UART1_SendByte (dec[x / div_val]);
    x %= div_val;
    div_val /= 10;
  }while(div_val);
}


int fputc(int ch, FILE *f)
{  
 //重定向printf函数,将Printf内容发往串口
  UART1_DR = (u8)ch;
  while (!(UART1_SR & UART1_FLAG_TXE));
  return (ch);
}
/*
如果TIM2_PSCR = 0x07，即fCK_CNT=fCK_PSC/2^7=125Khz
实验要求是产生一个100Hz的PWM信号，即周期为0.01秒。
因为125Khz/1.25Khz = 100hz
所以自动重装载寄存器ARR的值应当为(1250-1) = 1249， 
PWM的周期的占空比=(高电平时间 / 周期) * 100%
PWM的占空比通过捕获/比较寄存器CCR来决定
即CCR的值/ARR的值 = pwm占空比
*/

void Tim2_PWM_Init(){
  /*
  TIM2的时钟频率：125Khz
  TIM2的工作模式：比较模式
  PWM频率：100hz
  PWM周期：0.01s
  PWM模式：模式1
  中断使能：否
  使用TIM2_CH1(PD4)
  */
  TIM2_PSCR = 0x07;    //设置预分频器，时钟频率fCK_CNT等于fCK_PSC/2^(PSC[3:0])。
  TIM2_ARRH = 0x04;    //自动重装的值 = 1249 = 0x04e2 
  TIM2_ARRL = 0xe2;        
  TIM2_CCER1 |= 0x03;  //开启－ OC1信号输出到对应的输出引脚。OC1低电平有效
  TIM2_CCMR1 |= 0x68;  //开启预装载功能，PWM模式1。
  TIM2_IER = 0x00;     //中断使能寄存器。如果需要中断在这里开，参考手册232页
  TIM2_CR1 = 0x01;     //使能计数器
}


void DutyCycle(u16 PWM_DutyCycle){
  //设置占空比
  u16 DutyCycle = 0;
  DutyCycle = (u16)((PWM_DutyCycle/100.0)*1250);//转换
  TIM2_CCR1H = (u8)(DutyCycle>> 8);
  TIM2_CCR1L = (u8)(DutyCycle);
}


int main( void)
{ 
  int MyPWM = 0;
  USART1_RX_STA = 0x0000;    //初始化接收标志位
  CLK_CKDIVR &= 0x00;        //复位
  CLK_CKDIVR |= 0x00;        //置时钟为内部16M高速时钟
  UART_init();               //初始化uart
  Tim2_PWM_Init();           //时钟pwm初始化
  __enable_interrupt();      //开启全局中断
  while(1){
    if(USART1_RX_STA&0x80)//判断是否按下回车
    {
      u8 len = 0;
      len=USART1_RX_STA&0x7f;//得到数据长度
      //如果输入超过4个字符，提示错误
      if(len>0x04){UART1_SendString("input error\r\n");}
      //果输入小于4个字符，判断第0、1位是否是‘d’和空格，第2、3位是否是0~9
      else if((len<=0x04)&&(UART_RxBuffer[0]=='d')&&(UART_RxBuffer[1]==' ')\
              &&(UART_RxBuffer[2]>=0x30&&(UART_RxBuffer[2]<=0x39))\
              &&(UART_RxBuffer[3]>=0x30&&(UART_RxBuffer[3]<=0x39)))
        {
          //判断是否输入的是“d <一个数字>”的情况，若是则第2位赋给个位
          if((len==0x03))MyPWM = (int)((UART_RxBuffer[2]-'0'));
          //如果是“d <两个数字>”，则把第2位赋值为十位，第3位赋值为个位
          else MyPWM = (int)((UART_RxBuffer[2]-'0')*10) +(int)(UART_RxBuffer[3]-'0');
          {
            DutyCycle((u16)MyPWM);
	    printf("MyPWM = %d(Test)\r\n",MyPWM);//pwm调试用
          }
        }
       //判断是否输入两个字符，且第0位为'?'
       else if((len==0x01)&&(UART_RxBuffer[0]==('?')))
       {
         UART1_SendString("duty cycle is ");
         uart_SendInt(MyPWM);
         UART1_SendString("\r\n");
        }
       //如果都不是以上的情况，则输出error。比如"d -d""d 8-"这种错误输入
       else{printf("input error\r\n");}
      
       USART1_RX_STA=0x00; //将串口数据标志位清0

      }

    }
}


//接收中断服务函数
#pragma vector = UART1_R_RXNE_vector
__interrupt void UART1_R_RXNE_IRQHandler(void)
{
  u8 Res;
  if(( USART1_RX_STA&0x80)==0)
  {//如果没接收到回车
    Res =(u8)UART1_DR;         
    printf("%c",Res);          //向屏幕打印刚刚输入的字符  
    if(Res==0x0d)              //如果收到了回车
    {
      USART1_RX_STA|=0x80;     //标志位最高位置1
      printf("\r\n");          //为了使刚刚输入的字停留在屏幕不被覆盖
    }
    else
    {
      UART_RxBuffer[USART1_RX_STA&0X7f]=Res; //对没回车之前的字符存入RxBuffer
      USART1_RX_STA++;
      if(USART1_RX_STA > 31){USART1_RX_STA = 0;}
    }
  }
}


//发送中断服务函数
#pragma vector = UART1_T_TXE_vector
__interrupt void UART1_T_TXE_IRQHandler(void)
{
  if( TxWPtr == TxRPtr)
  {
   UART1_CR2 &= 0x7F;
   TxRPtr=0;                              
   TxWPtr=0;
  }
  else{UART1_DR = UART_TxBuffer[TxRPtr++];}   
}
```
