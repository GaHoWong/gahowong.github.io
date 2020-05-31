# STM8s_实现bootloader


### 一、实验目的与要求
1.	串口下载固件：把串口接收到的固件保存到指定的Flash地址
2.	中断向量表转向：bootloader的中断向量表要转向到固件的中断向量表（Reset中断除外）
### 二、实验原理
1.	通过串口烧写固件：
根据MyHairboot.c的流程，可分为以下几步
–	开始(BOOT_HEAD)
–	接收并写Flash(BOOT_WRITE)
•	可以忽略BOOT_WRITE_INTERRUPT_VEC
•	此步骤有个简单的验证算法，通过变量verify实现
–	结束(BOOT_OK或者BOOT_ERR)
–	下载完毕后，需要重启开发板
如果一段时间内没有从串口接收到数据，就会跳转到固件(开始执行固件)
–	asm(“JP $8200”);  // asm函数用于调用汇编指令
2.	中断向量表转向
1)	当固件的中断发生时，CPU会去查找位于0x8000的bootloader的中断向量表
2)	bootloader的中断向量表需要转向到固件的中断向量表，以便找到固件定义的中断处理函数
a)	Bootloader不知道固件中断处理函数的物理地址
固件中断处理函数的物理地址是在链接固件时产生的，并保存在固件的中断向量表
b)	Bootloader的Reset中断不用转向：这样当固件的Reset中断发生后，会调用bootloader的Reset中断处理函数__iar_program_start()，重启bootloader，然后由bootloader重新运行固件

### 三、实验过程
仔细阅读以下参考资料，特别是后两个，包括使用说明
1.	en.CD00176595.pdf和en.stsw-stm8006.zip：ST公司官方关于Bootloader的资料和例程；
2.	hairBoot-master.zip：资料1的简化版，针对STM8S低端型号，https://github.com/Zepan/hairBoot；
3.	stm8s_uploader.rar：资料2的优化版，并附带上位机程序，http://bbs.21ic.com/icview-1251224-1-1.html；
主要是使用它所带的上位机程序：stm8s_uploader.exe

注意：使用上位机(stm8s_uploader.exe)时，不能选中”自动调整中断映射表”。

实现一个Bootloader，能从串口下载并运行固件，步骤如下：

1.	创建一个新项目MyHairBoot，把MyHairBoot.c的内容复制到main.c。设置项目属性，在原来的基础上，还要设置以下几点：
(1)	C/C++ Compiler -> Optimizations -> High -> Balanced
 ![62](https://img-blog.csdnimg.cn/20200528222450146.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(2)	Output Converter -> Output -> Generate additional output -> Binary
 ![63](https://img-blog.csdnimg.cn/20200528222506639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(3)	Linker -> Library: 取消”Include C-SPY debugging support”选项
 ![64](https://img-blog.csdnimg.cn/20200528222520406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
完成以上设置后，编译项目后，会在项目目录下的Debug->Exe目录生成.bin文件，一定要保证.bin文件小于512字节。
 ![65](https://img-blog.csdnimg.cn/20200528222536721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

2. 阅读MyHairBoot.c，并结合参考资料，回答以下问题：
   (1)	MyHairBoot和上位机的串口通讯流程。
    ![66](https://img-blog.csdnimg.cn/20200528222603522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
   注：开发板启动后，在等待通讯前，LED会快闪烁。
   (2)	通过MyHairBoot从串口烧写程序时，被烧写的程序保存到Flash的什么位置(地址)？
   答：被烧写的程序保存到Flash的0x8200往后
   (3)	为什么MyHairBoot.bin的大小不能超过512字节？
   **答：因为MyHairBoot在flash保存的范围是0x8000-0x8200。因为16进制的0x200即十进制的512字节，因此不能溢出。** 

3. 用IAR编译并烧写MyHairBoot，并打开View菜单下的Disassembly和Memory窗口，回答以下问题：

   (1)	用二进制编辑器(例如UltraEdit, EditPlus, Windows 10商店里面的Hex Editor Pro)察看生成的bin文件，并于Flash的内容比较，生成的bin文件下载到Flash的什么位置(地址)？
    ![67](https://img-blog.csdnimg.cn/20200528222627109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
    ![68](https://img-blog.csdnimg.cn/20200528222643575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
   **答：生成的bin文件与IAR中观察flash的内容是一样的，生成的bin文件下载到Flash的0x8000**

(2)	Interrupt Vector(中断向量表)位于Flash的什么位置(地址)？有多大？

**答：中断向量表位于flash的0x8000~0x8080，大小为128字节。**

(3)	中断向量表的每个字节的含义是什么？

**答：前两个字节表示INT指令，如0x8200。后两个字节表示中断处理函数，如0x80DD:** 

**TIM2_OVR_UIF_IRQHandler，指如果发生TIM2 update/overflow中断，就跳转到函数**

**TIM2_OVR_UIF_IRQHandler**

(4)	当前的中断向量表存在什么问题？

**答：当前的中断向量表未转向到0x8200**

4.	用TestLED.c创建一个项目TestLED，编译并生成Bin文件。
(1)	设置项目属性时，除了指定CPU型号外，还要加上第1步第(2)小步的步骤；
(2)	不要用IAR下载该项目，否则会覆盖MyHairBoot

5.	用资料3中的上位机(stm8s_uploader.exe)下载TestLED.bin。
(1)	打开上位机，选择芯片类型和串口，然后指定TestLED.bin的路径；
注：其他选项不用修改，用默认即可。
(2)	按下开发板的Reset按钮，此时开发板的LED会以很快的速度闪烁；
(3)	在开发板的LED停止闪烁之前，按下上位机的”上传”按钮，成功的话会显示”上传完毕”提示
 ![69](https://img-blog.csdnimg.cn/20200528222704837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(4)	再次按下开发板的Reset按钮，可发现被下载的TestLED无法运行

6.	再次用IAR打开MyHairBoot项目并下载它
(1)	用二进制编辑器打开TestLED.bin，并与Flash的内容比较，判断TestLED.bin被下载到什么位置(地址)?
**答：TestLED.bin被下载到了flash的0x8200的位置**
![70](https://img-blog.csdnimg.cn/20200528222723713.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
 ![71](https://img-blog.csdnimg.cn/20200528222732838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

(2)	TestLED程序的中断向量表，分别位于bin文件和Flash的什么位置(地址)？有多大？

**答：TestLED程序的中断向量表，位于bin文件的0x00~0x7f的位置，大小为128字节。位于flash的0x8200-0x827f，大小为128字节**

(3)	TestLED被下载后无法运行的原因是什么？

提示：分析中断向量表

**答：当TestLED的中断发生时，中断向量表并没有跳转到0x8200固件的中断向量表，因而无法正确跳转到中断处理函数的物理地址。**

7.	按照第4步的方法，用TestLED.c创建TestLED2项目，设置项目属性时，增加以下一个额外的步骤：
(1)	Linker -> Config -> Override default: 然后my_lnkstm8s103f3.icf的路径代替原来的路径
(2)	新icf把原icf文件中的”define region NearFuncCode = [from 0x8000 to 0x9FFF]”改成了”define region NearFuncCode = [from 0x8200 to 0x9FFF]”
(3)	比较TestLED.bin和TestLED2.bin的不同
**答：TestLED2固件的起始地址相比于TestLED增加了0x200.**
![72](https://img-blog.csdnimg.cn/20200528222753755.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528222804628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

8.	按照第5步的方法下载TestLED2.bin，此时会发现TestLED2能正常运行。解释为什么TestLED不能正常运行而TestLED2能正常运行。
(4)	**答：TestLED不能正常运行而TestLED2能正常运行的原因是因为在TestLED2中将my_lnkstm8s103f3.icf中的define region NearFuncCode = [from 0x8000 to 0x9FFF]”改成了”define region NearFuncCode = [from 0x8200 to 0x9FFF]”使得了TestLED2的起始地址相较于TestLED增加了0x200，又因为 TestLED2没有使用到中断，因此即使前面bootloade的中断向量表没转向也不影响TestLED2的运行，但会影响后面使用了中断的Lab2b的运行**
9.	按照第7步和第8步的方法，打开Lab2b中实现的项目，并生成bin文件(Lab2b.bin)
(1)	用上位机把Lab2b.bin下载到Flash中，
(2)	用IAR再次下载MyHairBoot
(3)	在Memory窗口检查Flash内容，确保Lab2b.bin被正确下载到Flash中
(4)	退出IAR的Debug状态，按下开发板的Reset按钮，为什么此时的Lab2b.bin无法正常运行？
提示：分析中断向量表
**答：bootloader的中断向量表没有转向到固件Lab2b的中断向量表，找不到固件Lab2b定义的中断处理函数。固件的起始地址也没有修改为0x8200。**
(5)	如何解决上一步的问题？参考en.CD00176595.pdf的Figure 6，给出你的解决思路。
**答：①修改固件的起始地址②转向bootload的中断向量表**
    
10.	解决第9步的问题，用IAR下载MyHairBoot，使Flash中的Lab2b.bin能正常运行，并写出你的解决步骤。
提示：解决方法不止一种，例如参考资料1的汇编，参考资料2的手工修改bin文件等，或者lec13.ppt第7页提到的其他方法。
**答：①利用汇编语言将MyHairBootload中断向量表的地址转向到0x8200-0x827f，（修改stm8s_interrupt.s文件,将.s文件放到main.c的目录下编译）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528222836962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![中断向量表转向前的bin文件](https://img-blog.csdnimg.cn/2020052822284463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)中断向量表转向前的bin文件

![中断向量表转向后的bin文件](https://img-blog.csdnimg.cn/20200528222912958.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
中断向量表转向后的bin文件
②修改固件的起始地址在my_lnkstm8s103f3.icf中将define region NearFuncCode = [from 0x8000 to 0x9FFF]”改成了”define region NearFuncCode = [from 0x8200 to 0x9FFF]”


![77](https://img-blog.csdnimg.cn/20200528222947403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528223002188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)




![在这里插入图片描述](https://img-blog.csdnimg.cn/20200528223009502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



### 四、实现代码

**myhairboot.c**

```c
// Refernece: 
//   1. AN2659
//   2. https://github.com/Zepan/hairBoot
//   3. http://bbs.21ic.com/icview-1251224-1-1.html

#include <iostm8s103f3.h>
//#include <intrinsics.h>

#define UART1_FLAG_TXE  (unsigned char)0x80  // Tx Data Register Empty flag
#define UART1_FLAG_RXNE (unsigned char)0x20  // Rx Data Register Not Empty flag

#define BLOCK_BYTES   64
#define	BLOCK_SHIFT   6
#define BOOT_ERR      0xa1
#define BOOT_HEAD     0xa5
#define BOOT_OK       0xa0
#define BOOT_WRITE    0xa7
#define BOOT_WRITE_INTERRUPT_VEC 0xaa
#define FLASH_START   0x8000
//#define INIT_PAGE     0x08

void UART1_SendB(unsigned char ch)
{
  while (!(UART1_SR & UART1_FLAG_TXE));
  UART1_DR = ch;
}

unsigned char UART1_RcvB(void)
{
  while(!(UART1_SR & UART1_FLAG_RXNE));
  return ((unsigned char)UART1_DR);
}

//addr must at begin of block
__ramfunc void FLASH_ProgBlock(unsigned char *addr, unsigned char *Buffer)
{
  unsigned char i;
  FLASH_CR2 |= (unsigned char)0x01;     // programming time fixed at Tprog
  FLASH_NCR2 &= (unsigned char)(0xFE);  // block programming enabled
  // Copy data bytes from RAM to FLASH memory
  for (i=0; i<BLOCK_BYTES; i++)
    *((__near unsigned char*) (unsigned int)addr + i) = ((unsigned char)(Buffer[i]));
}

int main( void )
{
  unsigned int tryCnt = 65535;
  unsigned char ch, page;
  unsigned char i = 100;  //10 for 1s
  unsigned char buf[BLOCK_BYTES];
  unsigned char verify;
  unsigned char *addr;

  // setup clock
  CLK_CKDIVR &= (unsigned char)(~0x18);  // 16MHz high speed internal clock

  //LED: B5
  PB_DDR = 0x20;  // output
  //PB_CR1 = 0x20;  // push pull
  //PB_CR2 |= 0x20;  // 10MHz output speed
  
  // setup uart: 115200, 8-n-1
  UART1_CR2 = 0x0C;  // rx/tx enabled, rx int disabled, tx int disabled
  UART1_BRR2 = 0x0B;  // 115200 baud rate. must set BRR2 first
  UART1_BRR1 = 0x08;  // 115200 baud rate. must set BRR2 first
  
  // wait UART data
  while (i) {
    PB_ODR = i<<5;  // LOW is ON
    if (UART1_SR & UART1_FLAG_RXNE) {  //wait for head
      ch = (unsigned char)UART1_DR;
      if (ch == BOOT_HEAD)
        break;
    }
    tryCnt--;
    if (tryCnt == 0) i--;
  }
  //PB_ODR |= 0x20;  // HIGH if OFF
  
  if (i == 0) {
    FLASH_IAPSR &= (unsigned char)0xFD;  // lock flash
    asm("JP $8200");
  }
  else {
    FLASH_PUKR = (unsigned char)0x56;  // unlock flash
    FLASH_PUKR = (unsigned char)0xAE;  // unlock flash
    UART1_SendB(0xa8);
    while (1) {
      ch = UART1_RcvB();
      switch (ch) {
      
      case BOOT_WRITE:
      case BOOT_WRITE_INTERRUPT_VEC:
        page = UART1_RcvB();
        addr = (unsigned char*)(FLASH_START + (page << BLOCK_SHIFT));
        verify = 0;
        for(i = 0; i < BLOCK_BYTES; i++) {
          buf[i] = UART1_RcvB();
          verify += buf[i];
        }
        if (verify == UART1_RcvB()) {  // verify successed
          if (page == 0 && ch == BOOT_WRITE_INTERRUPT_VEC) { // restore original reset interrupt vec
            for (i = 0; i < 4; i++)
              buf[i] = addr[i];
          }
          FLASH_ProgBlock(addr, buf);
          UART1_SendB(BOOT_OK);
          break;
        }
        
        default:  // verify failed
          UART1_SendB(BOOT_ERR);
          break;        
        
      }
    }
  }  
}
```

**stm8s_interrupt.s**

```
/**************************************************
 *
 * System initialization code for the STM8 IAR Compiler.
 *
 * Copyright 2010 IAR Systems AB.
 *
 * $Revision: 1413 $
 *
 ***************************************************
 *
 * To add your own interrupt handler to the table,
 * give it the label _interrupt_N, where N is the
 * offset from the RESET vector.  Your label will
 * override the corresponding weak label declaration
 * on the unhandled_exception function.
 *
 **************************************************/

        MODULE   ?interrupt

        SECTION __DEFAULT_CODE_SECTION__:CODE

declare_label MACRO
        PUBWEAK _interrupt_\1
_interrupt_\1:
        ENDM

unhandled_exception:

        declare_label 1
        declare_label 2
        declare_label 3
        declare_label 4
        declare_label 5
        declare_label 6
        declare_label 7
        declare_label 8
        declare_label 9
        declare_label 10
        declare_label 11
        declare_label 12
        declare_label 13
        declare_label 14
        declare_label 15
        declare_label 16
        declare_label 17
        declare_label 18
        declare_label 19
        declare_label 20
        declare_label 21
        declare_label 22
        declare_label 23
        declare_label 24
        declare_label 25
        declare_label 26
        declare_label 27
        declare_label 28
        declare_label 29
        declare_label 30
        declare_label 31

        NOP                                   ;; put a breakpoint here
        JRA    unhandled_exception


/*
 * The interrupt vector table.
 */


        SECTION `.intvec`:CONST

define_vector MACRO
        DC8     0x82
        DC24    _interrupt_\1
        ENDM

        PUBLIC  __intvec
        EXTERN   __iar_program_start
        


__intvec:
        DC8     0x82
        DC24    __iar_program_start          ;; RESET    0x8000
        DC8     0x82
        DC24    0x8204
        DC8     0x82
        DC24    0x8208
        DC8     0x82
        DC24    0x820C
        DC8     0x82
        DC24    0x8210
        DC8     0x82
        DC24    0x8214
        DC8     0x82
        DC24    0x8218
        DC8     0x82
        DC24    0x821C
        DC8     0x82
        DC24    0x8220
        DC8     0x82
        DC24    0x8224
        DC8     0x82
        DC24    0x8228
        DC8     0x82
        DC24    0x822C
        DC8     0x82
        DC24    0x8230
        DC8     0x82
        DC24    0x8234
        DC8     0x82
        DC24    0x8238
        DC8     0x82
        DC24    0x823C
        DC8     0x82
        DC24    0x8240
        DC8     0x82
        DC24    0x8244
        DC8     0x82
        DC24    0x8248
        DC8     0x82
        DC24    0x824C
        DC8     0x82
        DC24    0x8250
        DC8     0x82
        DC24    0x8254
        DC8     0x82
        DC24    0x8258
        DC8     0x82
        DC24    0x825C
        DC8     0x82
        DC24    0x8260
        DC8     0x82
        DC24    0x8264
        DC8     0x82
        DC24    0x8268
        DC8     0x82
        DC24    0x826C
        DC8     0x82
        DC24    0x8270
        DC8     0x82
        DC24    0x8274
        DC8     0x82
        DC24    0x8278
        DC8     0x82
        DC24    0x827C

        END

```



### 五、异常问题

错误理解bootload和固件的关系，将固件的中断向量表转向到自己，造成无限死循环，而bootload的中断向量表反而没有转向。
##### 解决方法：
取消固件的中断向量表转向，把lab 2c.s文件添加到MyHairBoot工程下编译运行，实现bootload的中断向量表的转向。


### 六、总结：

#### 1、Bootloader程序流程图
![Bootloader程序流程图](https://img-blog.csdnimg.cn/2020052822310334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 2、中断向量表

（1）在stm6s中，中断向量表的地址是0x8000，它位于Flash的开头，大小是128字节，范围是0x8000 ~ 0x807F，中断向量表里面储存的内容叫做中断向量(Interrupt Vectors)，每个中断向量占4字节，STM8共有32个不同的中断向量，因此中断向量表的大小为128k。

（2）中断向量表的转向：就是当固件的中断发生时，CPU会去查找位于0x8000的bootloader的中断向量表。bootloader的中断向量表需要转向到固件的中断向量表，以便找到固件定义的中断处理函数。因为固件中断处理函数的物理地址是在链接固件时产生的，并保存在固件的中断向量表，所以Bootloader不知道固件中断处理函数的物理地址。因此实验要求将bootloader的中断向量表转向到0x8200固件的中断向量表上。

#### 3、固件
![固件](https://img-blog.csdnimg.cn/20200528223418427.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（1） 固件是担任着一个系统最基础最底层工作的软件。而在硬件设备中，固件就是硬件设备的灵魂，因为一些硬件设备除了固件以外没有其它软件组成，因此固件也就决定着硬件设备的功能及性能。在许多嵌入式设备中，如果设备需要升级，绝大多数都是通过固件升级的。
（2）STM8程序默认起始地址是0x8000即bootloader的起始地址。
（3）固件的起始地址也是固定的，在本实验中要求固件的起始地址是0x8200。
（4）通过.icf文件可以修改固件的起始地址，把.icf文件中以define region开头的行中的0x8000，修改为固件的实际起始地址0x8200
