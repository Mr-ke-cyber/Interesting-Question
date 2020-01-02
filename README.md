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

>详细解释：https://muyiy.cn/question/js/58.html

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

>详细解释：https://www.cnblogs.com/wenqiangit/p/9850553.html
----

2020-01-01
# 4.全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？

在ES5中，顶层对象的属性和全局变量是等价的，var 命令和 function 命令声明的全局变量，自然也是顶层对象。
但ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
通过在浏览器中设置断点，可以观察到：
用 let 和 const 声明的全局变量并没有在全局对象中，只是一个块级作用域（Script）中
怎么获取？在定义变量的块级作用域中就能获取啊，既然不属于顶层对象，那就不加 window（global）呗。

>详细解释：https://muyiy.cn/question/js/27.html
----

2020-01-02
# 5. for---in 循环出来的对象属性顺序是怎么样的？
比如 
```javascript
   let codes = {
    "49": "a",
    age: 30,
    "41": "b",
    name: "c",
    isAdmin: true,
    "1": "e",
    $2:'dd'
   };
  for (let key in codes) {
      console.log(key);
  }
  //最终遍历出来的结果是：
  1
  41
  49
  age
  name
  isAdmin
  $2
````
> 使用for-in会遍历数组所有的可枚举属性，包括原型。例如上例的原型方法method和name属性都会被遍历出来，通常需要配合hasOwnProperty()方法判断某个属性是否该对象的实例属性，来将原型对象从循环中剔除。
> 至于遍历顺序，根据在webstorm及chrome浏览器上的测试结果，先遍历出整数(此处的整数包括字符串形式的整数，比如"41""49"这种能转换为数字整型的)属性（integer properties，按照升序），然后其他字符串属性按照创建时候的顺序遍历出来，
如果有特殊字符，则在最后面。
其实这道题考查的知识点特别细，感觉没多大必要，因为我们很少在 for...in 中做依赖对象属性顺序的逻辑判断的,但是在实际面试过程中是真实遇到过的。如果想要依赖对象属性做逻辑判断，可以将属性按照你想要的顺序放进数组里，然后再依次遍历。

























