布局方式
***

1、Android 中常用的布局都有哪些？
* FrameLayout
* RelativeLayout
* LinearLayout
* AbsoluteLayout
* TableLayout
* GrideLayout
 Android 4.0新增的网格布局，性能及功能都要比tablelayout好，比如GridLayout布局中的单元格可以跨越多行，而tablelayout则不行

2、XML布局方式比Java代码布局方式好在哪里？
* 将程序的表现层和控制层分离；
* 在后期修改用户界面时，无须更改程序的源程序；
* 可通过可视化工具直接看到所设计的用户界面，有利于加快界面设计的过程

3、谈谈 UI 中, Padding 和 Margin 有什么区别?
* padding 是站在父 view 的角度描述问 题,它规定它里面的内容必须与这个父 view 边界的距离。
* margin 则是站在自己的角度描述问题,规定自己和其他(上下左右)的 view 之间的距离,如果同一级只有一个 view,那么它的效果基本上就和 padding 一样了。

4、android:layout_gravity 和 android:gravity 的区别? 
* android:layout_gravity 是让该布局在其父控件中的布局方式,
* android:gravity是该布局布置其字对象的布局方式。

5、[Android中布局的优化·查看详细内容](http://www.jianshu.com/p/ab15167c48da)
* 尽可能减少布局的嵌套层级 
* 使用<include>标签复用相同的布局代码 
* 使用<merge>标签减少视图层次结构 
* 通过<ViewStub>实现 View 的延迟加载

View相关
***

1、[View基础坐标及滑动相关知识问答·请参考](https://github.com/jasonLYF/jason-blog/blob/master/View%E5%9F%BA%E7%A1%80%E5%9D%90%E6%A0%87%E5%8F%8A%E6%BB%91%E5%8A%A8%E7%9B%B8%E5%85%B3%E7%9F%A5%E8%AF%86%E9%97%AE%E7%AD%94.md)
文章内容包括：
* View的坐标参数及其关系
* View中的几个重要方法
* 获取View的位置坐标失败问题怎么处理？
* [view滑动方式比较](http://www.jianshu.com/p/2b48551d5319)
* 其他View相关常见面试题

2、[View事件分发机制总结及常见相关面试题·请参考](https://github.com/jasonLYF/jason-blog/blob/master/View%E4%BA%8B%E4%BB%B6%E5%88%86%E5%8F%91%E6%9C%BA%E5%88%B6%E7%9B%B8%E5%85%B3%E9%97%AE%E7%AD%94.md)

3、[Android 之 自定义View·请参考](http://www.jianshu.com/p/7468e038825a)

动画
***
1、Android中的动画有哪些，区别是什么
* [逐帧动画(Drawable Animation)](https://github.com/jasonLYF/jason-blog/blob/master/View%20Animation%20%5BView%E5%8A%A8%E7%94%BB%5D.md)： 加载一系列Drawable资源来创建动画，简单来说就是播放一系列的图片来实现动画效果，可以自定义每张图片的持续时间
* [补间动画(Tween Animation)](https://github.com/jasonLYF/jason-blog/blob/master/View%20Animation%20%5BView%E5%8A%A8%E7%94%BB%5D.md)： Tween可以对View对象实现一系列简单的动画效果，比如位移，缩放，旋转，透明度等等。但是它并不会改变View属性的值，只是改变了View的绘制的位置，比如，一个按钮在动画过后，不在原来的位置，但是触发点击事件的仍然是原来的坐标。
* [属性动画(Property Animation)](https://github.com/jasonLYF/jason-blog/blob/master/Property%20Animation%20%5B%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB%5D.md)： 动画的对象除了传统的View对象，还可以是Object对象，动画结束后，Object对象的属性值被实实在在的改变了

屏幕适配
***

1、如何实现屏幕分辨率的自适应？
* 通过权重(layout_weight)的方式来分配每个组件的大小，也可以通过具体的像素(dip)来确定大小。
* 尽量使用Relativelayout 。
已知应用支持平台设备的分辨率,可以提供多个layout_320*480 ...
drawable-hdpi,drawable-mdpi,drawable-ldpi分别代表分辨率为480*800,360*480,240*360, 放置图片大小相差1.5倍
最后还需要在AndroidManifest.xml里添加下面一段，没有这一段自适应就不能实现：
<supports-screens
android:largeScreens="true"
android:normalScreens="true"
 android:anyDensity = "true"/>
在</application>标签和</manifest> 标签之间添加上面那段代码。即可。
备注：三者的解析度不一样，就像你把电脑的分辨率调低，图片会变大一样，反之分辨率高，图片缩小
还可以通过.9.png实现图片的自适应

2、怎么适配多种屏幕
* 支持屏幕类型
```
<supports-screens android:resizeable=["true"| "false"]
　　android:smallScreens=["true" | "false"]   //是否支持小屏
　　android:normalScreens=["true" | "false"]  //是否支持中屏
　　android:largeScreens=["true" | "false"]   //是否支持大屏
　　android:xlargeScreens=["true" | "false"]  //是否支持超大屏
　　android:anyDensity=["true" | "false"]     //是否支持多种不同密度的屏幕
　　android:requiresSmallestWidthDp=”integer”
　　android:compatibleWidthLimitDp=”integer”
　　android:largestWidthLimitDp=”integer”/>
```
* 对不同大小的屏幕提供不同的layout
比如，如果需要对大小为large的屏幕提供支持，需要在res目录下新建一个文件夹layout-large/并提供layout。当然，也可以在res目录下建立layout-port和layout-land两个目录，里面分别放置竖屏和横屏两种布局文件，以适应对横屏竖屏自动切换
* 对不同密度的屏幕提供不同的图片
* 通过.9图片实现图片的自适应
* 其他一些Tips
  * 通过权重(layout_weight)的方式来分配每个组件的大小，也可以通过具体的像素(dip)来确定大小
  * 尽量使用Relativelayout  。
  * 不要使用具体的像素来表示控件尺寸，如果长宽以像素为单位定义的界面元素（比如一个按钮），在低密度的屏幕上会显得很大，但在高密度的屏幕上则会显得很小。
  * 以中等密度的正常屏幕（HVGA）为基准进行界面布局

3、屏幕相关的一些专业术语
* 尺寸：通常是指屏幕的物理尺寸，是屏幕的对角线长度，单位是英寸。
* 分辨率：是指屏幕上拥有的像素的总数，通常使用“宽度×长度” ，但是屏幕的分辨率并不意味着屏幕比例；
* 屏幕比例：决定屏幕比例的是设备的物理长度和物理宽度
* 密度：是指以屏幕分辨率为基础，沿屏幕长宽方向排列的像素；密度较低的屏幕，在长和宽方向都只有比较少的像素，而高密度的屏幕通常则会有很多，甚至会非常非常多的像素排列在同一区域。
* 总结
  * 物理尺寸相同的情况下，分辨率越高，图像和文字看起来越小！
  * 物理尺寸相同的情况下，密度越高，则分辨率越高，图像和文字看起来越清晰！
  * 分辨率和密度相同的情况下，物理尺寸越小，图像和文字看起来越清晰！
* 总结
  * 密度是160dpi的屏幕，1dip等于1px。（160除以160）
  * 密度是240dpi的屏幕，1dip等于1.5px。（240除以160）
  * 密度是320dpi的屏幕，1dip等于2px。（320除以160）

