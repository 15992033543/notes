# reduce()方法详解

reduce()方法可用作数组累加，也可用作数组中元素的比较，该方法接收一个回调函数，回调函数中接收四个形参，分别是：

|    参数   |   描述   |
| ------------ | ------------------------------------------- |
| accumulator  | 必选，累加器，callback的返回值，首次执行为数组第一个元素 |
| currentValue | 必选，当前值                                       |
| index        | 可选，当前循环的index                               |
| array        | 可选，当前数组                             |

## 例子

例子一：对数组的值进行累加

    var nums = [1, 2, 3, 4];

    var total = nums.reduce(function (accumulator, currentValue) {
      return accumulator + currentValue;
    });

    console.log(total); // 10

例子二：比较数组中的元素

    // 获取数组中age最小的人
    var min = persons.reduce(function (accumulator, currentValue) {
      return accumulator.age > currentValue.age ? currentValue : accumulator;
    });

    console.log(min.name); // rose

    // 获取数组中age最大的人
    var max = persons.reduce(function (accumulator, currentValue) {
      return accumulator.age > currentValue.age ? accumulator : currentValue;
    })

    console.log(max.name); // tom

注意：

* 空数组调用reduce方法会报错
* 如果数组中只有一个元素，不会走callback，会直接返回数组中第一个元素

## 模拟reduce

先取出第一个和第二个元素，执行一次callback，将返回值作为新的accumulator；遍历剩下的元素，执行callback，并将返回值作为新的accumulator即可

    Array.prototype.myReduce = function (callback) {
      var accumulator = this.shift();
      var currentValue = this.shift();
      accumulator = callback(accumulator, currentValue);
      for (var i = 0; i < this.length; i++) {
        accumulator = callback(accumulator, this[i]);
      }
      return accumulator;
    }

    var min = persons.myReduce(function (accumulator, currentValue) {
      return accumulator.age > currentValue.age ? currentValue : accumulator;
    });

    console.log(min);