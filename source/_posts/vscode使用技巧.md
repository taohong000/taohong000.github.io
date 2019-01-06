---
title: vscode使用技巧
categories:
  - 技术
date: 2019-01-06 23:15:56
tags:
---

## 跨文件查找
在查询框下面的两个输入框，你可以选择包含和排除的文件。单击右侧图标来启用glob匹配模式语法：
- \* 匹配路径段中的一个或多个字符
- ? 匹配路径段中的一个字符
- ** 匹配任意数量的路径段，包括空
- {} 匹配分组条件（如： {**/*.html,**/*.txt} 匹配所有HTML和txt文件）
- [] 声明匹配一个范围内的所有字符（如：example.[0-9]匹配 example.0, example.1, …）

VS Code 默认会排除一些你不会感兴趣的文件夹（如： node_modules）来减少查询结果的数量，在设置文件中的files.exclude 和 search.exclude小节下可以修改这个规则。

> 小技巧： 可以在文件管理器中右键一个文件夹并选择 Find in Folder 来仅在选择文件夹下搜索。

## 常用快捷键

[官方快捷键大全](https://code.visualstudio.com/docs/customization/keybindings)

### 主命令框
最重要的功能就是F1或Ctrl+Shift+P打开的命令面板了，在这个命令框里可以执行VSCode的任何一条命令，甚至关闭这个编辑器。
按一下Backspace会进入到Ctrl+P模式里
在Ctrl+P下输入>又可以回到Ctrl+Shift+P模式。
在Ctrl+P窗口下还可以

直接输入文件名，跳转到文件
- ? 列出当前可执行的动作
- ! 显示Errors或Warnings，也可以`Ctrl+Shift+M
- : 跳转到行数，也可以Ctrl+G直接进入
- @ 跳转到symbol（搜索变量或者函数），也可以Ctrl+Shift+O直接进入
- @:根据分类跳转symbol，查找属性或函数，也可以Ctrl+Shift+O后输入:进入
- \# 根据名字查找symbol，也可以Ctrl+T

### 格式调整

- 代码行缩进Ctrl+[ Ctrl+]
- Ctrl+C Ctrl+V如果不选中，默认复制或剪切一整行
- 代码格式化：Shift+Alt+F，或Ctrl+Shift+P后输入format code
- 上下移动一行： Alt+Up 或 Alt+Down
- 向上向下复制一行： Shift+Alt+Up或Shift+Alt+Down
- 在当前行下边插入一行Ctrl+Enter
- 在当前行上方插入一行Ctrl+Shift+Enter

### 光标相关

- 移动到行首：Home
- 移动到行尾：End
- 移动到文件结尾：Ctrl+End
- 移动到文件开头：Ctrl+Home
- 移动到定义处：F12
- 定义处缩略图：只看一眼而不跳转过去Alt+F12
- 移动到后半个括号 Ctrl+Shift+]
- 选择从光标到行尾Shift+End
- 选择从行首到光标处Shift+Home
- 删除光标右侧的所有字Ctrl+Delete
- Shrink/expand selection： Shift+Alt+Left和Shift+Alt+Right
- Multi-Cursor：可以连续选择多处，然后一起修改，Alt+Click添加cursor或者Ctrl+Alt+Down 或 Ctrl+Alt+Up
- 同时选中所有匹配的Ctrl+Shift+L
- Ctrl+D下一个匹配的也被选中(被我自定义成删除当前行了，见下边Ctrl+Shift+K)
- 回退上一个光标操作Ctrl+U

### 重构代码

- 找到所有的引用：Shift+F12
- 同时修改本文件中所有匹配的：Ctrl+F12
- 重命名：比如要修改一个方法名，可以选中后按F2，输入新的名字，回车，会发现所有的文件都修改过了。
- 跳转到下一个Error或Warning：当有多个错误时可以按F8逐个跳转
- 查看diff 在explorer里选择文件右键 Set file to compare，然后需要对比的文件上右键选择Compare with 'file_name_you_chose'.

### 查找替换

- 查找 Ctrl+F
- 查找替换 Ctrl+H
- 整个文件夹中查找 Ctrl+Shift+F
- 匹配符：
  - \* to match one or more characters in a path segment
  - ? to match on one character in a path segment
  - ** to match any number of path segments ,including none
  - {} to group conditions (e.g. {\*\*/\*.html,\*\*/\*.txt} matches all html and txt files)
  - [] to declare a range of characters to match (e.g., example.[0-9] to match on example.0,example.1,…
  
### 显示相关

- 全屏：F11
- zoomIn/zoomOut：Ctrl + =/Ctrl + -
- 侧边栏显/隐：Ctrl+B
- 侧边栏4大功能显示：
  - Show Explorer Ctrl+Shift+E
  - Show SearchCtrl+Shift+F
  - Show GitCtrl+Shift+G
  - Show DebugCtrl+Shift+D
  - Show OutputCtrl+Shift+U






