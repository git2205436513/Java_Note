# Ajax
```
2020/3/25 fanfuni
在此感谢狂神说java老师
```
## 什么是Ajax
Asynchronous Javascript and XML(异步的JavaScript 和 xml)

Ajax是一种无需重新加载整个网页的情况下，能够更新部分网页的技术。

Ajax不是一种新的编程语言，二是一种用于创建更好更快以及交互性更强的Web应用程序技术

增强B/S体验性

## Ajax核心
Ajax核心是XMLHttpRequest对象（XHR）。XHR向服务器发送请求和解析服务器响应提供了接口。能够以异步方式从服务器获取新数据。

## Ajax入门

我们首先通过iframe标签来仿造Ajax
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script type="text/javascript">

    window.onload = function () {
        var myDate = new Date();
        document.getElementById('Time').innerText = myDate.toUTCString();
    }

    function loadPage() {
        var targetURL = document.getElementById('url').value;
        console.log(targetURL);
        document.getElementById('iframePosition').src = targetURL;

    }

</script>

<div>
    <p>当前的时间是：<span id = "Time"></span></p>
    <p>请输入要加载的地址：</p>
    <p>
        <input type="text" id="url" value="">
        <input type="button" value="提交" onclick="loadPage()">
    </p>
</div>

<div>
    <h3>
        加载页面位置:
    </h3>
    <iframe style="width:1000px;height:500px" id="iframePosition">
    </iframe>
</div>
</body>
</html>
```
接着我们使用Jquery来实现AJax

## Ajax总结
1. 编写对应处理Controller，返回消息或者自负床或者json格式数据
2. 编写ajax请求：
- url：Controller请求
- data：键值对
- success：回调函数
3. 给Ajax绑定事件，点击click，失去焦点onblur=，键盘谈起keyup