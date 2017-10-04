# css的优先级

优先级规则为：内联 > id选择器 > class选择器 > tag选择器

权重可分为：内联 = 1000，id选择器 = 100，class选择器 = 10，tag选择器 = 1

    #foo {
      color: red;
    }

    .bar {
      color: blue;
    }

    p {
      color: yellow;
    }

    <!-- red -->
    <p id="foo" class="bar">test</p>

    <!-- green -->
    <p class="bar" style="color: green">test</p>

如果权重相等，则采用后来定义的，并合并之前定义的样式。

    #foo {
      font-size: 30px;
      color: red;
    }

    #foo {
      color: green;
    }

    <!-- green, 30px -->
    <p id="foo" class="bar">test</p>

如果拥有多个选择器，则将权重相加取最大值。

    #foo {
      color: red;
    }

    #foo.bar {
      color: yellow;
    }

    <!-- yellow -->
    <p id="foo" class="bar">test</p>

如果设置了!important，则权重升至最高（大于内联）。

    #foo {
      color: red;
    }

    p {
      color: yellow !important;
    }

    <!-- yellow -->
    <p id="foo" style="color: green">test</p>