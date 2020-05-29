# 如何用VSCode配置一个超舒服的C/C++编程环境？




#### 一、安装

1、首先去[官网](https://code.visualstudio.com/)（https://code.visualstudio.com/）下载最新版的VSCode，下载好后双击安装→选择安装路径→安装。安装到这里时，推荐把所有选项都√勾上。
 ![所有都勾上哦](https://img-blog.csdnimg.cn/20200529100032218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70) 
 #### 二、配置编译环境

2、下载编译器mingw（[官网](http://www.mingw-w64.org/doku.php)|[天翼云盘](https://cloud.189.cn/t/RneYRvIzMbQf )https://cloud.189.cn/t/RneYRvIzMbQf (访问码:z9vo)）

 （1）下载后解压到：D:\Program Files

（2）然后复制D:\Program Files\mingw64\bin，这个目录

（3）右键“此电脑”→选择“属性”→在左侧选择“高级系统设置”→选择“高级”→选择“环境变量”→在用户那栏里双击“Path”→选择“新建”将刚刚复制bin文件夹的路径粘贴上去，点确定、
 ![](https://img-blog.csdnimg.cn/20200529103120768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

（4）测试是否成功，win+R键调出运行，输入cmd回车，然后输入`gcc -v`，弹出这样的提示，说明成功。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200529103421927.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

 #### 三、安装插件
 安装以下几个插件，对C/C++的编程具有极大的帮助。

 安装方法：打开VSCode后，打开插件商城，快捷键（`Ctrl + Shift + X`）打开插件商城，搜索插件，点击安装即可。

 1、**Chinese (Simplified) Language Pack for Visual Studio Code**

 描述：此中文（简体）语言包为 VS Code 提供本地化界面。（安装后需要重启）

 2、**Bracket Pair Colorizer**

 花括号扩展，可以帮助我们清晰的看出代码层次

 3、**Code Runner**

描述：可以帮助我们运行多种语言的代码段或代码文件，C, C++, Java, JavaScript, PHP, Python, Perl, Perl 6,Ruby, Go, Lua, Groovy等等，很有用

4、**C/C++**

描述：添加C/C++语法提示、调试等功能。

5、Atom One Dark Theme

描述：这是一个暗色的主题

6、**tabout**

描述：tabout插件可以使得按一下tab键直接从括号或者引号中跳出，不再需要去按方向键或者end键。喜欢tab代码补全的慎用！

7、**翻译(英汉词典)**

描述：有时候注释是英文的看不懂没关系。这个插件

8、**GitLens — Git supercharged**

描述：GitLens增强了Visual Studio Code中内置的Git功能。 它可以帮助您通过Git责备注释和代码镜头一目了然地看到代码作者的身份，无缝地导航和浏览Git存储库，通过强大的比较命令获得有价值的见解，等等。

 9、**settings sync**

描述：使用GitHub Gist在不同的机器之间同步设置，摘要，主题，图标，启动，键绑定，工作区和扩展。
(要科学上网，暂时不弄)

#### 四、C/C++配置
无论是Linux还是Windows，用户配置都放在.vscode下。这里说明一下用户配置和全局配置。用户配置是针对某一个工程或者文件夹而特别做的。所有配置文件都放在该文件夹下的.vscode隐藏文件夹中。

1、新建一个C文件夹，目录为E:\C\test

2、复制[配置](https://cloud.189.cn/t/amUna2iEzEBv )（访问码:6l5l）目录下的.vscode和del.bat到E:\C\test

3、新建一个.c文件编写代码，运行测试

```c
#include "stdio.h"
int main(void){
    int a=7;
    a+=6;
    printf("%d",a);
    printf("hello word");
}
```
4、按F5运行调试

5、del.bat运行一下，会把.c文件夹下的exe文件删除

**注意：VSCode需要为每一个文件夹做单独配置，所以建议把.vscode文件放在父级目录下，这些配置可以此父级文件夹内的所有子文件夹和文件都能使用。 这样就不用重复配置了。**

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
