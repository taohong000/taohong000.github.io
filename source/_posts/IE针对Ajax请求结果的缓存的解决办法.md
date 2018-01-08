---
title: IE针对Ajax请求结果的缓存的解决办法
date: 2017-11-08 14:26:24
tags:
- 浏览器
- ie
- ajax
---

# IE针对Ajax请求结果的缓存的解决办法 
 
> 本文转载自：https://www.cnblogs.com/artech/archive/2013/01/03/cache-4-ie.html

在默认情况下，IE会针对请求地址缓存Ajax请求的结果。换句话说，在缓存过期之前，针对相同地址发起的多个Ajax请求，只有第一次会真正发送到服务端。在某些情况下，这种默认的缓存机制并不是我们希望的（比如获取实时数据），这篇文章就来简单地讨论这个问题，以及介绍几种解决方案。

## 通过为URL地址添加后缀的方式解决问题 
一般在请求url后添加时间戳

## 通过JQuery的Ajax设置解决问题
jQuery具有针对这个的Ajax设置，我们只需要按照如下的方式调用$.ajaxSetup方法禁止掉Ajaz的缓存机制。
``` html
<!DOCTYPE html>
<html>

<head>
    <script type="text/javascript">
    $(function() {
        $.ajaxSetup({ cache: false });
        window.setInterval(function() {
            $.ajax({
                url: '@Url.Action("GetCurrentTime")',
                success: function(result) {
                    $("ul").append("<li>" + result + "</li>");
                }
            });
        }, 5000);
    });
    </script>
</head>

</html>
```
实际上jQuery的这个机制也是通过为请求地址添加不同的查询字符串后缀来实现的

## 通过定制响应解决问题
>HTTP/1.1 200 OK
>Server: ASP.NET Development Server/10.0.0.0
>Date: Thu, 03 Jan 2013 12:54:56 GMT
>X-AspNet-Version: 4.0.30319
>X-AspNetMvc-Version: 4.0
>**Cache-Control: no-cache**
>Pragma: no-cache
>Expires: -1
>Content-Type: text/html; charset=utf-8
>Content-Length: 10
>Connection: Close