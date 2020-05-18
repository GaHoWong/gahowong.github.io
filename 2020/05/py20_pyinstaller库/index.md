# PyInstaller库应用——科赫雪花小包裹


------

PyInstaller库是第三方库，因此需要pip安装，（用的是清华镜像）

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyinstaller
```
安装完会出现这样的提示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427192421561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 使用方法
cmd命令行

```bash
pyinstaller -F <文件名.py>
```

在.py文件的目录下执行这段代码，就可以得到一个exe文件了。
### 常用命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427192957637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 科赫雪花

```python
import turtle
def koch(size, n):
    if n == 0:
        turtle.fd(size)
    else:
        for angle in [0, 60, -120, 60]:
           turtle.left(angle)
           koch(size/3, n-1)
def main():
    turtle.setup(600,600)
    turtle.penup()
    turtle.goto(-200, 100)
    turtle.pendown()
    turtle.pensize(2)
    level = 3      # 3阶科赫雪花，阶数
    koch(400,level)     
    turtle.right(120)
    koch(400,level)
    turtle.right(120)
    koch(400,level)
    turtle.hideturtle()
main()

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427193417628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
将这段代码打包好，就可以发给好朋友，只要他双击exe文件就可以看到了，他也不需要安装任何python解释器。



------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
