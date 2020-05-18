# C 程序结构






------

C 程序主要包括以下部分：

 - 预处理器指令
 - 函数
 - 变量
 - 语句 & 表达式
 - 注释

```c
#include <stdio.h>
   /* 我的第一个 C 程序 */ 
int main()
{
   printf("Hello, World! \n");
   return 0;
}
```
1、程序的第一行 #include <stdio.h> 是预处理器指令，告诉 C 编译器在实际编译之前要包含 stdio.h 文件。
2、 /*...*/ 将会被编译器忽略，这里放置程序的注释内容。它们被称为程序的注释。
3、 int main() 是主函数，程序从这里开始执行。
4、 printf(...) 是 C 中另一个可用的函数，会在屏幕上显示消息 "Hello, World!"。

5、 return 0; 终止 main() 函数，并返回值 0。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


