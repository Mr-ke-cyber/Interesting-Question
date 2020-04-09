----
2020-04-05
# 1. 什么是CSS动画之硬件加速？
浏览器接收到页面文档后，会将文档中的标记语言解析为DOM树。DOM树和CSS结合后形成浏览器构建页面的渲染树。渲染树中包含了大量的渲染元素，
每一个渲染元素会被分到一个图层中，每个图层又会被加载到GPU形成渲染纹理，而图层在GPU中 transform 是不会触发 repaint 的，这一点非常类似3D绘图功能，
最终这些使用 transform 的图层都会由独立的合成器进程进行处理。秘诀就在于通过transform的层会使用GPU渲染，因而不需要重绘。   
不是所有的CSS都会在GPU执行，目前支持以下几种：
1. transform；
2. opacity
3. filter
在一些情况下，我们并不需要对元素应用3D变换的效果，那怎么办呢？这时候我们可以使用个小技巧“欺骗”浏览器来开启硬件加速（transform hack）。
虽然我们可能不想对元素应用3D变换，可我们一样可以开启3D引擎。例如我们可以用transform: translate3d(0, 0, 0); 来开启硬件加速 。
*注意事项*
1. 大部分重要的问题都是关于内存。GPU处理过多的内容会导致内存问题。这在移动端和移动端浏览器会导致崩溃。因此，通常不会对所有的元素使用硬件加速。
2. 在GPU渲染字体会导致抗锯齿（-webkit-font-smoothing）无效。这是因为GPU和CPU的算法不同。因此如果你不在动画结束的时候关闭硬件加速，会产生字体模糊。
> 参考: https://www.w3cplus.com/css3/introduction-to-hardware-acceleration-css-animations.html   
https://blog.csdn.net/u010377383/article/details/100548769
----
2020-04-09
# 2. css加载会阻塞DOM树的解析和渲染吗？
看到这个问题的第一反应是css显然会阻塞渲染的，但是不会阻塞DOM树的解析，如果css阻塞DOM树的解析，那它还拿什么渲染？渲染个锤子？
经搜集资料查证，发现得分情况对待：
1. DOM解析和CSS解析是两个并行的进程，所以这也解释了为什么CSS加载不会阻塞DOM的解析。
2. 然而，由于Render Tree是依赖于DOM Tree和CSSOM Tree的，所以他必须等待到CSSOM Tree构建完成，也就是CSS资源加载完成(或者CSS资源加载失败)后，才能开始渲染。因此，CSS加载是会阻塞Dom的渲染的。
3. 由于js可能会操作之前的Dom节点和css样式，因此浏览器会维持html中css和js的顺序。因此，样式表会在后面的js执行前先加载执行完毕。所以css会阻塞后面js的执行。
   
   **DOMContentLoaded**
补充说明一下DOMContentLoaded这个事件，对于浏览器来说，页面加载主要有两个事件，一个是DOMContentLoaded，另一个是onLoad。
而onLoad没什么好说的，就是等待页面的所有资源都加载完成才会触发，这些资源包括css、js、图片视频等。而DOMContentLoaded，顾名思义，
就是当页面的内容解析完成后，则触发该事件。那么，正如我们上面讨论过的，css会阻塞Dom渲染和js执行，而js会阻塞Dom解析。
所以：
1. 如果页面中同时存在css和js，并且存在js在css后面，则DOMContentLoaded事件会在css加载完后才执行。
2. 其他情况下，DOMContentLoaded都不会等待css加载，并且DOMContentLoaded事件也不会等待图片、视频等其他资源加载。
> 参考
https://zhuanlan.zhihu.com/p/43282197
---
2020-04-10
# 3.为什么不推荐使用@import?
这个问题，说实话有点偏，对于用惯了现代前端开发技术的人来说，大多一头雾水。经研究：
1. Link属于html标签，而@import是CSS中提供的；
2. 在页面加载的时候，link会同时被加载，而@import引用的CSS会在页面加载完成后才会加载引用的CSS，会引起重绘，降低页面性能；
3. @import只有在ie5以上才可以被识别，而link是html标签，不存在浏览器兼容性问题；
4. Link引入样式的权重大于@import的引用（@import是将引用的样式导入到当前的页面中）
> 参考
https://blog.csdn.net/qq_41813695/article/details/80489601

