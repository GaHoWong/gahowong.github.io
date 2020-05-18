# HT32F5232单片机学习之利用dealy延时函数实现呼吸灯效果




------

在上一篇文章中，我利用了延时函数实现了按键软件去抖的方法，现在利用系统的滴答定时器来实现精准延时的。在这块ESK32-30501v2.1的板子上有一个外部8MHZ高速晶振 (HSE)，它连接的是PB13和PB14。利用它作为时钟源，编写出精准的延时函数就是本篇文章的目的。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200217155616323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
**delay.c**

```c
#include "ht32_cm0plus_misc.h"
#include "delay.h"
//mS微秒级延时程序
void delay_us(u32 us)
{
	u32 i;
	SYSTICK_ClockSourceConfig(SYSTICK_SRC_STCLK);          //选择外部参考时钟作为SysTick时钟源。8MHZ
	SYSTICK_SetReloadValue(SystemCoreClock / 8 / 1000000); // 重装计数初值
	SYSTICK_IntConfig(DISABLE);                            // 是否开启中断
	SYSTICK_CounterCmd(SYSTICK_COUNTER_CLEAR);             //清空定时器
	SYSTICK_CounterCmd(SYSTICK_COUNTER_ENABLE);            //使能
	for( i = 0;i < us;i++ )
	{
		while( !( (SysTick->CTRL) & (1<<16) ) ); 
	}
 
	SYSTICK_CounterCmd(SYSTICK_COUNTER_DISABLE); //关闭
	SYSTICK_CounterCmd(SYSTICK_COUNTER_CLEAR);	 //复位清零
}

void delay_ms(u16 ms){ //mS毫秒级延时程序 	  
	while( ms-- != 0){
		delay_us(1000);	//调用1000微秒的延时
	}
}
 
void delay_s(u16 s){ //S秒级延时程序	 		  	  
	while( s-- != 0){
		delay_ms(1000);	//调用1000毫秒的延时
	}
} 


```
*毫秒级函数的实现方法就是调用1000次微秒级函数,秒级函数则是调用1000次毫秒级函数*
**delay.h**

```c
#ifndef __DELAY_H
#define __DELAY_H 			   
#include "ht32_cm0plus_misc.h"
void delay_s(u16 s);
void delay_ms(u16 ms);
void delay_us(u32 us);
#endif


```
**main()**

```c
#include "ht32.h"
#include "sys.h"
#include "led.h"
#include "delay.h"
#include "key.h"
int main()
{	
u8 MENU;
u16 t,i;
	//初始化程序
	RCC_Configuration(); //时钟设置
	LED_Init();
	//设置变量的初始值
	MENU = 0;
	t = 1;
	//主循环
	while(1){
		//菜单0
		if(MENU == 0){ //变亮循环
			for(i = 0; i < 2; i++){
GPIO_WriteOutBits(HT_GPIOC,LED1_PIN|LED2_PIN,SET);
				delay_us(t); //延时
GPIO_WriteOutBits(HT_GPIOC,LED1_PIN|LED2_PIN,RESET);
				delay_us(1001-t); //延时
			}
			t++;
			if(t==1000){
				MENU = 1;
			}
		}
		//菜单1
		if(MENU == 1){ //变暗循环
			for(i = 0; i < 2; i++){
GPIO_WriteOutBits(HT_GPIOC,LED1_PIN|LED2_PIN,SET);
				delay_us(t); //延时
GPIO_WriteOutBits(HT_GPIOC,LED1_PIN|LED2_PIN,RESET);
				delay_us(1001-t); //延时
			}
			t--;
			if(t==1){
				MENU = 0;
			}
		}		
	}
}
```
当时钟源被设定为8MHz时,如果要产生1ms 时间基准。那么他的计算方法就是(8M/8/1000) = 1ms.这里利用了延时函数实现了LED呼吸灯的效果 .

------

[^undefined]:

[配置环境参考](https://blog.csdn.net/fengge2018/article/details/104058625)
