# 获取dom的宽度、高度、位置

## 宽度与高度

dom提供了三个属性获取到宽度与高度，分别是：

* clientWidth（clientHeight）：可以获取到dom的实际宽度（高度），clientWidth = padding + content（注意滚动条会影响content的值）
* offsetWidth（offsetHeight）：可以获取到dom的实际宽度（高度），offsetWidth = border + padding + content
* scrollWidth（scrollHeight）：如果dom设置了固定的宽度（高度），且它没有子元素或者它的子元素的宽（高）并没有超出它的宽（高），那么scrollWidth = padding + content；如果它的子元素的（宽）高超出它的宽（高），那么scrollWidth = padding + 子元素的margin + 子元素的offsetWidth

css:

    .parent {
      border: 10px solid #CCC;
      width: 200px;
      height: 200px;
      padding: 10px;
      overflow: auto;
    }
    .child {
      width: 400px;
      height: 400px;
    }

html:

    <div class="parent">
      <div class="child">child</div>
    </div>

js:

    var parent = document.querySelector('.parent')
    // 203 / 203，因为parent出现了滚动条，滚动条的宽高（17px）占用了content的内容，所以10 + 10 + 200 - 17 = 203
    console.log(parent.clientWidth, parent.clientHeight)
    // 240 / 240
    console.log(parent.offsetWidth, parent.offsetHeight)
    // 410 / 420
    console.log(parent.scrollWidth, parent.scrollHeight)