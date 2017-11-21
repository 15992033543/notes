# position属性详解

position属性用于元素定位，元素使用定位后，可以通过left、right、top、bottom、z-index属性调整元素的位置。

## 常用属性

* static：默认值，没有定位
* relative：生成相对定位的元素，相对其父元素定位，可以使用left、top属性定位元素，不会脱离文档流，在定位时会受到其它元素的影响
* fixed：生成绝对定位的元素，相对浏览器窗口定位，可以使用left、right、top、bottom、z-index属性定位元素（right为0表示右对齐，bottom为0表示底部对齐，left、right、top、bottom都为0表示填充浏览器窗口），脱离文档离，在定位时不会受到其它元素的影响
* absolute：生成绝对定位的元素，如果它的父元素设置了position，且值为absolute、relative或fixed其中一个，那么它相对这个父元素定位，否则相对html标签定位，可以使用left、right、top、bottom、z-index属性定位元素（right为0表示右对齐，bottom为0表示底部对齐，left、right、top、bottom都为0表示填充父元素），脱离文档流，在定位时不会受到其它元素的影响

因为absolute属性脱离文档流，拥有此属性的元素可以悬浮起来，所以可以用来做模态框或者一些弹出层等等。有一点值得注意的是，如果父元素设置了overflow属性，且其值为非visible，那么可导致使用了absolute的元素不能悬浮。