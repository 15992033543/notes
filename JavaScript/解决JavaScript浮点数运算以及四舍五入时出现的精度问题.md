# 解决JavaScript浮点数运算以及四舍五入时出现的精度问题

## 浮点数运算时精度缺失

原因：JS在运算时会先将数值转为二进制，再进行运算，最后转回十进制。由于浮点数在转二进制时有可能会变为无限循环小数，计算机会对其进行截断，截断后计算的结果再转回十进制后就会有机率出现精度缺失。

解决思路：将浮点数转成整数后再进行运算。浮点数转整数时，应该直接将小数点去掉（不要使用乘10、乘100的方式转换，涉及小数的运算就有可能出现精度缺失），比较两个数的小数部分长度进行补0，最后除以10<sup>小数位数</sup>，转换成小数。

    // 处理程序
    var numHandler = function (num1, num2) {
      var n1, n2, l1, l2;
      try { l1 = num1.toString().split('.')[1].length; } catch (error) { l1 = 0; }
      try { l2 = num2.toString().split('.')[1].length; } catch (error) { l2 = 0; }
      var differ = l1 - l2;
      n1 = Number(num1.toString().replace('.', ''));
      n2 = Number(num2.toString().replace('.', ''));
      n1 = differ > 0 ? n1 : n1 * Math.pow(10, Math.abs(differ));
      n2 = differ < 0 ? n2 : n2 * Math.pow(10, Math.abs(differ));
      return { n1: n1, n2: n2, l1: l1, l2: l2 };
    }

    // 加
    var accAdd = function (num1, num2) {
      var o = numHandler(num1, num2);
      return (o.n1 + o.n2) / Math.pow(10, Math.max(o.l1, o.l2));
    }

    // 减
    var accSub = function (num1, num2) {
      var o = numHandler(num1, num2);
      return (o.n1 - o.n2) / Math.pow(10, Math.max(o.l1, o.l2));
    }

    // 乘
    var accMul = function (num1, num2) {
      var n1, n2, l1, l2;
      try { l1 = num1.toString().split('.')[1].length; } catch (error) { l1 = 0; }
      try { l2 = num2.toString().split('.')[1].length; } catch (error) { l2 = 0; }
      n1 = Number(num1.toString().replace('.', ''));
      n2 = Number(num2.toString().replace('.', ''));
      return (n1 * n2) / Math.pow(10, Math.max(l1 + l2));
    }

    // 除
    var accDiv = function (num1, num2) {
      var o = numHandler(num1, num2);
      return o.n1 / o.n2;
    }

## 使用toFixed()不能获取到正确值

JS提供了toFixed()方法用于保留N位小数并四舍五入，但这个方法有时会失灵，比如：

    4999.995.toFixed(2); // 4999.99

所以如果在项目中对数值精度的要求较高，不建议使用toFixed()，自己写程序解决。

解决思路：以保留两位小数为例，根据小数点将数值拆分成整数部分left与小数部分right，截取小数部分前两位n1以及第三位n2，判断：n1 = n2 >= 5 ? n1 + 1 : n1，最后将拼接left + '.' + n1。

    // 四舍五入
    var accRound = function (value, num) {
      if (value === null || value.toString().trim() === '' || isNaN(value)) {
        throw new Error('the parameter is not a number!');
      }
      num = isNaN(num) ? 2 : Number(num);
      if (num < 0) {
        throw new Error('the num can not be less than 0!');
      }
      var left, right, n1, n2;
      var data = value.toString().split('.');
      left = data[0];
      right = data[1];
      if (right) {
        n1 = right.substring(0, num);
        n2 = right.substring(num, num + 1);
        if (n2 && Number(n2) >= 5) {
          var length = n1.length;
          var newLength = (Number(n1) + 1).toString().length;
          if (newLength > length) {
            var sign = Number(value) < 0 ? '-' : '';
            left = sign + (Math.abs(Number(left)) + 1);
            n1 = new Array(num + 1).join('0');
          } else {
            n1 = new Array(length - newLength + 1).join('0') + (Number(n1) + 1);
          }
        } else {
          n1 += new Array(num - n1.length + 1).join('0');
        }
      } else {
        n1 = new Array(num + 1).join('0');
      }
      return num > 0 ? left + '.' + n1 : left;
    }