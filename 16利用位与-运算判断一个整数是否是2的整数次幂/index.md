# 利用位与 & 运算，判断一个整数是否是2的整数次幂






------

二进制数的位权是以2为底的幂，如果一个整数 m 是 2 的 n 次幂，那么转换为二进制之后只有最高位为 1，其余位置为 0，再观察 m-1 转换为二进制后的形式以及 m&(m-1) 的结果，例如：

```c
2 --> 0000 0010        1 --> 0000 0001        2&1 --> 0000 0010 & 0000 0001 = 0
4 --> 0000 0100        3 --> 0000 0011        4&3 --> 0000 0100 & 0000 0011 = 0
8 --> 0000 1000        7 --> 0000 0111        8&7 --> 0000 1000 & 0000 0111 = 0
```
可以看出所有的 1 完美的错过了，根据位与的特点可知 m&(m-1) 的结果为 0。

如果整数 m 不是 2 的 n 次幂，结果会怎样呢？例如 m=9 时：

```c
9 --> 0000 1001        8 --> 0000 1000        9&8 --> 0000 1001 & 0000 1000 != 0
```
利用这一特点，即可判断一个整数是否是2的整数次幂。

```c
int func(int num)
{
    return ((num > 0) && ((num & (num - 1)) == 0));//2的n次幂大于0
}
```

返回值为 1，则输入的正整数为 2 的整数次幂，返回值为 0 则不是。
#### 对于 2 的幂指数的详细程序
```c
#include <stdio.h>

int num;
int func(int num)
{
    if ((num>0)&&(num&(num-1))==0)
    {
        printf("%d是2的整数次幂",num);
    }
    else
    {
        printf("%d不是2的整数次幂",num);
    }
    return((num>0)&&(num&(num-1))==0);
}

int main()
{
    printf("请输入要查询的数\n");
    scanf("%d",&num);
    func(num);
}
```
> **注意：c语言不能对浮点型进行位运算。 位运算 只能 用于 整型。**

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


