# position属性详解

position属性用于元素定位，元素使用定位后，可以通过left、right、top、bottom、z-index属性调整元素的位置。

常用属性有：

* static：默认值，没有定位
* relative：生成相对定位的元素，之后可以使用left、right、top、bottom、z-index属性调整元素的位置。它相对其父元素定位
* fixed：生成绝对定位的元素，之后可以使用left、right、top、bottom、z-index属性调整元素的位置。它相对浏览器窗口定位
* absolute：生成绝对定位的元素，之后可以使用left、right、top、bottom、z-index属性调整元素的位置。如果它的父元素设置了position，且值为absolute、relative或fixed其中一个，那么它相对这个父元素定位，否则相对html标签定位