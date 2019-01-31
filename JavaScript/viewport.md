# viewport

在大屏触屏手机普及之后，越来越多人使用手机来浏览网页，但是当时的网页大多数是针对电脑端开发的（当时手机分辨率比电脑分辨率低），所以用手机浏览网页时，需要用手指拖动才能看到更多内容，而一些采用了流性布局的页面，比如一个div设置了20%的width，用电脑浏览器体验很好，但在手机上浏览时检验就很糟糕了，所以为了解决这个问题，苹果的safari率先推出了viewport技术。

viewport其实是一个虚拟的窗口，用于在手机浏览器中模拟电脑端分辨率，如果不设置，默认为980（大多数浏览器的默认值）。

这样用手机浏览电脑网页时，整个页面会全部展现出来，但是很小，用户可以用手指放大缩小，体验会好一点。但随着大屏手机的发展，出现了很多适配手机的网页，如果viewport的宽度为980的话，那么就手机就不适配这个页面了，所以可以指定viewport的宽度，width=device-width就是用于适配手机端。

content中的参数说明：

* width：viewport的宽，一般默认为980，可以指定一个值（如400），device-width为当前设备的分辨率除以dpr的值。

* height：viewport的高度。

* initial-scale：viewport缩放比例，默认1。

* maximum-scale：允许用户双指缩放到的最大比例。

* minimum-scale：允许用户双指缩放到的最小比例。

* user-scalable：用户是否可以手动缩放。

一般适配手机端可以设置为：

    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,minimum-scale=1,user-scalable=no"/>