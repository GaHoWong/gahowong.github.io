# 如何用VSCode配置一个超舒服的Python编程环境？


#### 一、安装

1、首先去[官网](https://code.visualstudio.com/)（https://code.visualstudio.com/）下载最新版的VSCode，下载好后双击安装→选择安装路径→安装。安装到这里时，推荐把所有选项都√勾上。
 ![所有都勾上哦](https://img-blog.csdnimg.cn/20200529100032218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70) 

#### 二、配置Python编译环境

 1、先去[官网](https://www.python.org/downloads/)下载python的解释器，安装。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200529142145728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

安装的时候记得勾选环境变量

 2、检查python是否安装成功

检查方法如下：打开cmd，输入`python`，点击回车。

#### 三、VSCode中python的配置
1、新建文件夹：E:\Python\code

2、打开VSCode，在E:\Python\code下创建1.py

3、编写代码

```python
print(hello word)
```
右键运行，然后会弹出询问是否安装python扩展，点安装，再次运行

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200529143736649.png)
#### 四、安装插件
 安装以下几个插件，对C/C++的编程具有极大的帮助。

 安装方法：打开VSCode后，打开插件商城，快捷键（`Ctrl + Shift + X`）打开插件商城，搜索插件，点击安装即可。

 1、**Chinese (Simplified) Language Pack for Visual Studio Code**

描述：此中文（简体）语言包为 VS Code 提供本地化界面。（安装后需要重启）

 2、**Bracket Pair Colorizer**

 花括号扩展，可以帮助我们清晰的看出代码层次

 3、**Code Runner**

描述：可以帮助我们运行多种语言的代码段或代码文件，C, C++, Java, JavaScript, PHP, Python, Perl, Perl 6, Ruby, Go, Lua, Groovy等等，很有用

4.更多好用的插件正在发现中。。。

#### 常用快捷键
官方快捷键手册：https://go.microsoft.com/fwlink/?linkid=832145

```bash

移动到行首： Home 

移动到行尾： End 

移动到文件结尾： Ctrl+End 

移动到文件开头： Ctrl+Home 

移动到定义处： F12 

定义处缩略图：只看一眼而不跳转过去 Alt+F12 

移动到后半个括号： Ctrl+Shift+] 

选择从光标到行尾： Shift+End 

选择从行首到光标处： Shift+Home 

删除光标右侧的所有字： Ctrl+Delete 

扩展/缩小选取范围： Shift+Alt+Left 和 Shift+Alt+Right 

多行编辑(列编辑)：Alt+Shift+鼠标左键，Ctrl+Alt+Down/Up 

同时选中所有匹配： Ctrl+Shift+L 

回退上一个光标操作： Ctrl+U 

找到所有的引用： Shift+F12 

同时修改本文件中所有匹配的： Ctrl+F12
```
