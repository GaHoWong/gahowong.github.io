# 步进电机运动控制板设计二






------

# 最小系统原理图设计

### 第一部分电机模块原理图设计


#### 1.	创建项目
用Altium Designer打开\Labs\StepperMoto目录下名为StepperMotor.PrjPcb的课程模板，为了教学的统一，这个模板里有两个库文件AD14.PcbLib封装库和AD14.SchLib原理图库，除此之外还有一个PCB模板和原理图模板。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513213929435.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

#### 2.	电源电路。
在选着电源芯片的时候，考虑到成本，我们选用了SPX3819电源芯片，它能把高电压转换为低电压，缺点就是转换效率较低，大功率（电流）时发热较大。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513213957268.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(1)	在VIN和VOUT引脚上增加滤波电容，电容的参数需要查找SPX3819 的Datasheet第7页，电容可选2.2μF的吕电解电容或1μF的钽电容，这里我们选择体积较小钽电容，封装为CAP_A。（注意：电容的极性，不能接反）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214226949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

(2)	在BYP引脚加上瓷电容（Datasheet如果没有特殊说明，则选择不分极性的瓷电容，在本实验种如无特殊说明，电容使用1608[0603]封装）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214253778.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214302300.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214310838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214317972.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(3)	增加一个电源(VCC)指示灯，参考《STM8S103F3最小系统原理图.pdf》的R2和GREEN1。(其中R1、R2阻值为1K使用J1-0603封装。LED2使用LED0805封装。)

#### 3.	CPU通讯接口

(1)	调试接口：决定P3的SWIM和NRST信号分别连到CPU的PD1和NRST引脚。 

(2)	串口：决定P4的UART1_TX和UART1_RX信号分别连到CPU的PD5和PD6引脚。 

(3)	I2C接口：决定P1的SDA和SCL信号分别连到CPU的PB4和PB5引脚。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321450382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 4.	CPU外围电路
(1)	VDD引脚连接1个100nF去耦电容，使用1608[0603]封装。

(2)	VCAP引脚的滤波电容，电容值参考en.CD00226640.pdf52页的Table 19可知，外部滤波电容的容值最小460nF，最大3300nF。（C6容值1000nF，使用1608[0603]封装）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214614618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
(3)	复位电路：参考《STM8S103F3最小系统原理图.pdf》的R3和C4，不需要按钮S1。（R3阻值10K，使用J1-0603封装。C4容值100nF，使用1608[0603]封装）

(4)	调试LED：参考《STM8S103F3最小系统原理图.pdf》的R1和RED1，连接到CPU的PA3引脚。（R1阻值1K，使用J1-0603封装。）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321471464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214757735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
**第一部分电机模块原理图设计完成**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513214805535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 第二部分：电机驱动原理图设计
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215032198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
根据A4988-Datasheet的经典应用图可知，A4988的电路原理设计如下

（1）	在VDD, VBB1, VBB2, VREG引脚各增加1个220nF滤波/去耦电容

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215051533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（2）	给12V电源增加一个100uF的供电/滤波电容(铝电解电容)，封装为CAP6.3x7.7
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215119872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（3）	给VCP, CP1, CP2引脚接上电容
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215152123.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（4）	给nSLEEP接20K上拉电阻，nENABLE和ROSC各接20K下拉电阻，至于为什么接的是20k电阻，而不是Datasheet上的5k，其实是为了采购的时候可以少采购不同种类的电阻，降低成本。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215236541.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（5）	在SENSE1和SENSE2引脚各接一个0.11欧姆大功率(1W)电流探测电阻，封装为12Z-2010
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020051321531382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
（6）	利用串联分压电路给REF引脚提供电压，限制电机的最大电流为0.72A
要求：两个串联电阻的阻值的和为12K，计算两个串联电阻的阻值
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215356152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

（7）	以方便布线为原则，把MS1, MS2, MS3, nRESET, nSLEEP, STEP, DIR, nENABLE这八个引脚分别连到单片机的I/O口

（8）	连接电源引脚VDD,VBB1, VBB2, GND

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200513215432597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

**大功告成！**


