# 关于函数参数传递常用的三种方式






------

# 值传递

```c
void Exchg1(int x, int y)
{
   int tmp;
   tmp = x;
   x = y;
   y = tmp;
   printf("x = %d, y = %d\n", x, y);
}
main()
{
   int a = 4,b = 6;
   Exchg1(a, b);
   printf("a = %d, b = %d\n", a, b);
   return(0);
}
```
输出结果：

```bash
x = 6，y = 4
a = 4，b = 6
```
函数在调用时是隐含地把实参a、b 的值分别赋值给了x、y，之后在你写的Exchg1函数体内再也没有对a、b进行任何的操作了。交换的只是x、y变量。并不是a、b。因此a、b的值没有改变，函数只是把a、b的值通过赋值传递给了x、y，函数里头操作的只是x、y的值并不是a、b的值。
# 指针传递

```c
void Exchg2(int *px, int *py)
{
   int tmp = *px;
   *px = *py;
   *py = tmp;
   printf("*px = %d, *py = %d.\n", *px, *py);
}
main()
{
   int a = 4;
   int b = 6;
   Exchg2(&a, &b);
   printf("a = %d, b = %d.\n", a, b);
   return(0);
}
```
输出结果：

```bash
*px = 6, *py = 4
a = 6, b = 4
```
指针px、py的值已经分别是a、b变量的地址值了。接下来，对px、py的操作当然也就是对a、b变量本身的操作了。所以函数里头的交换就是对a、b值的交换了，这就是所谓的地址传递（传递a、b的地址给了px、py）
# 引用传递
```c
void Exchg3(int &x, int &y)
{
   int tmp = x;
   x = y;
   y = tmp;
   printf("x = %d,y = %d\n", x, y);
}
main()
{
   int a = 4;
   int b = 6;
   Exchg3(a, b);
   printf("a = %d, b = %d\n", a, b);
   return(0);
}

```
输出结果：

```bash
x = 6, y = 4.
a = 6, b = 4
```
&在c语言中是取地址符号，调用Exchg3时函数会将a、b 分别代替了x、y，我们称：x、y分别引用了a、b变量。这样函数里头操作的其实就是实参a、b本身，也就是说函数里是可以直接修改到a、b的值。

> 注：严格来说，C语言中是没有引用传递，这是C++中语言特性，因此在.c文件中使用引用传递会导致程序编译出错。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


