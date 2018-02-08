# css的优先级

优先级规则为：内联 > id > class > tag

权重可分为：内联 = 1000，id = 100，class = 10，tag = 1

css:

    #foo {
      color: red;
    }

    .bar {
      color: blue;
    }

    p {
      color: yellow;
    }

html:

    <!-- red -->
    <p id="foo" class="bar">test</p>

    <!-- green -->
    <p class="bar" style="color: green">test</p>

如果权重相等，则后者合并前者。

css:

    #foo {
      font-size: 30px;
      color: red;
    }

    #foo {
      color: green;
    }

html:

    <!-- green, 30px -->
    <p id="foo" class="bar">test</p>

如果拥有多个选择器，则将权重相加。

css:

    #foo {
      color: red;
    }

    #foo.bar {
      color: yellow;
    }

html:

    <!-- yellow -->
    <p id="foo" class="bar">test</p>

如果设置了!important，则权重升至最高（大于内联）。

css:

    #foo {
      color: red;
    }

    p {
      color: yellow !important;
    }

html:

    <!-- yellow -->
    <p id="foo" style="color: green">test</p>