# STM8s_步进电机运动控制板-PCB设计






------

# PCB设计
用Altium Designer打开StepperMotor.PrjPcb（File -> Open Project…），在PCB图MotorStepper.PcbDoc(已定义好规则)完成以下步骤。

#### 1.准备工作
(1)	核对原理图，并确保每个元件的编号不重复。

(2)	核对封装(Footprint) ，在本实验中除了2个钽电容和1个铝电解电容，其他所有电容封装为1608[0603]；除了2个0.11欧姆的Sense电阻，其他所有电阻封装为J1-0603。

(3)	把原理图导入PCB图，直到提示”No Difference Detected”注意：如果提示”Component Links”错误，打开PCB图，选择”Project”->”Component Links…” -> “Add Pairs Matched By” -> “Perform Update”，然后进入原理图，重新导入PCB

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215958204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(4)	打开PCB图，如果看不见元件，可以使(V、D)切换到视角，删除Room(红色区域)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220018117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(5)	设置Grid属性
在PCB图上按右键，选择Snap Grid -> Grid Properties。（或快捷键Ctrl+G）把Step X和Step Y设为0.05mm，把Multiplifier设置20x Grid Step。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220047907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(6)	根据个人喜好，自定义窗口，其中“PCB”和”PCB Inspector”属性窗口经常使用到，所以最好是自定义这两个窗口方便打开，步骤如下：按右下角的“PCB”按钮，分别打开“PCB”和“PCB Inspector”属性窗口。将其拖动到右下角固定，“PCB”窗口的Compoment和Net可以显示元件和网络信息(例如哪些网络是Un-routed)，“PCB Inspector”可以显示元件坐标等信息。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220113640.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(7)	字符的Height和Width分别设为0.8mm和0.08mm按下Ctrl+A选中所有元件，然后在“PCB Inspector”窗口点击“all types of objects”，然后选“Display only” -> “Text”，就可以一次过修改所有字体。修改完毕后要还原成显示“all types of objects”。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220205870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(8)	 利用快捷键L打开view configurations，确保Top layer、Buttom layer、四个mask layers和DrillDrawing是打钩状态，这样才不会因被隐藏而看不见。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220223720.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 2.布局与布线
在本实验中，应该遵循如下规则
–	Pad, Track, Via相互之间的间距不得小于0.3mm
–	Polygon与Pad, Track, Via之间的间距不得小于0.4mm
–	字符与Pad, Via之间的间距不得小于0.1mm
–	线宽不得小于0.2mm
–	Via的内径0.3mm，外径0.6mm
–	字符的Height和Width分别为0.8mm和0.08mm
–	字符和焊盘、过孔之间的距离至少0.1mm
–	线一定要从IC的焊盘的中间引出
–	12V电源线主干部分设为0.8mm
–	VCC电源线主干部分设为0.6mm
–	12V和VCC电源线的分支部分(包括GND)在不违反间距的规则下设为0.3mm
–	信号线宽0.2mm
–	普通过孔内径和外径分别设为0.3mm和0.6mm
–	大电流过孔(主干12V, VCC, GND)内径和外径分别设为0.5mm和1.0mm
–	对大电流线路：SPX3819的12V和VCC部分、A4988的12V, COILA+, COILA-, COILB+, COILB-以及连接两个Sense电阻的部分、进行铺铜
–	布好所有线后，给正反面(Top Layer & Bottom Layer)的GND铺铜
–	铺铜设置时，要选中”Is Poured”和”Remove Dead Copper”，下拉框选择”Pour All Same Net Objects”
（1）根据规则，已完成布线的效果如下图。![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220627409.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（2）铺铜后最终完成的效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220652324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220702329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)


#### 操作异常问题与解决方案
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513220953298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
**异常问题一：按照教程进行PCB的标签字符大小设置，但设置后大小依然不统一。**

解决方法： Ctrl+A选中所有元件，然后在“PCB Inspector”窗口点击“all types of objects”，然后选“Display only” -> “Text”，就可以一次过修改所有字体。修改后需要按Enter键确认，否则修改无效！修改完毕后要还原成显示“all types of objects”
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513221026611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
**异常问题二：过孔无法连线**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513221117890.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
解决方法：新添加的过孔，除了需要设置内外径，还需要设置NET,你需要那种线可以连接过孔，就选相应的即可。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513221152999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

### Pcb制作的总结：
1、	pcb设计必须满足生产工艺的要求，如果做出来的pcb无法生产也就没有了意义。通常Pad, Track, Via相互之间的间距不得小于0.3mm。Polygon与Pad, Track, Via之间的间距不得小于0.4mm；字符与Pad, Via之间的间距不得小于0.1mm；线宽不得小于0.2mm；Via的内径0.3mm，外径0.6mm；字符的Height和Width分别为0.8mm和0.08mm。
2、	pbc的布线和布局要考虑信号质量的问题
3、	所有的导线，都又寄生电阻电容电感，RLC效应会影响对电流电压，对高速传输的电路影响非常大。因此在布线的时候要考虑两根导线的距离不能太近。
4、	电源和地线间的回路需要尽量的短，尽量使用星型布线，减少点对点的距离，电源和地线处理不好，会使产品的性能下降，有时甚至影响到整个系统的稳定性。所以对电源、地线的布线要认真对待，把电源、地线所产生的噪音干扰降到最低限度。
5、	尽量将大的干扰源放在pcb板角落，比如开关电源、电源芯片、晶振等，实在无法做到可以GND隔离。
6、	将多引脚与其它模块连接的芯片放在板子中间，比如单片机。
7、	布局时分区，将模拟电路和数字电路隔开。
8、	数字、模拟元器件及其相应走线尽量远离并限定在各自的布线区域内。
9、	对于大电流线路，需要铺铜，有效增大连通率，散热等。
10、	插座之类的，放到pcb板旁
11、	为了拥有更好的去耦效果，尽量将去耦电容靠近芯片电源引脚。
12、	尽量遵循，尽可能的减少回路、线路的长度的布线原则。对于信号线，越多信号质量越好，有时还需要考虑时序的问题。
13、	优先先布关键的信号线，例如内存的数据线和地址线。
14、	转弯时不要用锐角，除非有分支，尽量少用直角，尽量用钝角。
15、	布局时，元器件周围留出电源和地走线的空间。
16、	确定元器件放置方向，尽量使数字信号及模拟信号引脚朝向各自布线区域。
17、	对于模拟器件，相互靠近且放置在PCB上包含SDA、SCL、TXA1、TXA2、RIN、VC、VREF信号走线的一面
18、	SDA、SCL、TXA1、TXA2、RIN、VC、VREF信号走线周围避免放置高噪声元器件。尤其是iic协议这些对信号完整性要求非常高的
19、	旁路电容到相应IC的走线尽量避免使用过孔。
20、	高频信号应减少使用过孔连接，避免使用直角转弯，尽量使用45°角代替。

### 最后的总结：
Pcb的布线与布局，是整个项目中难度最高的，虽然有参考图，降低了难度，但在实际工作中，我们没有参考，也没有人给我们制定规则，这就需要我们学会如何布局和布线。布线是本项目中的重要步骤，可以说前面的原理图设计和各种准备工作都是为它而做的。在整个PCB板设计中，布线的设计过程工作量最大。PCB板布线分单面布线、 双面布线及多层布线。布线规则可以预先设定，包括走线的弯曲次数、导通孔的数目、步进的数目等。刚开始，我和我的小组成员跟着模板一步一步的模仿布线，花了半天时间就差不多完成了，后来，我们尝试了自己布局自己布线，结果花了几天时间，无功而返。出现了很多问题，比如布局好了之后布线，发现线太绕了、空间不够、信号质量问题、连通性等等的问题，于是我们又重新改重新布线，如此反复。花费了大量的时间和精力，却无功而返。其实对于初学者而已，我们先学会模仿，在模仿中慢慢领悟，pcb制作并不可以快速精通，如果没学会走路就先不要想跑，这样会产生挫败感，打击我们对学习pcb制作的信心。
