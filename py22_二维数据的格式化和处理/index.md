# 二维数据的格式化和处理


------

### 什么是二维数据？
我的理解跟c语言的二位数组的理解一致，例如这个列表list[0][1]就是二维数据，它跟数学上的二维是一致的。

```python
list = [[1,2,3],[4,5,6],[7,8,9]]
list[0][1]
>>>2
```
说到二维数据的存储，不得不说csv。
 **CSV数据存储格式：**

CSV:(Comma-Separated Values),国际通用的一二维数据存储格式，一般.csv扩展名。每行一个一维数据，采用逗号分隔，无空行。Excel和一般编辑软件都可以读入或另存为csv文件。

在csv中，如果某个元素缺失，逗号仍要保留。二维数据的表头可以作为数据存储，也可以另行存储。逗号为英文半角逗号，逗号与数据之间无额外空格。

数据的存储方式，按行存或者按列存都可以，具体由程序决定。一般索引习惯：ls[row][column]，先行后列。根据一般习惯，外层列表每个元素是一行，按行存。

#### 从CSV格式的文件中读出数据
首先，CSV格式的文件，是以英文逗号分隔元素，以回车换行，因此要把回车给替换成空格式，将每行的元素用逗号分隔开，并且添加到ls这个列表里，这样就作为其中的一个元素。

```python
fo  =  open(fname)
ls = []
for line in fo :
	line = line.replace("\n","" )
	ls.append(line.split(",")) 
fo.close()
```
#### 从CSV格式的文件中写入数据

```python
ls = [[],[],[]] #二维列表
f = open(fname, 'w')
for item in ls:
	f.write(','.join(item) + '\n') 
f.close()
```
#### 二维数据的逐一处理
采用两层循环
```python
ls = [[1,2], [3,4], [5,6]] #二维列表
for row in ls:
	for column in row:
	print(column)

```

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
