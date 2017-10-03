# 解决使用float属性后产生的副作用

问题一：元素使用了float属性后，前面或后面没有使用float属性的元素会紧跟在其后面。

    <div>
      <div style="float: left">hello</div>
      <div>world</div>
    </div>

上面的代码，两个子元素都是块级元素，hello与world理应换行，但实际上变成同行显示。

解决办法：后面的元素使用clear属性清除浮动。

    <div>
      <div style="float: left">hello</div>
      <div style="clear: both">world</div>
    </div>

问题二：子元素使用float属性后，不会撑开父元素。

    .parent {
      background-color: red;
    }

    .child {
      width: 100px;
      height: 100px;
      background-color: blue;
      float: left;
    }

    <div class="parent">
      <div class="child"></div>
    </div>

上面的代码，父元素理应会被子元素撑开，高度会是100px，但实际上父元素高度为0。

解决办法一：新增一个子元素，并使用clear属性清除浮动。

    .parent {
      background-color: red;
    }

    .child {
      width: 100px;
      height: 100px;
      background-color: blue;
      float: left;
    }

    <div class="parent">
      <div class="child"></div>
      <div style="clear: both"></div>
    </div>

解决办法二：父元素也使用float属性，前提是父元素中需要有其它内容。

    .parent {
      background-color: red;
      float: left;
    }

    .child {
      width: 100px;
      height: 100px;
      background-color: blue;
      float: left;
    }

    <div class="parent">
      hello
      <div class="child"></div>
    </div>

解决办法三：父元素使用overflow属性。

    .parent {
      background-color: red;
      overflow: hidden;
    }

    .child {
      width: 100px;
      height: 100px;
      background-color: blue;
      float: left;
    }

    <div class="parent">
      <div class="child"></div>
    </div>

overflow属性规定当内容溢出元素框时发生的事情。有以下几个值：

* visible: 默认值，内容不会被修剪，会呈现在元素框之外
* hidden: 内容会被修剪，并且其余内容是不可见的
* scroll: 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容
* auto: 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容
* inherit: 规定应该从父元素继承overflow属性的值

可以单独设置某个方向的overflow（overflow-x与overflow-y）