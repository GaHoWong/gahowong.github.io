# 2.5CRC校验及其实现


循环冗余校验是一种基于模2运算建立编码规则的校验码。对数据进行多项式计算，并将得到的结果附在帧的后面，接收设备也执行类似的算法，以保证数据传输的正确性和完整性。CRC在磁存储器和计算机通信方面应用较多。 
### 1.模2运算

#### 1)模2加减运算
模2加减运算就是按位加减运算，即不带进位和借位的二进制加法和减法运算。模2加与模2减运算的结果相同。
运算规则如下：0±0 =  0，0±1 =  1，1±0 =  1，1士1 =  0
#### 2)模2乘运算
模2乘运算即按模2加求部分积之和，无进位。例2. 2 1按模2乘法运算法则求1101与101之积。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405210209911.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### 3 )模2除运算
模2除运算即按模2减求部分余数，不借位。其上商原则是：(1)部分余数首位为1时，商为1，减除数。(2)部分余数首位为0时，商为0，减0。( 3 )当部分余数的位数小于除数的位数时，该余数为最后余数.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405210322112.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

### CRC基本原理
#### （1）生成多项式G(x)
收发双方约定的一个(r + 1)位二进制数，发送方利用G(X)对信息多项式做模2除运算,生成校验码。接收方利用G(X)对收到的编码多项式做模2除运算检测差错及错误定位。
**G(x)应满足的条件：**
A、最高位和最低位必须为1；
B、当被传送信息（CRC码）任何一位发生错误时，被生成多项式做除后应该使余数不为0；
C、不同位发生错误时，模2除运算后余数不同；
D、对不为0余数继续进行模2除运算应使余数循环。
**常见生成多项式G(x)**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405210641586.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

#### （2）CRC 编码方法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405210913563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040521092347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
#### （3）CRC的检错与纠错
假如无错则余数为0，假如出错则余数不为0，纠错需要查看表
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405211130603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405211206448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200405211304683.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)
### CRC软件实现

```c
******该文件使用查表法计算CCITT 标准的CRC-16检验码，并附测试代码********/
#include
#define CRC_INIT 0xffff//CCITT初始CRC为全1
#define GOOD_CRC 0xf0b8//校验时计算出的固定结果值
/****下表是常用ccitt16,生成式1021反转成8408后的查询表格****/
unsigned short crc16_ccitt_table[256] =
{      
0x0000, 0x1189, 0x2312, 0x329b, 0x4624, 0x57ad, 0x6536, 0x74bf,0x8c48, 0x9dc1, 0xaf5a, 0xbed3, 0xca6c, 0xdbe5, 0xe97e,0xf8f7,0x1081, 0x0108, 0x3393, 0x221a, 0x56a5, 0x472c, 0x75b7, 0x643e,0x9cc9, 0x8d40, 0xbfdb, 0xae52, 0xdaed, 0xcb64, 0xf9ff, 0xe876,0x2102, 0x308b, 0x0210,0x1399, 0x6726, 0x76af, 0x4434, 0x55bd,0xad4a, 0xbcc3, 0x8e58, 0x9fd1, 0xeb6e, 0xfae7, 0xc87c, 0xd9f5,0x3183, 0x200a, 0x1291, 0x0318, 0x77a7, 0x662e, 0x54b5,0x453c,0xbdcb, 0xac42, 0x9ed9, 0x8f50, 0xfbef, 0xea66, 0xd8fd, 0xc974,0x4204, 0x538d, 0x6116, 0x709f, 0x0420, 0x15a9, 0x2732, 0x36bb,0xce4c, 0xdfc5, 0xed5e,0xfcd7, 0x8868, 0x99e1, 0xab7a, 0xbaf3,0x5285, 0x430c, 0x7197, 0x601e, 0x14a1, 0x0528, 0x37b3, 0x263a,0xdecd, 0xcf44, 0xfddf, 0xec56, 0x98e9, 0x8960, 0xbbfb,0xaa72,0x6306, 0x728f, 0x4014, 0x519d, 0x2522, 0x34ab, 0x0630, 0x17b9,0xef4e, 0xfec7, 0xcc5c, 0xddd5, 0xa96a, 0xb8e3, 0x8a78, 0x9bf1,0x7387, 0x620e, 0x5095,0x411c, 0x35a3, 0x242a, 0x16b1, 0x0738,0xffcf, 0xee46, 0xdcdd, 0xcd54, 0xb9eb, 0xa862, 0x9af9, 0x8b70,0x8408, 0x9581, 0xa71a, 0xb693, 0xc22c, 0xd3a5, 0xe13e,0xf0b7,0x0840, 0x19c9, 0x2b52, 0x3adb, 0x4e64, 0x5fed, 0x6d76, 0x7cff,0x9489, 0x8500, 0xb79b, 0xa612, 0xd2ad, 0xc324, 0xf1bf, 0xe036,0x18c1, 0x0948, 0x3bd3,0x2a5a, 0x5ee5, 0x4f6c, 0x7df7, 0x6c7e,0xa50a, 0xb483, 0x8618, 0x9791, 0xe32e, 0xf2a7, 0xc03c, 0xd1b5,0x2942, 0x38cb, 0x0a50, 0x1bd9, 0x6f66, 0x7eef, 0x4c74,0x5dfd,0xb58b, 0xa402, 0x9699, 0x8710, 0xf3af, 0xe226, 0xd0bd, 0xc134,0x39c3, 0x284a, 0x1ad1, 0x0b58, 0x7fe7, 0x6e6e, 0x5cf5, 0x4d7c,0xc60c, 0xd785, 0xe51e,0xf497, 0x8028, 0x91a1, 0xa33a, 0xb2b3,0x4a44, 0x5bcd, 0x6956, 0x78df, 0x0c60, 0x1de9, 0x2f72, 0x3efb,0xd68d, 0xc704, 0xf59f, 0xe416, 0x90a9, 0x8120, 0xb3bb,0xa232,0x5ac5, 0x4b4c, 0x79d7, 0x685e, 0x1ce1, 0x0d68, 0x3ff3, 0x2e7a,0xe70e, 0xf687, 0xc41c, 0xd595, 0xa12a, 0xb0a3, 0x8238, 0x93b1,0x6b46, 0x7acf, 0x4854,0x59dd, 0x2d62, 0x3ceb, 0x0e70, 0x1ff9,0xf78f, 0xe606, 0xd49d, 0xc514, 0xb1ab, 0xa022, 0x92b9, 0x8330,0x7bc7, 0x6a4e, 0x58d5, 0x495c, 0x3de3, 0x2c6a, 0x1ef1,0x0f78 };
unsigned short do_crc(unsigned short reg_init, unsigned char *message, unsigned intlen)
{
	unsigned short crc_reg= reg_init;
	while (len--)
	crc_reg= (crc_reg>> 8)^ crc16_ccitt_table[(crc_reg^*message++) & 0xff];
	return crc_reg; 
}

```
### CRC的应用
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020040521191766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L09sZEh1YW5nQw==,size_16,color_FFFFFF,t_70)

------

[^undefined]:

[参考资料](http://www.icourse163.org/learn/HUST-1003159001?tid=1206076221#/learn/announce)



