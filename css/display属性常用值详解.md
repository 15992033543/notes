# display属性常用值详解

display属性最常用的三个值分别是inline，block，inline-block。其它属性可以查看：[display属性](http://www.w3school.com.cn/cssref/pr_class_display.asp)

inline：

* 行内标签，不会占用一行
* 宽度默认为元素内容的宽度
* 设置width、height无效
* 设置水平方向的margin、padding有效，垂直方向无效

block：

* 块级标签，占用一行
* 宽度默认为所在父元素的宽度
* 设置width、height有效
* 设置margin、padding有效

inline-block：

* 行内块标签，不会占用一行
* 宽度默认为元素内容的宽度
* 设置width、height有效
* 设置margin、padding有效

注意block有一个边距合并的特性，两个相邻的block元素，如果第一个元素设置了margin-bottom，第二个元素设置了margin-top，那么他们之间的边距就会取值比较大的。

    <div style="margin-bottom: 20px">box1</div>
    <div style="margin-top: 70px">box2</div>

如上，由于边距合并的特性，所以上面两个div相距为70px。