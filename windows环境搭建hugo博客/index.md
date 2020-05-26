# Windows环境搭建Hugo博客






**！！！前提条件：在搭建Hugo博客之前，需要配置好git环境**
### 下载hugo
在这里（https://github.com/gohugoio/hugo/releases） [下载hugo](https://github.com/gohugoio/hugo/releases)，推荐下载hugo_extended版本，很多hugo主题需要扩展版支持。如果你发现下载太慢，可以给试试我上传到csdn的资源（[点这里跳转](https://download.csdn.net/download/OldHuangC/12458839)）
### 添加环境变量
下载好之后，选择把他复制到安装路径解压，我的是（D:\Program Files\hugo）然后复制你的路径，将它添加到系统的环境变量
![添加环境变量](https://img-blog.csdnimg.cn/20200525215236460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)![添加环境变量](https://img-blog.csdnimg.cn/20200525215245275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

添加完之后，右键打开git bash 输入

```bash
hugo version
```
出现版本信息，说明成功了
![成功](https://img-blog.csdnimg.cn/20200525215526651.png)
### 创建hugo博客
1、选择你的博客本地保存地址，在博客本地保存地址下打开git bash输入命令行

```bash
hugo new site blog  
```

注：blog为文件夹名字


#### 2、下载设置主题
**方法一**：去https://themes.gohugo.io/选择自己喜欢的主题进行下载，并把它放在：你保存的的位置/myblog/themes下，解压

**方法二**：使用git clone，找到你喜欢的主题在GitHub的仓库地址，执行以下命令，这里以LoveIt主题为例。

```bash
~/blog $ cd blog
~/blog $ git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

**（推荐）方法三：**前面两种方法当主题有更新时，不利于升级，推荐用第三种方法，通过创建

```bash
~ $ cd blog
~/blog $ git init
~/blog $ git submodule add --depth 1 https://github.com/dillonzq/LoveIt.git themes/meme
```
通过方法三创建的blog如何更新？

```bash
~/blog $ git submodule update --rebase --remote
```



#### 3、启动博客命令

```bash
hugo server -t 主题名字 --buildDrafts
```

### 把博客部署到github page上
#### 1、新建仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020052522313449.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

以本人博客为例，我的GitHub名称为GaHoWong，那么前面那个红框框里，就必须填你的GitHub名称，**且必须将大写转换为小写，有标点符号的加上标点符号**，然后在名称后面加上`.github.io`比如我的博客就是`gahowong.github.io`以后进入博客的网址也就是这个了。（这一步必须这样做，要不然网站部署不上去）

#### 2、将博客地址与hugo链接
在../myblog目录下打开git bash，执行下面那个命令
```bash
hugo --theme=m10c --baseUrl="https://你的GitHub名称.github.io/" --buildDrafts
```
![关联](https://img-blog.csdnimg.cn/20200525224221802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

然后你会发现在../myblog目录下多了一个public文件

#### 3、将博客链接到GitHub仓库并推送

```bash
cd public #切换到public目录下
git init #初始化git
git add .
git commit -m "第一次提交"
git remote add origin 整个仓库的地址.git
```
![关联](https://img-blog.csdnimg.cn/20200525225459265.png)

> 如果出现了红色框框的错误，只需要执行`git remote rm origin`后再次执行就行了，第一次创建的话是不会出现这种错误的。
#### 4、将博客push到GitHub仓库

```bash
git push -u origin master
```
期间只要登陆github即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200525225956676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 最后访问你的博客，大功告成
在浏览器输入你的博客地址，回车即可看见你刚刚创建的博客了。

再推荐一款免费轻巧的markdown文本编辑器:[Typora](https://www.typora.io/)
