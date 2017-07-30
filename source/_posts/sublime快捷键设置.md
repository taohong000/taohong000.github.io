---
title: sublime快捷键设置
date: 2017-07-16 18:53:07
tags: sublime
---

# sublime快捷键设置

## 快捷键设计原则
1. 尽量不改变原有快捷键
2. 手指跨幅不要太大
3. 语义化表达
4. 相反操作用ctrl和ctrl+alt操作

## 快捷键配置
```json
[
	// 字母大小写
	{ "keys": ["ctrl+k", "ctrl+u"], "command": "title_case" },
	{ "keys": ["ctrl+u", "ctrl+u"], "command": "upper_case" },
	{ "keys": ["ctrl+k", "ctrl+l"], "command": "lower_case" },

	// 折叠代码操作
	{ "keys": ["ctrl+shift+["], "command": "fold" },
	{ "keys": ["ctrl+shift+]"], "command": "unfold" },
	{ "keys": ["ctrl+k", "ctrl+1"], "command": "fold_by_level", "args": {"level": 1} },
	{ "keys": ["ctrl+k", "ctrl+2"], "command": "fold_by_level", "args": {"level": 2} },
	{ "keys": ["ctrl+k", "ctrl+3"], "command": "fold_by_level", "args": {"level": 3} },
	{ "keys": ["ctrl+k", "ctrl+4"], "command": "fold_by_level", "args": {"level": 4} },
	{ "keys": ["ctrl+k", "ctrl+5"], "command": "fold_by_level", "args": {"level": 5} },
	{ "keys": ["ctrl+k", "ctrl+6"], "command": "fold_by_level", "args": {"level": 6} },
	{ "keys": ["ctrl+k", "ctrl+7"], "command": "fold_by_level", "args": {"level": 7} },
	{ "keys": ["ctrl+k", "ctrl+8"], "command": "fold_by_level", "args": {"level": 8} },
	{ "keys": ["ctrl+k", "ctrl+9"], "command": "fold_by_level", "args": {"level": 9} },
	{ "keys": ["ctrl+k", "ctrl+0"], "command": "unfold_all" },
	{ "keys": ["ctrl+alt+k", "ctrl+alt+0"], "command": "fold_all" },
	{ "keys": ["ctrl+k", "ctrl+j"], "command": "unfold_all" },
	{ "keys": ["ctrl+alt+k", "ctrl+alt+j"], "command": "fold_tag_attributes" },
]

```
## 快捷键
### 导航/跳转
ctrl+p:根据文件名快速打开文件
+@:跳转到所定义模块
+#:跳转到关键字
+:跳转到行

### tabs
ctrl+shift+t:打开最近关闭的tab
ctrl+pgUp:逆序显示tab
ctrl+pgUp:顺序序显示tab
ctrl+w:关闭tab

### 分屏
alt+shift+2
alt+shift+1

### 编辑
删除
ctrl+x:删除行
ctrl+kk:删除光标后所有内容
ctrl+j:将下一行与当前行连接

移位
ctrl+回车:在当前行下方另起一行
ctrl+shift+回车:在当前行上方另起一行
ctrl+shift+上下键:交换上下行

选中
ctrl+方向键:选中
ctrl+shift+左右键:以词为单位选中
ctrl+shift+m:选中封闭模块内容
ctrl+m:跳转到模块处
ctrl+d:选中相同词
ctrl+K,ctrl+d:跳过最后选中的词
alt+f3:选择全部相同单词
ctrl+l:选中行
ctrl+shift+l

### 查询
ctrl+f:查找
ctrl+h:替换

### 标签
ctrl+f2:设置书签
f2:上一个书签
shift:下一个书签
ctrl+shift+f2:清除所有书签

### 文本操作

ctrl+ku 首字母大小写
ctrl+uu 大写
ctrl+kl 小写

### 代码折叠

ctrl+shift+[:折叠
ctrl+shift+]:取消折叠
ctrl+k, ctrl+0:取消所有折叠
ctrl+alt+k, ctrl+alt+0:折叠全部
ctrl+k, ctrl+j:取消所有折叠
ctrl+alt+k, ctrl+alt+j:折叠所有标签

