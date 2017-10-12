# call、apply、bind方法的区别

函数中有一个this对象，在不同的情况下this的指向会不一样，call、apply与bind方法都可以改变函数中this的指向。

    var scope = 'window';

    var obj = {
      scope: 'obj'
    }

    function func(name, msg) {
      console.log(this.scope + ' - ' + name + ' : ' + msg);
    }

    func('tom', 'hi'); // window - tom : hi

    func.call(obj, 'rose', 'hello'); // obj - rose : hello

    func.apply(obj, [ '小明', '你好' ]); // obj - 小明 : 你好

    func.bind(obj, '小华', '啦啦')(); // obj - 小华 : 啦啦

由上面代码可知，call()与apply()的用法几乎一样，唯一不同的是形参的传入方式，call()传入的是不定参数，apply()传入的是数组。而bind()的返回值是一个新的function，需要在后面加括号调用这个function才能生效。

apply()的妙用：取出数组中最大的元素。

Math.max()可以取出最大的一个形参：

    var max = Math.max(1, 3, 5, 7, 9);
    console.log(max); // 9

它的参数传入的是不定参数，不能传入数组，所以不能取出数组中最大的元素：

    var arr = [ 1, 3, 5, 7, 9 ]
    var max = Math.max(arr);
    console.log(max); // NaN

但配合apply()使用就可以实现此功能：

    var arr = [ 1, 3, 5, 7, 9 ]
    var max = Math.max.apply(Math, arr);
    console.log(max); // 9