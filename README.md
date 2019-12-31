# Interesting-Question
每天弄懂一道遇到的有意思的题目，督促自己进步！加油🆙🆙🆙
----
2019-12-29

# 1.箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

>1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

>2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

>3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

>4、不可以使用 new 命令，因为：

>没有自己的 this，无法调用 call，apply。没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__

----
2019-12-30

# 2.你知道null和undefined有什么区别么？

大多数计算机语言，有且仅有一个表示"无"的值，比如，C语言的NULL，Java语言的null，Python语言的None，Ruby语言的nil。

有点奇怪的是，JavaScript语言居然有两个表示"无"的值：undefined和null。在JavaScript中，将一个变量赋值为undefined或null，老实说，几乎没区别。

根据C语言的传统，null被设计成可以自动转为0。

但是，JavaScript的设计者Brendan Eich，觉得这样做还不够，有两个原因。

首先，null像在Java里一样，被当成一个对象。但是，JavaScript的数据类型分成原始类型（primitive）和合成类型（complex）两大类，Brendan Eich觉得表示"无"的值最好不是对象。

其次，JavaScript的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich觉得，如果null自动转为0，很不容易发现错误。

因此，Brendan Eich又设计了一个undefined。

>详细解释：http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html

----
2019-12-31
# 3.Object.is()和===有什么相同点和不同点？

Object.is()是es6新增的用来比较两个值是否严格相等的方法，与===的行为基本一致。但是也有不同点：
* +0 不等于-0
* NaN 等于自身

关于==，如果两个值类型不同，那么他们可能相等。通常根据下面规则进行类型转换，然后再进行比较：
1.如果一个是null, 一个是undefined，那么相等；
2.如果一个是字符串，一个是数值，那么把字符串转换成数值再进行比较。
3.如果任一值是true，把它转换成 1 再比较；如果任一值是false，把它转换成 0 再比较。
4.如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。
5.JS的核心内置类，会尝试valueOf先于toString；但有一个是例外——Date，Date利用的是toString转换。



























