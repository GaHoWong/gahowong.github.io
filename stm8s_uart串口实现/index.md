# STM8s_UART串口的实现




### 一、实验目的
1.	在程序中定义一个全局变量：int MyPWM = 0;
2.	在终端输入命令：d num回车，把num的值赋给MyPWM，其中num的范围为1~99
3.	在终端输入命令：?回车，显示：duty cycle is num，其中num指MyPWM的值
4.	串口接收使用中断
5.	串口发送使用中断。
### 二、实验原理
1.	串口发送原理：把要发送的字符写入到UART_DR(Data register)寄存器，单片机的串口控制器就会自动把这个字符发送出去，但发送前要检查串口控制器是否空闲，即上一次的数据是否发送完毕。
2.	串口发接收原理：当串口接收到一个字符后，串口控制器会自动把该字符保存到UART_DR寄存器，并把UART_SR的RXNE bit设为1。程序读接收到的字符之前，要先检查RXNE bit是否为1。
3.	串口发送与接收中断：
如果要使用中断，必须激活全局中断，_enable_interrupt()是激活全局中断函数。
TXE中断的触发条件是：发送数据寄存器为空。如果要使用中断的方式发送字符串，需要以下几个步骤：
a)	定义一个数组来缓冲将要发送的字符串，例如: unsigned char UART_TxBuffer[64];

b)	在设置阶段，把TXE中断禁用(TXE中断要根据需要打开和关闭)，把RXNE中断打开，例如UART1_CR2 = 0x2C;

c)	uart_SendString()和uart_SendInt()不用修改，但要对uart_SendByte()函数进行修改，例如：

```c
void uart_SendByte(unsigned char data)
{
  // 把data保存到UART_TxBuffer[]中合适的位置

  UART1_CR2 |= 0x80;  // 打开TXE中断
}


```

d)	TXE中断处理函数流程如下：

```c
#pragma vector = UART1_T_TXE_vector
__interrupt void UART1_T_TXE_IRQHandler(void)
{
  // 检查UART_TxBuffer[]数组的内容是否全部发送完成c
  if (没有全部完成)
  {
    // 记住当前发送到第几个字符
    UART1_DR = UART_TxBuffer[合适的下标];
  }
  else 
    UART1_CR2 &= 0x7F;  // 关闭TXE中断
}
```



### 三、实现代码

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

u8 TxWPtr=0, TxRPtr=0;   //发送接收标志
u8 UART_RxBuffer[32];
u8 UART_TxBuffer[64];
u8 USART1_RX_STA = 0;//接收状态标记，最高两位是状态标志位，其余低位是数据位
int MyPWM;

void UART_init()
{                    
  UART1_BRR2 = 0x0B;  //设置波特率为115200
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

  if (x < 0)
  {
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




int main( void)
{ 
  int MyPWM = 0;
  USART1_RX_STA=0x0000;
  CLK_CKDIVR &= 0x00; //复位
  CLK_CKDIVR |= 0x00; //置时钟为内部16M高速时钟
  UART_init();        //初始化uart
  __enable_interrupt();//开启全局中断
  while(1){
      if(USART1_RX_STA&0x80)//判断是否按下回车
      { 
        u8 len = 0;
        len=USART1_RX_STA&0x7f;//得到数据长度
        if(len>0x04){UART1_SendString("input error\r\n");}//如果输入超过4个字符，提示错误
          //如果输入小于4，判断第0、1位是否是‘d’和空格，第2、3位是否是0~9
        else if((len<=0x04)&&(UART_RxBuffer[0]=='d')&&(UART_RxBuffer[1]==' ')&&\
          (UART_RxBuffer[2]>=0x30&&(UART_RxBuffer[2]<=0x39))&&\
          (UART_RxBuffer[3]>=0x30&&(UART_RxBuffer[3]<=0x39)))
         {  
            //判断是否输入的是“d <一个数字>”的情况，第2位赋给个位
           if((len==0x03))MyPWM = (int)((UART_RxBuffer[2]-'0'));
            //如果是“d <两个数字>”，则把第2位赋值为十位，第3位赋值为个位
           else 
           {
             MyPWM = (int)((UART_RxBuffer[2]-'0')*10) +(int)(UART_RxBuffer[3]-'0');
             printf("MyPWM = %d(Test)\r\n",MyPWM);//pwm调试用
             
           }
          }
         //判断是否输入两个字符，且第0位为'?'
        else if((len==0x01)&&(UART_RxBuffer[0]=='?'))
        {
          UART1_SendString("duty cycle is ");
          uart_SendInt(MyPWM);
          UART1_SendString("\r\n");
        }
        else{printf("input error\r\n");}//如果用户输入类似“d -d”“d 8-”则提示错误
        
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
  }
  else
  { 
    UART1_DR = UART_TxBuffer[TxRPtr++];
  } 
}
```
### 异常问题
#### 异常问题一：时钟分频寄存器(CLK_CKDIVR)配置错误，导致主频与预期不一致的问题。
问题分析：解决这个问题之前，首先我们明确了要使用uart的波特率为115200 ，而设置波特率是通过UART_BRR2和UART_BRR1这两个寄存器设置的。这两个寄存器设置的方法根据公式（图1.1）通过配置16位除法器UART_DIV来设置。设置方法如（图1.2）

![图1.1](https://img-blog.csdnimg.cn/20200528175448345.png)
图1.1
![图1.2](https://img-blog.csdnimg.cn/20200528175544136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

图1.2

我们需要单片机以最高速度16Mhz运行，uart的波特率为115200,因此UART_DIV = 16 000 000/115200，UART_DIV = 139d = 008bh，因此在此实验中，UART_BBR1 = 08, UART_BBR1 = 0b。也就是这两行代码的由来。
![代码](https://img-blog.csdnimg.cn/20200528175816362.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528175840655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

由上面的分析和时钟树可以得出，如果我们需要正确的配置波特率，是由主频决定的。如果主频与预期的配置不一致，会导致单片机串口波特率与PC端不一致，最终发生错误，串口接收不到任何数据。
![1](https://img-blog.csdnimg.cn/20200528175904722.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

最终我发现，在我的代码中，有一段是这样写的，`CLK_CKDIVR|= 0x00`;于是我定于了一个全局变量x，将 `CLK_CKDIVR|= 0x00`的值赋给x来查看寄存器当前的值。
![1](https://img-blog.csdnimg.cn/20200528175945999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

通过查询参考手册，我发现这个寄存器有一个复位值：0x18，这是至关重要的点，正是由于忽略了这个复位值，导致了整个项目运行出错。在stm8s中，寄存器的复位值是一开始就存在的，而我刚开始用0x00相或，得出的结果还是0x18。并没有对寄存器做任何操作。而我们想要的是内部高速时钟频率为16Mhz，cpu工作频率也为16Mhz，这样才能使uart串口的波特率为115200。因此时钟分频寄存器(CLK_CKDIVR)的值应为0x00。
![1](https://img-blog.csdnimg.cn/20200528180010663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

#### 解决方法：
（1）因为相或的方法更利于代码的阅读，因此如果继续利用相或的方法，在此之前，要先查询参考手册，确认寄存器是否有复位值，如果有复位值，则先进行一次与运算，复位寄存器，再进行或运算。例如：

```c
CLK_CKDIVR &= (unsigned char)(~0x18);//复位
CLK_CKDIVR |=0x00;
```

或是

```c
CLK_CKDIVR &=0x00;//复位
CLK_CKDIVR |=0x00;
```

(2)直接利用与运算，但如果要设置其它分频的话就没那么直观了。

```c
CLK_CKDIVR &= (unsigned char)(~0x18);
```
#### 异常问题二：重定向printf函数问题
**问题描述：** IAR编译报错：Error[Pe020]: identifier "FILE" is undefined.Error while running C/C++ Compiler

**问题分析：** printf 是“stdio.h”中的函数，STM8想要用printf就必须用fputc去重定向，才能使用printf函数。在IAR中，库默认为常规配置。也就是没有语言环境接口、C语言环境，不支持文件描述符，在printf和scanf中没有多字节，并且在strtod中没有十六进制浮点数。导致了编译出错。因此，我们要使用完整的配置。

#### 解决方法：
需要在IAR的Project -> Options -> General Options ->Library Configuration里设置一下函数库，不然printf函数不对，将Library Configuration 中的Library 设置由"Normal"改为"Full"就可以了。 
![1](https://img-blog.csdnimg.cn/20200528180310446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 异常问题三：发送中断的溢出导致程序崩溃
问题描述：在发送一个字节时，定义了一个数组来接收数据，但数组的大小只要64，最初我们并没有意识到数组溢出的问题。直到有一次发送了很多字节，导致了程序崩溃。

```c
//刚开速的代码
void UART1_SendByte(u8 data)
{
  UART_TxBuffer[TxWPtr]=data;
  TxWPtr++;
  UART1_CR2 |= 0x80;
}
```
#### 解决方法：
增加溢出中断的检测，如果发现超出数组的大小，数组下标复位。
```c
//改进后的代码
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
```
