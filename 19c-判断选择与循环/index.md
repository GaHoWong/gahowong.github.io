# C 判断、选择与循环






------

# 判断结构
### if else
#### 通常情况：
C 语言把任何非零和非空的值假定为 true，把零或 null 假定为 false。

```c
if(判断条件){
    语句块1
}else{
    语句块2
}
```
由于if else 语句可以根据不同的情况执行不同的代码，所以也叫分支结构或选择结构。

 - 在 if 语句中，判断条件必须用括号括起来。
 - 有时候，可以只使用if，而不一定使用else。
 - if(判断条件)后不能加；分号，初学者最容易错的就是这里。
 - if（判断条件）后的第一条语句可不加{}，但建议养成好习惯，加上{}，即使只有一条语句。
 - 语句块由{ }包围，但要注意的是在}之后不需要再加分号;（当然加上也没错）。

#### 多个if else语句

```c
if(判断条件1){
    语句块1
} else  if(判断条件2){
    语句块2
}else  if(判断条件3){
    语句块3
}else  if(判断条件m){
    语句块m
}else{
     语句块n
}
```
意思是，从上到下依次检测判断条件，当某个判断条件成立时，则执行其对应的语句块，然后跳到整个 if else 语句之外继续执行其他代码。如果所有判断条件都不成立，则执行语句块n，然后继续执行后续代码。

**也就是说，一旦遇到能够成立的判断条件，则不再执行其他的语句块，所以最终只能有一个语句块被执行。**
#### if else嵌套时
C语言规定，else 总是与它前面最近的 if 配对

### ? : 运算符(三元运算符)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200503205511403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
`表达式1 ? 表达式2 : 表达式3`

条件运算符是C语言中唯一的一个三目运算符，其求值规则为：如果表达式1的值为真，则以表达式2 的值作为整个条件表达式的值，否则以表达式3的值作为整个条件表达式的值。条件表达式通常用于赋值语句之中。


```c
#include<stdio.h>
 
int main()
{
    int num;
 
    printf("输入一个数字 : ");
    scanf("%d",&num);
 
    (num%2==0)?printf("偶数"):printf("奇数");
}
```
# 选择
### switch case
C语言虽然没有限制 if else 能够处理的分支数量，但当分支过多时，用 if else 处理会不太方便，而且容易出现 if else 配对出错的情况。例如，输入一个整数，输出该整数对应的星期几的英文表示：

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    switch(a){
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;
        case 4: printf("Thursday\n"); break;
        case 5: printf("Friday\n"); break;
        case 6: printf("Saturday\n"); break;
        case 7: printf("Sunday\n"); break;
        default:printf("error\n"); break;
    }
    return 0;
}
```

case可以理解为一个站点，程序选择了这个站点，从这个站点执行，**如果没有break，程序会一直执行下去，直到遇见break**。例如

```c
 switch(1){
        case 1: printf("1\n"); 
        case 2: printf("2\n"); 
        case 3: printf("3\n"); break;
        case 4: printf("Thursday\n"); break;
```

```
1
2
3
```
**default**由于 `default` 是最后一个分支，匹配后不会再执行其他分支，所以也可以不添加`break;`语句
**default** 不是必须的。当没有 `default` 时，如果所有 `case` 都匹配失败，那么就什么都不执行。

**break** 是C语言中的一个关键字，专门用于跳出 `switch` 语句。所谓“跳出”，是指一旦遇到 `break`，就不再执行 `switch` 中的任何语句，包括当前分支中的语句和其他分支中的语句；也就是说，整个 `switch` 执行结束了，接着会执行整个 `switch` 后面的代码。

# 循环
### while 循环
当给定条件为真时（非0），重复语句或语句组。它会在执行循环主体之前测试条件。

```c
while(表达式){
	语句块;
}
```
{}后面可不加；，加了也没所谓。
#### 关于while死循环
while （1）循环会一直执行下去，永不结束，成为“死循环”。

```bash
#include <stdio.h>
int main(){
    while(1){
        printf("1");
    }
    return 0;
}
```
死循环可以用break；跳出。
### do-while循环

```c
do{
    语句块;
}while(表达式);
```
与while相同，但do while是先做一次do里面的程序，然后再判断。
### for循环
多次执行一个语句序列，简化管理循环变量的代码。

```c
for ( init; condition; increment )
{
   statement(s);
}
```

 1. init 会首先被执行，且只会执行一次。这一步允许您声明并初始化任何循环控制变量。您也可以不在这里写任何语句，只要有一个分号出现即可。
 2. 接下来，会判断 condition。如果为真，则执行循环主体。如果为假，则不执行循环主体，且控制流会跳转到紧接着 for 循环的下一条语句。
 3. 在执行完 for 循环主体后，控制流会跳回上面的 increment 语句。该语句允许您更新循环控制变量。该语句可以留空，只要在条件后有一个分号出现即可。
 4. 条件再次被判断。如果为真，则执行循环，这个过程会不断重复（循环主体，然后增加步值，再然后重新判断条件）。在条件变为假时，for 循环终止。


```c
#include "stdio.h"
c99写法，编译器可能会报错：
[Error] 'for' loop initial declarations are only allowed in C99 or C11 mode
int main(void){
	for(int i = 0;i<100;i++)
	{
		printf("%d",i);
	}
}
输出0-99
//另一种写法
int main(void){
	int i;
	i = 0; 
	for(;i<100;i++)//i=0可写（）括号里面，i++可写{}大括号里面，效果一样
	{
		printf("%d",i);
	}
输出0-99


int main(void){
	int i;
	i = 0; 
	for(;i<100;)
	{
		i++;
		printf("%d",i);
	}
}
输出1-100

所以这就是第2，3点的步骤，要记清。
```
#### 关于for死循环
如果条件永远不为假，则循环将变成无限循环。for 循环在传统意义上可用于实现无限循环。由于构成循环的三个表达式中任何一个都不是必需的，您可以将某些条件表达式留空来构成一个无限循环。

```c
#include <stdio.h>
 
int main ()
{
   for( ; ; )
   {
      printf("该循环会永远执行下去！\n");
   }
   return 0;
}
```
当条件表达式不存在时，它被假设为真。您也可以设置一个初始值和增量表达式，但是一般情况下。

```
可以按 Ctrl + C 键终止一个无限循环。
```
### beak和continue
#### beak
C 语言中 break 语句有以下两种用法：

 1. 当 break 语句出现在一个循环内时，循环会立即终止，且程序流将继续执行紧接着循环的下一条语句。
 2. 它可用于终止 switch 语句中的一个 case
如果您使用的是嵌套循环（即一个循环内嵌套另一个循环），break 语句会停止执行最内层的循环，然后开始执行该块之后的下一行代码。

#### continue
C 语言中的 continue 语句有点像 break 语句。**但它不是强制终止，continue 会跳过当前循环中的代码，强迫开始下一次循环。**

**对于 for 循环，continue 语句执行后自增语句仍然会执行**。对于 while 和 do...while 循环，continue 语句重新执行条件判断语句。

#### beak 和 continue的区别
beak是终止本次循环，循环不再继续。continue是跳出当前循环，进入下一次循环，循环还在继续。

### goto 
C 语言中的 goto 语句允许把控制无条件转移到同一函数内的被标记的语句。
在任何编程语言中，都不建议使用 goto 语句。因为它使得程序的控制流难以跟踪，使程序难以理解和难以修改。任何使用 goto 语句的程序可以改写成不需要使用 goto 语句的写法。
老师虽然说这是个万能的语句，它可以做任何事，事实也是如此，但不建议在程序上使用。哈哈哈。

```c
goto label;
..
.
label: statement;
```
label 可以是任何除 C 关键字以外的纯文本，它可以设置在 C 程序中 goto 语句的前面或者后面。
它可以往后跳，也可以往前跳。反正就是，最好不要跳，不然腿被打断了都不知道怎么回事。
# 总结
C语言中常用的编程结构有三种（其它编程语言也是如此），它们分别是：

 1. **顺序结构**：代码从前往后依次执行，没有任何“拐弯抹角”，不跳过任何一条语句，所有的语句都会被执行到。
 2. **选择结构**：也叫分支结构。代码会被分成多个部分，程序会根据特定条件（某个表达式的运算结果）来判断到底执行哪一部分。
 3. **循环结构**：程序会重新执行同一段代码，直到条件不再满足，或者遇到强行跳出语句（break 关键字）。
### 选择结构
选择结构（分支结构）涉及到的关键字包括 if、else、switch、case、break，还有一个条件运算符? :（这是C语言中唯一的一个三目运算符）。其中，if...else 是最基本的结构，switch...case 和? :都是由 if...else 演化而来，它们都是为了让程序员书写更加方便。

你可以只使用 if，也可以 if...else 配对使用。另外要善于使用 switch...case 和? :，有时候它们看起来更加清爽。

if...else 可以嵌套使用，原则上嵌套的层次（深度）没有限制，但是过多的嵌套层次会让代码结构混乱。
### 循环结构
C语言中常用的循环结构有 while 循环和 for 循环，它们都可以用来处理同一个问题，一般可以互相代替。

除了 while 和 for，C语言中还有一个 goto 语句，它也能构成循环结构。不过由于 goto 语句很容易造成代码混乱，维护和阅读困难，饱受诟病，不被推荐，而且 goto 循环完全可以被其他循环取代，所以后来的很多编程语言都取消了 goto 语句，我们也不再讲解。

> 国内很多大学仍然讲解 goto 语句，但也仅仅是完成教材所设定的课程，goto 语句在实际开发中很难见到。
> 对于 while 和 do-while 循环，循环体中应包括使循环趋于结束的语句。

对于 while 和 do-while 循环，循环变量的初始化操作应该在 while 和 do-while 语句之前完成，而 for 循环可以在内部实现循环变量的初始化。

for 循环是最常用的循环，它的功能强大，一般都可以代替其他循环。

最后还要注意 break 和 continue 关键字用于循环结构时的区别：

break 用来跳出所有循环，循环语句不再有执行的机会；

continue 用来结束本次循环，直接跳到下一次循环，如果循环条件成立，还会继续循环。

此外，break 关键字还可以用于跳出 switch...case 语句。所谓“跳出”，是指一旦遇到 break，就不再执行 switch 中的任何语句，包括当前分支中的语句和其他分支中的语句；也就是说，整个 switch 执行结束了，接着会执行整个 switch 后面的代码。

此外，如果循环次数确定，优先使用for循环，不确定用while循环。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


