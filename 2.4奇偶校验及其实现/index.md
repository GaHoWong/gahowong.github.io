# 2.4奇偶校验及其实现


### 什么是奇偶校验？

奇偶校验是一种常见的简单校验。根据被传输的一组二进制代码的数位中“1”的个数是奇数或偶数来进行校验。采用奇数的称为奇校验，反之，称为偶校验。采用何种校验是事先规定好的。通常专门设置一个奇偶校验位，用它使这组代码中“1”的个数为奇数或偶数。若用奇校验，则当接收端收到这组代码时，校验“1”的个数是否为奇数，从而确定传输代码的正确性。
### 奇偶校验的优缺点
1、编码与检错简单
2、编码效率高
3、不能检测偶数位错误, 无错结论不可靠，是一种错误检测码
4、不能定位错误，因此不具备纠错能力
### 奇偶校验的码距
根据上一节我们得知，奇偶校验的码距为2，因为奇偶校验对于偶数位的错误是检测不出来的，所以他的最小码距是2，举例个例子说明11000011 和**0**100001**0**。因此，奇偶校验可以检测出一位错误，无纠错能力。
### 练习
1假设下列字符中有奇偶校验，但没有发生错误，其中采用的是奇校验的是 （     ）（单选）
A.11011001
B.11010111
C.11010100
D.11110110
正确答案：A，因为字符码第一位是校验位，由于D的真值有四个1，所以校验位写成1凑成奇数，这也叫奇校验。
2下列关于奇偶校验的描述中，正确的是 （  ）  （多选）
A.奇校验和偶校验的码距都为1
B.编码时使用的校验位位数与被校验数据的长度无关
C.校验时得到的无错结论不可信
D.校验时得到的有错结论不可信
正确答案：B、C
3设奇偶校验编码总长度大于3位，下列关于基本奇偶校验检错与纠错能力的描述，正确的是 （   ） （多选）
A.可以检测1位错误
B.可以检测2位错误
C.可以检测3位错误
D.不能纠正错误
正确答案：A、C、D，奇偶校验可检测奇数个错误，无纠错能力。

------

[^undefined]:

[参考资料](http://www.icourse163.org/learn/HUST-1003159001?tid=1206076221#/learn/announce)



