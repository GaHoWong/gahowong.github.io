# 用Python绘图之Turtle(海龟)库详解




Python的学习总是很有趣，强大的库让人欲罢不能，即使作为初学者的我来说，用代码绘图是极具诱惑力的，而Turtle库是Python语言中一个很流行的绘制图像的函数库，它的使用方法也很简答，只要我们把自己想象成一个小乌龟，在一个横轴为x、纵轴为y的坐标系原点，(0,0)位置开始，它根据一组函数指令的控制，在这个平面坐标系中移动，从而在它爬行的路径上绘制了图形。只要了解一些基本概念就可以快速上手。
### 1. 画布(canvas)

画布就是turtle为我们展开用于绘图区域，我们可以设置它的大小和初始位置。

#### 设置画布大小
1、
```bash
turtle.screensize(canvwidth=None, canvheight=None, bg=None) 
```
参数分别为画布的宽(单位像素), 高, 背景颜色。
如：
```bash
turtle.screensize(800,600, "green")
turtle.screensize() #返回默认大小(400, 300)
```
2、
```bash
turtle.setup(width=0.5, height=0.75, startx=None, starty=None)
```

参数：width, height: 输入宽和高为整数时, 表示像素; 为小数时, 表示占据电脑屏幕的比例，(startx, starty): 这一坐标表示矩形窗口左上角顶点的位置, 如果为空,则窗口位于屏幕中心。
如：
```bash
turtle.setup(width=0.6,height=0.6)
turtle.setup(width=800,height=800, startx=100, starty=100)
```
### 2、画笔

 在画布上，默认有一个坐标原点为画布中心的坐标轴，坐标原点上有一只面朝x轴正方向小乌龟。这里我们描述小乌龟时使用了两个词语：坐标原点(位置)，面朝x轴正方向(方向)， turtle绘图中，就是使用位置方向描述小乌龟(画笔)的状态。

1) 设置画笔的宽度，单位为像素；
```bash
turtle.pensize()：
```
2)没有参数传入，返回当前画笔颜色，传入参数设置画笔颜色，可以是字符串如"green", "red",也可以是RGB 3元组。
```bash        
 turtle.pencolor()：
```
3)设置画笔移动速度，画笔绘制的速度范围[0,10]整数，数字越大越快。
 ```bash     
 turtle.speed(speed)：
 ```
#### (1)画笔运动命令

| 命令                      | 说明                                               |
| ------------------------- | -------------------------------------------------- |
| turtle.forward(distance)  | 向当前画笔方向移动distance像素长度                 |
| turtle.backward(distance) | 向当前画笔相反方向移动distance像素长度             |
| turtle.right(degree)      | 顺时针移动degree°                                  |
| turtle.left(degree)       | 逆时针移动degree°                                  |
| turtle.pendown()          | 移动时绘制图形，缺省时也为绘制                     |
| turtle.goto(x,y)          | 将画笔移动到坐标为x,y的位置                        |
| turtle.penup()            | 提起笔移动，不绘制图形，用于另起一个地方绘制       |
| turtle.circle()           | 画圆，半径为正(负)，表示圆心在画笔的左边(右边)画圆 |
| setx( )                   | 将当前x轴移动到指定位置                            |
| sety( )                   | 将当前y轴移动到指定位置                            |
| setheading(angle)         | 设置当前朝向为angle角度                            |
| home()                    | 设置当前画笔位置为原点，朝向东。                   |
| dot( r )                  | 绘制一个指定直径和颜色的圆点                       |
#### (2)画笔控制命令

| 命令                          | 说明                                      |
| ----------------------------- | ----------------------------------------- |
| turtle.fillcolor(colorstring) | 绘制图形的填充颜色                        |
| turtle.color(color1, color2)  | 同时设置pencolor=color1, fillcolor=color2 |
| turtle.filling()              | 返回当前是否在填充状态                    |
| turtle.begin_fill()           | 准备开始填充图形                          |
| turtle.end_fill()             | 填充完成                                  |
| turtle.hideturtle()           | 隐藏画笔的turtle形状                      |
| turtle.showturtle()           | 显示画笔的turtle形状                      |

#### (3)全局控制命令

| 命令                                                        | 说明                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| turtle.clear()                                              | 清空turtle窗口，但是turtle的位置和状态不会改变               |
| turtle.reset()                                              | 清空窗口，重置turtle状态为起始状态                           |
| turtle.undo()                                               | 撤销上一个turtle动作                                         |
| turtle.isvisible()                                          | 返回当前turtle是否可见                                       |
| stamp()                                                     | 复制当前图形                                                 |
| turtle.write(s [,font=("font-name",font_size,"font_type")]) | 写文本，s为文本内容，font是字体的参数，分别为字体名称，大小和类型；font为可选项，font参数也是可选项 |

#### (4)其它

| 命令                             | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| turtle.mainloop()或turtle.done() | 启动事件循环 -调用Tkinter的mainloop函数。必须是乌龟图形程序中的最后一个语句。 |
| turtle.mode(mode=None)           | 设置乌龟模式（“standard”，“logo”或“world”）并执行重置。如果没有给出模式，则返回当前模式。standard向右（东）逆时针；logo向上（北）顺时针 |
| turtle.delay(delay=None)         | 设置或返回以毫秒为单位的绘图延迟。                           |
| turtle.begin_poly()              | 开始记录多边形的顶点。当前的乌龟位置是多边形的第一个顶点。   |
| turtle.end_poly()                | 停止记录多边形的顶点。当前的乌龟位置是多边形的最后一个顶点。将与第一个顶点相连。 |
| turtle.get_poly()                | 返回最后记录的多边形。                                       |
### 3、命令详解
```bash
turtle.circle(radius, extent=None, steps=None)
```
描述：以给定半径画圆

        参数：
    
        radius(半径)：半径为正(负)，表示圆心在画笔的左边(右边)画圆；
    
        extent(弧度) (optional)；
    
        steps (optional) (做半径为radius的圆的内切正多边形，多边形边数为steps)。


举例:

circle(50) # 整圆;

circle(50,steps=3) # 三角形;

circle(120, 180) # 半圆

```python
#第一次用turtle画画。
import turtle
a = 0
turtle.setup(800, 500) #设置宽为800高为500窗口位于屏幕中心
turtle.penup()         #把笔抬起来
turtle.fd(-250)        #向前移动-250个像素点（也就是后退250个像素点）
turtle.pendown()       #把笔放下
turtle.pensize(5)     #设置笔的大小为25个像素
turtle.pencolor("red")#设置笔的颜色为紫色
turtle.seth(-40)         #海龟头的方向为-40°
for i in range(5):       #循环执行四次
    turtle.circle(40, 80)#以40像素为半径（左边）绘制80度的弧度
    a = a+3             #让蛇的身体慢慢变大
    print(a)
    turtle.pensize(5 + a)
    turtle.circle(-40, 80)#以40像素为半（右边）径绘制80度的弧度
    a = a+3
    print(a)
    turtle.pensize(5 + a)
turtle.seth(0)            #将龟头调整到零度，也就是向X轴正方向
turtle.fd(50)             #前进50个像素
turtle.circle(30, 180)    #以像素30为半径向此时龟头方向以左的方向绘制180度弧形
turtle.pensize(40)        #画笔变大
turtle.fd(40)             #前进40个像素
turtle.pensize(7)         #画笔变小
turtle.fd(30)            #前进30像素
turtle.pensize(4)        #画笔大小为4像素
turtle.circle(15,60)      #画弧
turtle.circle(15,-60)     #画弧，返回
turtle.circle(-15,60)     #画弧
turtle.circle(-15,-60)     #画弧，返回
turtle.backward(40)        #后退40
turtle.seth(90)            #龟头方向调整为90
turtle.fd(10)               #前进10
turtle.pencolor("black")    #改变颜色为黑
turtle.circle(5)            #画眼睛
turtle.penup()              #抬起笔
turtle.backward(20)        #后退20
turtle.pendown()           #放下笔
turtle.circle(5)           #画眼睛
turtle.hideturtle()        #隐藏乌龟
turtle.done()              #保持窗口不关闭
```
效果如下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200416224344412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
''后来又参考别人的代码做了个时钟。。。

```python
# coding=utf-8
import turtle
from datetime import *
# 抬起画笔，向前运动一段距离放下
def Skip(step):
    turtle.penup()
    turtle.forward(step)
    turtle.pendown()


def mkHand(name, length):
    # 注册Turtle形状，建立表针Turtle
    turtle.reset()
    Skip(-length * 0.1)
    # 开始记录多边形的顶点。当前的乌龟位置是多边形的第一个顶点。
    turtle.begin_poly()
    turtle.forward(length * 1.1)
    # 停止记录多边形的顶点。当前的乌龟位置是多边形的最后一个顶点。将与第一个顶点相连。
    turtle.end_poly()
    # 返回最后记录的多边形。
    handForm = turtle.get_poly()
    turtle.register_shape(name, handForm)


def Init():
    global secHand, minHand, hurHand, printer
    # 重置Turtle指向北
    turtle.mode("logo")
    # 建立三个表针Turtle并初始化
    mkHand("secHand", 135)
    mkHand("minHand", 125)
    mkHand("hurHand", 90)
    secHand = turtle.Turtle()
    secHand.shape("secHand")
    minHand = turtle.Turtle()
    minHand.shape("minHand")
    hurHand = turtle.Turtle()
    hurHand.shape("hurHand")

    for hand in secHand, minHand, hurHand:
        hand.shapesize(1, 1, 3)
        hand.speed(0)

        # 建立输出文字Turtle
    printer = turtle.Turtle()

    # 隐藏画笔的turtle形状
    printer.hideturtle()
    printer.penup()


def SetupClock(radius):
    # 建立表的外框
    turtle.reset()
    turtle.pensize(7)
    turtle.pencolor("black")
    turtle.fillcolor("green")

    for i in range(60):
        Skip(radius)
        if i % 5 == 0:
            turtle.forward(20)
            Skip(-radius - 20)

            Skip(radius + 20)
            if i == 0:
                turtle.write(int(12), align="center", font=("Courier", 14, "bold"))
            elif i == 30:
                Skip(25)
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
                Skip(-25)
            elif (i == 25 or i == 35):
                Skip(20)
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
                Skip(-20)
            else:
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
            Skip(-radius - 20)
        else:
            turtle.dot(5)
            Skip(-radius)
        turtle.right(6)


def Week(t):
    week = ["星期一", "星期二", "星期三",
            "星期四", "星期五", "星期六", "星期日"]
    return week[t.weekday()]


def Date(t):
    y = t.year
    m = t.month
    d = t.day
    return "%s-%d-%d" % (y, m, d)


def Tick():
    # 绘制表针的动态显示
    t = datetime.today()
    second = t.second + t.microsecond * 0.000001
    minute = t.minute + second / 60.0
    hour = t.hour + minute / 60.0
    secHand.setheading(6 * second)
    minHand.setheading(6 * minute)
    hurHand.setheading(30 * hour)

    turtle.tracer(False)

    printer.forward(65)
    printer.write(Week(t), align="center",
                  font=("Courier", 14, "bold"))
    printer.back(130)
    printer.write(Date(t), align="center",
                  font=("Courier", 14, "bold"))
    printer.home()
    turtle.tracer(True)

    # 100ms后继续调用tick
    turtle.ontimer(Tick, 100)


def main():
    # 打开/关闭龟动画，并为更新图纸设置延迟。
    turtle.tracer(False)
    Init()
    SetupClock(160)
    turtle.tracer(True)
    Tick()
    turtle.mainloop()


if __name__ == "__main__":
    main()
```
效果如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200416224902539.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
再后来我我看见了git优秀的项目。。。

```python
# coding:utf-8
from turtle import*

def nose(x,y):#鼻子
    pu()
    goto(x,y)
    pd()
    seth(-30)
    begin_fill()
    a=0.4
    for i in range(120):
        if 0<=i<30 or 60<=i<90:
            a=a+0.08
            lt(3) #向左转3度
            fd(a) #向前走a的步长
        else:
            a=a-0.08
            lt(3)
            fd(a)
    end_fill()

    pu()
    seth(90)
    fd(25)
    seth(0)
    fd(10)
    pd()
    pencolor(255,155,192)
    seth(10)
    begin_fill()
    circle(5)
    color(160,82,45)
    end_fill()

    pu()
    seth(0)
    fd(20)
    pd()
    pencolor(255,155,192)
    seth(10)
    begin_fill()
    circle(5)
    color(160,82,45)
    end_fill()


def head(x,y):#头
    color((255,155,192),"pink")
    pu()
    goto(x,y)
    seth(0)
    pd()
    begin_fill()
    seth(180)
    circle(300,-30)
    circle(100,-60)
    circle(80,-100)
    circle(150,-20)
    circle(60,-95)
    seth(161)
    circle(-300,15)
    pu()
    goto(-100,100)
    pd()
    seth(-30)
    a=0.4
    for i in range(60):
        if 0<=i<30 or 60<=i<90:
            a=a+0.08
            lt(3) #向左转3度
            fd(a) #向前走a的步长
        else:
            a=a-0.08
            lt(3)
            fd(a)
    end_fill()


def ears(x,y): #耳朵
    color((255,155,192),"pink")
    pu()
    goto(x,y)
    pd()
    begin_fill()
    seth(100)
    circle(-50,50)
    circle(-10,120)
    circle(-50,54)
    end_fill()

    pu()
    seth(90)
    fd(-12)
    seth(0)
    fd(30)
    pd()
    begin_fill()
    seth(100)
    circle(-50,50)
    circle(-10,120)
    circle(-50,56)
    end_fill()


def eyes(x,y):#眼睛
    color((255,155,192),"white")
    pu()
    seth(90)
    fd(-20)
    seth(0)
    fd(-95)
    pd()
    begin_fill()
    circle(15)
    end_fill()

    color("black")
    pu()
    seth(90)
    fd(12)
    seth(0)
    fd(-3)
    pd()
    begin_fill()
    circle(3)
    end_fill()

    color((255,155,192),"white")
    pu()
    seth(90)
    fd(-25)
    seth(0)
    fd(40)
    pd()
    begin_fill()
    circle(15)
    end_fill()

    color("black")
    pu()
    seth(90)
    fd(12)
    seth(0)
    fd(-3)
    pd()
    begin_fill()
    circle(3)
    end_fill()


def cheek(x,y):#腮
    color((255,155,192))
    pu()
    goto(x,y)
    pd()
    seth(0)
    begin_fill()
    circle(30)
    end_fill()


def mouth(x,y): #嘴
    color(239,69,19)
    pu()
    goto(x,y)
    pd()
    seth(-80)
    circle(30,40)
    circle(40,80)


def body(x,y):#身体
    color("red",(255,99,71))
    pu()
    goto(x,y)
    pd()
    begin_fill()
    seth(-130)
    circle(100,10)
    circle(300,30)
    seth(0)
    fd(230)
    seth(90)
    circle(300,30)
    circle(100,3)
    color((255,155,192),(255,100,100))
    seth(-135)
    circle(-80,63)
    circle(-150,24)
    end_fill()


def hands(x,y):#手
    color((255,155,192))
    pu()
    goto(x,y)
    pd()
    seth(-160)
    circle(300,15)
    pu()
    seth(90)
    fd(15)
    seth(0)
    fd(0)
    pd()
    seth(-10)
    circle(-20,90)

    pu()
    seth(90)
    fd(30)
    seth(0)
    fd(237)
    pd()
    seth(-20)
    circle(-300,15)
    pu()
    seth(90)
    fd(20)
    seth(0)
    fd(0)
    pd()
    seth(-170)
    circle(20,90)

def foot(x,y):#脚
    pensize(10)
    color((240,128,128))
    pu()
    goto(x,y)
    pd()
    seth(-90)
    fd(40)
    seth(-180)
    color("black")
    pensize(15)
    fd(20)

    pensize(10)
    color((240,128,128))
    pu()
    seth(90)
    fd(40)
    seth(0)
    fd(90)
    pd()
    seth(-90)
    fd(40)
    seth(-180)
    color("black")
    pensize(15)
    fd(20)

def tail(x,y):#尾巴
    pensize(4)
    color((255,155,192))
    pu()
    goto(x,y)
    pd()
    seth(0)
    circle(70,20)
    circle(10,330)
    circle(70,30)

def setting():          #参数设置
    pensize(4)
    hideturtle()
    colormode(255)
    color((255,155,192),"pink")
    setup(840,500)
    speed(10)

def main():
    setting()           #画布、画笔设置
    nose(-100,100)      #鼻子
    head(-69,167)       #头
    ears(0,160)         #耳朵
    eyes(0,140)         #眼睛
    cheek(80,10)        #腮
    mouth(-20,30)       #嘴
    body(-32,-8)        #身体
    hands(-56,-45)      #手
    foot(2,-177)        #脚
    tail(148,-155)      #尾巴
    done()              #结束

main()
```

效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200416232600571.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)


```python
import turtle as T
import random
import time


# 画樱花的躯干(60,t)
def Tree(branch, t):
    time.sleep(0.0005)
    if branch > 3:
        if 8 <= branch <= 12:
            if random.randint(0, 2) == 0:
                t.color('snow')  # 白
            else:
                t.color('lightcoral')  # 淡珊瑚色
            t.pensize(branch / 3)
        elif branch < 8:
            if random.randint(0, 1) == 0:
                t.color('snow')
            else:
                t.color('lightcoral')  # 淡珊瑚色
            t.pensize(branch / 2)
        else:
            t.color('sienna')  # 赭(zhě)色
            t.pensize(branch / 10)  # 6
        t.forward(branch)
        a = 1.5 * random.random()
        t.right(20 * a)
        b = 1.5 * random.random()
        Tree(branch - 10 * b, t)
        t.left(40 * a)
        Tree(branch - 10 * b, t)
        t.right(20 * a)
        t.up()
        t.backward(branch)
        t.down()


# 掉落的花瓣
def Petal(m, t):
    for i in range(m):
        a = 200 - 400 * random.random()
        b = 10 - 20 * random.random()
        t.up()
        t.forward(b)
        t.left(90)
        t.forward(a)
        t.down()
        t.color('lightcoral')  # 淡珊瑚色
        t.circle(1)
        t.up()
        t.backward(a)
        t.right(90)
        t.backward(b)


# 绘图区域
t = T.Turtle()
# 画布大小
w = T.Screen()
t.hideturtle()  # 隐藏画笔
t.getscreen().tracer(5, 0)
w.screensize(bg='wheat')  # wheat小麦
t.left(90)
t.up()
t.backward(150)
t.down()
t.color('sienna')

# 画樱花的躯干
Tree(60, t)
# 掉落的花瓣
Petal(200, t)
w.exitonclick()
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200416225654898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
还有经典的二叉树问题！厉害👍，教练我想...(啪)
```python
from turtle import *
from random import *
from math import *


def tree(n, l):
    pd()  # 下笔
    # 阴影效果
    t = cos(radians(heading() + 45)) / 8 + 0.25
    pencolor(t, t, t)
    pensize(n / 3)
    forward(l)  # 画树枝

    if n > 0:
        b = random() * 15 + 10  # 右分支偏转角度
        c = random() * 15 + 10  # 左分支偏转角度
        d = l * (random() * 0.25 + 0.7)  # 下一个分支的长度
        # 右转一定角度,画右分支
        right(b)
        tree(n - 1, d)
        # 左转一定角度，画左分支
        left(b + c)
        tree(n - 1, d)
        # 转回来
        right(c)
    else:
        # 画叶子
        right(90)
        n = cos(radians(heading() - 45)) / 4 + 0.5
        pencolor(n, n * 0.8, n * 0.8)
        circle(3)
        left(90)
        # 添加0.3倍的飘落叶子
        if (random() > 0.7):
            pu()
            # 飘落
            t = heading()
            an = -40 + random() * 40
            setheading(an)
            dis = int(800 * random() * 0.5 + 400 * random() * 0.3 + 200 * random() * 0.2)
            forward(dis)
            setheading(t)
            # 画叶子
            pd()
            right(90)
            n = cos(radians(heading() - 45)) / 4 + 0.5
            pencolor(n * 0.5 + 0.5, 0.4 + n * 0.4, 0.4 + n * 0.4)
            circle(2)
            left(90)
            pu()
            # 返回
            t = heading()
            setheading(an)
            backward(dis)
            setheading(t)
    pu()
    backward(l)  # 退回


bgcolor(0.5, 0.5, 0.5)  # 背景色
ht()  # 隐藏turtle
speed(0)  # 速度 1-10渐进，0 最快
# tracer(0,0)  # 这一行决定是否动态
pu()  # 抬笔
backward(100)
left(90)  # 左转90度
pu()  # 抬笔
backward(300)  # 后退300
tree(12, 100)  # 递归7层
done()
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200416230306212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)



