# 利用异或 ^ 来交换两个数的值，而且不引入其他变量的方法






------

通常我们交换一个数字，需要引入第三个变量来储存才能进行交换，但利用异或运算可以不需要第三个变量作为中间量。

```c
#include "stdio.h"
int a = 1;
int b = 2; 
int t;
int main(viod){
	printf("a=%d,b=%d\n",a,b);
	t = a;
	a = b;
	b = t;
	printf("a=%d,b=%d\n",a,b);
}
```
结果输出：
```
a=1,b=2
a=2,b=1
```
如果用异或来进行交换：

```
unsigned int a=60;  //0011 1100
unsigned int b=13;  //0000 1101
a=a^b;              //a=a^b=0011 0001
b=a^b;              //b=a^b=0011 1100  相当于b1=(a^b)^b
a=a^b;              //a=a^b=0000 1101  相当于a1=(a^b)^((a^b)^b)
```

```c
#include<stdio.h>

int main( )
{
    unsigned int a=60;         //0011 1100
    unsigned int b=13;         //0000 1101
    printf("a=%d,b=%d",a,b);   //输出a，b的值
    printf("\n");
    a=a^b;                     //a=a^b=0011 0001
    b=a^b;                     //b=a^b=0011 1100
    a=a^b;                     //a=a^b=0000 1101
    printf("a=%d,b=%d",a,b);   //输出a，b的值
}
```
结果输出：
```
a=60,b=13
a=13,b=60
```

> 整型负数也可交换

**注意：位运算只支持整数运算，不支持浮点型运算。因此上述的方法用在浮点型运算编译不会通过。**

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


