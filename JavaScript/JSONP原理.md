# JSONP原理

JSONP是一个非官方协议，可以用于解决跨域请求数据的问题。

地址`http://open.iciba.com/dsapi`返回了json数据，如下：

    {
      "sid": "3030",
      "tts": "http:\/\/news.iciba.com\/admin\/tts\/2018-06-14-day.mp3",
      "content": "With great power there must come great responsibility.",
      "note": "有多大能力就会有多大责任。",
      "love": "1177",
      "translation": "词霸小编：投稿来自甘肃名族师范学院的学生东之伊。这句话出自电影《蜘蛛侠》。在一次抢劫中由于彼得的任性，导致他的叔叔被劫犯重伤。在临终前叔叔对彼得说了这句话，从那以后彼得开始行侠仗义，并把这句话当成了自己的座右铭。",
      "picture": "http:\/\/cdn.iciba.com\/news\/word\/20180614.jpg",
      "picture2": "http:\/\/cdn.iciba.com\/news\/word\/big_20180614b.jpg",
      "caption": "词霸每日一句",
      "dateline": "2018-06-14",
      "s_pv": "0",
      "sp_pv": "0",
      "tags": [{
        "id": null,
        "name": null
      }],
      "fenxiang_img": "http:\/\/cdn.iciba.com\/web\/news\/longweibo\/imag\/2018-06-14.jpg"
    }

直接使用script标签加载这个地址：

    <script src="http://open.iciba.com/dsapi"></script>

控制台报错：

    Uncaught SyntaxError: Unexpected token :

这是一个js的语法错误问题，由此可以得知使用script引入的资源会被浏览器当成js文件来处理，在解析时碰到了语法错误，所以报错。（可以将上述json内容复制到一个js文件中引入，会出现一样的问题）

在js代码中加入一个function，并在地址中将这个方法传给查询参数callback：

    <script>
      function getDataCallback (data) {
        console.log(data);
      }
    </script>

    <script src="http://open.iciba.com/dsapi?callback=getDataCallback"></script>

可以发现代码不报错了，而且可以正常输出返回值，查看响应体发现：

    getDataCallback({
      "sid": "3030",
      "tts": "http:\/\/news.iciba.com\/admin\/tts\/2018-06-14-day.mp3",
      "content": "With great power there must come great responsibility.",
      "note": "有多大能力就会有多大责任。",
      "love": "1177",
      "translation": "词霸小编：投稿来自甘肃名族师范学院的学生东之伊。这句话出自电影《蜘蛛侠》。在一次抢劫中由于彼得的任性，导致他的叔叔被劫犯重伤。在临终前叔叔对彼得说了这句话，从那以后彼得开始行侠仗义，并把这句话当成了自己的座右铭。",
      "picture": "http:\/\/cdn.iciba.com\/news\/word\/20180614.jpg",
      "picture2": "http:\/\/cdn.iciba.com\/news\/word\/big_20180614b.jpg",
      "caption": "词霸每日一句",
      "dateline": "2018-06-14",
      "s_pv": "0",
      "sp_pv": "0",
      "tags": [{
        "id": null,
        "name": null
      }],
      "fenxiang_img": "http:\/\/cdn.iciba.com\/web\/news\/longweibo\/imag\/2018-06-14.jpg"
    })

返回的响应调用了getDataCallback()方法，并将原本要返回的数据作为参数传入。

到这里就搞懂JSONP的原理了，JSONP实际上使用了script标签可以跨域访问非同源资源的特性，使用script标签请求资源时，只要被请求的资源能够返回可执行的javascript代码即可，这个过程需要后台配合（后台判断是否有查询参数callback，如果有则获取到callback的值，动态拼接一段调用此方法的js代码返回）。
