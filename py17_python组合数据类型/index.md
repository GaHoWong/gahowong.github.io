# Python组合数据类型




------



之前写了一篇文章是Python基本数据类型，主要记录了python的整数类型、浮点数类型、负数类型和常用的数值运算函数。除了基本类型，python还有组合数据类型：集合类型、序列类型、字典类型

## 集合类型
集合（set）是集合是多个元素的无序组合。

集合类型与数学中的集合概念是一致的

**集合元素之间无序，每个元素唯一，不存在相同元素**

集合元素不可更改，不能是可变数据类型

可以使用大括号 { } 或者 set() 函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。

```python
A = {"python", 123, ("python",123)} #使用{}建立集合
>>>{123, 'python', ('python', 123)}

B = set("pypy123")         #使用set()建立集合
>>>{'1', 'p', '2', '3', 'y'}
```
#### 集合间操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427132345277.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427132352385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427132358756.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 集合处理方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427135333900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427135341371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

```python
A = {"p",  "y" , 123
for item in A
    print(item, end="" ):
>>>p123y
A
>>>{'p', 123, 'y'}
```
#### 集合类型应用场景
**1、包含关系比较**

```python
包含关系比较
"p" in {"p", "  y" , 123}
>>>True
{"p", "y"} >={"p",  "y" , 123}
>>>False
```
**2、数据去重：集合类型所有元素无重复**

```python
ls = ["p",  "p",  "y", "y", 123]
s = set(ls)    # 利用了集合无重复元素的特点
>>>{'p', 'y', 123}

lt  = list(s)   # 还可以将集合转换为列表
>>>['p', 'y', 123]
```

## 序列类型
**序列**是Python中最基本的数据结构。序列中的每个元素都分配一个数字，它的位置，或索引，第一个索引是0，第二个索引是1，依此类推。序列类型支持**反向递减序号和正向递增序号**

Python有6个序列的内置类型，但最常见的是字符串、列表和元组。

序列都可以进行的操作包括索引，切片，加，乘，检查成员。

此外，Python已经内置确定序列的长度以及确定最大和最小的元素的方法。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427133132152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

### 序列类型通用操作符
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427133435475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

```python
ls  = [ "python", 123, ".io "]
ls[::-1]
>>>['. io', 123, 'python']

s = "python123.io"
s[::-1]
>>>'oi.321nohtyp'
```

### 序列类型通用函数和方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427133459109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

```python
ls = ["python", 123,".io "
len(ls)
>>>3

s = "python123.io"
max(s)
>>>'y'
```

### 元组
Python 的元组是序列类型的一种扩展，与列表类似，不同之处在于元组的元素不能修改。

使用小括号() 或tuple() 创建，元素间用逗号, 分隔。

可以使用或不使用小括号，函数return时就是返回一个元组类型

#### 元组创建
```python
creature = "cat ", "  dog  ", "tiger", "human"
creature 
>>>('cat', 'dog', 'tiger', 'human')

color = (0x001100, "blue", creature)
color
>>>(4352, 'blue', ('cat', 'dog', 'tiger', 'human'))
```
#### 元组类型操作
元组继承序列类型的全部通用操作，元组因为创建后不能修改，因此没有特殊操作，使用或不使用小括号都行。



### 列表
列表是最常用的Python数据类型，它可以作为一个方括号内的逗号分隔值出现。

列表的数据项不需要具有相同的类型，无长度限制

列表是一种序列类型，创建后可以随意被修改

使用方括号[] 或list() 创建，元素间用逗号, 分隔
#### 列表的创建

```python
ls = ["cat ", "  dog ", "tiger", 1024]
ls
>>>['cat', 'dog', 'tiger', 1024] 
lt  = ls
lt
>>>['cat', 'dog', 'tiger', 1024] 
```
注意：lt = ls 这条赋值语句，it并不是真正创建了一个列表，赋值仅传递引用。类似c语言的指针，lt和ls都是指向['cat', 'dog', 'tiger', 1024] ，都是它的名字。
#### 列表类型操作函数和方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427134715183.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

```python
ls = ["cat ", "  dog ", "tiger", 1024]
ls[1:2]=  [1, 2, 3, 4]
>>>['cat', 1, 2, 3, 4, 'tiger', 1024]

del ls[::3]
>>>[1, 2, 4, 'tiger']

ls *2
[1, 2, 4, 'tiger', 1, 2, 4, 'tiger']
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427134914196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

```python
#在最后添加元素
ls = ["cat ", "  dog ", "tiger", 1024]
ls .append(1234)
>>>['cat', 'dog', 'tiger', 1024, 1234]

#指定位置增加元素
ls.insert(3, "human")
['cat', 'dog', 'tiger', 'human', 1024, 1234]

#元素反转
ls.reverse()
[1234, 1024, 'human', 'tiger', 'dog', 'cat']
```
### 序列类型应用场景
数据表示：元组和列表

1、元组用于元素不改变的应用场景，更多用于固定搭配场景

2、列表更加灵活，它是最常用的序列类型

3、最主要作用：表示一组有序数据，进而操作它们

4、如果不希望数据被程序所改变，转换成元组类型

```python
ls = ["cat ", "  dog ", "tiger", 1024]
lt  = tuple(ls)  #转换为元组类型
lt
>>>('cat', 'dog', 'tiger', 1024)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042714542235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427145430927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



## 字典类型
字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值(key=>value)对用冒号(:)分割，每个对之间用逗号(,)分割，整个字典包括在花括号({})中 ,格式如下所示：

```python
d = {key1 : value1, key2 : value2 }
```
键必须是唯一的，但值则不必。

值可以取任何数据类型，但键必须是不可变的，如字符串，数字或元组。

一个简单的字典实例：

```python
dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}

dict1 = { 'abc': 456 }
dict2 = { 'abc': 123, 98.6: 37 }
```
**访问字典里的值**

```python
dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
 
print ("dict['Name']: ", dict['Name'])
print ("dict['Age']: ", dict['Age'])
>>>
dict['Name']:  Runoob
dict['Age']:  7
```
### 字典类型操作函数和方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427145010247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427145017498.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### 字典类型应用场景
1、映射无处不在，键值对无处不在

2、例如：统计数据出现的次数，数据是键，次数是值

3、最主要作用：表达键值对数据，进而操作它们

4、d[key] 方式既可以索引，也可以赋值
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427145232441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
