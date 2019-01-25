# void 0代替undefined的好处

在JavaScript中，void后跟一个表达式，会执行这个表达式，并返回undefined

好处一：ES5前，某些浏览器允许重写undefined，重写后undefined就不等于undefined，在判断时就会出现不相等的情况，而void 0则会永远返回undefined

好处二：void 0比undefined少了三个字节
