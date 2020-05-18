# | 和 ||，& 和 && 的区别






------

我们将 `||` 和 `&&` 定义为逻辑运算符，而 `|` 和 `&` 定义为位运算符。

`&&` 如果两个操作数都非零，则条件为真；

`||` 如果两个操作数中有任意一个非零，则条件为真。

`&` 按位与操作，按二进制位进行"与"运算。

`|` 按位或运算符，按二进制位进行"或"运算

#### 那么，问题来了，在判断语句中，用 | 还是 ||，& 还是 &&？
判断语句中为布尔类型，值只有 true 和 false（如果变量值为 0 就是 false，否则为 true）

```c
举个例子，a=1 b=2
所以 a>0 这个值为true    b>1 这个值为true     b>2 这个值为 false
如 if(a>0&b>1)  我们可以得出 if(true&true)，条件成立（true不为0，所以true&true不为0）
如 if(a>0&&b>1)  我们可以得出 if(true&&true),条件成立（&&两边操作数都非零，所以条件成立）
如 if(b>2&a>0) 我们可以得出 if(false&true)，条件不成立（false为0，false&true为0，条件不成立）
如 if(b>2&&a>0) 我们可以得出 if(false&&a>0)，条件不成立（&&左侧为false，&&运算到此结束，条件不成立）
```
可以看出 `&` 和 `&&` 在判断语句中都可以实现“和”这个功能，不过区别在于 `&` 两边都运算，而 `&&` 先算 `&&` 左侧，若左侧为 false 那么右侧就不运算了。因此从效率上来说，判断语句中推荐使用 `&&`（换句话就是逻辑运算就老老实实用逻辑运算符，不然它为啥叫逻辑运算符呢？）

而 | 和 || 的比较与上类似，不做赘述。


总结：从效率上来说，判断语句中推荐使用 `&&    ||`逻辑运算。

------

**参考资料** 



[程序设计入门--C翁恺](http://www.icourse163.org/learn/ZJU-199001?tid=1450247457#/learn/announce)

[《*c语言*程序设计》--谭浩强](https://baike.baidu.com/item/c%E8%AF%AD%E8%A8%80%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/19471979?fr=aladdin)

[《C Primer Plus》--Stephen Prata](https://baike.baidu.com/item/c%20primer%20plus/4851344?fr=aladdin)

[C语言中文网](http://c.biancheng.net/)

[C语言教程|菜鸟教程](https://www.runoob.com/cprogramming/c-tutorial.html)


