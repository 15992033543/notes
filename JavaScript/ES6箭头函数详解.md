# ES6中箭头函数详解

ES6中新增了一个声明函数的语法糖——箭头函数，它的出现大大简化了函数的声明。

具体用法如下：

    var foo = msg => {
      console.log(msg);
    }

    foo('hello');

箭头前为函数的参数，箭头后为函数体。

上面写法相当于：

    var foo = function (msg) {
      console.log(msg);
    }

    foo('hello');

又或者：

    function foo (msg) {
      console.log(msg);
    }

    foo('hello');

## 形参

如果函数没有参数或者有多个参数，箭头前需要加小括号。

    // 无参
    var foo = () => {
      console.log('hello');
    }

    // 多参
    var foo = (name, msg) => {
      console.log(name + 'say' + msg);
    }

## 函数体

箭头后的函数体可以不加大括号，不加大括号时，方法默认会将结果return。

    var foo = num => num + 1;

    var result = foo(1);

    console.log(result); // 2

相当于：

    var foo = function (num) {
      return num + 1;
    }

    var result = foo(1);

    console.log(result); // 2

## 作为构造函数

箭头函数不能作为构造函数

    var Foo = () => {
      this.name = 'Foo';
    }

    var f = new Foo(); // TypeError: Foo is not a constructor

因为箭头函数中并没有this，它的this取的是离它最近的外层作用域的this，所以作为构造函数时会报错。

## 箭头函数中this的指向

由于this的指向涉及的内容比较多，所以另起了一篇文章，详情请看《JavaScript中this的指向》