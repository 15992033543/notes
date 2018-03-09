# position属性详解

position属性用于元素定位，元素使用定位后，可以通过left、right、top、bottom、z-index属性调整元素的位置。

## static

默认值，没有定位

## relative

生成相对定位的元素，相对其父元素定位，可以使用left、top属性定位元素，不会脱离文档流，在定位时会受到其它元素的影响

## fixed

生成绝对定位的元素，相对浏览器窗口定位，可以使用left、right、top、bottom、z-index属性定位元素（right为0表示右对齐，bottom为0表示底部对齐，left、right、top、bottom都为0表示填充浏览器窗口），脱离文档离，在定位时不会受到其它元素的影响

## absolute

生成绝对定位的元素，如果它的父元素设置了position，且值为absolute、relative或fixed其中一个，那么它相对这个父元素定位，否则相对html标签定位，可以使用left、right、top、bottom、z-index属性定位元素（right为0表示右对齐，bottom为0表示底部对齐，left、right、top、bottom都为0表示填充父元素），脱离文档流，在定位时不会受到其它元素的影响

因为absolute属性脱离文档流，拥有此属性的元素可以悬浮起来，所以可以用来做模态框或者一些弹出层等等。有一点值得注意的是，如果父元素设置了overflow属性，且其值为非visible，那么可导致使用了absolute的元素不能悬浮到最上层

## sticky

粘性固定，可以说是relative与fixed的混合，可以看下面这张效果图，当滚动到导航条的时候，导航条会固定在顶部：

![效果图](https://upload-images.jianshu.io/upload_images/4655331-0610bd0d1cd53bf8.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/384)

这种效果可以通过监听滚动事件，滚动到导航条的位置时，将导航条布局的position设置为fixed即可。但现在可以通过css来实现：

css:

    .item {
      height: 600px;
      background-color: red;
    }

    .nav {
      height: 100px;
      position: sticky;
      top: 0;
      background-color: blue;
    }

html:

    <div>
      <div class="item">item</div>
      <div class="nav">nav</div>
      <div class="item">item</div>
      <div class="item">item</div>
    </div>