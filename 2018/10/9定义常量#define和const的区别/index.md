# 定义常量#define和const的区别






------

define是预编译指令，在预编译的时候仅仅进行字符替换，预编译后符号常量就不存在了，例如define PI 3.1415926 ，编译以后就不存在PI了，所有的PI都被换成了3.1415926。而且PI没有存储单元。而常变量const变量要占用存储单元，有变量值，只是值不能改，它有符号变量的优点，而且比较方便。


#define 是宏定义，它不能定义常量，但宏定义可以实现在字面意义上和其它定义常量相同的功能，本质的区别就在于 #define 不为宏名分配内存
### 两者的区别
#### (1) 编译器处理方式不同
#define 宏是在预处理阶段展开。

 const 常量是编译运行阶段使用。
#### (2) 类型和安全检查不同
 #define 宏没有类型，不做任何类型检查，仅仅是展开，仅仅是简单的字符串替换。

 const 常量有具体的类型，在编译阶段会执行类型检查。


> 简单的字符串替换会导致边界效应

#### (3) 存储方式不同
#define宏仅仅是展开，有多少地方使用，就展开多少次，不会分配内存。（宏定义不分配内存，变量定义分配内存。）

const常量会在内存中分配(可以是堆中也可以是栈中)。

define预处理后占用代码空间，而const占用数据段空间。 
#### (4) const 可以节省空间，避免不必要的内存分配。 例如：

```c
#define NUM 3.14159 //常量宏
const doulbe Num = 3.14159; //此时并未将Pi放入ROM中 ......
double i = Num; //此时为Pi分配内存，以后不再分配！
double I= NUM; //编译期间进行宏替换，分配内存
double j = Num; //没有内存分配
double J = NUM; //再进行宏替换，又一次分配内存！
```

const 定义常量从汇编的角度来看，只是给出了对应的内存地址，而不是象 #define 一样给出的是立即数，所以，const 定义的常量在程序运行过程中只有一份拷贝（因为是全局的只读变量，存在静态区），而 #define 定义的常量在内存中有若干个拷贝。
#### (5) const提高了效率。
 编译器通常不为普通const常量分配存储空间，而是将它们保存在符号表中，这使得它成为一个编译期间的常量，没有了存储与读内存的操作，使得它的效率也很高。
#### (6)  宏替换只作替换不做计算
 宏替换只作替换，不做计算，不做表达式求解;宏预编译时就替换了，程序运行时，并不分配内存。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


