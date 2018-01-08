---
title: ' hexo-设置阅读全文'
date: 2017-11-08 10:19:50
tags:
- hexo
---

# hexo-设置阅读全文

1. 在文章中使用`<!--more--> `手动进行截断
这种方法可以根据文章的内容，自己在合适的位置添加`<!--more-->`标签，使用灵活，也是Hexo推荐的方法 ，这种方式也可以让 Hexo 中的插件更好的识别。
```markdown
---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).
<!--more-->
## Quick Start

### Create a new post

```

2. 在文章中的front-matter中添加description，并提供文章摘录
这种方式只会在首页列表中显示文章的摘要内容，进入文章详情后不会再显示。
```markdown
---
title: Hello World
description: 这是摘要,进入文章详情后不会再显示。
---
```

3. 自动形成摘要，在主题配置文件中添加
默认截取的长度为 150 字符，可以根据需要自行设定
``` yaml
auto_excerpt:
	enable: true
	length: 150
```
