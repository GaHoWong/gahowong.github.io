# HT32F5232学习之USART串口实现发送接收以及对三种接收方式的思考




------

HT32F52352的资料在网上实在是太少了（相对于stm32），花了很长时间，利用的是ESK32-30501v2.1这块板子终于实现了USART串口的发生和接收，接收中断、FIFO功能。以后开发其它模块可以直接拿来就用了。

由于这块板子相当于已经集成了CH340TTL转usb模块，**无需外接CH340**就可以在电脑端接收和发生数据。（*注意：板子上中间的跳帽需跳到左边才能实现此功能*）这块mcu一共有两组USART串口（USART0和USART1）在数据手册可以看到，可用于TX,RX的口却有非常多个。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200218160635415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
USART通常有三种模式接收方式，分别是**查询方式、中断方式、中断＋FIFO方式。**
**查询方式**就是代码不断的去查询寄存器状态。这种方式的好处就是实时性比较高，缺点就是太占用单片机的性能了，毕竟单片机也不单只是去查usart串口有无数据吧。

**中断方式**比较常用，这种方式的好处就是实时性比较高，实用于实时性很高的场景，缺点也很明显，那就是比较占用单片机性能。

**中断＋FIFO方式**则是对中断的补充，使用FIFO，可以在连续接收若干个数据后才产生一次中断，然后一起进行处理。这样可以提高接收效率，避免频繁进中断，适用于大数据传输。 但是使用FIFO接收多字节数据进中断不好的地方是实时性会受到一定的影响，对某些实时性要求高的场合，比如说要求UART收到某个特定字符就立刻停止发送数据这样一个场合，使用FIFO每多个字节进一次中断就不合适了。 所以说使用FIFO好处是1、避免频繁进中断，提高吞吐率 2、避免数据因没有及时处理而丢失。不好的地方是实时性受影响。


![在这里插入图片描述](https://img-blog.csdn.net/20150908134718702) 
**在现实中具体用那种方式，需要按照需求来确定。以下是HT32F52352的实现代码。**



**usart.c**
```c
#include "usart.h"
#include "ht32f5xxxx_gpio.h"




/**************************实现函数********************************************
函数说明：配置usart串口

*******************************************************************************/ 
void USART_Configuration(void)
{
  USART_InitTypeDef USART_InitStructure;

  AFIO_GPxConfig(USART_GPIO_GROUP, USART_TX_PIN, AFIO_FUN_USART_UART);
  AFIO_GPxConfig(USART_GPIO_GROUP, USART_RX_PIN, AFIO_FUN_USART_UART);

  /*
		波特率： 115200
		长度：   8bits
		停止位： 1位
	  校验位： 无			
	  模式：   正常模式
  */
  USART_InitStructure.USART_BaudRate = 115200;
  USART_InitStructure.USART_WordLength = USART_WORDLENGTH_8B;
  USART_InitStructure.USART_StopBits = USART_STOPBITS_1;
  USART_InitStructure.USART_Parity = USART_PARITY_NO;
  USART_InitStructure.USART_Mode = USART_MODE_NORMAL;
  USART_Init(COM1_PORT, &USART_InitStructure);

  // 使能 COM1_PORT  发送和接收 
  USART_TxCmd(COM1_PORT, ENABLE);
  USART_RxCmd(COM1_PORT, ENABLE);
	
	 //中断设置    
   NVIC_EnableIRQ(COM1_IRQn);

   USART_IntConfig(COM1_PORT, USART_FLAG_RXDR , ENABLE);
	 USART_IntConfig(COM1_PORT, USART_FLAG_TXDE , ENABLE);
	 
	 
	   /* 设置FIFO接收等级                                                                                   */
  USART_RXTLConfig(COM1_PORT, USART_RXTL_04);
}


/**************************实现函数********************************************
函数说明：接收中断服务函数

*******************************************************************************/ 
void COM1_IRQHandler(void)
{
	u8 data;
	
	if( USART_GetFlagStatus(COM1_PORT, USART_FLAG_RXDR ) )         //接收中断
	{
		data = USART_ReceiveData(COM1_PORT);                         //读取接收到的数据
		printf("data = %c\n",data);                                  //把收到的数据发送回电脑		
	}
}


/**************************实现函数********************************************
函数说明：FIFO

*******************************************************************************/ 
void USART_Tx(const char* TxBuffer, u32 length)
{
  int i;

  for (i = 0; i < length; i++)
  {
    while (!USART_GetFlagStatus(COM1_PORT, USART_FLAG_TXC));
    USART_SendData(COM1_PORT, TxBuffer[i]);
  }
}


/**************************实现函数********************************************
函数说明：发送一个字节

*******************************************************************************/ 
void Usart_Sendbyte(HT_USART_TypeDef* USARTx, u8 data)
{
	USART_SendData(USARTx, data);
	while (USART_GetFlagStatus(USARTx, USART_FLAG_TXDE) == RESET);		
}
 
/**************************实现函数********************************************
函数说明：发送数组

*******************************************************************************/ 
void Usart_SendArray(HT_USART_TypeDef* USARTx, u8 *array,u8 num)
{
	u8 i;
	for( i = 0;i < num;i++)
	{
		Usart_Sendbyte(USARTx,*array);
		array++;
	}
}
 /**************************实现函数********************************************
函数说明：发送字符串

*******************************************************************************/ 

void Usart_SendStr(HT_USART_TypeDef* USARTx, uint8_t *str)
{
	uint8_t i;
	for(i = 0;str[i] != '\0';i++)
	{
		Usart_Sendbyte(USARTx,str[i]);
	}
}

```
**usart.h**

```c
#ifndef __USART_H
#define __USART_H 			   
#include "ht32f5xxxx_usart.h"
#include "sys.h"
#define USART_GPIO_GROUP             (GPIO_PA)
#define USART_TX_PIN                 (GPIO_PIN_4)
#define USART_RX_PIN                 (GPIO_PIN_5)
#define USART_AFIO_MODE              (AFIO_FUN_USART_UART) //默认模式：AFIO_MODE_DEFAULT ，AFIO_MODE_1~15对应模式1~15
#define COM1_PORT                    (HT_USART1)
#define COM1_IRQn                    (USART0_IRQn)

void USART_Configuration(void);
void COM1_IRQHandler(void);
void Usart_Sendbyte(HT_USART_TypeDef* USARTx, u8 data);
void Usart_SendArray(HT_USART_TypeDef* USARTx, u8 *array,u8 num);
void Usart_SendStr(HT_USART_TypeDef* USARTx, uint8_t *str);
void USART_Tx(const char* TxBuffer, u32 length);
#endif

```

------

[^undefined]:

[配置环境参考](https://blog.csdn.net/fengge2018/article/details/104058625)
