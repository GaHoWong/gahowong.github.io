# 关于内部函数和外部函数






------

根据函数能否被其他源文件调用，将函数区分为内部函数和外部函数。
# 内部函数
如果一个函数只能被本文件中其他函数所调用，它称为内部函数。在定义内部函数时，在函数名和函数类型的前面加 static，即

```c
static 类型名 函数名 （形参表）
```
例如，函数的首行：

```c
static int max(int a,int b)
```
内部函数又称静态函数。使用内部函数，可以使函数的作用域只局限于所在文件。即使在不同的文件中有同名的内部函数，也互不干扰。提高了程序的可靠性。
# 外部函数
如果在定义函数时，在函数的首部的最左端加关键字 extern，则此函数是外部函数，可供其它文件调用。

如函数首部可以为

```c
extern int max (int a,int b)
```
C 语言规定，如果在定义函数时省略 extern，则默认为外部函数。

在需要调用此函数的其他文件中，需要对此函数作声明（不要忘记，即使在本文件中调用一个函数，也要用函数原型来声明）。在对此函数作声明时，要加关键字 extern，表示该函数是在其他文件中定义的外部函数。

实例
以下实例通过多个文件的函数实现输入一串字符串，然后删除指定的字符：

**file1.c(文件1)**

```c
#include <stdio.h>

static void delete_string(char str[],char ch);
int main()
{
    extern void enter(char str[]); // 对函数的声明
    extern void print(char str[]); // 对函数的声明
    char c,str[100];
    enter(str);
    scanf("%c",&c);
    delete_string(str,c);
    print(str);
    return 0;
}

static void delete_string(char str[],char ch)//内部函数
{
    int i,j;
    for(i=j=0;str[i]!='\0';i++)
    if(str[i]!=ch)
    str[j++]=str[i];
    str[j]='\0';
}
```
**file2.c(文件2)**

```c
#include <stdio.h>

void enter(char str[100]) // 定义外部函数 enter
{
    fgets(str, 100, stdin); // 向字符数组输入字符串
}
```
**file3.c(文件3)**

```c
#include <stdio.h>

void print(char str[]) // 定义外部函数 print
{
    printf("%s\n",str);
}
```
输入字符串"abcdef"，给字符数组 str，在输入要删去的字符'd'。 运行结果:

```bash
$ gcc file1.c file2.c file3.c 
$ ./a.out
abcdef                   # 输入的字符串
d                        # 要删除的字符
abcef                    # 删除后的字符串
```

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


