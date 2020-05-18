# #define引起的“边缘效应”






------

### 定义
边缘效应是指在两个或两个以上不同性质的生态系统交互作用处，由于某些生态因子(物质、能量、信息、时机或地域)或系统属性的差异和协合作用而引起系统某些组分及行为的较大变化。
这个定义晦涩难懂！

### 为何#define会引起边缘效应？

```c
#define N 2+3
```
这时 N 的值是 5。

```c
#include <stdio.h>
#define N 2+3
//正确写法 #define N (2+3)
int main(){   
    double a ;
    a = (float)N/(float)2;
    printf("a 的值为 : %.2f", a);     
    return 0;
}
```
在编译时我们预想 `a=2.5`，实际打印结果是 `3.5` 原因是在预处理阶段，编译器将 `a=N/2` 处理成 `a=2+3/2`，这就是 define 宏的边缘效应。
### 如何解决？
所以我们应该写成 `#define N (2+3)`。所以要用两个个括号括起来，就不怕边界效应了。

利用define进行宏定义的时候，需要格外注意边缘效，有时一不小心就会造成严重的后果。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


