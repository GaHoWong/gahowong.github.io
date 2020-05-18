# HT32F5232单片机学习之四种方法实现按键控制LED灯




------

学会了如何点亮LED灯，那么接下来的就是如何把手中的这颗LED灯玩出新花样，在上一篇文章的基础上优化了第一次写代码的不足，加入了四种按键控制led灯 的方法。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200216224915803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
根据电路原理图可知，按键一端分别连接PA0和PA1，另一端连接GND。也就是说当PA0或PA1按下的时候，按键与GND导通，使得IO端口短接到GND，只要我们把IO口设为高电平然后通过程序读取IO端口的电平状态就能判断按键状态，*如果为低电平则表示按键为按下状态，如果为高电平则按键为松开状态*这种连接方式是最经典的按键连接方式。

**key.c**
```c
#include "key.h"

void KEY_Init(void){ 
	
	 HT_GPIO_TypeDef* GPIO_GROUP;                                                 //枚举
   GPIO_GROUP = HT_GPIOA;                                                       //以后只要修改这里即可                                    
	 AFIO_GPxConfig(KEY_GPIO_GROUP, KEY1_PIN|KEY2_PIN, KEY_AFIO_MODE);            //配置AFIO模式	
	 GPIO_InputConfig(GPIO_GROUP,KEY1_PIN|KEY2_PIN, ENABLE);	                     //输入使能函数，此函数可实现GPIO口变为输入模式，上拉电阻，默认电流。因此这个函数可替换以下三个被注释的函数
//  GPIO_DirectionConfig(GPIO_GROUP,KEY1_PIN|KEY2_PIN, GPIO_DIR_IN);             //配置GPIO模式：GPIO_DIR_OUT输出orGPIO_DIR_IN输入                                                                             */
//  GPIO_PullResistorConfig(GPIO_GROUP,KEY1_PIN|KEY2_PIN, GPIO_PR_UP);           //上拉电阻，使IO口为高电平
//	GPIO_DriveConfig(GPIO_GROUP,LED1_PIN|LED2_PIN, GPIO_DV_4MA);                 //选择指定GPIO引脚的驱动电流，可选GPIO_DV_4/8/12/16MA 


}
 
```
GPIO_InputConfig()这个函数可以取代GPIO_DirectionConfig（） GPIO_PullResistorConfig（）GPIO_DriveConfig（）这个三个函数的功能，此函数可实现GPIO口变为输入模式，上拉电阻，默认电流。


**key.h**
```c
#ifndef __KEY_H
#define __KEY_H	 
#include "sys.h"


#define KEY_GPIO_GROUP             (GPIO_PA)
#define KEY1_PIN                   (GPIO_PIN_0)
#define KEY2_PIN                   (GPIO_PIN_1)
#define KEY_AFIO_MODE              (AFIO_MODE_DEFAULT) //默认模式：AFIO_MODE_DEFAULT ，AFIO_MODE_1~15对应模式1~15

void KEY_Init(void);//初始化

		 				    
#endif

```
**led.c**
```c
#include "led.h"
 

 
void LED_Init() //led初始化函数
{
	
	 HT_GPIO_TypeDef* GPIO_GROUP;                                                 //枚举
   GPIO_GROUP = HT_GPIOC;                                                       //以后只要修改这里即可 
 	 AFIO_GPxConfig(LED_GPIO_GROUP, LED1_PIN|LED2_PIN, LED_AFIO_MODE);            //配置AFIO模式	
   GPIO_DirectionConfig(GPIO_GROUP,LED1_PIN|LED2_PIN, GPIO_DIR_OUT);            //配置GPIO模式：GPIO_DIR_OUT输出orGPIO_DIR_IN输入                                                                              */
   GPIO_PullResistorConfig(GPIO_GROUP,LED1_PIN|LED2_PIN, GPIO_PR_DISABLE);      //上拉电阻，使IO口为高电平
	 GPIO_DriveConfig(GPIO_GROUP,LED1_PIN|LED2_PIN, GPIO_DV_8MA);                 //选择指定GPIO引脚的驱动电流，可选GPIO_DV_4/8/12/16MA 

}


```

**led.h**

```c
#ifndef _LED_H
#define _LED_H
 
#include "sys.h"


#define LED_GPIO_GROUP             (GPIO_PC)
#define LED1_PIN                   (GPIO_PIN_14)
#define LED2_PIN                   (GPIO_PIN_15)
#define LED_AFIO_MODE              (AFIO_MODE_DEFAULT) //默认模式：AFIO_MODE_DEFAULT ，AFIO_MODE_1~15对应模式1~15
void LED_Init(void);

#endif
```
**sys.c**
```c
#include "sys.h"

void NVIC_Configuration(void){ //嵌套中断向量控制器 的设置
    //NVIC_PriorityGroupConfig(NVIC_PriorityGroup_2);	//设置NVIC中断分组2:2位抢占优先级，2位响应优先级
}

void RCC_Configuration(void){ //RCC时钟设置  
	
	CKCU_PeripClockConfig_TypeDef CCLOCK = {{0}};//不开启外设时钟相应功能无法使用
	CCLOCK.Bit.PA    = 1;//开启PA时钟
	CCLOCK.Bit.PB    = 0;//开启PB时钟
	CCLOCK.Bit.PC    = 1;//开启PC时钟
	CCLOCK.Bit.PD    = 0;//开启PD时钟
	CCLOCK.Bit.AFIO  = 1;//开启复用功能时钟
	CKCU_PeripClockConfig(CCLOCK, ENABLE);//使能时钟
}

```
**sys.h**
```c
#ifndef __SYS_H
#define __SYS_H	
#include "ht32f5xxxx_01.h"
																	    

void NVIC_Configuration(void); //嵌套中断控制器的设置
void RCC_Configuration(void); //RCC时钟类的设置

#endif

```

**main()**

```c
int main (void){//主程序
	u8 a; //定义变量
	//初始化程序
	RCC_Configuration(); //时钟设置
	LED_Init();//LED初始化
	KEY_Init();//按键初始化

	//主循环
	while(1){

		//示例1：无锁存
		if(GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)){ //读按键接口的电平
			GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,RESET); //LED灯亮 
		}else{	
        	GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,SET); //LED灯灭
		}

		//示例2：无锁存
  GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN));
		//示例3：有锁存
		if(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)){ //读按键接口的电平
			delay_ms(20); //延时去抖动
			if(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)){ //读按键接口的电平
				GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,(1-GPIO_ReadOutBit (HT_GPIOC,LED1_PIN))); //LED取反
				while(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)); //等待按键松开 
				}
		}

		//示例4：有锁存
		if(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)){ //读按键接口的电平
			delay_ms(20); //延时20ms去抖动
			if(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)){ //读按键接口的电平
				//在2个LED上显示二进制加法
				a++; //变量加1
				if(a>3){ //当变量大于3时清0
					a=0; 
				}
				GPIO_SetOutBits (HT_GPIOC,LED1_PIN); //直接数值操作将变量值写入LED（LED在GPIOB组的PB0和PB1上）
				while(!GPIO_ReadInBit(KEY_GPIO_GROUP,KEY1_PIN)); //等待按键松开 
			}
		}
	}
}

```

代码中的两次判断延时20ms是为了按键去抖动，这些都是非常成熟的去抖方法了，这里就不再多说。![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zczAuYmFpZHUuY29tLzZPTldzamlwMFFJWjh0eWhucS9pdC91PTg4MzU5MzU0MCwxNTg2MDA0OTQ3JmZtPTE3MyZhcHA9NDkmZj1KUEVH?x-oss-process=image/format,png)

本人在学习stm32时，是杜洋老师教的，上面四种方式点亮led灯也是他教的，受他影响比较大，习惯性区块化编程，在上面示例三中会发现，编译器会发生警告
`..\User\main.c(83): warning:  #188-D: enumerated type mixed with another type
  				GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,(1-GPIO_ReadOutBit (HT_GPIOC,LED1_PIN))); //LED取反
..\User\main.c: 1 warning, 0 errors`
这是由于GPIO_ReadOutBit（）这个函数返回的是RESET OR SET,而不是0和1，在HT的标准库中没有STM32库的`typedef enum
{ Bit_RESET = 0,
  Bit_SET
}BitAction;`枚举定义，因此在HT中需要用户自己定义。才不会弹出warning。

------

[^undefined]:

[配置环境参考](https://blog.csdn.net/fengge2018/article/details/104058625)
