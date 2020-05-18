# C 基本语法






------

### C 的令牌（Tokens）
C 程序由各种令牌组成，令牌可以是关键字、标识符、常量、字符串值，或者是一个符号。例如，下面的 C 语句包括五个令牌：

```c
printf("Hello, World! \n");
```
这五个令牌分别是：

```c
printf
(
"Hello, World! \n"
)
;
```
#### 分号 ;
C 语言有两种注释方式：单行注释（//）和多行注释（/* */）

```c
//这是单行注释
/*这是
多行
注释*/
```
> 不能在注释内嵌套注释，注释也不能出现在字符串或字符值中。


#### 标识符
C 标识符是用来标识变量、函数，或任何其他用户自定义项目的名称。一个标识符以字母 A-Z 或 a-z 或下划线 _ 开始，后跟零个或多个字母、下划线和数字（0-9）。

C 标识符内不允许出现标点字符，比如 @、$ 和 %。C 是区分大小写的编程语言。因此，在 C 中，OldHuang 和 oldhuang 是两个不同的标识符。

### 关键字
下表列出了 C 中的保留字。这些保留字不能作为常量名、变量名或其他标识符名称。

```c
关键字	说明
auto	声明自动变量
break	跳出当前循环
case	开关语句分支
char	声明字符型变量或函数返回值类型
const	定义常量，如果一个变量被 const 修饰，那么它的值就不能再被改变
continue结束当前循环，开始下一轮循环
default	开关语句中的"其它"分支
do	    循环语句的循环体
double	声明双精度浮点型变量或函数返回值类型
else	条件语句否定分支（与 if 连用）
enum	声明枚举类型
extern	声明变量或函数是在其它文件或本文件的其他位置定义
float	声明浮点型变量或函数返回值类型
for 	一种循环语句
goto	无条件跳转语句
if	    条件语句
int 	声明整型变量或函数
long	声明长整型变量或函数返回值类型
register声明寄存器变量
return	子程序返回语句（可以带参数，也可不带参数）
short	声明短整型变量或函数
signed	声明有符号类型变量或函数
sizeof	计算数据类型或变量长度（即所占字节数）
static	声明静态变量
struct	声明结构体类型
switch	用于开关语句
typedef	用以给数据类型取别名
unsigned声明无符号类型变量或函数
union	声明共用体类型
void	声明函数无返回值或无参数，声明无类型指针
volatile说明变量在程序执行中可被隐含地改变
while	循环语句的循环条件
```
#### C 中的空格
只包含空格的行，被称为空白行，可能带有注释，C 编译器会完全忽略它。

空格分隔语句的各个部分，让编译器能识别语句中的某个元素（比如 int）在哪里结束，下一个元素在哪里开始。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


