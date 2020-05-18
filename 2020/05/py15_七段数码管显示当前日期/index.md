# 七段数码管显示当前日期


作为电子信息工程的一名学生，七段数码管可以说是很常见的了。七段数码管通常用来显示时间。今天就利用七段数码管的例子要巩固刚刚所学的函数使用方法。

------

要实现标题的目的，首先要了解七段数码管。要绘制图，我想到了刚开始学python时的turtle库，利用turtle函数完成绘制。

### 基本思路

**步骤1：绘制单个数字对应的数码管**

**步骤2：获得一串数字，绘制对应的数码管**

**步骤3：获得当前系统时间，绘制对应的数码管**

#### 步骤1：绘制单个数字对应的数码管
七段数码管由7个基本线条组成，不同的线条之间的组合可以表示不同的数字和字母。这样走，可以一次性走完七段。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424213345341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
因此只要确定在哪一条线段画笔时抬起，还是放下。就可以实现绘制。

```python
import turtle
def drawLine(draw):   #绘制单段数码管
    turtle.pendown() if draw else turtle.penup()
    turtle.fd(40)
    turtle.right(90)
def drawDigit(digit): #根据数字绘制七段数码管
    drawLine(True) if digit in [2,3,4,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,1,3,4,5,6,7,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,3,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,6,8] else drawLine(False)
    turtle.left(90)
    drawLine(True) if digit in [0,4,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,3,5,6,7,8,9] else drawLine(False)
    drawLine(True) if digit in [0,1,2,3,4,7,8,9] else drawLine(False)
    turtle.left(180)
    turtle.penup()
    turtle.fd(20) 
```
### 步骤2：获得一串数字，绘制对应的数码管

```python
import turtle
def drawLine(draw):   #绘制单段数码管
    turtle.pendown() if draw else turtle.penup()
    turtle.fd(40)
    turtle.right(90)
def drawDigit(digit): #根据数字绘制七段数码管
    drawLine(True) if digit in [2,3,4,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,1,3,4,5,6,7,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,3,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,6,8] else drawLine(False)
    turtle.left(90)
    drawLine(True) if digit in [0,4,5,6,8,9] else drawLine(False)
    drawLine(True) if digit in [0,2,3,5,6,7,8,9] else drawLine(False)
    drawLine(True) if digit in [0,1,2,3,4,7,8,9] else drawLine(False)
    turtle.left(180)
    turtle.penup()
    turtle.fd(20) 
def drawDate(date):  #获得要输出的数字
    for i in date:
        drawDigit(eval(i))  #通过eval()函数将数字变为整数
def main():
    turtle.setup(800, 350, 200, 200)
    turtle.penup()
    turtle.fd(-300)
    turtle.pensize(5)
    drawDate('20181010')
    turtle.hideturtle()
    turtle.done()
main()    

```
效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424214527789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

### 步骤3：获得当前系统时间，绘制对应的数码管
用time库获取当前时间，并用time.strftime格式化时间，约定好- = + 分别表示年月日。利用turtle.write函数，写下”年月日“这三个汉字。最后再优化一下，比如画笔之间的间隔长短，让七段数码管不在连起来，而是有段落感，更像现实中的七段数码管。再比如用turtle.pencolor改变颜色，等等

```python
#SevenDigitsDrawV2.py
import turtle, time
def drawGap(): #绘制数码管间隔
    turtle.penup()
    turtle.fd(5)
def drawLine(draw):   #绘制单段数码管
    drawGap()
    turtle.pendown() if draw else turtle.penup()
    turtle.fd(40)
    drawGap()
    turtle.right(90)
def drawDigit(d): #根据数字绘制七段数码管
    drawLine(True) if d in [2,3,4,5,6,8,9] else drawLine(False)
    drawLine(True) if d in [0,1,3,4,5,6,7,8,9] else drawLine(False)
    drawLine(True) if d in [0,2,3,5,6,8,9] else drawLine(False)
    drawLine(True) if d in [0,2,6,8] else drawLine(False)
    turtle.left(90)
    drawLine(True) if d in [0,4,5,6,8,9] else drawLine(False)
    drawLine(True) if d in [0,2,3,5,6,7,8,9] else drawLine(False)
    drawLine(True) if d in [0,1,2,3,4,7,8,9] else drawLine(False)
    turtle.left(180)
    turtle.penup()
    turtle.fd(20)
def drawDate(date):
    turtle.pencolor("red")
    for i in date:
        if i == '-':
            turtle.write('年',font=("Arial", 18, "normal"))
            turtle.pencolor("green")
            turtle.fd(40)
        elif i == '=':
            turtle.write('月',font=("Arial", 18, "normal"))
            turtle.pencolor("blue")
            turtle.fd(40)
        elif i == '+':
            turtle.write('日',font=("Arial", 18, "normal"))
        else:
            drawDigit(eval(i))
def main():
    turtle.setup(800, 350, 200, 200)
    turtle.penup()
    turtle.fd(-350)
    turtle.pensize(5)
#    drawDate('2018-10=10+')
    drawDate(time.strftime('%Y-%m=%d+',time.gmtime()))
    turtle.hideturtle()
    turtle.done()
main()

```
最终效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200424214513362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
