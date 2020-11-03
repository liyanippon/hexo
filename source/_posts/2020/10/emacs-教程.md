---
title: emacs 教程
date: 2020-10-31 13:16:43
categories: 技术
tags: 指导
---

*emaces*是非常难学的编辑器，虽然功能很强大，至今无法掌握。难点是快捷键太多，操作记忆起来很困难。虽然也可以编程，但是开发语言很多人都不使用了。样式设置起来没有vi/vim方便简单。下面是我从网络找的教程，自己也都测试过，更多的没有试过。以后有机会在学习吧

<!-- more -->

## 安装
brew install emacs --with-cocoa
只在终端使用时brew install emacs即可
brew services start emacs
**学习网站**https://learnxinyminutes.com

## 基本快捷键

```bash
C-x C-f # 打开/新建文件
C-x C-v # 关闭当前Buffer并打开新文件
M-g M-g ‘line’ # 跳转到line行
M-x linum-mode # 显示行号
```
## 文件操作

| 快捷键 | 英文解释 | 中文含义 |
| :-----| :---: | :----: |
| C-x C-f | find-file | 打开文件或目录 |
| C-x C-c | save-buffers-kill-emacs | 退出 |
| C-x C-z | iconify-or-deiconify-frame |    挂起（最小化）|
| C-x C-f | find-file | 打开文件、目录 |
| C-x C-r | find-file-read-only | 以只读模式打开 |
| C-x i   | insert-file | 插入文件 |
| C-x C-s | save-buffer | 保存 |
| C-x s   | save-some-buffers | 保存所有未保存的缓冲区 |
| C-x C-w | write-file | 另存为文件 |
| C-x d   | dired | 进入目录列表模式 |
| C-x C-d | list-directory | 获取文件列表（简洁）|
## 光标定位

| 向前 | 向后 | 向下 | 向上 |
| :-----| :---: | :----: | :----: |
| 翻页 | C-v | M-v |
| 字符 | C-f | C-b | C-n | C-p|
| 单词 | M-f | M-b|
| 句 | M-a | M-e |
| 行 | C-a | C-e |
| 段落 | M-{ | M-} |
| 缓冲区 | M-< | M-> |



## 其它

| 快捷键 | 英文解释 | 中文含义 |
| :-----| :---: | :----: |
|M-g M-g|     (goto-line)   |                   跳转到某行|
|M-x     |         (goto-char)       |             跳转到字符位置|
|C-M-l  |        (reposition-window)    |将当前行卷至页面中部|
|C-l|(recenter)|刷新页面，将将当前行卷至页面中部 （使用数字参数指定行）|
|M-r M-x|(move-to-window-line) |移动光标至页面中间的行 |（使用数字参数指定行）|

## 删除

| 操作 | 向后 |
| :-----| :---: |
|字符|C-d DEL|
|单词|M-d M-DEL|
|行|C-k（删除至行尾）|
|整行|C-S-backspace|
|按表达式删除|C-M-k|
|区块|C-w|
|删除连续空格| M-x delete-horizontal-space|
注：单词、行、区块的删除是kill，相当于剪切，会被放入kill-ring，字符及空格的删除是delete


## 复制粘贴

复制前要先选择:C-@开始区块选择，然后移动光标，选中的区域会高亮
剪切:前面"删除"的部分包括了一些剪切操作，对区块的剪切用C-w
复制:区块复制用M-w，至于复制1行，复制1个单词之类的功能，自己想办法吧:(
粘贴: C-y:粘贴kill-ring堆栈的最后一次的内容
C-y 之后可以继续M-y, 对Kill-ring中的内容依次召回
## 撤销重做
撤销: C-/  (每插入20个字符，视为一个 undo 的单位)
重做: C-/ 后，依次输入C-g C-/ 就可以redo

## 缓冲区管理
Emacs中，打开新的buffer，不会关闭原有buffer，而是需要手工操作

| 快捷键 | 英文解释 | 中文含义 |
| :-----| :---: |:---: |
| C-x C-b | list-buffers | 查看缓冲区列表|
| C-x b | switch-to-buffer | 切换缓冲区 |
| C-x k | kill-buffer | 关闭缓冲区 |

## 一般搜索

| 快捷键 | 英文解释 | 中文含义 |
| :-----| :---: |:---: |
| M-x | search-forward | 向前搜索 |
| M-x | search-backward | 向后搜索 |
| M-x| search-forward-regexp | 正则表达式向前搜索 |
| M-x | search-backward-regexp | 正则表达式向后搜索 |

## 替换

| 快捷键 | 英文解释 | 中文含义 |
| :-----| :---: |:---: |
| M-x | replace-string | 替换 |
| M-x | replace-regexp | 正则表达式替换 |

## 批量处理
1.选中区域， M-x untabify：将 TAB 字符转换为空格
2.选中区域， M-x indent-region：对齐文本块
