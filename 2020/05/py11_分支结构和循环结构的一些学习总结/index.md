# 分支结构和循环结构的一些学习总结




------

### 分支结构
1、对于二分支结构，除了常用的if<条件>：<执行的语句>这种结构之外，还有一种紧凑型结构。<表达式1> if<条件> else<表达式2>

```python
guess = eval(input())
print("猜{}了".format("对"ifguess==99 else "错"  )
```
2、对于多分支结构的代码编写时，需要注意多条件之间的包含关系，变量取值范围的覆盖。
3、在python中，除了常用的条件判断操作符之外，还有三个比较容易理解的保留字，and、or、not
4异常处理，如果程序异常，比如要求输入的是一个数字，但用户输入了一个字母，程序就会产生异常，这时候就需要程序进行异常处理，在python中提供了try except这种方式，当try下面的语句出现了异常，就会执行except下的语句，如第一种方式，如果采用的是第二种方式，则指定了必须由这种异常产生才会执行except下的语句。finally对应的语块4一定会执行，else对应的语块3在不发生异常时执行。

```python
第一种方式：
try:
<语句块1>
except :
<语句块2>

第二种方式：
try:
<语句块1>
except <异常类型>:
<语句块2>
else：
<语句块3>
finally：
<语句块4>
```

### 循环结构
#### 1、遍历循环 for in
```python
for i in range(N):
<语句块>
```
遍历由range()函数产生的数字序列，产生循环。range(<起始>，<结束>，<步长>)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200418210331270.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
遍历循环还可以应用到字符串、列表、文件等等。
#### 2、无限循环while
反复执行语句块，直到条件不满足时结束

```python
while <条件>：
    <语句>
```
**循环控制保留字**
-**break**跳出并结束当前整个循环，执行循环后的语句
-**continue**结束当次循环，继续执行后续次数循环
-break和continue可以与for和while循环搭配使用

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
