---
2020-03-30
# 1. webpack中的loader的原理？plugin的运作原理？
loader可以简单的理解成一个文件处理器，webpack使用loader来处理各类文件，比如.scss转换成css文件，小图片转换成base64图片。
本质上讲，loader只是导出为函数的JavaScript模块，webpack打包的时候，会调用这个函数，把上一个loader产生的结果或资源文件(resource file) 传入进去。
Loader就像是一个翻译员，能把源文件经过转化后输出新的结果，并且一个文件还可以链式的经过多个翻译员翻译。但是单个Loader的职责是单一的，只需要完成一种转换。
如果一个源文件需要经历多步转换才能正常使用，就通过多个Loader去转换。在调用多个Loader去转换一个文件时，每个Loader会链式的顺序执行，第一个Loader将会拿到
需要处理的原内容，上一个Loader处理后的结果会传给下一个接着处理，最后的Loader将处理后的最终结果返回给webpack。
webpack是运行在Node.js之上的，一个Loader其实就是一个Node.js模块，这个模块需要导出一个函数。这个导出的函数的工作就是获得处理前的原内容，对原内容执行处理
后，返回处理后的内容。一个简单的Loader源码如下：
````javascript
module.exports = function(source) {
    // source 为compiler传递给Loader的一个文件原内容
    // 该函数需要返回处理后的内容，这里简单起见，直接把原内容返回了，相当于该‘Loader’没有做任何转换。
    return source;
}
````
由于loader运行在node.js之中，所以你可以调用任何Node.js自带的API，或者安装第三方模块进行调用。

插件(plugin)是 webpack 的支柱功能。webpack 自身也是构建于，你在 webpack 配置中用到的相同的插件系统之上！插件目的在于解决 loader 无法实现的其他事。
webpack 插件本质上是一个类，使用时候是一个具有 apply 属性的 JavaScript 对象。apply 属性会被 webpack compiler 调用，并且 compiler 对象可在整个编译生命周期访问。
多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 new 来创建它的一个实例。
通过Plugin机制使得webpack更加灵活，能适应各种应用场景。在Webpack运行的生命周期中会广播出许多事件，Plugin可以监听这些事件，在合适的时机通过Webpack提供的API改变输出结果。


> 参考: https://www.jianshu.com/p/c021b78c9ef2





