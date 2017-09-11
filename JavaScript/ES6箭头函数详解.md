# ES6中箭头函数详解

ES6中新增了一个声明函数的语法糖，箭头函数，它的出现大大简化了函数的声明。

具体用法如下：

    var foo = msg => {
      console.log(msg);
    }
    foo('hello');

箭头前为函数的参数，箭头后为函数体。

上面写法相当于：

    var foo = function(msg) {
      console.log(msg);
    }
    foo('hello');

又或者：

    function foo(msg) {
      console.log(msg);
    }
    foo('hello');

## 形参问题

如果函数没有参数或者需要多个参数，需要加了小括号。

    // 无参
    var foo = () => {
      console.log('hello');
    }
    // 多参
    var foo = (name, msg) => {
      console.log(name + 'say' + msg);
    }

## 函数体问题

箭头后的函数体可以不加大括号，不加大括号时，方法默认会将结果return。

    var foo = num => num + 1;
    var result = foo(1);
    console.log(result); // 2

函数体可以嵌套，如：

    var foo = () => () => console.log('hello');
    foo()(); // hello

## 箭头函数中this的指向

详情请看另一篇文章，《JavaScript中this的指向》