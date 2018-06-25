---
title: 'DataURL与File,Blob,canvas对象之间的互相转换'
date: 2018-01-30 14:34:49
categories: 
- 技术
tags:
- js
- DataURL
- File
- Blob
- canvas
---
# DataURL与File,Blob,canvas对象之间的互相转换

## canvas转换为dataURL
``` javascript
var dataurl = canvas.toDataURL('image/png');
var dataurl2 = canvas.toDataURL('image/jpeg', 0.8);
```

## File对象、Blob对象转换为dataURL
File对象也是一个Blob对象，二者的处理相同。
``` javascript
function readBlobAsDataURL(blob, callback) {
    var a = new FileReader();
    a.onload = function(e) {callback(e.target.result);};
    a.readAsDataURL(blob);
}
//example:
readBlobAsDataURL(blob, function (dataurl){
    console.log(dataurl);
});
readBlobAsDataURL(file, function (dataurl){
    console.log(dataurl);
});
```

## dataURL转换为Blob对象
``` javascript
function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {type:mime});
}
//test:
var blob = dataURLtoBlob('data:text/plain;base64,YWFhYWFhYQ==');
```

## dataURL图片数据绘制到canvas
先构造Image对象，src为dataURL，图片onload之后绘制到canvas
``` javascript
var img = new Image();
img.onload = function(){
    canvas.drawImage(img);
};
img.src = dataurl;
```

File,Blob的图片文件数据绘制到canvas
还是先转换成一个url，然后构造Image对象，src为dataURL，图片onload之后绘制到canvas

利用上面的 readBlobAsDataURL 函数，由File,Blob对象得到dataURL格式的url，再参考 dataURL图片数据绘制到canvas

``` javascript
readBlobAsDataURL(file, function (dataurl){
    var img = new Image();
    img.onload = function(){
        canvas.drawImage(img);
    };
    img.src = dataurl;
});
```

## Canvas转换为Blob
``` javascript
var dataurl = canvas.toDataURL('image/png');
var blob = dataURLtoBlob(dataurl);
```

## Blob转换为File
``` javascript
function blobToFile(theBlob, fileName){
    //A Blob() is almost a File() - it's just missing the two properties below which we will add
    theBlob.lastModifiedDate = new Date();
    theBlob.name = fileName;
    return theBlob;
}

// Usage
var myBlob = new Blob();

//do stuff here to give the blob some data...

var myFile = blobToFile(myBlob, "my-image.png");
```

可以使用File构造器
``` javascript
var file = new File([myBlob], "name");
```

##dataUrl转换为File
如果想用Ajax传递，不需要使用File，使用Blob即可
``` javascript
function dataURLtoBlob(dataurl) {
    var arr = dataurl.split(','), mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]), n = bstr.length, u8arr = new Uint8Array(n);
    while(n--){
        u8arr[n] = bstr.charCodeAt(n);
    }
    return new Blob([u8arr], {type:mime});
}

var dataurl = 'data:text/plain;base64,aGVsbG8gd29ybGQ=';
var blob = dataURLtoBlob(dataurl);
var fd = new FormData();
fd.append("file", blob, "hello.txt");
var xhr = new XMLHttpRequest();
xhr.open('POST', '/server.php', true);
xhr.onload = function(){
    alert('upload complete');
};
xhr.send(fd);
```

可以使用fetch将url转化为File
代码很简洁（可在chrome和firefox中使用）
``` javascript
//load src and convert to a File instance object
//work for any type of src, not only image src.
//return a promise that resolves with a File instance

function srcToFile(src, fileName, mimeType){
    return (fetch(src)
        .then(function(res){return res.arrayBuffer();})
        .then(function(buf){return new File([buf], fileName, {type:mimeType});})
    );
}
```
例子1：转化为File对象
``` javascript
srcToFile(
    'data:text/plain;base64,aGVsbG8gd29ybGQ=',
    'hello.txt',
    'text/plain'
)
.then(function(file){
    console.log(file);
})
```

例子2：转化为File对象并发送到服务器
``` javascript
srcToFile(
    'data:text/plain;base64,aGVsbG8gd29ybGQ=',
    'hello.txt',
    'text/plain'
)
.then(function(file){
    console.log(file);
    var fd = new FormData();
    fd.append("file", file);
    return fetch('/server.php', {method:'POST', body:fd});
})
.then(function(res){
    return res.text();
})
.then(console.log)
.catch(console.error)
;
```
参考：
1. http://blog.csdn.net/cuixiping/article/details/45932793
2. https://stackoverflow.com/questions/6850276/how-to-convert-dataurl-to-file-object-in-javascript
3. https://stackoverflow.com/questions/27159179/how-to-convert-blob-to-file-in-javascript
