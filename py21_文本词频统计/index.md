# 用python实现文本词频统计


------

### 英文文本
对于英文文本，首先要把文本放到当前代码.py文件的目录下。

1、对文本进行open操作

2、由于英文单词有大小写的，将所有大写转换为小写（这一步可以不用，但最好就是转换一下，英文大写的首字母和单词本身的意思并没有改变）

3、将文本中的特殊标点符号替换成空格。对文本进行遍历操作，用replace（）替换成空格，

4、主函数中，对文本进行读取

5、由于单词之间用空格的分隔，可以用.split的方法，将它们变成一个列表

6、定义一个空字典

7、用当前的某一个英文单词索引键，如果它在字典里面，则返回它的次数，后面再加1，说明这个单词又出现了一次。如果单词不在这个字典中，那就把单词加到字典中，并且赋给当前的值为0，然后加1，说明这个单词出现了一次。

（radiansdict.get(key, default=None)

返回指定键的值，如果值不在字典中返回default值）

8、为了方便单词进行排序，将字典转变为列表

9、利用list.sort( key=None, reverse=False)函数进行排序。（items.sort(key=lambda x: x[1],reverse=True) 中 items.sort() 为待排序的对象；key=lambda x: x[1] 为对前面的对象中的第二维数据（即value）的值进行排序。key=lambda  变量：变量[维数] 。维数可以按照自己的需要进行设置。）

10、将单词和数值输出。

```python
#英文文本的词频统计
def getText():
    txt = open("哈姆雷特.txt","r").read()
    #txt = txt.lower()
    for ch in '!"#$%&()*+,-./:;<=>?@[\\]^_‘{|}~':
        txt = txt.replace(ch," ")
    return txt
hamletTxt = getText()
words = hamletTxt.split()
counts = {}
for word in words:
    counts[word] = counts.get(word,0) + 1
items = list(counts.items())
items.sort(key=lambda x:x[1],reverse=True)
for i in range(20):
    word,count = items[i]
    print("{0:<10}{1:>5}".format(word,count))
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427183404578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
发现“the” 和“and”是最多的。哈哈哈

### 中文文本
按照英文文本的方法，这次使用了jieba库对中文进行分词，由于中文没有大小写的问题和特殊字符的问题，所以相较于英文要简单一点，在判断次数那里，加入了是不是一个中文的判断，如果是一个中文则跳过。以下是代码和效果。。。

```python
import jieba
txt = open("三国演义.txt","r",encoding="utf-8").read()
words = jieba.lcut(txt)
counts = {}
for word in words:
    if len(word) == 1:#只有一个字的去除
        continue
    else:
        counts[word] = counts.get(word,0) + 1
items = list(counts.items())
items.sort(key = lambda x:x[1],reverse=True)
for i in range(15):
    word,count = items[i]
    print("{0:<10}{1:>5}".format(word,count))
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427183453206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

发现并没有达到预期的效果，比如孔明和孔明曰就是同一个人，还有“将军”“却说”“二人”等这些也不是人名，因此需要改进。

```python
#CalThreeKingdomsV2.py
import jieba
excludes = {"将军","不知","都督","人马","一人","陛下","魏兵","却说","荆州","二人","不可","不能","如此","如何","今日","不敢","主公","军士","左右","军马","引兵","次日","大喜","天下","东吴","于是","商议"}
txt = open("三国演义.txt", "r", encoding='utf-8').read()
words  = jieba.lcut(txt)
counts = {}
for word in words:
    if len(word) == 1:
        continue
    elif word == "诸葛亮" or word == "孔明曰":
        rword = "孔明"
    elif word == "关公" or word == "云长":
        rword = "关羽"
    elif word == "玄德" or word == "玄德曰":
        rword = "刘备"
    elif word == "孟德" or word == "丞相":
        rword = "曹操"
    else:
        rword = word
    counts[rword] = counts.get(rword,0) + 1
for word in excludes:
    del counts[word]
items = list(counts.items())
items.sort(key=lambda x:x[1], reverse=True) 
for i in range(10):
    word, count = items[i]
    print ("{0:<10}{1:>5}".format(word, count))

```
经过一轮又一轮的数据清洗，最终得出：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427190850286.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

三国演义居然不是刘备出现得最多，要知道三国演义更偏向于汉室的。震惊！

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
