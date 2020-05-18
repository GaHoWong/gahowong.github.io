# 1.2计算机系统性能评价


对一台计算机的性能进行科学合理的评估是一项重点工作。计算机系统设计者要利用性能评估来对计算机中新增功能的有效性进行评价；制造商在计算机销售过程中要使用该性能指标进行宣传；计算机用户在购买计算机时要使用性能指标进行合理的选择。计算机系统性能评价一般由非时间指标（机器字长、总线宽度、主存容量与存储带宽）和非时间指标（主频f/时钟周期T , 外频、倍频、cpi、MIPS、cpu时间）决定。
### 时间指标
##### 1)机器字长:  指机器一次能处理的二进制位数
机器字长是指计算机进行一次整数运算所能处理的二进制数据的位数（整数运算即定点整数运算）。因为计算机中数的表示有定点数和浮点数之分，定点数又有定点整数和定点小数之分，这里所说的整数运算即定点整数运算。机器字长也就是运算器进行定点数运算的字长，通常也是CPU内部数据通道的宽度。
##### 2)总线宽度：数据总线一次能并行传送的最大信息的位数
总线（Bus）是计算机各种功能部件之间传送信息的公共通信干线。这里一般指运算器与存储器之间的数据总线位数。有些计算机内部与外部数据总线宽度不一致。
##### 3)主存容量与存储带宽
主存容量：是指一台计算机主存所包含的存储单元总数。
存储带宽：指单位时间内与主存交换的二进制信息量，常用单位B/s（字节/秒）。(影响存储带宽的指标包括数据位宽和数据传输速率)。
### 非时间指标
##### 1) 主频f/时钟周期T , 外频、倍频
**主频f**指CPU内核工作的时钟频率，即CPU内数字脉冲信号振荡的速率，与CPU实际的运算能力之间不是唯一的、直接关系。
**时钟周期T**也称节拍周期，是计算机中最基本的、最小的时间单位。在一个时钟周期内，CPU仅完成一个最基本的动作。
**f与T的关系**互为倒数，f 越高，T就越小(f =100MHz时T=10ns，f =1GHz时T=1ns)
**外频**指CPU(内存)与主板之间同步的时钟频率(系统总线的工作频率)。
**倍频**CPU主频与外频之间的倍数。

> 主频=  外频×倍频 如：Pentium 4 2.4G CPU主频2400M = 133M (外频) ×18 (倍频)
##### 2) CPI (Clock cycles Per Instruction)
**CPI**指执行一条指令(平均)需要的时钟周期数(即T周期的个数)。有单条指令CPI 、一段程序中所有指令的CPI、指令系统CPI 等。
**CPI计算方法：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200331183515833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

PS：实际上频率和IPC在真正影响CPU性能。准确的CPU性能判断标准应该是：CPU性能=IPC(CPU每一时钟周期内所执行的指令多少)×频率(MHz时钟速度)--由英特尔提出并被业界广泛认可。
[IPC:每个时钟周期内执行的指令条数(并行)]
##### 3) MIPS (Million Instructions Per Second)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200331184136789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70#pic_center)
##### 4) CPU时间
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200331184339382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70#pic_center)
**CPU时间的计算方法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200331184432607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70#pic_center)

#### 问题与思考
**1、某程序的目标代码主要由4类指令组成，它们在程序中所占的比例和各自的CPI如表1.2所示**
| 指令类型     | CPI  | 所占比例 |
| ------------ | ---- | -------- |
| 算术逻辑运算 | 1    | 60%      |
| 内存读写     | 2    | 18%      |
| 转移         | 4    | 12%      |
| 其他         | 8    | 10%      |

(1)求该程序的CPI。16
(2)若该CPU的主频为400MHz，求该机的MIPS。
解：
（1）CPI =   1 X 0. 6 + 2 X 0. 18 + 4 X 0. 12 + 8 X 0. 1 =  2. 24
（2）根据公式MIPS=时钟频率/CPI*10^6 得：
MIPS=(400X106)/(2. 24X 106) = 178. 6 
**2、假设计算机A和B是基于相同指令集设计的两种不同类型的计算机，机器A的时钟周期为2ns，某程序在机器A上运行时的C P I为3. 0。机器B的时钟周期为4ns，同一程序在机器B上运行的C PI为2,对这个程序而言，计算机A与B哪个机器更快？快多少？**
解：根据CPU时间定义：CP U时间A = CP U时钟周期八X CPIAX指令条数八=2X 3 X指令条数八 CP U时间B =  CP U时钟周期8 X CPIBX指令条数1! =  4X2X指令条数B 由于是同一程序在A ,B两机器上运行，因此指令数量相同，由此得到同一程序在两机器上的CP U时间之比为
CPU 时间A/CPU时间B  =6/8 =0.75
即计算机A快，且其速度约为机器B速度的1.3倍。  

------

[^undefined]:

[参考资料](http://www.icourse163.org/learn/HUST-1003159001?tid=1206076221#/learn/announce)



