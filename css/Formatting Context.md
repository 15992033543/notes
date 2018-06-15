# Formatting Context

Formatting Context是W3C CSS2.1规范中的一个概念，它是页面中的一块渲染区域，有自己的一套渲染规则，它决定了其子元素将如何定位以及和其它元素的关系和相互作用。

## Block Formatting Context

块级格式化上下文，它是一个独立的渲染区域，这个区域与外部互不相干，反之亦然。

形成BFC的条件：

* 根元素
* float属性不为none时
* position为absolute或者fixed时
* display为inline-block、table-cell、table-caption、flex、inline-flex时
* overflow不为visible时

BFC的布局规则：

> 计算BFC高度时，浮动元素也参与计算

典型的例子就是子元素都为浮动元素时，父元素高度塌陷，这时只需将父元素设置成BFC元素，就可以避免这个问题。

css:

    .container {
      overflow: hidden;
      border: 1px solid #CCC;
    }

    .box {
      float: left;
      width: 100px;
      height: 100px;
      border: 1px solid #000;
    }

html:

    <div class="container">
      <div class="box">box1</div>
      <div class="box">box2</div>
    </div>

> BFC不会与浮动元素重叠

两个兄弟元素，如果一个元素设置了浮动，另一个元素并非BFC元素，那么这个元素会与浮动元素产生重叠，元素的内容会围绕浮动元素显示。典型的例子文字围绕图片显示：

css:

    .container {
      width: 500px;
      border: 1px solid #CCC;
    }

    img {
      width: 200px;
      float: left;
    }

html:

    <div class="container">
      <img src="http://img.hc360.com/broadcast/info/images/201006/201006111053487505.jpg" />
      <p>此处省略一万字...</p>
    </div>

如果想要图片和文字分两栏显示，只需要为p元素添加overflow: hidden即可（要注意的是，同是BFC元素，它们所产生的效果有所不同，这里的效果使用overflow是最合适的）。

使用浮动和overflow: hidden的方式还可以实现左边固定宽度，右边宽度自适应的效果：

css:

    .box1 {
      width: 200px;
      float: left;
      background-color: red;
    }

    .box2 {
      overflow: hidden;
      background-color: blue;
    }

html:

    <div class="box1">固定宽度</div>
    <div class="box2">自适应宽度</div>

> BFC可以阻止块级元素上下外边距合并

块块元素有一个特性，就是相邻的两个块级元素，元素的上下边距会合并（取两者间的最大值），如下，正常情况来说两者应该相距30px，但实际上只相距20px：

css:

    .box1 {
      width: 100px;
      height: 100px;
      background-color: red;
      margin-bottom: 10px;
    }

    .box2 {
      width: 100px;
      height: 100px;
      background-color: blue;
      margin-top: 20px;
    }

html:

    <div class="box1"></div>
    <div class="box2"></div>

只需要添加一个BFC的容器，就可以阻止这个特性：

css:

    .box1 {
      width: 100px;
      height: 100px;
      background-color: red;
      margin-bottom: 10px;
    }

    .box2 {
      width: 100px;
      height: 100px;
      background-color: blue;
      margin-top: 20px;
    }

    .bfc-box {
      overflow: hidden;
    }

html:

    <div class="box1"></div>
    <div class="bfc-box">
      <div class="box2"></div>
    </div>