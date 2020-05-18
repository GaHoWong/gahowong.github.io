# a++ 与 ++a 的区别






------

 - **a++ 是先返回百a的值，再执行++运算。**
 - **++a 是先执行++运算，在返回a的值。**

```c
#include <stdio.h>

int main(int argc, char **argv) {
    int a=100;
    printf("a = %d\n",a);
    printf("a++ = %d\n",a++);
    printf("a = %d\n",a);
    printf("++a = %d\n",++a);
    printf("a = %d\n",a++);
    return 0;
}
```

```
a = 100
a++ = 100
a = 101
++a = 102
a = 102
```
++a和a++等价的结果一样，但是运算过程不同，a++ 是先使用a的值，然后再对a做加1处理，++a是先对a作加1处理，然后再使用a的值。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


