# GitHub打开速度慢的解决方法




由于此博客是部署于GitHub平台的，如果打开此博客很慢，说明打开GitHub也很慢，因此寻找了一些加快打开速度的方法
# 方法一：改hosts文件（简单有效）
#### 
1、用记事本打开 **C:\Windows\System32\drivers\etc** 一个名叫hosts的文件（无后缀）

2.打开https://www.ipaddress.com/查询下面三个网站的ip地址，复制。

```bash

github.com

assets-cdn.github.com

github.global.ssl.fastly.net
```
![](https://img-blog.csdnimg.cn/20200427203511406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)



比如我查询的是这些，加入hosts文件后面即可

```bash
140.82.114.4 github.com
199.232.69.194 github.global.ssl.fastly.net
185.199.110.153 assets-cdn.github.com
```


3.保存hosts文件后刷新DNS 解析缓存。CMD输入：

```
ipconfig /flushdns
```



# 方法二：科学上网（简单粗暴）
备用机场：
https://www.wujievpn.xyz/
https://jianguo01.com/user



