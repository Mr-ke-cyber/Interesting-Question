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






