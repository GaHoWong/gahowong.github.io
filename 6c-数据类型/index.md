# C语言模板






------

在 C 语言中，数据类型指的是用于声明不同类型的变量或函数的一个广泛的系统。变量的类型决定了变量存储占用的空间，以及如何解释存储的位模式。

C 中的类型可分为以下几种:

**1   基本类型：**

它们是算术类型，包括两种类型：整数类型和浮点类型。

**2	枚举类型：**

它们也是算术类型，被用来定义在程序中只能赋予其一定的离散整数值的变量。

**3	void 类型：**

类型说明符 void 表明没有可用的值。

**4	派生类型：**

它们包括：指针类型、数组类型、结构类型、共用体类型和函数类型。

> 数组类型和结构类型统称为聚合类型。函数的类型指的是函数返回值的类型。
### 整数类型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501233340868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

> 编译器可以根据自身硬件来选择百合适的大小，但是需要满足约束：short和int型至少为16位，long型至少为32位，并且度short型长度不能超过int型，而int型不能超过long型。这即是说各个类型的变量长度是由编译器来决定的，而当前主流的编译器中一般是32位机器知和64位机器中int型都是4个字节。
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501233802383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

为了得到某个类型或某个变量在特定平台上的准确大小，您可以使用 sizeof 运算符。表达式 sizeof(type) 得到对象或类型的存储字节大小。下面的实例演示了获取 int 类型的大小：

```c
#include <stdio.h>
#include <limits.h>
 
int main()
{
   printf("int 存储大小 : %lu \n", sizeof(int));
   
   return 0;
}

>>>
int 存储大小 : 4 
```
### 浮点类型
下表列出了关于标准浮点类型的存储大小、值范围和精度的细节：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200501233942398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### void 类型
void 类型指定没有可用的值。它通常用于以下三种情况下：
**函数返回为空**

C 中有各种函数都不返回值，或者您可以说它们返回空。不返回值的函数的返回类型为空。例如 void exit (int status);
**函数参数为空**

C 中有各种函数不接受任何参数。不带参数的函数可以接受一个 void。例如 int rand(void);
**指针指向 void**

类型为 void * 的指针代表对象的地址，而不是类型。例如，内存分配函数 void *malloc( size_t size ); 返回指向 void 的指针，可以转换为任何数据类型。

### 其它：
#### 常用基本数据类型占用空间（64位机器为例）
 char ： 1个字节

 int ：4个字节

 float：4个字节

 double：8个字节

#### 基本类型书写

 - 默认为10进制 ，10 ，20
 - 以0开头为8进制，045，021
 - 以0b开头为2进制，0b11101101
 - 以0x开头为16进制，0x21458adf

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


