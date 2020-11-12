----
2019-12-29

# 1.箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异：

>1、函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。

>2、不可以使用 arguments 对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

>3、不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。

>4、不可以使用 new 命令，因为：

>没有自己的 this，它只会从自己的作用域链的上一层继承this, 任何方法都改变不了其指向，无法调用 call，apply，bind。没有 prototype 属性 ，而 new 命令在执行时需要将构造函数的 prototype 赋值给新的对象的 __proto__

>参考：https://muyiy.cn/question/js/58.html

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

>参考：http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html

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

>参考：https://www.cnblogs.com/wenqiangit/p/9850553.html
----

2020-01-01
# 4.全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？

在ES5中，顶层对象的属性和全局变量是等价的，var 命令和 function 命令声明的全局变量，自然也是顶层对象。
但ES6规定，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性，但 let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。
通过在浏览器中设置断点，可以观察到：
用 let 和 const 声明的全局变量并没有在全局对象中，只是一个块级作用域（Script）中
怎么获取？在定义变量的块级作用域中就能获取啊，既然不属于顶层对象，那就不加 window（global）呗。

>参考：https://muyiy.cn/question/js/27.html
https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/30
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

> 参考：https://blog.csdn.net/JEFF_luyiduan/article/details/100532371

----
2020-01-03  
# 6. indexOf 和 includes的区别在哪？
这道面试题相信大多数人都能回答出来，但是大多数人应该回答的不完整。
> indexOf返回的是查找元素的位置，即number型的，而includes返回的是布尔值，判断是否包含想查询的元素。
> 这两个方法不传入参数的时候，默认其传入的参数为undefined,都可以支持第二个参数，而且第二个参数都支持负数形式。第二个参数是可选的整数参数。规定在字符串中开始检索的位置。它的合法取值是0到stringObject.length - 1。如果省略该参数，则将从字符串的首字符开始检索。
> indexOf不能判断NaN,判断稀疏数组结果不同  
> 这两者均能判断undefined null  “ ”（空字符串）
**注意** 这两个方法都对大小写敏感
```` javascript
var array = [NaN];
console.log(array.indexOf(NaN) //-1
console.log(array.includes(NaN) //true

var array2 = [,,,];
console.log(array2.indexOf(undefined) // -1
console.log(array2.includes(undefined) // true
````
> 参考：https://blog.csdn.net/wu_xianqiang/article/details/78681609
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
> 参考：https://blog.csdn.net/github_39319000/article/details/89472033
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
> 参考：https://muyiy.cn/question/js/106.html
----
2020-01-06
 # 9.对Promise掌握有多少？可通过八段代码了解透彻。  
 Promise是异步编程的一种解决方案：从语法上讲，promise是一个对象，从它可以获取异步操作的消息；从本意上讲，它是承诺，承诺它过一段时间会给你一个结果。promise有三种状态，pending(等待态)，fulfiled(成功态)，rejected(失败态)；
 状态一旦改变，就不会再变。创造promise实例后，它会立即执行。
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
 > 参考：https://juejin.im/post/597724c26fb9a06bb75260e8 并逐段代码练习。
 ----
 2020-01-07
 # 10. Promise.all和Promise.race的区别和相同点？Promise.finally又是啥？
 race方法与all方法类似，都可以将多个Promise实例包装成一个新的Promise实例
 不同点：在使用all方法时大Promise的状态由多个小Promise共同决定，所有的小promise都成功了，则为成功态。若有一个promise失败，则为失败态。而race方法则由第一个转变状态的小Promise的状态决定，第一个若是成功态，则转成功态，第一个若是失败态，则转失败态。
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
Promise.finally方法用于指定不管Promise对象最后状态如何，都会执行的操作，使用方法如下：
````javascript
Promise
	.then(result => { ··· })
	.catch(error => { ··· })
	.finally(() => { ··· })
````
finally特点：
* 不接受任何参数。
* finally本质上是then方法的特例。
````javascript
Promise.prototype.finally = function (callback) {
  let P = this.constructor
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  )
}

````
> 参考：https://juejin.im/post/5ab20c58f265da23a228fe0f#heading-2
----
2020-01-08
# 11.什么是暂时性死区？
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

> 参考：https://segmentfault.com/a/1190000018113011
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
> 参考：https://juejin.im/post/5d6aa4f96fb9a06b112ad5b1#heading-3  
----
2020-01-10
# 13. ES6中的Set、Map、WeakSet和WeakMap的相同点与不同点都有哪些？
在昨天学习的深拷贝中我们用到了WeakMap这一数据结构，对es6的新数据结构掌握的不是很熟练透彻，平常应用的不多，今天特地深入学习一下。
> Set和Map主要的应用场景在于数据**数据重组**和**数据储存**，Set是一种叫做**集合**的数据结构，Map是一种叫做**字典**的数据结构。
## 1.集合
> ES6 新增的一种新的数据结构，类似于数组，但成员是唯一且无序的，没有重复的值。  
## 2.WeakSet
> WeakSet 对象允许你将弱引用对象储存在一个集合中  
WeakSet 与 Set 的区别：
- WeakSet 只能储存对象引用，不能存放值，而Set对象都可以
- WeakSet 对象中储存的对象值都是被弱引用的，即垃圾回收机制不考虑 WeakSet 对该对象的应用，如果没有其他的变量或属性引用这个对象值，则这个对象将会被垃圾回收掉（不考虑该对象还存在于 WeakSet 中），所以，WeakSet 对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了（被垃圾回收了），WeakSet 对象是无法被遍历的（ES6 规定 WeakSet 不可遍历），也没有办法拿到它包含的所有元素。
## 3.字典（Map）
> 集合 与 字典 的区别： 
* 1.共同点：集合、字典可以储存不重复的值； 
* 2.不同点：集合是以[value, value]的形式储存元素，字典是以[key,value]的形式储存。
## 4. WeakMap
> WeakMap 对象是一组键值对的集合，其中的**键是弱引用对象，而值可以是任意**。

注意：**WeakMap 弱引用的只是键名，而不是键值。键值依然是正常引用。**  
WeakMap 中，每个键对自己所引用对象的引用都是弱引用，在没有其他引用和该键引用同一对象，这个对象将会被垃圾回收（相应的key则变成无效的），所以，WeakMap 的 key 是不可枚举的。
> 参考：https://blog.csdn.net/duyujian706709149/article/details/96310651   
----
2020-01-11
# 14. JavaScript中的垃圾回收和内存泄漏了解多少？
* 在昨天的学习中有提到垃圾回收，正好复习一下这块的知识。
在Js中会自动进行内存的分配和回收，因为这样，很多开发者都以为我们是不需要去关心内存方面的问题，这显然是错误的看法。关注内存的管理，避免内存泄露也是性能优化的重要方面。
## 变量的生命周期
对于全局变量，生命周期会持续到页面关闭。而对于局部变量，在所在的函数的代码执行之后，局部变量的生命周期结束，他所占用的内存会通过垃圾回收机制释放（垃圾回收）。
## 垃圾回收机制
垃圾回收机制分为两种：
* **引用计数**
这种算法在MDN文档中被称为最"天真"的垃圾回收算法;核心原理是: 判断一个对象是否要被回收就是要看是否还有引用指向它,如果是"零引用",那么就回收.说这种算法天真,是因为它存在着较为严重的缺陷---循环引用.
IE中有一部分对象并不是原生JavaScript对象，例如，其BOM和DOM中的对象就是使用C++ 以COM对象的形式实现的。而COM对象的垃圾收集机制采用的就是引用计数策略。因此，即使IE的JavaScript引擎是使用标记清除
策略来实现的，但JavaScript访问的COM对象依然是基于引用计数策略的。换句话来说，只要在IE中涉及COM对象，就会存在循环引用的问题。
* **标记清除**
最常用的方式就是标记清除，具体作用机制是，当变量进入环境时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流
进入相应的环境，就可能会用到它们。而当变量离开环境的时候，则将其标记为“离开环境”。  
可以使用任何方式来标记变量，到目前为止，几乎所有现代浏览器都使用了标记清除的这种垃圾回收策略。
> https://juejin.im/post/5cb33660e51d456e811d2687
《JavaScript高级程序设计》第三版 4.3节垃圾收集  
----
2020-01-12
# 15. JSON.stringify()了解多少？
> 感觉这个方法在大多数人眼里最常用的就是JSON.stringify(对象)，和JSON.parse搭配使用，主要用于将对象序列化，平淡无奇，好像没什么好深究的，其实大错特错，我们一起来看下这个方法的九大特性。
* 第一大特性：undefined、任意的函数以及 symbol 作为对象属性值时 JSON.stringify() 对跳过（忽略）它们进行序列化；undefined、任意的函数以及 symbol 作为数组元素值时，JSON.stringify() 将会将它们序列化为 null；
undefined、任意的函数以及 symbol 被 JSON.stringify() 作为单独的值进行序列化时，都会返回 undefined；  
* 第二大特性：非数组对象的属性不能保证以特定的顺序出现在序列化后的字符串中。  
* 第三大特性：转换值如果有 toJSON() 函数，该函数返回什么值，序列化结果就是什么值，并且忽略其他属性的值。  
* 第四大特性：JSON.stringify() 将会正常序列化 Date 的值，实际上 Date 对象自己部署了 toJSON() 方法（同Date.toISOString()），因此 Date 对象会被当做字符串处理。
* 第五大特性：NaN 和 Infinity 格式的数值及 null 都会被当做 null。
* 第六大特性：基本类型的序列化比如（布尔值、数字、字符串）他们的包装对象在序列化过程中会自动转换成相应的原始值。  
* 第七大特性：对于其他类型的对象，包括 Map/Set/WeakMap/WeakSet，仅会序列化可枚举的属性。  
* 第八大特性：对包含循环引用的对象（对象之间相互引用，形成无限循环）执行此方法，会抛出错误。这也就是为什么用序列化去实现深拷贝时，遇到循环引用的对象会抛出错误的原因。  
* 第九大特性： 最后，关于 symbol 属性还有一点要说的就是：所有以 symbol 为属性键的属性都会被完全忽略掉，即便 replacer 参数中强制指定包含了它们。
## 强大的第二个参数 replacer
replacer 参数有两种形式，可以是一个函数或者一个数组。  
### 作为函数时
> 它有两个参数，键（key）和值（value），函数类似就是数组方法 map、filter 等方法的回调函数，对每一个属性值都会执行一次该函数。如果 replacer 是一个数组，数组的值代表将被序列化成 JSON 字符串的属性名。
第二个参数replacer 非常强大， replacer 作为函数时，我们可以打破九大特性的大多数特性，可以从代码中直观的感受到：
```` javascript
const data = {
  a: "aaa",
  b: undefined,
  c: Symbol("dd"),
  fn: function() {
    return true;
  }
};
// 不用 replacer 参数时
JSON.stringify(data); 

// "{"a":"aaa"}"
// 使用 replacer 参数作为函数时
JSON.stringify(data, (key, value) => {
  switch (true) {
    case typeof value === "undefined":
      return "undefined";
    case typeof value === "symbol":
      return value.toString();
    case typeof value === "function":
      return value.toString();
    default:
      break;
  }
  return value;
})
// "{"a":"aaa","b":"undefined","c":"Symbol(dd)","fn":"function() {\n    return true;\n  }"}"
````
### 作为数组时
> 结果非常简单，数组的值就代表了将被序列化成 JSON 字符串的属性名。
````
const jsonObj = {
  name: "JSON.stringify",
  params: "obj,replacer,space"
};

// 只保留 params 属性的值
JSON.stringify(jsonObj, ["params"]);
// "{"params":"obj,replacer,space"}" 
````
> 参考：https://juejin.im/post/5decf09de51d45584d238319
----
2020-01-13
# 16. js中有几种原生数据类型？ES6新增的symbol数据类型了解有多少？
说到原生数据类型，我们通常想到的有这6种：number、string、undefined、null、boolean、object,但在es6中新增了一种原生数据类型Symbol,在谷歌67版本中还出现了一种bigInt类型的数据，它是指安全存储、操作的大整数。
今天来学习一下Symbol.  
* 1.Symbol的作用非常的专一，换句话来说其设计出来的主要目的是，**作为对象属性的唯一标识符**，防止对象属性冲突发生。那么Symbol值通过Symbol函数生成，例如：
````javascript
let s = Symbol();
typeof s // symbol
````
* 2.上面的代码表示创建一个Symbol变量，值得注意的是，**Symbol函数前不能使用new命令**，否则会报错，也就是说Symbol是一个原始数据类型的值，不是对象，也不能添加属性；  
Symbol函数可以接受一个字符串作为参数来表示对Symbol实例的描述，主要用于区分不同的Symbol变量；  
````javascript
let s1 = Symbol('a');
let s2 = Symbol('b');

s1.toString()  // 'Symbol(a)'
s2.toString()  // 'Symbol(b)'
s1 == s2 //false
````
* 3.Symbol函数的参数只是表示对当前Symbol值的描述，即使相同参数的Symbol函数的返回值依然是不相等的。  
* 4.Symbol值不能与其他类型的值进行运算，但是可以转为布尔值，却不能转为数值。  
* 5.Symbol用于对象的属性名时，可以保证不会出现同名的属性，对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或者覆盖；**特别注意**的是，Symbol值作为对象的属性名时，不能用点运算符，因为点运算符后面是一个字符串；  
* 6.Symbol 作为属性名，不会被常规方法遍历得到，即该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回，但是，它并不是私有属性，可以使用 Object.getOwnPropertySymbols 方法，可以获取指定对象的所有 Symbol 属性名；
````javascript
var obj = {};
var a = Symbol('a');
var b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';
obj.c  = 'Mine';

for( let key in obj ){
   console.log(key)         // c
}

var objectSymbols = Object.getOwnPropertySymbols(obj);

console.log(objectSymbols) // [Symbol(a), Symbol(b)]
````
* 7.Symbol.for方法接受一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值。如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值；它与Symbol()不同的是，Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值，而 Symbol()每次都会返回3不同的Symbol值；
````javascript
Symbol.for("name") === Symbol.for("name")
// true

Symbol("name") === Symbol("name")
// false
````
**注意：** Symbol.for为Symbol值登记的名字，是全局环境的，可以在不同的 iframe 或 service worker 中取到同一个值
* 8.Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key，而Symbol()写法是没有登记机制的；
````javascript
var s1 = Symbol.for("name");
Symbol.keyFor(s1) // "name"

var s2 = Symbol("name");
Symbol.keyFor(s2) // undefined
````
> 参考: https://www.jianshu.com/p/4da037782be9
----
2020-01-30
# 17. js中Number.MAX_SAFE_INTEGER和Number.MAX_VALUE有什么区别?这两个参数的作用体现在哪里？
Number.MAX_SAFE_INTEGER 是可以在计算中安全使用的最大整数，由于js用的是IEEE754双精度浮点，可安全地表示[-2^53+1, 2^53-1]这个范围，对应的还有Number.MIN_SAFE_INTEGER。
以上两个常量是ES6引入的，在此之前只能作为事实标准。  
例如， Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2 为true - 任何大于MAX_SAFE_INTEGER的整数都不能始终准确地在内存中表示。所有位用于表示数字的数字。
另一方面，Number.MAX_VALUE 是使用双精度浮点表示表示的最大数字。一般来说，数字越大，准确度越低。  
Number.MAX_VALUE则表示js里最大的数值，比这更大的表示Infinity，与之相应的是Number.MIN_VALUE。这两个是最早的JS标准ECMAScript 262 1st Edition就有的。
> 参考: https://www.zhihu.com/question/36680964/answer/68561445
---
2020-02-27
# 18. 数组的sort方法是作用于原数组（即改变了原数组）还是不改变原数组？内部原理是啥？
sort方法是作用于原数组的，会对原数组排序，默认是将待排序数据转换为字符串，并按照Unicode序列排序；当然，比较函数可以自定义，自定义排序函数需要返回值，其返回值为-1，0，1，分别表示a<b, a=b, a>b.
其内部原理主要为：当数组长度小于等于10的时候，采用插入排序，大于10的时候，采用快排。对于长度大于1000的数组，采用的是快排与插入排序混合的方式进行排序的，因为，当数据量很小的时候，插入排序效率优于快排。
快排的平均时间复杂度是nlogn，在排序算法中属于效率最高的。快排是一种不稳定的排序算法，但是一般情况下稳定或者不稳定对我们没有特别大的影响，但是对稳定性要求高的排序，就不能使用快排了。
> 参考: https://zhuanlan.zhihu.com/p/33626637
---
2020-02-28
# 19. 在js中为什么0.1 + 0.2不等于0.3?
这是因为，在计算机中所有的数据都是以二进制存储的，所以在计算时计算机要把数据先转换成二进制进行计算，然后在把计算结果转换成十进制。
当出现有些小数值转换成二进制后是无限循环的小数情况时，会发生精度丢失，因而再转换成十进制后得到的值与预期的结果不符。
> 参考: https://juejin.im/post/5cec1bcff265da1b8f1aa08f#heading-23
---
2020-02-29
# 20. 谈谈js中的原型和继承？
原型与继承是js中非常核心的内容，不是三言两语就能说清楚的，需要时常翻阅，牢记，揣摩领会。
> 参考: https://github.com/mqyqingfeng/Blog
---
2020-03-01
# 20. 谈谈js中的词法作用域和动态作用域？
作用域是指程序源代码中定义变量的区域。
作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。
JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。
词法作用域，指的是函数的作用域在函数定义的时候就决定了。
而与词法作用域相对的是动态作用域，函数的作用域是在函数调用的时候才决定的。
> 参考: https://github.com/mqyqingfeng/Blog/issues/3
---
2020-03-02
# 21.什么是闭包？闭包有哪些优点？哪些缺点？是如何回收的？
MDN 对闭包的定义为：闭包是指那些能够访问自由变量的函数。
那什么是自由变量呢？
自由变量是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量。
由此，我们可以看出闭包共有两部分组成：
闭包 = 函数 + 函数能够访问的自由变量
通俗来讲，就是根据js词法作用域的规则，内部函数总是可以访问其外部函数中声明的变量，当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，
但是内部函数引用外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包。
优点：1.能将一个变量长期保存在内存中；2.避免全局变量污染；3.私有成员的存在
缺点：1.常驻内存，增加能存使用量；2.使用不当会造成内存泄露；  
回收： 手动解除其引用，使其被垃圾回收机制回收。
> 参考: https://github.com/mqyqingfeng/Blog/issues/3
---
2020-03-08
# 22.js模块化规范了解哪些？
在前端工程化进程中主要产生了四种规范，分别是：CommonJS规范，AMD规范，CMD规范，ES6标准。这其中：
* CommonJS规范主要用于服务端编程，加载模块是同步的，这并不适合在浏览器环境，因为同步意味着阻塞加载，浏览器资源是异步加载的，因此有了AMD CMD解决方案。
* AMD规范在浏览器环境中异步加载模块，而且可以并行加载多个模块。不过，AMD规范开发成本高，代码的阅读和书写比较困难，模块定义方式的语义不顺畅。
* CMD规范与AMD规范很相似，都用于浏览器编程，依赖就近，延迟执行，可以很容易在Node.js中运行。不过，依赖SPM 打包，模块的加载逻辑偏重。
* ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。
> 参考: https://github.com/ljianshu/Blog/issues/48
---
2020-03-09
# 23.CommonJS和ES6模块化有什么区别，设计一个方法，让CommonJS导出的模块也能改变其内部变量？
有两个区别：
* CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
* CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。
来看一个例子:
```javascript
// lib.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
  counter: counter,
  incCounter: incCounter,
};
```
上面代码输出内部变量counter和改写这个变量的内部方法incCounter。
来看执行结果：
```javascript
// main.js
var counter = require('./lib').counter;
var incCounter = require('./lib').incCounter;

console.log(counter);  // 3
incCounter();
console.log(counter); // 3
```
我们的目标是要设计一个方法，使得CommonJS导出的模块也能改变其内部变量。使用函数将输出模块包装返回即可实现。
```javascript
var counter = 3;
var counters = function () {
    return counter;
};

function incCounter() {
    counter++;
    console.log(counter, 'jjj')
}
module.exports = {
    counter: counters,
    incCounter: incCounter,
};
// 执行结果：
// main.js
var counter = require('./test1').counter;
var incCounter = require('./test1').incCounter;

console.log(counter());  // 3
incCounter();
console.log(counter()); // 4
```
> 参考: https://github.com/ljianshu/Blog/issues/48
---
2020-03-11
# 24.能否设计一个通过拖拽，然后交换位置的组件？
主要思路：1，触发拖拽开始事件的时候在datatransfer对象中记录此时被拖拽元素的id,在触发拖拽结束事件时，获取当前的DOM元素，即event.target,获取之前记录的元素document.getElmentById('id')。
2，创建一个交换元素的方法，入参为两个dom元素a, b。在方法内部需要判断这两个入参之间的关系，若相等，则直接返回。获取这两个入参a,b的父节点，若与a同级的元素和b相等，则在a之前插入b，
若与b同级的元素和a相等，则在b之前插入a。
3，如果a元素包含了b，则在a的父元素插入b,b的父元素插入a。
4，如果都不满以上几种情况，则在b的父元素内插入a, a的父元素内插入b，核心api是insertBefore。
> 参考: https://blog.csdn.net/qq_37339364/article/details/89352354
---
2020-03-16
# 25.JSONP和ajax有什么区别，手写一个JSONP(promise版的)？
相同点：都是请求一个url。
不同点：ajax的核心是通过xmlHttpRequest获取内容；jsonp的核心则是动态添加<script>标签来调用服务器 提供的js脚本。
手写JSONP如下：
```javascript
const JSONP = function (url, data) {
    return new Promise((resolve, reject) => {
        let flag = url.indexOf("?") > 0 ? '&' : '?';
        let callbackName = `jsonpCB_${Date.now()}`;
        url += `${flag}callback=${callbackName}`;
        let node = document.createElement("script");
        if (data) {
            for (let key of data) {
                url += `&${key}=${data[key]}`;
            }
        }
        node.src = url;
        window[callbackName] = result => {
            delete window[callbackName];
            document.body.removeChild(node);
            if (result) {
                resolve(result);
            } else {
                reject('no data');
            }
        };
        node.addEventListener('error', () => {
            delete window[callbackName];
            document.body.removeChild(node);
            reject('error');
        }, false);
        document.body.appendChild(node);
    });
};
JSONP('http://192.168.0.103:8081/jsonp',{a:'t',b:'e'}).then((result) => {
    console.log(result);
}).catch((error) => {
    console.log(error);
});
```
> 参考: https://zhangguixu.github.io/2016/12/02/jsonp/
---
2020-03-18
# 26.instanceof的作用是什么，原理又是啥？
MDN上是这样描述instanceof的：
> instanceof 运算符用于测试构造函数的prototype属性是否出现在对象原型链中的任何位置    

因而，instanceof其原理其实就是一个查找原型链的过程，回顾一下原型链的相关知识：
1.所有 JavaScript 对象都有 __proto__ 属性，只有 Object.prototype.__proto__ === null ；
2.构造函数的 prototype 属性指向它的原型对象，而构造函数实例的 __proto__ 属性也指向该原型对象；
 > 参考: https://juejin.im/post/5cb3e7e0e51d456e896349d3
---
2020-03-19
# 27.自己实现一个instanceOf,并手写出来。
```javascript
const myInstance = function(target, origin) {
      const proto = target.__proto__;
      if (proto) {
          if (origin.prototype === proto) {
              return true;
          } else {
              return myInstance(proto, origin);
          }
      } else {
          return false;
      }
};
```
---
2020-03-20
# 28.Promise.all如何控制并发数？
promise并不是因为调用Promise.all才执行，而是在实例化promise对象的时候就执行了，在理解这一点的基础上，是实现控制并发数的限制，因而可以从promise实例化上下手。
具体的思路可以总结为：
1. 从array第1个元素开始，初始化promise对象，同时用一个executing数组保存正在执行的promise  
2. 不断初始化promise，直到达到poolLimt
3. 使用Promise.race，获得executing中promise的执行情况，当有一个promise执行完毕，继续初始化promise并放入executing中
4. 所有promise都执行完了，调用Promise.all返回
 > 参考: https://blog.csdn.net/weixin_33928137/article/details/88754909
---
2020-03-21
# 29.Javascript是单线程还是多线程？
js是单线程，那么为什么它是单线程呢？这与它的用途有关。作为浏览器脚本语言，Javascript的主要用途是与用户互动，以及操作DOM。
这决定了它只能是单线程，否则会带来很严重的同步问题。比如，假如JavaScript同时有两个线程，一个线程在某个某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？
为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。
 > 参考: https://www.cnblogs.com/keang001/p/11294388.html
---
2020-03-23
# 30.es6中find和some的区别？
some()是很好的一个方法，用于检查数组中是否有某些符合条件。如果有，则返回true。只要有一个满足即返回true，之后的不再执行(所以说对性能很友好！)。   
find()顾名思义，就是用来在数组中找到我们所需要的元素，只要有一个满足即返回该元素，不会多余遍历，对性能很友善。
他们的主要区别在于：some()返回的是布尔值，而find返回的是具体的那个元素。
 > 参考: https://juejin.im/post/5ca96c76f265da24d5070563#heading-11
---
2020-03-25
# 31.es6中的class继承为何在子类的constructor方法中必须调用super方法？
如果不调用super方法，新建实例时会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，
然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。
 > 参考: https://es6.ruanyifeng.com/#docs/class-extends
---
2020-03-27
# 32. new操作符的实现过程是咋样的？
new操作符用来生成一个新的对象, 它后面必须跟上一个函数(否则, 会抛出TypeError异常), 这个函数就是我们常说的构造函数.
二. new操作构造函数生成实例的过程
1. 首先, 当我们使用new操作符时, js会先创建一个空的对象;      
2. 然后, 构造函数中的this指向该空对象;
3. 其次, 在构造函数中通过操作this, 来给这个空对象赋予相应的属性;
4. 最后, 返回这个经过处理的"空对象"(此时, 对象已经不是空的了).
>注意事项：

如果构造函数的返回值是一个原始类型(非引用对象, 如字符串), 那么返回值会忽略这个原始类型，真实返回为new创建的"对象"；  
如果构造函数的返回值是一个引用对象(数组, 对象, 函数等), 那么返回值会覆盖new创建的"空对象"。
 > 参考: https://www.cnblogs.com/guorange/p/7146581.html
---
2020-03-28
# 33. ES5/ES6 的继承除了写法以外还有什么区别？
1. class声明会提升，但不会初始化赋值。类似于let/const声明变量。
2. class声明内部会启用严格模式。
3. class的所有方法（包括静态方法和实例方法）都是不可枚举的。
4. class的所有方法（包括静态方法和实例方法）都没有原型对象prototype，不能使用new来调用。
5. 必须使用new调用class。
6. class内部无法重新类名。
 > 参考: https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/20
---
2020-03-31
# 34. async和defer有什么区别？
HTML 4.01为<script>标签定义了defer属性。这个属性表明脚本在执行时不会影响页面的构造。就是说，脚本会被延迟到整个页面都解析完毕后再运行。因为，在<script>元素中
设置defer属性，相当于告诉浏览器立即下载，但延迟执行。  
HTML5为<script>标签定义了async属性，这个属性与defer属性类似，都用于改变处理脚本的行为，同样与defer类似，async只适用于外部脚本文件，并告诉浏览器立即下载文件。但与defer
不同的是，标记为async的脚本并不保证按照指定他们的先后顺序执行。
总结：
1. script没有defer和async情况下，会停止（阻塞）dom树构建，立即加载，并执行脚本；
2. script带async情况下，不会阻塞dom树构建，立即异步加载，加载好以后立即执行；
3. script带defer情况下， 不会阻塞dom树构建，立即异步加载。加载好以后，如果dom树还没构建好，则等dom树构建好以后再执行；如果dom树已经准备好，则立即执行。
4. 规范要求多个defer文件会按顺序执行，但在现实中延迟脚本并不一定会按照顺序执行。
5. 多个async的script文件的执行顺序不能保证按照顺序执行；
 > 参考: https://www.cnblogs.com/caizhenbo/p/6679478.html  
 https://developer.mozilla.org/zh-CN/docs/Web/Events/DOMContentLoaded
---
2020-04-01
# 35. 防抖和节流的原理及应用场景？
防抖（debounce）的原理：不管事件触发频率有多高，一定在事件触发n秒之后才执行，如果你在一个事件触发的n秒内又触发了这个事件，就以新的事件的时间为起准，n秒后才执行，
总之，触发完事件n秒内不再触发事件，n秒后再执行。
应用场景：  窗口大小变化，调整样式； 搜索框，输入后1000毫秒再搜索；表单的验证，在输入1000毫秒后验证。  

节流（throttle）的原理：不管事件触发频率多高，只在单位时间内执行一次。
应用场景：   比如要监听计算滚动条位置的时候，不必每次都触发节流，可以降低计算的频率而不必去浪费资源；  做商品预览图的放大镜效果时，不必每次移动鼠标都计算位置。
> 参考: https://www.codercto.com/a/35263.html
---
2020-04-08
# 36. 为什么我们要使用async/await ?
async 可以看做是promise与Generator的语法糖，但对其做了改进，是一个优秀的异步解决方案。内置执行器，Generator的执行必须依靠执行器，但async的执行器与生俱来，使得async函数与普通函数的调用别无二致。
更好的语义化，返回值是promise，对开发者友好。
我们知道，promise的出现主要是为了解决回调地狱，而async/await的优势就在于处理then式调用链。在少量then式调用的时候优势不明显，当多个then式调用的时候，其优势就凸显了。
当处理多个异步请求，且这几个异步请求之间有互相依赖关系时，使用await会临时阻塞后续代码的执行，这就代表着接口会按照我们的代码顺序“同步”执行（说是
同步，实际上还是异步请求，表现为同步行为是语法糖的原因）。这样，代码就回按照我们的预期执行，接口依赖关系也更加明了，维护起来也是更赏心悦目的。
除此之外，还具备如下优点：
1. 语法简洁，代码可读性更高；
2. 能使用try catch捕获异常；
3. 使代码更加符合思维逻辑；
缺点：
1. 需要babel编译；
2. 缺少abort请求中断，缺少异步控制流程；
3. 异常捕获较为麻烦； 
4. 没有依赖关系的请求需要借助promise.all；
> 参考: https://www.cnblogs.com/cuiyansong/p/7424997.html
https://blog.csdn.net/weixin_34212189/article/details/91383653
---
2020-04-11
# 37. 为什么通常在发送数据埋点请求的时候使用的是1×1像素的透明GIF图片？
主要是出于一下几点原因：   
1. 避免跨域；
2. 利用空白gif或1×1 px的img是互联网广告或网站监测方面常用的手段，简单、安全、相比PNG/JPG体积小，1px透明图，对网页内容的影响几乎没有影响，这种请求用在很多地方，比如浏览、点击、热点、心跳等等；
3. 图片请求不占用Ajax请求限额；
4. 不会阻塞页面加载，影响用户的体验，只要new Image对象就好了，一般情况下也不需要append到DOM中，通过它的onerror和onload事件来检测发送状态；
> 参考: https://www.cnblogs.com/wangxi01/p/11224534.html
---
2020-04-12
# 38. 如何正确判断this的指向？
如果用一句话说明 this 的指向，那么即是: 谁调用它，this 就指向谁。
但是仅通过这句话，我们很多时候并不能准确判断 this 的指向。因此我们需要借助一些规则去帮助自己：
this 的指向可以按照以下顺序判断:

**全局环境中的 this**

浏览器环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象 window;
node 环境：无论是否在严格模式下，在全局执行环境中（在任何函数体外部），this 都是空对象 {};

**是否是 new 绑定**

如果是 new 绑定，并且构造函数中没有返回 function 或者是 object，那么 this 指向这个新对象。

**函数是否通过 call,apply 调用，或者使用了 bind 绑定，如果是，那么this绑定的就是指定的对象【归结为显式绑定】**
   
如果 call,apply 或者 bind 传入的第一个参数值是 undefined 或者 null，严格模式下 this 的值为传入的值 null /undefined。
非严格模式下，实际应用的默认绑定规则，this 指向全局对象(node环境为global，浏览器环境为window)

**箭头函数的情况**

箭头函数没有自己的this，继承外层上下文绑定的this。
> 参考: https://github.com/YvetteLau/Blog/issues/35
---
2020-04-15
# 39. module.exports和exports有什么区别？
module.exports和exports一开始都是一个空对象{}，实际上，这两个对象指向同一块内存。这也就是说module.exports和exports是等价的
（有个前提：不去改变它们指向的内存地址）。
> 参考: https://www.jianshu.com/p/beafd9ac9656










