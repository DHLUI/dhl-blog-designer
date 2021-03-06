---
title: 设计师需要了解的一些移动端适配原理｜仅针对Native
subtitle:   " \"DPI、PPI、DP、PT、PX...\""
date: 2017-01-02 20:26:33
header-img: ppi_bg.jpg
tags:
    - 开发对接
---

> 注：移动端的页面设计一般包括wap和native两种；本文的内容仅针对native的UI设计做总结归纳。
### 一.先来熟悉几个知识点
__ 1.分辨率__ 
我们在做设计的时候，新建一个页面，这个页面会有自己的宽高像素值，这个数值，即可称为这个页面的分辨率。比如对于iPhone6而言，其宽高是750x1334px，我们可以称它的分辨率为750x1334。
这里要特别说明一下，我们在PS中新建一个画布的时候，弹窗中的这个“分辨率”其实并不是UI意义上的分辨率（这个是平面设计上的分辨率，因为PS诞生的初期，很多功能的设定是为了平面设计而生），下图PS弹窗中的这个“分辨率”，对应到UI设计的话，其实是PPI（后面会讲到）
![](http://oi68vw4nk.bkt.clouddn.com/ppi01.png)
__ 2.屏幕大小__ 
屏幕大小是指屏幕对角线两点之间的物理长度，一般以英寸（inch）为单位。比如小米Note4的屏幕为5.5英寸，就是指这款手机屏幕对角线实际长度为5.5英寸。

__ 3.屏幕密度PPI(pixel per inch)__ 
PPI在移动端设计中是一个很重要的概念，即屏幕密度。可以理解为每一英寸上的像素点个数。如下图：

![](http://oi68vw4nk.bkt.clouddn.com/ppi05.png)

举个例子，图中是一个iPhone6，它的屏幕宽度是2.3inch，在水平方向上，一共有750个像素点，那么它的ppi＝750px/2.3inch，约等于326ppi*（这个结论建立在方形像素的基础上的，因为牵涉到屏幕工艺，这里暂不深究，新手可忽略这句话）*

对于ppi的解释，很多文章喜欢用“勾股定理”的方式去计算：即用勾股定理计算出屏幕对角线的像素点个数，然后再除以屏幕大小，得出ppi

![](http://oi68vw4nk.bkt.clouddn.com/ppi02.png)

利用勾股定理的计算结果是没有问题的，但是在理解上，可能会有点难理解。为什么这么说呢？

![](http://oi68vw4nk.bkt.clouddn.com/ppi06.png)

譬如这张图，试问我该如何去考虑这个对角线上面有多少个像素点呢？这根对角线跟部分方形像素点的交集可能只有一个小角，那么这个像素点我到底该不该算做是这个对角线上的呢？显然，是没法统计的。

那为什么说用“勾股定理”的方式计算是合理的呢？笔者这里解释一下“勾股定理”方式计算ppi的原理：


__ 我们来做一组讨论：__ 

设屏幕水平方向为x inch，竖直方向为y inch，屏幕水平向的像素个数是α，屏幕竖直向的像素个数是β，屏幕的密度ppi＝p。

根据ppi的定义，则会有α=xp,β=yp。可以推导出：x=α/p;y=β/p;

根据勾股定理，设屏幕的对角线长度z inch，根据勾股定理，有：z²=x²+y² 进一步可得出： z = √(x²+y²)

好，又因为x=α/p;y=β/p;
所以：z = √(α/p)²+(β/p)² = √(α²/p²)+(β²/p²) =√(α²+β²)/√p² =√(α²+β²/p

除数和被除数对调位置，即: p = √(X²+Y²）/ z
这也正是ppi的勾股定理计算由来。


__ 4.DPI(一般用于对打印精度的描述)__ 
用于平面行业，是指打印的分辨率，每英寸所能打印的点数(dot per inch)。

但是很多UI设计师也喜欢把DPI用于屏幕精度，如果是针对屏幕的话，其实大家所说的也就是PPI，只不过这里的DPI和PPI之间有一定的换算关系：在72dpi的情况下，dpi＝ppi，如果是144dpi，则2dpi＝1ppi，有倍数关系；所以我们在ps中新建画布的话，一般以72dpi为基准（不论你设计的屏幕是多少的ppi），因为在这样的基准下，ppi在数值上等同于dpi，方便计算（但它们本质是两个不同的事物哦）

__ 5.DIP(DP)__ 
设备独立像素（Device Independent pixels）。是安卓开发用的单位，1dp表示在屏幕点密度为160ppi时1px长度。dp更类似一个物理尺寸，同样宽高dp的一张图片，在不同ppi上看起来是差不多大小的。

__ 6.SP(scaled pixels)__ 
这也是安卓的常用单位，和DP不一样，SP主要用于安卓的文字。SP和PX的换算关系与DP和PX的换算关系一样（后面会做详细阐述）；那么两者的区别是啥呢？DP标注的元素，无法进行系统缩放；而SP标注的元素则可以进行系统缩放；关于系统缩放，笔者这里先不做深入阐述，可以简单的理解为：我在系统设置里面，可以去调整手机字号的大小。那么可以被调整的元素，则是使用SP标注的。

__ 7.PT__ 
iOS使用的开发单位，也称为逻辑像素。在iOS的一倍图（163PPI），如iPod Touch第一代中，1pt＝1px。

巧合的是，在平面设计中也有一个pt的单位，它最早源自于传统的铅字印刷，是用来表示铅字块的尺寸，在印刷和平面设计的世界里，是一个标准的长度单位，1pt＝1/72英寸。

### 二.DP与PX的转换

就如上文中提到的dp是安卓开发中用的单位，同样dp大小的元素，在不同屏幕上看起来，是差不多的。

好，我们来举一个比较通俗的例子：

现在有这么两个屏幕，我设定它的宽高都是1 inch，但一个水平方向的像素个数为10px，一个为20px。

![](http://oi68vw4nk.bkt.clouddn.com/ppi07.png)

我们在左侧的屏幕做了一个正方形，这个正方形占一个像素点；那么，平移到另外一个屏幕上时，为了保证这个正方形在不同的屏幕上看起来差不多大，在另外一个屏幕上，这个正方形需要占四个像素点，对于安卓工程师而言，这两个屏幕上，这个点的宽高竖直是一定的，也就是他们的dp值是一样的，但是由于屏幕密度的差异，左侧屏幕上的点宽高为1px，右侧屏幕上的点宽高为2px。

下面来看一个表格：

![](http://oi68vw4nk.bkt.clouddn.com/ppi031.png)

这是安卓各种屏幕和其对应ppi的关系。就如上文中提到的，在屏幕密度为160ppi，即mdpi的时候，1dp＝1px，相当于这是一个基准屏幕。那么我们适配到其他密度的安卓屏幕的时候，该如何处理呢？

举个例子，我针对mdpi做了一个icon,在适配到hdpi的时候，我需要放大到1.5倍，适配到xhdpi的时候，需要放大到两倍。具体放大或者缩小到多少，需要看对应屏幕的密度和mdpi屏幕密度的比例关系（也就是表格中提到的比率）。

![](http://oi68vw4nk.bkt.clouddn.com/ppi04.png)

譬如我这个icon的宽度设计的时候是100px，那么在mdpi中，其宽度就是100dp，在hdpi中图片会被拉伸至就是150px，到了xhdpi中，图片会被拉伸至200px（但是不论在哪个屏幕中，它宽度的dp数值均是100dp）所以针对hdpi和xhdpi的屏幕，我们需要单独的输出一张放大到1.5倍和一张放大到2倍的图片资源（避免机器自己拉图片伸导致的失真问题）。

关于sp：对于安卓系统中的文字，当系统字号设为“普通”时，sp与px的尺寸换算和dp与px是一样的。

### 三.PT与PX的转换

相对于安卓各种各样的ppi屏幕，苹果的ppi种类要相对少很多。如图：

![](http://oi68vw4nk.bkt.clouddn.com/ppi08.png)理解完安卓的DP和PX之间的关系后，就很好理解iOS中的PT和PX的关系了。
163ppi的苹果机器，相当于安卓的mdpi；
326ppi的苹果机器，相当于安卓的xhdpi；
401ppi的苹果机器，相当于安卓的xxhdpi；

这三种不同ppi，也就是我们常说的@1x，@2x，@3x；所以iOS的屏幕密度种类，相对安卓少很多，并且其三种屏幕ppi密度比例，约等于1:2:3所以我们在一倍图中做了一个icon宽高为10px，那么适配到二倍图就需要把图片放大到20px，适配到三倍图就需要把图片放大到30px。### 四.工作中的适配问题基于现在敏捷型开发的工作模式，很多时候我们没有时间针对各种ppi的屏幕都输出一套设计稿（成本太大）。更多的时候，我们会针对iOS或者安卓单独输出一套图，然后去适配其他ppi的屏幕；甚至一稿适配双平台所有的屏幕。
譬如目前苹果的主流使用机型是iPhone6*（这个结论在不同的时间点有不同的答案，设计师需要根据当下的市场情况去选择）*我们在做UI设计的时候，会依照iPhone6的设计稿去做图，对于设计中需要输出给开发的图片资源，会在@2x的基础上，往上乘以1.5倍制作@3x的图、往下乘以0.5倍制作@1x的图给到开发。不过现在使用一倍图手机的用户已经很少很少了，所以目前iOS的@1x基本不用考虑，实际工作中，输出@2x和@3x即可
安卓的一般会按照xhdpi去做图，然后去适配其他的屏幕。即在720×1280中做图，让开发人员放到drawable-xhdpi的资源文件夹中即可，对于ppi较低屏幕的话，没有必要单独输出一套图，一般直接用xhdpi去在低密度手机上运行，如果出现卡顿的现象，可以单独针对特殊场景输出图片资源。
__ 这里着重说一下一稿适配双平台：__
目前的话，一般会以iPhone6，即750x1334px的设计稿去做图，然后输出iOS的@1x以及@3x图片资源给到开发。
那么我们怎么利用iOS的图去适配给到安卓开发呢？
我们可以仔细看一下，安卓的xhdpi和iOS的750x1334其实在横向的像素值只差了30px，除以2也就是15pt／dp，对应的折合到屏幕两边的差距，约等于7～8个dp／pt，这个差值其实并不是很大。另外在ppi方面，这两个屏幕一个是320（xhdpi），一个是326（@2x），仅仅差了6ppi，所以在做设计的时候，我们可以使用iPhone6的设计稿，以及其切图输出的资源，直接适配到xhdpi。这样的做法可以满足绝大多数场景，譬如表单设计等等；但是对于一些特殊的场景，如若适配不能够解决问题，我们则需要单独输出一套安卓的设计稿。
随后我们可以再输出一套1.5倍的图片资源，去适配iPhone6p以及安卓的xxhdpi。
那么有一些图片资源，我们想让它在不同的屏幕中始终保持一个固定的像素值该怎么办？可以让工程师把这张图片放置于drawable-nodpi中，这样这张图在不同的ppi中，可以始终保持着其原先的px值。

*本文针对了移动端屏幕的知识点做了归纳，希望可以对大家有所帮助，同时也是对自己的知识做一个阶段性的总结，谢谢。*