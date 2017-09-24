# JavaScript中this的指向

之前一直不明白JavaScript中this的指向，觉得很容易混淆，今天研究了一下，豁然开朗，分享一下。

## 全局变量this

在browser环境中，this指向的是window对象。

    console.log(this); // window

在node环境中，this指向的不是global对象，它指向一个空对象，而这个空对象其实是module.exports。

    console.log(this); // {}
    console.log(this === module.exports); // true

## 函数中的this

一个函数，其this指向全局对象。

    function foo() {
      console.log(this);
    }

    foo(); // browser指向window，node指向global

如果使用了严格模式，this都指向undefined。

    'use strict';

    function foo() {
      console.log(this);
    }

    foo(); // browser和node都指向undefined

我们可以使用call()或者apply()来改变函数运行时this的指向。

    function foo() {
      console.log(this.name)
    }

    foo.call({ name: 'obj' }); // obj

由于函数的this指向全局对象，所以如果函数中嵌套一个函数，就算在调用时改变了外层函数this的指向，但被嵌套函数的this也会重新指向全局对象。这其实是JavaScript中的一个设计缺陷，正常来讲，被嵌套函数的this应该指向其上一级函数的this。

    var name = 'global';

    function foo() {
      console.log(this.name); // obj

      function bar() {
        console.log(this.name); // global
      }

      bar();
    }

    foo.call({ name: 'obj' });

可以这样来解决：

    var name = 'global';

    function foo() {
      console.log(this.name); // obj

      var that = this;

      function bar() {
        console.log(that.name); // obj
      }

      bar();
    }

    foo.call({ name: 'obj' });

## 构造函数中的this

如果在函数前加new，那么this指向这个构造函数实例。

    function Foo() {
      console.log(this);
    }

    new Foo(); // Foo() {}

## 对像方法中的this

谁调用这个方法，this就指向谁。

    var name = 'global';

    var o1 = {
      name: 'o1',
      foo: function() {
        console.log(this.name);
      }
    }

    var o2 = {
      name: 'o2',
      foo: function() {
        console.log(this.name);
      }
    }

    o1.foo(); // o1

    o2.foo(); // o2

    var f = o1.foo;
    f(); // global

    o1.foo.call(o2); // o2

使用一个变量接收方法后再调用，相当于调用了一个函数，根据JavaScript的设计，函数的this指向全局对象，所以输出global。（node环境会输出undefined）

## ES6箭头函数的this

箭头函数是ES6的新特性，它的作用是：

* 语法糖，简化了函数的声明
* 修复了JavaScript函数中this的指向问题

使用传统的方式声明函数，函数中的this指向全局变量，在嵌套函数的情况下，这个特性就比较糟糕了，而ES6新出现的箭头函数修正了这个问题。

箭头函数中并没有自己的this，它的this指向其所在作用域中的this，在编写代码时就能体验出来：**声明时就已经确定指向，在哪个作用域中声明，就指向那个作用域中的this，且运行时不能改变其指向。**

    var name = 'global';

    var foo = () => {
      console.log(this.name);
    }

    foo(); // global

    foo.call({ name: 'obj' }); // global

可以看到，foo是在全局作用域中声明的，全局作用域中this指向window对象，所以foo()的this也指向window，它的指向是声明时就已经确定，所以使用call()不能改变其this。

另一个例子：

    var name = 'global';

    function foo() {
      var bar = () => {
        console.log(this.name);
      }

      bar();
    }

    foo.call({ name: 'obj' }); // obj

foo()函数是声明式函数，可以使用call()改变其this的指向，而bar是箭头函数，它在foo()函数的作用域中声明，所以其this的指向为foo()中this的指向，最终输出obj。

由于箭头函数没有自身的this，所以它不能用作构造函数。

    var Foo = () => {
      this.name = 'foo';
    }

    new Foo(); // TypeError: Foo is not a constructor