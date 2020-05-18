# 2.6海明校验及其实现


海明校验码是由理查德.海明（Richard H a m m i n g )于1950年提出的。它不仅具有检测错误的能力，同时还具有给出错误所在准确位置的能力。
### 1、增加冗余码（校验位）

海明校验不仅具有检测错误的能力，同时还具有给出错误所在准确位置的能力，但是因为这种海明校验的方法只能检测和纠正一位出错的情况。所以如果有多个错误，就不能查出了。

如要能检出与自动校正一位错，并能同时发现哪位错，此时校验位的位数r和数据位的位数k应满足下述关系:
2^r-1 ≥ k + r （有效信息(k位)校验信息(r位））

设计海明码编码的关键技术，是合理地把每个数据位分配到r个校验组中，以确保能发现码字中任何一位出错；若要实现纠错，还要求能指出是哪一位出错，对出错位求反则得到该位的正确值。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405203812454.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 举个例子
#### 传送1011000
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405203823492.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 当传输无错时
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405203857918.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 当传输出错时
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405203914412.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 缺陷
#### 指错字G4G3G2G1= 0000 不一定无错（利用偶校验的特点去判断）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405204053572.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 指错字不一定能区别一位错与两位错
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405204131333.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

[参考资料](http://www.icourse163.org/learn/HUST-1003159001?tid=1206076221#/learn/announce)



