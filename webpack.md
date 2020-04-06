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
---
2020-04-07
# 2. webpack的优化策略有哪些？
优化方式非常之多，可以逐个在项目中尝试配置，主要从以下几个方面展开：  

1. 使用 purgecss-webpack-plugin来去除无用的样式；
2. 将一些js库文件存在CDN上，减少webpack打包出来的js体积，在index.html中通过<script>标签引入，自动化实现可以使用插件html-webpack-externals-plugin;
3. DllPlugin动态链接库。原理：很多时候我们在开发时无论是用React还是Vue，我们都不希望这个开发的主力框架每次都被打包一遍，这样也是费时费力的事情。所以，出现了DllPlugin这种插件，它纯属webpack内置的，可以放心使用。
我们新建一个 webpack 的配置文件，来专门用于编译动态链接库，例如名为: webpack.config.dll.js。生成动态链接库后，再将其引入。
4. 抽取公共代码。开发的时候，经常会有不同的模块引用了同一个第三方包，因而我们可以把这些模块共用的第三方包抽取出来，以减小打包后的体积。
webpack4中自带了抽取公共代码的方法，通过optimization里的splitChunks来做到。
5. IgnorePlugin。使用IgnorePlugin来剥离掉我们不需要的冗余资源，减小打包后的代码体积。
6. Include和Exclude。include: 包含指定目录下的文件解析，exclude: 排除指定目录不进行解析，exclude的优先级高于include。
7. happypack。开启多进程打包操作，充分发挥CPU的威力。
8. tree-shaking。 如果使用ES6的import 语法，那么在生产环境下，会自动移除没有使用到的代码。有副作用，需要注意配置，在package.json中增加配置。
9. cache-loader。 在一些性能开销较大的 loader 之前添加 cache-loader，将结果缓存中磁盘中。默认保存在 node_modueles/.cache/cache-loader 目录下。
> 参考: https://juejin.im/post/5e6cfdc85188254913107c1f#heading-2  
https://juejin.im/post/5cceecb7e51d453ab908717c#heading-4
---


