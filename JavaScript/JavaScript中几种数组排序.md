# JavaScript中几种数组排序

## 冒泡排序

冒泡循环的原理是取出第一个元素与第二个元素对比，如果第一个元素大于第二个元素，交换位置，否则不变；之后取出第二个元素与第三个元素作对比，如果第二个元素大于第三个元素，交换位置，否则不变，以此类推。

    function bubbleSort (arr) {
      let length = arr.length;
      for (let i = 0; i < length; i++) {
        for (let j = 0; j < length - 1 - i; j++) {
          if (arr[j] > arr[j + 1]) {
            let temp = arr[j];
            arr[j] = arr[j + 1];
            arr[j + 1] = temp;
          }
        }
      }
    }

    let arr = [22, 7, 3, 2, 1];
    bubbleSort(arr);
    console.log(arr);

一次fori循环后，数组中最大的数移动到了最后面。

五次fori循环执行完毕后数组变化：

1. 第一次：[ 7, 3, 2, 1, 22 ]
1. 第二次：[ 3, 2, 1, 7, 22 ]
1. 第三次：[ 2, 1, 3, 7, 22 ]
1. 第四次：[ 1, 2, 3, 7, 22 ]
1. 第五次：[ 1, 2, 3, 7, 22 ]

forj循环里有两个优化。

第一个优化是length - 1，当倒数第二个元素与最后一个比较完后，就代表所有数都比较完了，所以没有必要再拿最后一个数与其后一个元素比较（这里也防止了异常的发生，因为已经越界了，虽然在JS中不会报错，但有些语言如Java就会报越界异常），一次fori循环可以减少一次forj循环。

第二个优化是length - i，由于一次fori循环执行完后，最大的数移动到了最后，所以执行第二次fori循环时，已经不需要再对这个数进行比较了，对其进行排除，这样在本次fori循环中，forj循环的执行次数都会比上一次的fori循环少一次。

## 选择排序

选择排序的原理是取出第一个元素与第二个元素比较，如果第一个元素大于第二个元素，交互位置，否则不变；之后取出第一个元素与第三个元素比较，如果第一个元素大于第三个元素，交互位置，否则不变，以此类推。

    function selectSort (arr) {
      let length = arr.length;
      for (let i = 0; i < length - 1; i++) {
        for (let j = i + 1; j < length; j++) {
          if (arr[i] > arr[j]) {
            let temp = arr[j];
            arr[j] = arr[i];
            arr[i] = temp;
          }
        }
      }
    }

    let arr = [22, 7, 3, 2, 1];
    selectSort(arr);
    console.log(arr);

一次fori循环后，数组中最小的数移动到了最前面。

四次fori循环执行完毕后数组变化：

1. 第一次：[ 1, 22, 7, 3, 2 ]
1. 第二次：[ 1, 2, 22, 7, 3 ]
1. 第三次：[ 1, 2, 3, 22, 7 ]
1. 第四次：[ 1, 2, 3, 7, 22 ]

forj循环中的j = i + 1是为了一开始排除掉第一个元素，以后排除掉已经比较过的元素，这样做在的好处是本次fori循环中，forj循环的执行次数都会比上一次的fori循环少一次。

fori循环中length - 1，是因为执行完第length - 1次fori循环后，数组中的所有元素已经比较完了，所以没有必要再进行下去了（这里也防止了异常的发生，因为执行forj循环时，数组已经越界了，虽然在JS中不会报错，但有些语言如Java就会报越界异常）

## 快速排序

快速排序的原理是先获取数组的基准点并从数组中删除（一般是数组正中间），然后将数组中小于基准点的元素放到左边，大于基准点的数放到右边。不断递归，直至数组长度为1为止。

    function quickSort (arr) {
      let length = arr.length;
      // 当数组长度为1时，返回此数组
      if (length < 2) {
        return arr;
      }
      // 获取基准点的角标，向下取整
      let midIndex = Math.floor(length / 2);
      // 从数组中删除基准点并获取（splice()的返回值为被删除的那个元素）
      let midValue = arr.splice(midIndex, 1);
      // 创建两个数组，小于基准点的放左边，大于基准点的放右边
      let left = [];
      let right = [];
      // 因为已经删除了一个数，长度少了1
      for (let i = 0; i < length - 1; i++) {
        let e = arr[i];
        if (e < midValue) {
          left.push(e);
        } else {
          right.push(e);
        }
      }
      // 递归，最后将left，基准点，right合拼成新数组返回
      return quickSort(left).concat(midValue, quickSort(right));
    }

    let arr = [22, 7, 3, 2, 1];
    console.log(quickSort(arr));