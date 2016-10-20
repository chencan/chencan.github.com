---
layout: post
title: "iOS 多尺寸屏幕适配背景知识"
description: ""
category: 库
tags: [iOS]
author: 陈灿
---  



# iOS 多尺寸屏幕适配背景知识


iDevice设备屏幕大小，在iPhone 6和iPhone Plus出现后，iPhone界面设计和开发面临了新的任务，即对不同设备屏幕进行适配。  
苹果官方也推出来新的技术更新，来帮助我们进行设计和开发：

* Auto Layout 
* Trait Collections, Size Class
* Storyboards
* Xcode6中使用矢量图

我们需要理解和运用苹果提供给我们的技术、并制定自己的适配方案。  
这篇文档暂时***并没有***去讲如何运用苹果的这些技术进行适配，只是对背景知识一个大概的讲解。  
稍后的更新会有详尽的补充。


## 相关术语
* Point  
 Point可以理解为***iOS程序员***眼中的大小单位。它是iOS操作系统中的抽象的概念。
 >At the beginning, coordinates of all drawings are specified in points.
 >Points are abstract units, they only make sense in this mathematical coordinate space.
 
* Rendered Pixels  
 Rendered Pixels可以理解为***UI设计师***眼中的大小单位。
 >Point-based drawings are rendered into pixels. This process is known as **rasterization**.
 >Point coordinates are multiplied by scale factor to get pixel coordinates. Higher scale factors result in higher level of detail.

* Physical Pixels  
 设备屏幕硬件像素。
 >The device screen may have lower pixel resolution than the image rendered in previous step.
 >Before the image can be displayed, it must be **downsampled** (resized) to lower pixel resolution.
 
* Physical Device  
 设备名称
 >Finally, computed pixels are displayed on the physical screen.
 >The PPI number tells you how many pixels fit into one inch and thus how large the pixels appear in the real world.

* 切图倍数：1x、2x和3x  
 这些表示Rendered Pixels到Point的倍数关系。  
 iPhone4之前，是1倍；在iPhone6到iPhone4，都是2倍；iPhone 6Plus往后，是3倍。
 
Point和Rendered Pixels是我们需要关心的，Point是开发人员在Coding时，屏幕的逻辑大小单位，Rendered Pixels是设计师出图的大小单位。
 

## 目前需要支持的所有尺寸
| Point     | Rendered Pixels	| iDevice 		| Ratio
| :-------- | :-------- | :-------- | :-------- | 
| 320 * 480 | 320 * 480	(1x)	| iPhone3和3gs	| 1.5
| 320 * 480 | 640 * 960	(2x)	| iPhone4和4s	| 1.5
| 320 * 568 | 640 * 1136(2x)	| iPhone5和5s	| 1.775
| 375 * 667 | 750 * 1334(2x)	| iPhone6 		| 1.77866667
| 414 * 736 | 1242 * 2208(3x)	| iPhone6Plus 	| 1.77777778

这个只是iPhone方面，iPad方面还没有考虑，这个是因为一些App在iOS和iPhone上面的UI设计有很大的不同，需要两套设计和代码。

我们开发一个iPhone的App，需要适配后四种尺寸，第一种由于太古老，不需要考虑。

>Note: 如果你不进行适配，iOS也会自动帮你适配，但它只是简单的缩放，会导致UIKit控件模糊（如果提供了适配的图片，图不会模糊）。


## 适配需要做的工作

本来对于不同的屏幕大小，我们应该分别进行设计和开发，比如在Auto layout出现之前，经常会用代码判断设备类型，然后将控件设置成不同的大小或位置。

由于iPhone屏幕尺寸变化不是很大，运用一些适配原则，可以在保证较高的UI效果前提下，减少适配的工作量。Auto laytou可以帮助我们制定适配原则。

也就是：
>一套UI设计方案 ＋ 一些适配原则 ＝ 适配所有的机型

那么有哪些设配原则，我们需要怎么样的适配原则？这个需要针对不同的界面，或者一个界面的不同部分，采用不同的适配原则。

我们举个例子，说说什么样的适配原则是好的。    
Bad sample:  
![](http://chencan.github.io/attachment/iOS_multi_screen/4196_140915090929_1.jpg)

Good sample:  
![](http://chencan.github.io/attachment/iOS_multi_screen/4196_140915091053_1.jpg)

Good sample相比Bad sample，将上部分的空间都等比例放大了。

对于Point大小不变的控件，比如例子中左上角的按钮，它的point都是44*44，设计师只是需要提供2x和3x的切图即可。  

| Point    	| *Rendered Pixels*	| iDevice 		|
| :-------- | :-------- | :-------- |  
| 44 * 44 	| 88 * 88(2x)		| iPhone5和5s	| 
| 44 * 44 	| 88 * 88(2x)		| iPhone6 		| 
| 44 * 44 	| 132 * 132(3x)		| iPhone6Plus 	| 

对于Point大小有改变的控件，比如例子中的任务头像，它在三个设备中的Point大小都不一样，设计师需要分别出三张图。

| Point    	| *Rendered Pixels*	| iDevice 		|
| :-------- | :-------- | :-------- | 
| 80 * 80 	| 88 * 88(2x)		| iPhone5和5s	| 
| 100 * 100	| 200 * 200(2x)		| iPhone6 		| 
| 120 * 120	| 360 * 360(3x)		| iPhone6Plus 	| 

上面的情况中，320 Point宽度的设备和375 Point款度的设备，他们都是2x，如果在两种设备上需要使用不同的图，该怎么命名图片呢？[这个帖子](http://stackoverflow.com/questions/26859336/xcode-6-how-to-set-separate-2x-images-for-iphone-5-and-6-devices)，还有[这个](http://stackoverflow.com/questions/25969533/how-to-handle-image-scale-3x-and-2x-properly-on-new-iphone-6-and-iphone-6-pl)里有说明该怎么做：  

<pre><code>
image-320@2x//iPhone 5
image-375@2x//iPhone 6

NSNumber *screenWidth = @([UIScreen mainScreen].bounds.size.width);
NSString *imageName = [NSString stringWithFormat:@"image-%@", screenWidth];
UIImage *image = [UIImage imageNamed:imageName];
</code></pre>

### 参考
[Adaptive your user interface](https://developer.apple.com/design/adaptivity/)  
[poster_iphones](http://chencan.github.io/attachment/poster_iphones.pdf)  
[iPhone 6/6 Plus 出现后，如何改进工作流以实现一份设计稿支持多个尺寸？](http://www.cocoachina.com/design/20140915/9617.html)  
[Beginning Auto Layout Tutorial in iOS 7: Part 1](http://www.raywenderlich.com/50317/beginning-auto-layout-tutorial-in-ios-7-part-1)  
[Beginning Auto Layout Tutorial in iOS 7: Part 2](http://www.raywenderlich.com/50317/beginning-auto-layout-tutorial-in-ios-7-part-2)  
[iPhone 6/6 Plus 出现后，如何改进工作流以实现一份设计稿支持多个尺寸？](http://www.cocoachina.com/ios/20141205/10534.html)  
[新特性Size Class介绍](http://blog.csdn.net/yongyinmg/article/details/41045069)  
[在xcode6中使用矢量图(iPhone6置配UI)](http://blog.csdn.net/cuibo1123/article/details/39486197)

