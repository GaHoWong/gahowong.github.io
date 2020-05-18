# time库的使用




------

终于学到time库了，我之前学Linux时已经吃透了，所以学习python的time库毫无压力。无非是复习之前的知识而已。但博文还是要写的，毕竟记录的东西，不写以后忘了就难受了。
### time的起始
利用time.time()可以获取一长串的浮点数，这一串浮点数就是从1970年1月1日0时0分0秒开始算起的。很多编程语言起源于UNIX系统，而UNIX系统认为1970年1月1日0点是时间纪元，所以我们常说的UNIX时间戳是以1970年1月1日0点为计时起点时间的。

最初计算机操作系统是32位，而时间也是用32位表示。
Integer在JAVA内用32位表示，因此32位能表示的最大值是2147483647。另外1年365天的总秒数是31536000，2147483647/31536000 = 68.1，也就是说32位能表示的最长时间是68年，从1970年开始的话，加上68.1，实际最终到2038年01月19日03时14分07秒，便会到达最大时间，过了这个时间点，所有32位操作系统时间便会变为10000000 00000000 00000000 00000000，算下来也就是1901年12月13日20时45分52秒，这样便会出现时间回归的现象，很多软件便会运行异常了。

到这里，我想问题的答案已经显现出来了，那就是:因为用32位来表示时间的最大间隔是68年，而最早出现的UNIX操作系统考虑到计算机产生的年代和应用的时限综合取了1970年1月1日作为UNIX TIME的纪元时间(开始时间)，至于时间回归的现象相信随着64为操作系统的产生逐渐得到解决，因为用64位操作系统可以表示到292,277,026,596年12月4日15时30分08秒，相信我们的N代子孙，哪怕地球毁灭那天都不用愁不够用了，因为这个时间已经是千亿年以后了。

### time函数
这里就主要记录一下常用的几个函数

-时间获取：time()  ctime()  gmtime()

-时间格式化：strftime()   strptime()

-程序计时：sleep(), perf_counter()

#### 时间获取
```python
time()
获取当前时间戳，即计算机内部时间值，浮点数
>>>time.time()
>1516939876.6022282
```

```python
ctime()
获取当前时间并以易读方式表示，返回字符串
>>>time.ctime()
>'Fri Jan 26 12:11:16 2019'
```

```python
gmtime()
获取当前时间，表示为计算机可处理的时间格式
>>>time.gmtime()
time.struct_time(tm_year=2019,tm_mon=1,tm_mday=26,tm_hour=4, tm_min=11,tm_sec=16,tm_wday=4,tm_yday=26,tm_isdst=0)
```
#### 时间格式化
格式化：类似字符串格式化，需要有展示模板，展示模板由特定的格式化控制符组成
**strftime(tpl, ts )是个模板函数。需要什么样的输出格式用户可以自定义**
```python
strftime(tpl, ts )
tpl是格式化模板字符串，用来定义输出效果ts是计算机内部时间类型变量
>>>t = time.gmtime()
>>>time.strftime("%Y -%m-%d %H:%M:%S",t)
>'2018-01 -26 12:55:20
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200418190256980.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200418190307423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
**把字符串变成计算机内部可以操作的时间。与strftime(）互补**

```python
strptime(str,  tpl)
str是字符串形式的时间值tpl是格式化模板字符串，用来定义输入效果
>>>timeStr= '2018-01-  26 12:55:20'
>>>time.strptime(timeStr, "%Y -%m -%d %H:%M:%S")
time.struct_time(tm_year=2018,tm_mon=1,tm_mday=26,tm_hour=4, tm_min=11,tm_sec=16,tm_wday=4,tm_yday=26,tm_isdst=0)
```
他们之间的关系
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200418191345219.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 程序计时
```python
perf_counter()
返回一个CPU级别的精确时间计数值，单位为秒由于这个计数值起点不确定，连续调用差值才有意义
>>>start = time.perf_counter()
318.66599499718114
>>>end = time.perf_counter() 
341.3905185375658
>>end - start
22.724523540384666
```

```python
sleep(s)
s拟休眠的时间，单位是秒，可以是浮点数
>>>def wait():
    time.sleep(3.3)
>>>wait()
程序将等待3.3秒后再退出
```

资料来源：
https://docs.python.org/zh-cn/3/

------

[^undefined]:

参考资料:[Python语言程序设计基础(第2版)》嵩天、礼欣、黄天羽著，高等教育出版社，2017.2（讲授Python 3版本）](https://item.jd.com/12128326.html?dist=jd)

[视频课程](http://www.icourse163.org/course/BIT-268001)

[Python3菜鸟教程](https://www.runoob.com/python3/python3-number.html)

[Python官方手册](https://docs.python.org/zh-cn/3/)


