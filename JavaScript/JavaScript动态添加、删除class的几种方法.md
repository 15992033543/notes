# JavaScript动态添加、删除class的几种方法

## setAttribute()与removeAttribute()

使用setAttribute('class', 'className')可以为dom添加class，多个class可以用空格隔开；使用removeAttribute('class')可以删除dom所有的class。

html:

    <p id="hello" class="hello">Hello</p>

js:

    var hello = document.getElementById('hello');

    // 添加class1、class2
    hello.setAttribute('class', 'class1 class2');

    // 删除class属性
    hello.removeAttribute('class');

setAttribute()方法相当给dom设置了一个新的class属性，所以会覆盖之前的class；removeAttribute()则把整个class属性都删除了；所以这两个方法具有局限性，并不灵活。

## classList

html5中提供了classList用于管理dom的样式，它有以下几个api:

* add(class1, class2, ...)：为指定元素添加一个或多个class
* contains(class)：判断元素中是否拥有指定class
* item(index)：通过索引获取class，若无返回null
* remove(class1, class2, ...)：删除元素中一个或多个class
* toggle(class, ture|false)：dom存在class时，则移除，不存在时，则新增

由于classList为新特性，在ie中不完全支持，慎用。

## className属性

dom提供了一个className属性，可以获取dom所有的class，这个方法兼容所有浏览器。

html:

    <p id="hello" class="class1 class2">Hello</p>

js:

    var hello = document.getElementById('hello');

    // class1 class2
    hello.className;

设置className属性，可以为dom动态添加class。

    var hello = document.getElementById('hello');

    // 添加class3
    hello.className = hello.className + ' class3';

    // 删除class3
    hello.className = hello.className.replace('class3', '').trim();

可以将几个常用的操作封装成方法，方便使用：

    // 是否存在class
    HTMLElement.prototype.hasClass = function (name) {
      return this.className.indexOf(name) !== -1;
    }

    // 添加class
    HTMLElement.prototype.addClass = function () {
      for (var i = 0; i < arguments.length; i++) {
        var name = arguments[i];
        if (typeof name === 'string' && !this.hasClass(name)) {
          this.className = this.className + ' ' + name;
        }
      }
    }

    // 删除class
    HTMLElement.prototype.removeClass = function () {
      for (var i = 0; i < arguments.length; i++) {
        var name = arguments[i];
        if (typeof name === 'string' && this.hasClass(name)) {
          var className = this.className.split(' ');
          for (var j = 0; j < className.length; j++) {
            if (className[j] === name) {
              className.splice(j, 1);
            }
          }
          this.className = className.join(' ');
        }
      }
    }

    // 调用
    var hello = document.getElementById('hello');
    hello.addClass('a', 'b', 'c', 'd');
    hello.removeClass('a', 'b');

## 使用jQuery

    $('#hello').addClass('a b c d');
    $('#hello').removeClass('a b');
    $('#hello').hasClass('c');