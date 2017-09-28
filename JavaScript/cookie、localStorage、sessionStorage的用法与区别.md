# cookie、localStorage、sessionStorage的用法与区别

## cookie

cookie在本地的html文件下使用是无效的，需要部署到服务器上。

新增：

以键值对的方式来新增值，每次调用会先检测是否有此值，不存在则新增，存在则更新。

    var name = 'tom'
    document.cookie = 'name=' + name; // 新增
    name = 'jack';
    document.cookie = 'name=' + name; // 更新
    var gender = 'male';
    document.cookie = 'gender=' + gender; // 新增

cookie默认浏览器关闭后清除，如果需要设置更长的过期时间，在值后面紧跟expires即可。

    var exp = new Date();
    // 5秒后过期
    exp.setTime(exp.getTime() + 5000);
    document.cookie = 'name=tom;expires=' + exp.toGMTString();

新增值时，最好使用escape()编码，避免特殊字符，取值时可以使用unescape()解码。

    var name = 'tom';
    document.cookie = 'name=' + escape(name);

读取：

可以通过document.cookie来读取值，但这样会读取出所有cookie，它为一个字符串。

    var name = 'jack';
    var age = 12;
    document.cookie = 'name=' + escape(name);
    document.cookie = 'name=' + escape(age);
    var cookie = unescape(document.cookie);
    console.log(cookie); // name=jack; age=12
    console.log(typeof cookie); // string

可以通过正则取出想要的值。

    var reg = new RegExp('(^| )name=([^;]*)(;|$)');
    var arr = document.cookie.match(reg);
    console.log(arr[2]); // jack

删除：

将过期时间设置为当前日期即可删除cookie。

    document.cookie = 'name=jack';
    document.cookie = 'age=12';
    console.log(document.cookie); // gender=male; name=jack; age=12
    var exp = new Date();
    exp.setTime(exp.getTime());
    document.cookie = 'name=jack;expires=' + exp.toGMTString();
    console.log(document.cookie); // gender=male

## localStorage、sessionStorage

localStorage、sessionStorage是Html5新增的本地存储技术。

新增：

使用setItem()方法即可新增一个值，每次调用会先检测是否有此值，不存在则新增，存在则更新。

    window.localStorage.setItem('name', 'jack');

读取：

使用getItem()方法即可读取值。

    console.log(window.localStorage.getItem('name')); // jack

删除：

使用removeItem()或者clear()方法即可删除值，removeItem()删除单个值，clear()删除所有值。

    window.localStorage.removeItem('name');
    console.log(window.localStorage.getItem('name')); // jack

事件：

webStorage有一个监听值改变的事件，可以在不同的html文件中实现通讯，前提是同源。

a.html：

    // 注册事件
    window.addEventListener('storage', function(e) {
      console.log(e);
    });

b.html：

    window.localStorage.clear();
    window.localStorage.setItem('name', 'jack');

先运行a.html，再运行b.html，回到a.html，打开控制台，就能看到输出。

## 区别

* cookie的API比较麻烦（取值后需要自行处理）；localStorage与sessionStorage的API更易用
* cookie需要部署到服务器才能使用；localStorage与sessionStorage可以在本地html中使用
* cookie在请求时会被附加到请求头中；localStorage与sessionStorage则不会
* cookie最大能存储4kb左右的数据（因为请求会被附加到请求头，所以不适合用于存储大数据）；localStorage与sessionStorage则能存储更大的数据
* cookie有时间限制，过期后自动删除；localStorage会一直保存在本地，除非手动删除；sessionStorage在一个会话结束后自动删除
* cookie与localStorage可以在同源不同窗口中共享数据；sessionStorage只能在同一个会话中共享数据