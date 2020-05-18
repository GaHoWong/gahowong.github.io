# HT32F5232单片机学习之点亮LED




------

  因为疫情的原因，今年在家实在是太无聊了，突然想起自己去年报名参加了个合泰杯比赛，本人也是菜鸟一个，之前从未接触过合泰的单片机，刚好手头上有一块HT32F52352的开发板，型号是ESK32-30501U2.1。上一届师兄扔在实验室里，我捡回来的，刚好也就把它带了回家。

对于学习一款单片机，最开始的任务配置完开发环境后，第一件事就是点亮LED了。

  关于这款单片机的资料在网上少之又少，在网上找了大量的资料，本着能不自己造轮子绝不自己造轮子的原则，终于完成了隆重的点灯仪式。

![点亮板载led](https://img-blog.csdnimg.cn/20200204180222760.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204180513754.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
由原理图可知，控制这两个LED灯的分别是PC14和PC15这两个脚，所以我们只要控制这两个口就能实现led的亮灭了。

**LED.H**
```c
#ifndef _LED_H
#define _LED_H
 
#include "ht32f5xxxx_01.h"
 
#define LED_GPIO_GROUP             (GPIO_PC)
#define LED1_PIN                   (GPIO_PIN_14)
#define LED2_PIN                   (GPIO_PIN_15)
#define LED_AFIO_MODE              (AFIO_FUN_GPIO)	
void LED_Init(void);

#endif

```
**LED.C**

```c
#include "led.h"
 
static void LED_CKCU_Config()  //配置时钟
{
	CKCU_PeripClockConfig_TypeDef CCLOCK = {{0}};//不开启外设时钟相应功能无法使用
	
	CCLOCK.Bit.PC    = 1;//开启PC时钟
	CCLOCK.Bit.AFIO  = 1;//开启复用功能时钟
	CKCU_PeripClockConfig(CCLOCK, ENABLE);//使能时钟
}
 
static void LED_GPIO_Config()  //配置GPIO口，使用ht32f5xxxx_gpio.c里的库函数配置IO功能
{
	HT_GPIO_TypeDef* GPIOgroup; 
	GPIOgroup = HT_GPIOC;  
	
	AFIO_GPxConfig(LED_GPIO_GROUP, LED1_PIN|LED2_PIN, LED_AFIO_MODE); //配置GPIO模式：AFIO_MODE_DEFAULT 默认，AFIO_MODE_1~15模式1~15
    GPIO_DirectionConfig(GPIOgroup,LED1_PIN|LED2_PIN, GPIO_DIR_OUT); //配置GPIO引脚的方向：GPIO_DIR_OUT输出orGPIO_DIR_IN输入                                                                              */
	GPIO_PullResistorConfig(GPIOgroup,LED1_PIN|LED2_PIN, GPIO_PR_DISABLE);//配置指定GPIO引脚的上下拉电阻。GPIO_PR_UP 带内部上拉电阻的引脚GPIO_PR_DOWN 带内部上拉电阻的引脚 GPIO_PR_DISABLE 没有拉电阻的引脚。 
	GPIO_DriveConfig(GPIOgroup,LED1_PIN|LED2_PIN, GPIO_DV_8MA);    //选择指定GPIO引脚的驱动电流，可选GPIO_DV_4/8/12/16MA 

}
 
void LED_Init() //led初始化函数
{
	LED_CKCU_Config();
	LED_GPIO_Config();
}


```
**main.c**

```c
#include "ht32.h"
#include "ht32_board.h"
#include "led.h"
#include "delay.h"
int main()
{
		LED_Init();
	while(1){
		
		GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,SET);
				GPIO_WriteOutBits(HT_GPIOC,LED2_PIN,RESET);
		delay_s(1);
		GPIO_WriteOutBits(HT_GPIOC,LED1_PIN,RESET);
				GPIO_WriteOutBits(HT_GPIOC,LED2_PIN,SET);
		delay_s(1);
	}
}


```
编译之前，记得设置好以下参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204181932657.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204181941580.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204182209449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
最后看下效果。。。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200204183055404.gif)
点灯仪式正式完成，欢迎大家一起交流学习！

------

[^undefined]:

[配置环境参考](https://blog.csdn.net/fengge2018/article/details/104058625)
