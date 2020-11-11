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

## React路由实现
需要使用第三方插件**react-router-dom**
```bash sudo cnpm install react-router-dom --save```
App.js
```javascript
import { BrowserRouter, Route } from 'react-router-dom';

function App() {
  return (
    <div> 
      <head />
      <BrowserRouter>
        <div>
          {/*exact 完全匹配*/}
          <Route path='/' exact render={()=><div>home</div>}></Route>
          <Route path='/detail' exact render={()=><div>detail</div>}></Route>
        </div>
      </BrowserRouter>
    </div>
  );
}
```
## PropTypes和DefaultProps的应用
+ **proptypes**用于限制父组件组件向子组件传递**参数类型强校验**
```javascript
//引入
import PropTypes from 'prop-types';
//在最后的位置添加
TodoItem.PropTypes = {
    //isRequired 含义：如果没有传值也会报错
    test: PropTypes.string.isRequired,
    //字符串
    content: PropTypes.string,
    //函数
    deleteItem: PropTypes.func,
    //数值型
    index: PropTypes.number
}
export default TodoItem;
```
+ DefaultProps默认值设定
```javascript
//默认值，没有接收到值，显示默认值
TodoItem.defaultProps = {
    test: 'hello word'
}
```
## shouldComponentUpdate
```javascript
    // 避免重复渲染
    shouldComponentUpdate(nextProps,nextState){
        if(nextProps.content != this.props.content){
            return true;
        }else{
            return false;
        }
    }
```
## ajax
    //ajax请求
    componentDidMount(){
        axios.get('/api/todolist').then((res) => {
            console.log(res.data);
            this.setState(() => ({
                list: [...res.data]
            }));
        }).catch(() => {
            //alert('error')
        })
    }

## Redux使用 （数据管理）
因为关系比较复杂，我使用流程图来说明
![avator](2020111101.jpeg)

