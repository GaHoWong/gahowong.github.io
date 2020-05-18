# 利用蒙特卡罗方法计算圆周率


蒙特卡罗方法是一种计算方法。原理是通过大量随机样本，去了解一个系统，进而得到所要计算的值。

------

### 圆周率的标准公式

在讲蒙特卡罗方法之前，先了解一下圆周率的近似计算公式。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200418220032681.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



```python
#CalPiV1.py
pi = 0
N = 1000
for k in range(N):
    pi += 1/pow(16,k)*( \
              4/(8*k+1) - 2/(8*k+4) - \
              1/(8*k+5) - 1/(8*k+6) ) 
print("圆周率值是: {}".format(pi))
```
结果
```
圆周率值是: 3.141592653589793
```
### 蒙特卡罗方法计算
蒙特卡罗方法是一种计算方法。原理是通过大量随机样本，去了解一个系统，进而得到所要计算的值。

它非常强大和灵活，又相当简单易懂，很容易实现。对于许多问题来说，它往往是最简单的计算方法，有时甚至是唯一可行的方法。

它诞生于上个世纪40年代美国的"曼哈顿计划"，名字来源于赌城蒙特卡罗，象征概率。
对于圆周率的计算，在一个正方形内部，随机产生10000个点（即10000个坐标对 (x, y)），计算它们与中心点的距离，从而判断是否落在圆的内部。

如果这些点均匀分布，那么圆内的点应该占到所有点的 π/4，因此将这个比值乘以4，就是π的值。
下面通过python来实现

```python
from random import random
from time import perf_counter #调用time库计时函数
DARTS = 10000*10000           #抛洒点的总数量，数值越大运算越久
hits = 0.0                    #抛进圆内的点个数
start = perf_counter()        #开始时间
for i in range(1,DARTS+1):    #循环遍历，从1开始到DARTS+1
    x, y =random(),random()   #抛点，利用随机数参数xy轴，0-1之间
    dist = pow(x **2 + y **2,0.5) #判断点是否在圆内的关键，这里是求点到圆心的距离
    if dist <=1.0:         #如果点到圆心距离小于1，则hits命中加一
        hits+=1
pi = 4*(hits/DARTS)  #命中数除于总数，再乘于四就是Π了
print("圆周率值是：{}".format(pi))
print("运行时间是：{:.5f}".format(perf_counter()-start))#结束时间-起始时间=实际运算时间
```
DARTS 的值越大，运算时间越长

```
圆周率值是：3.14171636
运行时间是：133.30979
```
蒙特卡罗方法应用的范围非常广泛。以后有机会再去深入学习。

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
