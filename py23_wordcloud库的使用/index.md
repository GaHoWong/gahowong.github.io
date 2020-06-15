# wordcloud库


wordcloud是优秀的词云展示第三方库，它可以根据文本中词语出现的频率等参数绘制词云，词云的绘制形状、尺寸和颜色都可以设定。

------

### 安装

安装jieba库（中文分词库）

```
pip install jieba-i https://pypi.doubanio.com/simple/
```

安装wordcloud（用来生成图云）
```
pip install wordcloud  -i https://pypi.doubanio.com/simple/
```
安装imageio（用来获取图像）

```
pip install imageio  -i https://pypi.doubanio.com/simple/
```

### 常规使用方法：
**步骤1：配置对象参数**

**步骤2：加载词云文本**

**步骤3：输出词云文件**

```python
import wordcloud
c = wordcloud.WordCloud()
c.generate("wordcloudby Python")
c.to_file("pywordcloud.png")
```
#### 配置参数

```python
w = wordcloud.WordCloud(<参数>)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200428203643594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

如果要输出指定形状的词云，需要一张白色背景的图片。例如
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200428211531733.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200428211610766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
```python
#GovRptWordCloudv2.py
import jieba
import wordcloud
import imageio
mask = imageio.imread("liubei.jpg")
excludes = { }
f = open("三国演义.txt", "r", encoding="utf-8")
t = f.read()
f.close()
ls = jieba.lcut(t)
txt = " ".join(ls)
w = wordcloud.WordCloud(\
    width = 1000, height = 700,\
    background_color = "white",
    font_path = "msyh.ttc", mask = mask
    )
w.generate(txt)
w.to_file("grwordcloudm.png")
```
------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)
