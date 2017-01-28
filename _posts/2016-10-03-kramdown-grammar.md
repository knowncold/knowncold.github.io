---
layout: page
title: Kramdown 蛋疼的语法
category: wiki
---
多级标题无法解析，#形同虚设

之前改过一次Jekyll的配置文件，把Markdown引擎改成了Kramdown，导致各种标题无法解析，一开始以为是引擎选错了，或者这个Kramdown引擎有特别的地方。  

翻了各种文档，并没有提到特别需要注意之处，终于刚刚搜到Kramdown的语法中，需要在#和标题之间加一个空格。

而且正好在写这个的时候，发现头信息中的layout，title冒号后面也要加一个空格。

还有Github的引擎要求引用`>`之前需要一个空行呢

>「厉害了我的哥」
