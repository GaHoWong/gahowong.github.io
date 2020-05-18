# C 数组






------

C 语言支持数组数据结构，它可以存储一个固定大小的相同类型元素的顺序集合。数组是用来存储一系列数据，但它往往被认为是一系列相同类型的变量。

数组的声明并不是声明一个个单独的变量，比如 number0、number1、...、number99，而是声明一个数组变量，比如 numbers，然后使用 numbers[0]、numbers[1]、...、numbers[99] 来代表一个个单独的变量。数组中的特定元素可以通过索引访问。

所有的数组都是由连续的内存位置组成。最低的地址对应第一个元素，最高的地址对应最后一个元素。

```c
#include "stdio.h"
main()
{
	int a[6]={1,2,3};	
	printf("栈区-地址    a[0]：%p\n", &a[0]);
	printf("栈区-地址    a[1]：%p\n", &a[1]);
	printf("栈区-地址    a[2]：%p\n", &a[2]);
}
```
编译运行输出结果：

```bash
栈区-地址    a[0]：000000000062FDF0
栈区-地址    a[1]：000000000062FDF4
栈区-地址    a[2]：000000000062FDF8
```

### 声明数组
使用一个数组之前必须要进行声明。

```c
type arrayName [ arraySize ];
```
这叫做一维数组。**arraySize 必须是一个大于零的整数常量**，type 可以是任意有效的 C 数据类型。

比如

```c
int laohuang[10]
```
现在laohuang是一个可用的数组，可以容纳 10 个类型为 int 的数字。
### 初始化数组
```c
int laohuang[5] = {1,2,3,4,5};
```

 1. 大括号 { } 之间的值的数目不能大于我们在数组声明时在方括号 [ ] 中指定的元素数目。
 2. 如果省略掉[]里面的数字，则这个数组的大小为初始化时元素的大小。但如果强行加上去一个元素，这数组的大小还是初始时元素的大小。
```c
#include "stdio.h"
main()
{
	
	int a[]={1};
	int i =1;
	int b[3]={1,2};
	printf("Asize1 = %d,Bsize1 = %d\n",sizeof(a),sizeof(b));
	a[1] = 4;//给数组a添加一个元素 
	printf("%d,size2 = %d",b[2],sizeof(a));
}

```
```bash
Asize1 = 4,Bsize1 = 12
0,size2 = 4
```
**这样做会导致数组越界！** 

 3. 如果一个数组[]定义了大小，初始化时并未全部初始化，但在内存空间中已经占用了初始时[]设定的大小，当赋值的元素少于数组总体元素的时候，不同类型剩余的元素自动初始化值说明如下：
对于 short、int、long，就是整数 0；
对于 char，就是字符 '\0'；
对于 float、double，就是小数 0.0。


 4. 数组初始化技巧: 将元素全部置零 {0}。比如`double arr[10] = {0};`那么arr的所有元素都是0.000000了

```c
#include "stdio.h"
int main(void){
	double a[5]={0};
	int i = 0;
	for(i=0;i<5;i++){
		printf("%lf\n",a[i]);
	}
} 
```
输出结果：
```bash
0.000000
0.000000
0.000000
0.000000
0.000000

```






### 访问数组元素
数组元素可以通过数组名称加索引进行访问。元素的索引是放在方括号内，跟在数组名称的后边。例如：

```bash
double salary = balance[9];
```
上面的语句将把数组中第 10 个元素的值赋给 salary 变量。下面的实例使用了上述的三个概念，即，声明数组、数组赋值、访问数组：

```c
#include <stdio.h>
 
int main ()
{
   int n[ 10 ]; /* n 是一个包含 10 个整数的数组 */
   int i,j;
 
   /* 初始化数组元素 */         
   for ( i = 0; i < 10; i++ )
   {
      n[ i ] = i + 100; /* 设置元素 i 为 i + 100 */
   }
   
   /* 输出数组中每个元素的值 */
   for (j = 0; j < 10; j++ )
   {
      printf("Element[%d] = %d\n", j, n[j] );
   }
 
   return 0;
}
```
编译运行结果：
```bash
Element[0] = 100
Element[1] = 101
Element[2] = 102
Element[3] = 103
Element[4] = 104
Element[5] = 105
Element[6] = 106
Element[7] = 107
Element[8] = 108
Element[9] = 109
```
### 判断数组大小

在我们没有明确数组的元素个数时，在程序中想知道数组单元个数可以使用 sizeof(a)/sizeof(a[0]), sizeof(a) 是得到数组 a 的大小，sizeof(a[0]) 是得到数组 a 中单个元素的大小（因此可以不必要是a[0],a[i]都行）

```c
#include<stdio.h>

int main(int argc,char *grgv[])
{
    int a[]={1,2,3,4,5};
    int b;
    b=sizeof(a)/sizeof(a[0]);
    printf("数组元素个数为：%d",b);
    return 0; 
}
```

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


