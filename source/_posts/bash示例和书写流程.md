---
title: bash示例和书写流程
date: 2017-07-14 12:56:42
tags: 
- bash
- Linux
---

# bash示例和书写流程

##  新建文件test.sh
```bash
touch test.sh
```

## 添加可执行权限
```bash
$ chmod +x test.sh
```

## 编辑test.sh，test.sh内容如下：
```bash
#!/bin/bash

echo "hello bash"

exit 0
```

说明：

- \#!/bin/bash : 它是bash文件声明语句，表示是以/bin/bash程序执行该文件。它必须写在文件的第一行！
- echo "hello bash" : 表示在终端输出“hello bash”
- exit 0 : 表示返回0。在bash中，0表示执行成功，其他表示失败。

## 执行bash脚本
```bash
./test.sh
#或者
sh test.sh
bash test.sh
```
在终端输出“bash hello”

参考：
[Linux bash总结](http://www.cnblogs.com/skywang12345/archive/2013/05/30/3106570.html)