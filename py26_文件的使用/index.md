# 文件的使用




------

### 定义
文件是数据的抽象和集合

是存储在辅助存储器上的数据序列

是数据存储的一种形式

#### 文件的展现形态：文本文件和二进制文件
文件文件和二进制文件只是文件的展示方式，本质上，所有文件都是二进制形式存储。形式上，所有文件采用两种方式展示。

**文本文件**

由单一特定编码组成的文件，如UTF-8编码

-由于存在编码，也被看成是存储着的长字符串

-适用于例如：.txt文件、.py文件等

**二进制文件**

-直接由比特0和1组成，没有统一字符编码

-一般存在二进制0和1的组织结构，即文件格式

-适用于例如：.png文件、.avi文件等

### 文件的打开
Python open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。
```python
<变量名>=  open(<文件名>,   <打开模式>)
```
文件名：可以是绝对路径，也可以是相对路径

打开模式：文本or 二进制，读or 写
**完整格式：**

```python
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```
参数说明:
file: 必需，文件路径（相对或者绝对路径）。

mode: 可选，文件打开模式

buffering: 设置缓冲

encoding: 一般使用utf8

errors: 报错级别

newline: 区分换行符

closefd: 传入的file参数类型

opener:
注：一般只使用第一第二个参数打开就行了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200428212836947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

### 文件的操作
#### 读
1、读出全部内容，如果给出参数，读入前size长度
```python
<f>.read(size=-1)
```
2、读出一行内容，如果给出参数，读出该行前size长度

```python
<f>.readline(size=-1)
```
3、读出文件所有行，以每行为元素形成列表如果给出参数，读出前hint行

```python
<f>.readlines(hint=-1)
```

**遍历全文本：一次性读出所以内容，统一处理**

```python
fname= input("请输入要打开的文件名称:")
fo  = open(fname,"r" )
txt = fo.read()
#对全文txt进行处理
fo.close()
```
**遍历全文本：按数量读入，逐步处理**

```python
fname= input("请输入要打开的文件名称:")
fo  = open(fname,"r" )
txt = fo.read(2)
whiletxt != "":  #对txt进行处理
    txt= fo.read(2)
fo.close()
```
**逐行遍历文件：一次读入，分行处理**

```python
fname= input("请输入要打开的文件名称:")
fo  = open(fname,"r" )
for line in fo.readlines():
	print(line)
fo.close()
```
**逐行遍历文件：分行读入，逐行处理**

```python
fname= input("请输入要打开的文件名称:")
fo  = open(fname,"r" )
for　line in　fo:
	print(line)
fo.close()
```

#### 写
1、向文件写入一个字符串或字节流

```python
<f>.write(s)
```
2、将一个元素全为字符串的列表写入文件

```python
<f>.writelines(lines)
```
3、改变当前文件操作指针的位置，offset含义如下：0   –文件开头；1   –当前位置；2   –文件结尾

```python
<f>.seek(offset)
```

```python
fo  = open("output.txt","w+")
ls = ["中国","法国","美国"]
fo.writelines(ls)
fo.seek(0)#让指针回到刚开始的位置
for line in fo :
	print(line)
fo.close()
```

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
