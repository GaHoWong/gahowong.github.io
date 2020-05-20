# Python format 格式化函数及槽的用法




女朋友今天居然问我format函数的用法，索性我就一次性整理完所有关于格式化输出的知识吧，以后忘了至少还能回头看看。
### format的基本用法
一般最常见的就是这种用法了
```python
print("{} {}".format("hello", "world"))
```
```
 hello world
```
前面的两个{}，我们称为槽，既然是槽，那么可以理解为它是给后面的.format函数装东西的。如果槽里面是空的，那么它会自动排序，比如上面的第一个槽，表示第0个元素“hello”第二个槽代表第1个元素“world”。当然我们也可以改变他们的位置，比如：

```python
print("{1} {0}".format("hello", "world"))
```

```
 world hello
```
但如果你填了，请务必把所有的槽都编上号，否则就会出错，比如下面这种错误的用法。

```python
print("{0} {} {}".format("1", "2","3"))
```
```
ValueError: cannot switch from manual field specification to automatic field numbering
```
### format设置参数

```python
print("网站名：{name}, 地址：{url}".format(name="黄家豪的博客", url="https://gahowong.github.io/"))
 
# 通过字典设置参数
site = {"name": "黄家豪的博客", "url": "https://gahowong.github.io/"}
print("网站名：{name}, 地址 {url}".format(**site))
 
# 通过列表索引设置参数
my_list = ['黄家豪的博客', 'https://gahowong.github.io/']
print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的
```
```python
网站名：黄家豪的博客, 地址：https://gahowong.github.io/
```
### 更多的格式化输出
#### （一）数字格式
**1、比如最常用的就是去除后面的小数点**
```python
print("{:.2f}".format(3.1415926));
```
```
3.14
```
**带符号保留小数点后两位**
```python
print("{:+.2f}".format(-3.1415926));
```

```
-3.14
```
**当然，也可以不带小数点输出**
```python
print("{:+.0f}".format(-3.1415926));
```
```
-3
```
**2、以逗号分隔的数字格式**

```python
print("	{:,}".format(10000000))
```

```
10,000,000
```
**3、百分比格式**

```python
print("	{:.2%}".format(0.23))
print("	{:.5%}".format(0.23))
```

```
23.00%
23.00000%
```
**4、指数记法**

```python
print("	{:.2e}".format(23333333))
print("	{:.5e}".format(23333333))

```
```
2.33e+07
2.33333e+07
```

#### （二）填充和对齐
**1、左填充**，比如左填充7个字符，填充的字符为“0”
```python
print("	{:0>7}".format(3.14))
```

输出
```
0003.14
```
**2、右填充**比如右填充7个字符，填充的字符为“a”
```python
print("	{:a<7}".format(3.14))
```

输出
```
3.14aaa
```

**3、对齐**
```python
print("	{:>7}".format(1)) #右对齐
print("	{:<7}".format(1)) #左对齐
print("	{:^7}".format(1)) #中对齐
```

```
      1
1
   1   
```
#### （三）进制转换

```python
print('{:b}'.format(24))#二进制
print('{:#b}'.format(24))#带标识符的二进制小写形式
print('{:d}'.format(24))#十进制
print('{:o}'.format(24))#八进制
print('{:x}'.format(24))#十六进制
print('{:#x}'.format(24))#带标识符的十六进制小写形式
print('{:#X}'.format(24))#带标识符的十六进制大写形式
```

```
11000
0b11000
24
30
18
0x18
0X18
```

#### （四）其它
1、转移大括号
有时候我们需要打印大括号，但大括号被识别为槽了，这时只要打大括号嵌套大括号就可以解决了。

```python
print ("{} 在我心目中的位置是 {{0}}".format("昭昭"))
```

```
昭昭在我心目中的位置是 {0}
```
