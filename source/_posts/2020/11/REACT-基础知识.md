---
title: REACT 基础知识
date: 2020-11-12 11:13:29
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
```
  Action   -->   Store <==> Reducers
 Creators          |    (newState)
   |               |
   |      (state)  V
      <--  React Components
```
由于不能使用流程图，图片也支持的不好只好使用字符来代替，大致的流程图就是这样.源码我会在项目中附上，这里就不再说明了

## 使用常量代替字符串
新创建一个文件 **actionTypes.js**
```jsx
export const CHANGE_INPUT_VALUE = 'change_input_value';
export const ADD_TODO_ITEM = 'add_todo_item';
export const DELETE_TODO_ITEM = 'delete_todo_item
```
使用 **reducer.js**
```jsx
import { ADD_TODO_ITEM, CHANGE_INPUT_VALUE, DELETE_TODO_ITEM } from './actionTypes';
// 把字符串替换掉
```

## 使用 ***actionCreator***统一创建action
创建文件actionCreator.js
```jsx
import { ADD_TODO_ITEM, CHANGE_INPUT_VALUE, DELETE_TODO_ITEM} from './actionTypes'

export const getInputChangeAction = (value) => ({
    type: CHANGE_INPUT_VALUE,
    value
});

export const getAddItemAction = (value) => ({
    type: ADD_TODO_ITEM
});

export const getDeleteTodoItem = (index) => ({
    type: DELETE_TODO_ITEM
});
```
在TodoList.js中使用
```jsx
import { getInputChangeAction, getAddItemAction, getDeleteTodoItem } from './store/actionCreators';

handleInputChange(e){
        /*const action = {
            type: CHANGE_INPUT_VALUE,
            value: e.target.value
        }*/
        const action = getInputChangeAction(e.target.value);
        store.dispatch(action);
        console.log(e.target);
}

handleBtnClick(){
        /*const action ={
            type: ADD_TODO_ITEM
        };*/
        const action = getAddItemAction();
        store.dispatch(action);
}

handleItemDelete(index){
        /*const action = {
            type: DELETE_TODO_ITEM,
            index
        }*/
        const action = getDeleteTodoItem();
        store.dispatch(action);
}
```
**目前store文件夹的目录结构如下**
```
store
  |
  |-----actionCreators.js
  |
  |-----actionType.js
  |
  |-----index.js
  |
  |-----reducer.js
```

## UI界面和容器分离
创建一个新文件 ***TodoListUI.js***
注意 ***handleItemDelete***函数的写法 
```jsx
import React, { Component } from 'react';
import { Input, Button, List, Typography, Divider} from 'antd';
import 'antd/dist/antd.css'; // or 'antd/dist/antd.less'

class TodoListUI extends Component {
    render() {
        return (
            <div style={{marginTop:'10px',marginLeft:'10px'}}>
                <div>
                    <input 
                        value={this.props.inputValue} 
                        placeholder='todo info' 
                        style={{marginRight:'10px',width:'230px'}} 
                        onChange={this.props.handleInputChange}
                    />
                    <Button type="primary" onClick={this.props.handleBtnClick}>提交</Button>
                </div>
                <List
                    style={{marginTop: '10px',width: '300px'}}
                    bordered
                    dataSource={this.props.list}
                    renderItem={(item,index) => (
                        <List.Item onClick={(index) => {this.props.handleItemDelete(index)}}>
                            {item}
                        </List.Item>
                    )}
                />
            </div>
        )
    }
}
export default TodoListUI;
```

修改 ***TodoListUI.js*** 传值
```jsx
    render(){
        return(
            <TodoListUI
                inputValue ={this.state.inputValue}
                handleInputChange = {this.handleInputChange}
                handleBtnClick = {this.handleBtnClick}
                list = {this.state.list}
                handleItemDelete = {this.handleItemDelete} 
            />
        )
    }
```
***bind***绑定在构造函数完成

```jsx
constructor(props){
        super(props);
        this.handleInputChange = this.handleInputChange.bind(this);
        console.log(store.getState());
        this.state = store.getState();
        this.handleStoreChange = this.handleStoreChange.bind(this);
        this.handleBtnClick = this.handleBtnClick.bind(this);
        this.handleItemDelete = this.handleItemDelete.bind(this);
        store.subscribe(this.handleStoreChange)
    }
```

## 无状态组件 
性能高，只有render函数（UI组件）
```jsx
const TodoListUI= (props) => {
    return (
        <div style={{marginTop:'10px',marginLeft:'10px'}}>
            <div>
                <input 
                    value={props.inputValue} 
                    placeholder='todo info' 
                    style={{marginRight:'10px',width:'230px'}} 
                    onChange={props.handleInputChange}
                />
                <Button type="primary" onClick={props.handleBtnClick}>提交</Button>
            </div>
            <List
                style={{marginTop: '10px',width: '300px'}}
                bordered
                dataSource={props.list}
                renderItem={(item,index) => (
                    <List.Item onClick={(index) => {props.handleItemDelete(index)}}>
                        {item}
                    </List.Item>
                )}
            />
        </div> 
    )
}
```

## Ajax请求初始化
前一段时间使用软件没有成功，这一次自己通过springboot返回数据，成功了
主要改变的代码如下
***TodoList.js***
```jsx
import axios from 'axios';

    componentDidMount(){
        axios.get("http://192.168.43.254:8080/say").then((res)=>{
            console.log(res.data);
            const data = res.data;
            const action = initListAction(data);
            store.dispatch(action);
        });
    }
```
***actionCreators.js***
```jsx
export const initListAction = (data) => ({
    type: INIT_LIST_ACTION,
    data
})
```
***reducer.js***
```jsx
    if(action.type === INIT_LIST_ACTION){
        const newState = JSON.parse(JSON.stringify(state));
        newState.list = action.data;
        return newState;
    }

```
