---
title: REACT 基础知识
date: 2020-11-02 22:28:29
categories: 技术
tags: 指导
---

react是facebook使用的框架，很多的企业都在使用。目前非常流行，学习起来也很容易
接下来我讲详细记录我的学习过程

<!-- more -->

## 命令行

**环境构建**
```bash
npx create-react-app my-app #建立新的工程
cd my-app 
npm run build #构建项目
npm start #启动项目
```
## 简单例子（显示文字，单选框）
**开始部分**
index.js
```javascript
ReactDOM.render(
  <React.StrictMode>
    <Blog />

  </React.StrictMode>,
  document.getElementById('root')
);
```
**组件类**
Blog.jsx
```javascript
import React from 'react'
import Article from './Article'

class Blog extends React.Component{
    constructor(props){
        super(props);
        this.state={
            isPublished : false,
            order:1
        }
    }
    // 公开状态取反函数
    togglePublished = () =>{
        this.setState({
            isPublished:!this.state.isPublished
        })
    };
    render(){
        return (
            <div>
                <Article title={"React"}
                isPublished = {this.state.isPublished}
                toggle = { () => this.togglePublished()}
                />

            </div>
        )
    }
}
export default Blog;
```

**函数**
Article.jsx

```javascript

import React from 'react';
const Article = (props) => {
    return (
        <div>
            <h2>{props.title}</h2>
            <label htmlFor="check">state</label>

            设置状态<input id="check" type="checkbox" checked={props.isPublished}
                onClick={() => props.toggle()}
            />
        </div>
    )
}

export default Article;

```



