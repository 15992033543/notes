# css3的transition属性

transition是css3的属性，用于制作过渡效果。transition可以为元素指定一个css属性，元素的这个css属性发生变化时，会产生过渡效果。

例子一：

    #div {
      width: 100px;
      height: 100px;
      background-color: red;
      transition: width 500ms;
    }

    #div:hover {
      width: 400px;
    }

上面的代码，当鼠标移至这个div时，div的宽度会变成400px，且有一个时间为500ms的过渡效果，当鼠标离开后，div的宽度会以相同的动画效果恢复至100px。

例子二：

css:

    #div {
      width: 100px;
      height: 100px;
      background-color: red;
      transition: width 1s;
    }

js:

    var div = document.getElementById('div');
    div.onclick = function () {
      div.style.width = '400px';
    }

上面的代码，点击div后，div的宽度会变成400px，且有一个时间为1s的过渡效果，宽度400px会停留在最终效果，不会恢复。

transition有四个属性，分别是：

* transition-property：设置过渡的css属性
* transition-duration：设置过渡持续的时间
* transition-timing-function：设置速度效果
* transition-delay：设置延迟执行

可以用transition简写：

    transition: property duration timing-function delay;