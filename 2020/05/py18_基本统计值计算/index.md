# 基本统计值计算


为了加深组合数据类型的理解，用python实现基本数值的计算，根据用户给出的n个数值，分别求出平均值、方差、中位数。

------



1、要求平均值，那么就需要求和，用for...in累加求和，然后返回求和/数据长度就可求出平均值了。

2、计算方差：需要累加每个数与平均数的平方，各数据与平均数差的平方和的平均数

3、中位数：排序后，奇数找中间1个，偶数找中间2个取平均



```python
#基本统计值计算
def getNum():
    nums = []
    iNumStr = input("请输入数字（回车退出）")
    while iNumStr !="":
        nums.append(eval(iNumStr))
        iNumStr = input("请输入数字（回车退出）")
    return nums
def mean(numbers):
    s = 0.0
    for num in numbers:
        s = s + num
    return s / len(numbers)
def dev(numbers,mean):#计算方差
    sdev = 0.0
    for num in numbers:
        sdev = sdev + (num - mean)**2
    return pow(sdev / (len(numbers)-1),0.5)
def median(numbers):
    sorted(numbers)
    size = len(numbers)
    if size % 2 ==0:
        med = (numbers[size//2-1] + numbers[size//2])/2
    else:
        med = numbers[size//2]
    return med
n = getNum()
m = mean(n)
print("平均值：{}\n方差：{}\n中位数：{}\n".format(m,dev(n,m),median(n)))
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427143151356.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
