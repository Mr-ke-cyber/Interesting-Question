----
2019-12-29

# 1.箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

>1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

>2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

>3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

>4、不可以使用 new 命令，因为：

>没有自己的 this，无法调用 call，apply。没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__

>更详细解释，可参考：https://muyiy.cn/question/js/58.html

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

>更详细解释，可参考：http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html

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

>更详细解释，可参考：https://www.cnblogs.com/wenqiangit/p/9850553.html
----

2020-01-01
# 4.全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？

在ES5中，顶层对象的属性和全局变量是等价的，var 命令和 function 命令声明的全局变量，自然也是顶层对象。
但ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
通过在浏览器中设置断点，可以观察到：
用 let 和 const 声明的全局变量并没有在全局对象中，只是一个块级作用域（Script）中
怎么获取？在定义变量的块级作用域中就能获取啊，既然不属于顶层对象，那就不加 window（global）呗。

>更详细解释，可参考：https://muyiy.cn/question/js/27.html
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

> 其实这道题考查的知识点特别细，个人感觉没多大必要，因为我们很少在 for...in 中做依赖对象属性顺序的逻辑判断的,但是在实际面试过程中是真实遇到过的。如果想要依赖对象属性做逻辑判断，可以将属性按照你想要的顺序放进数组里，然后再依次遍历。

> 更详细解释，可参考：https://blog.csdn.net/JEFF_luyiduan/article/details/100532371

----
2020-01-03  
# 6. indexOf 和 includes的区别在哪？
这道面试题相信大多数人都能回答出来，但是大多数人应该回答的不完整。
> indexOf返回的是查找元素的位置，即number型的，而includes返回的是布尔值，判断是否包含想查询的元素。
> 这两个方法不传入参数的时候，默认其传入的参数为undefined,都可以支持第二个参数，而且第二个参数都支持负数形式。第二个参数是可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是0到stringObject.length - 1。如果省略该参数，则将从字符串的首字符开始检索。
> indexOf不能判断NaN,判断稀疏数组结果不同
```` javascript
var array = [NaN];
console.log(array.indexOf(NaN) //-1
console.log(array.includes(NaN) //true

var array2 = [,,,];
console.log(array2.indexOf(undefined) // -1
console.log(array2.includes(undefined) // true
````
> 更详细解释，可参考：https://blog.csdn.net/wu_xianqiang/article/details/78681609
----
2020-01-04
# 7.下面代码会输出什么？
````javascript
[1, 2, 3].map(n => { number: n })
//[undefined, undefined, undefined]
````
> 此题考查的是箭头函数什么时候不需要return就可以有返回值，属于es6里面比较细的知识点。我们知道，箭头函数可以有一个“简写体”或常见的“块体”。在一个简写体中，只需要一个表达式，并附加一个隐式的返回值，而在块体中，必须使用明确的return语句。
> 在箭头函数中用简单的语法返回对象字面量这种方式是行不通的，因为花括号（{}）里面的代码被解析为一系列语句，因此，如果想要实现简写，可以用圆括号把对象字面量给包起来。
例如：

````
let result = [1, 2, 3].map(n => ({number:n}));  //[{number:1}, {number:2}, {number:3}]
也可以这样：
let result = [1, 2, 3].map(n => (function(){return n})());  //[1, 2, 3]
````
> 更详细解释，可参考：https://blog.csdn.net/github_39319000/article/details/89472033
----
2020-01-05
# 8.分别写出如下代码的返回值
```` javascript
String('11') == new String('11'); //true
String('11') === new String('11'); // false
````
> 当String() 和运算符 new 一起作为构造函数使用时，它返回一个新创建的String对象，存放的是字符串s或s的字符串表示。
> 当不用new运算符调用String()时，它只把s转换成原始的字符串，并返回转换后的值。``
所以String()返回的是字符串，new String()返回的是对象。  
> 更详细解释，可参考：https://muyiy.cn/question/js/106.html
----
2020-01-06
 # 9.对Promise掌握有多少？可通过八段代码了解透彻。  
 > 主要总结：  
 ①Promise是立即执行的，立即执行性。  
 ②Promise具备三种状态，pending/resolved/rejected。  
 ③Promise的状态时不可逆的，即当promise的状态一旦变成了resolved或reject时，Promise的状态和值就固定下来了，不论你后续再怎么调用resolve或者reject方法，都不能改变它的状态和值。  
 ④Promise对象的then方法返回一个新的Promise对象，因此可以通过链式调用then方法。then方法接收两个函数作为参数，第一个参数是Promise执行成功时的回调，第二个参数是Promise执行失败时的回调。两个函数只会有一个被调用，函数的返回值将被用作创建then返回的Promise对象。  
 ⑤Promise接收的函数参数是同步执行的，但是then方法中的回调函数执行则是异步的。  
 ⑥Promise中的异常由then参数中第二个回调函数（Promise执行失败的回调）处理，异常信息将作为Promise的值。异常一旦得到处理，then返回的后续Promise对象将恢复正常，并会被Promise执行成功的回调函数处理。  
 ⑦Promise.resolve(---)可以接收一个值或者是一个Promise对象作为参数。当参数是普通值时，它返回一个resolved状态的Promise对象，对象的值就是这个参数。当参数是一个Promise对象时，它直接返回这个Promise参数。  
 ⑧Promise回调函数中的第一个参数resolve，会对Promise执行“拆箱”操作。当resolve的参数是一个Promise对象时，resolve会“拆箱”获取这个Promise对象的状态和值，但这个过程是异步的。
 但是在Promise回调函数中，第二个参数reject不具备“拆箱”的能力，reject的参数会直接传递给then方法中的rejected回调，可以理解为这个过程是同步立即执行的。    
 注意：  
 Promise.resolve('成功')等同于 new Promise(function(resolve){resolve('成功')）})    
 Promise.reject('出错了')等同于 new Promise((resolve, reject) => reject('出错了')  
 > 更详细解释，可参考：https://juejin.im/post/597724c26fb9a06bb75260e8 并逐段代码练习。
 ----
 2020-01-07
 # 10. Promise.all和Promise.race的区别和相同点？
 race方法与all方法类似，都可以将多个Promise实例包装成一个新的Promise实例
 不同点：在使用all方法时大Promise的状态由多个小Promise共同决定，而race方法则由第一个转变状态的小Promise的状态决定，第一个若是成功态，则转成功态，第一个若是失败态，则转失败态。
 来看下面两个例子：
 ````javascript
 let p1 = new Promise((resolve, reject) => {
     resolve('成功了')
 });
 
 let p2 = new Promise((resolve, reject) => {
     resolve('success')
 });
 
 let p3 = Promise.reject('失败');
 
 Promise.all([p1, p2]).then((result) => {
     console.log(result)               //['成功了', 'success']
 }).catch((error) => {
     console.log(error)
 });
 
 Promise.all([p1,p3,p2]).then((result) => {
     console.log(result)   
 }).catch((error) => {
     console.log(error)  //失败
 });
 
 Promise.race([p1,p3,p2]).then((result) => {
     console.log(result)   // 成功了
 }).catch((error) => {
     console.log(error)
 });
````
> 更详细解释，可参考：https://juejin.im/post/5ab20c58f265da23a228fe0f#heading-2
----
2020-01-08
# 11.什么是暂时性死区？
（今天非常之忙，需要开会及评审需求，还有个需求周五要提测，故以温故为主）  
暂时性死区是指在使用let声明变量之前，该变量都是不可用的。在语法上，成为“暂时性死区”。（temporal dead zone）
只要块级作用域存在let命令，它所声明的变量就“绑定”这个区域，不再受外部的影响。这么说可能有些抽象，举个例子：
````javascript
var temp = 123;
if(true) {
    console.log(temp);
    let temp;
}

结果：
> ReferenceError: temp is not defined
````
> ES6规定暂时性死区和let、const语句不出现变量提升，主要是为了减少运行时错误，防止在变量声明前就使用这个变量，从而导致意料之外的行为。

> 更详细解释，可参考：https://segmentfault.com/a/1190000018113011
----
2020-01-09
# 12.深拷贝和浅拷贝的区别在哪？什么样的深拷贝才算合格？能不能手写一个深拷贝出来？
这道题可以说非常常见，但常见并不一定代表吃透了。  
先来回答区别，最根本区别在于是否真正获取了一个对象的复制实体，而不是引用。我们知道，对象是引用数据类型，引用数据类型的值实际存储在堆内存中，它在栈中只存储了一个固定长度的地址，这个地址指向堆内存中的值，浅拷贝只是拷贝了它的地址。 如果原地址存储的对象发生改变，那么所有浅拷贝出来的对象会跟着改变。  
深拷贝则是将一个对象从内存中完整的拷贝一份出来，从堆内存中开辟一个新的区域来存放新对象，且修改新对象不会影响原对象。    
合格的深拷贝，应该考虑的多个方面，主要包括考虑数组、循环引用、克隆函数、性能优化等方面综合考虑。  
手写一个深拷贝如下：
````javascript
function clone(target, map = new WeakMap()) {
    if (typeof target === 'object') {
        let cloneTarget = Array.isArray(target) ? [] : {};
        if (map.get(target)) {
            return map.get(target);
        }
        map.set(target, cloneTarget);
        for (const key in target) {
            cloneTarget[key] = clone(target[key], map);
        }
        return cloneTarget;
    } else {
        return target;
    }
}
````
> 更详细解释，可参考：https://juejin.im/post/5d6aa4f96fb9a06b112ad5b1#heading-3  
----
2020-01-10
# 13. ES6中的Set、Map、WeakSet和WeakMap的相同点与不同点都有哪些？
在昨天学习的深拷贝中我们用到了WeakMap这一数据结构，对es6的新数据结构掌握的不是很熟练透彻，平常应用的不多，今天特地深入学习一下。
> Set和Map主要的应用场景在于数据**数据重组**和**数据储存**，Set是一种叫做**集合**的数据结构，Map是一种叫做**字典**的数据结构。
## 1.集合
ES6 新增的一种新的数据结构，类似于数组，但成员是唯一且无序的，没有重复的值。  
## 2.WeakSet
WeakSet 对象允许你将弱引用对象储存在一个集合中  
WeakSet 与 Set 的区别：
- WeakSet 只能储存对象引用，不能存放值，而Set对象都可以
- WeakSet 对象中储存的对象值都是被弱引用的，即垃圾回收机制不考虑 WeakSet 对该对象的应用，如果没有其他的变量或属性引用这个对象值，则这个对象将会被垃圾回收掉（不考虑该对象还存在于 WeakSet 中），所以，WeakSet 对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了（被垃圾回收了），WeakSet 对象是无法被遍历的（ES6 规定 WeakSet 不可遍历），也没有办法拿到它包含的所有元素。
## 3.字典（Map）
> 集合 与 字典 的区别：1.共同点：集合、字典可以储存不重复的值； 2.不同点：集合是以[value, value]的形式储存元素，字典是以[key,value]的形式储存。
## 4. WeakMap
WeakMap 对象是一组键值对的集合，其中的**键是弱引用对象，而值可以是任意**。
注意：**WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。**
WeakMap 中，每个键对自己所引用对象的引用都是弱引用，在没有其他引用和该键引用同一对象，这个对象将会被垃圾回收（相应的key则变成无效的），所以，WeakMap 的 key 是不可枚举的。






















