---
title: VIM编辑器的使用
date: 2020-10-29 21:33:51
categories: 技术
tags: 指导
---

作为历史非常悠久的编辑器，VIM一直保有非常高的人气，被人们奉为编辑器之神。虽然很多人至今仍然不会使用，但是对于程序员而言。特别是在使用Linux，仍然是必须要会的。我经过长时间的学习，掌握了其中的一部分。感觉配置还是比较麻烦的。由于不能使用鼠标，相对于在任意位置编辑不是特别快捷。通常Linux没有图形界面，鼠标也失去了用武之地。我将分享使用时候主要的问题。

<!-- more -->

# 必要的配置 

修改home下的.vimrc
```bash
" 高亮显示
syntax·on 
" 显示行数
set·nu! 

"·设置tab,space,enter显示样式 ->tab,space,还有enter都显示出来
set·list 
set·listchars=tab:··,trail:·,eol:↓,extends:»,precedes:«,space:·
```

# 使用教程
### 以单词为单位的移动
> w：word，移动到下一个首字符
> b：backword，前一个首字符
> e：end of backword，下一个尾字符
> ge：前一个尾字符
### 删除功能
> dd：删除一行
> ndd：删除多行
> dw，daw，bdw：删除单词
> v x:删除当前光标下的字符
### VIM 文件加密
比较简单不写了

