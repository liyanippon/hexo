---
title: React TodoList小项目学习
date: 2020-11-10 10:57:56
categories: 技术
tags: 指导
---
在TodoList小项目中学习React的相关语法
<!-- more -->
# 本篇文章将从例子中学习react具体语法
## 简单实现
隐藏最外层的标签：使用**Fragment**
完整的小例子（*目前输入框还有些问题，数据不能回显*）
```javascript
import React,{ Component,Fragment } from 'react';
// Fragment 可以将最外层的标签隐藏掉
class TodoList extends Component{
    // 定义数据
    constructor(props){
        super(props);
        this.state={
            inputValue:'yy',
            list:['学习英文','学习react'],
        }

    }

    handleInputChange(e){
        console.log(e.target.value);
        this.setState=({
            inputValue : e.target.value
        })
    }

    handleBtnClick(){
        this.setState({
            list : [...this.state.list,this.state.inputValue],
            inputValue:''
        })
    }

    handleItemDelete(index){
        // immutable
        // state不允许做更改，否则性能优化有问题
        console.log(index);
        const list = [...this.state.list];
        list.splice(index,1);
        this.setState({
          list:list
        })
    }

    render(){
        return(
            <Fragment>
                <div>
                    <input 
                        value={this.state.inputValue}
                        onChange={this.handleInputChange.bind(this)}
                    />
                    <button onClick={this.handleBtnClick.bind(this)}>提交</button>
                </div>
                <ul>
                    {
                        this.state.list.map((item,index) => {
                            return (
                                <li key={index} 
                                    onClick={this.handleItemDelete.bind(this,index)}
                                    >
                                        {item}
                                        </li>
                                    )
                        })
                    }
                </ul>
            </Fragment>

        )
    }
}
export default TodoList;
```
## TodoList 代码优化(输入框仍然不好用😂)
使用了组件，子组件删除时调用父组件的方法。为了提高运行速度，将事件绑定在构造函数中实现。
下面是优化后的代码
TodoList.jsx
```javascript
import React,{ Component,Fragment } from 'react';
import TodoItem from './TodoItem'
// Fragment 可以将最外层的标签隐藏掉
class TodoList extends Component{
    // 定义数据
    constructor(props){
        super(props);
        this.state={
            inputValue:'jj',
            list:['学习英文','学习react'],
        }
        this.handleInputChange = this.handleInputChange.bind(this);
        this.handleItemDelete = this.handleItemDelete.bind(this);
        this.handleBtnClick = this.handleBtnClick.bind(this);
    }

    handleInputChange(e){
        console.log(e.target);
        const value = e.target.value;
        this.setState=(() => ({
            inputValue: value
        }));
    }

    handleBtnClick(){
        this.setState((prevState) => ({
            list : [...prevState.list,prevState.inputValue],
            inputValue:''
        }));
    }

    handleItemDelete(index){
        // immutable
        // state不允许做更改，否则性能优化有问题
        console.log(index);
        this.setState((prevState) => {
            const list = [...prevState.list]
            list.splice(index,1);
            return {list}
        });
    }

    // 循环列表
    getTodoItem(){
        return this.state.list.map((item,index) => {
            return (
                    <TodoItem
                        key={index}
                        content={item}
                        index={index}
                        deleteItem={this.handleItemDelete}
                    />
            )
        })
    }

    render(){
        return(
            <div>
                <div>
                    <input
                        value={this.state.inputValue}
                        onChange={this.handleInputChange}
                    />
                    <button onClick={this.handleBtnClick}>提交</button>
                </div>
                <ul>
                    {this.getTodoItem()}
                </ul>
            </div>

        )
    }
}

export default TodoList;
```
子组件 ***TodoItem.jsx***
```javascript
import React,{Component} from 'react'
class TodoItem extends Component{
    constructor(props){
        super(props);
        this.handClick = this.handClick.bind(this);
    }
    handClick(){
        //调用父组件方法
        const {deleteItem,index} = this.props;
        deleteItem(index);
        //this.props.deleteItem(this.props.content)
    }

    // 避免重复渲染
    shouldComponentUpdate(nextProps,nextState){
        if(nextProps.content != this.props.content){
            return true;
        }else{
            return false;
        }
    }

    render(){
        return (
            <div onClick = {this.handClick}>
                {test} - {content}
            </div>
        )
    }
}

TodoItem.propType = {
    //isRequired 含义：如果没有传值也会报错
    test: PropTypes.string.isRequired,
    //字符串
    content: PropTypes.string,
    //函数
    deleteItem: PropTypes.func,
    //数值型
    index: PropTypes.number
}

//默认值，没有接收到值，显示默认值
TodoItem.defaultProps = {
    test: 'hello word'
}
export default TodoItem;
```
## redux 实现todolist功能
**使用redux完成todolist功能** ，
样式使用了**antd**这次 ***input彻底好用了***
**主要文件**
index.js
```js
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);

export default store
```

TodoList.js
```jsx
import React,{ Component } from 'react';
import 'antd/dist/antd.css'; // or 'antd/dist/antd.less'
import { Input, Button, List, Typography, Divider} from 'antd';
import store from './store/index.js';

//list测试数据
const data = [
    'Racing car sprays burning fuel into crowd.',
    'Japanese princess to wed commoner.',
    'Australian walks 100km after outback crash.',
    'Man charged over missing wedding girl.',
    'Los Angeles battles huge wildfires.',
  ];
  
/**
 * 使用antd ui框架 https://ant.design/components/list-cn/#header
 * 使用 redux 框架
 * Input 可以正常使用了
 */
class TodoList extends Component{
    constructor(props){
        super(props);
        this.handleInputChange = this.handleInputChange.bind(this);
        console.log(store.getState());
        this.state = store.getState();
        this.handleStoreChange = this.handleStoreChange.bind(this);
        this.handleBtnClick = this.handleBtnClick.bind(this);
        store.subscribe(this.handleStoreChange)
    }

    handleInputChange(e){
        const action = {
            type: 'change_input_value',
            value: e.target.value
        }
        store.dispatch(action);
        console.log(e.target);
    }

    handleStoreChange(){
        this.setState(store.getState());
    }

    handleBtnClick(){
        const action ={
            type: 'add_todo_item'
        };
        store.dispatch(action);
    }

    handleItemDelete(index){
        const action = {
            type: 'delete_todo_item',
            index
        }
        store.dispatch(action);
    }

    render(){
        return(
            <div style={{marginTop:'10px',marginLeft:'10px'}}>
                <div>
                    <input 
                        value={this.state.inputValue} 
                        placeholder='todo info' 
                        style={{marginRight:'10px',width:'230px'}} 
                        onChange={this.handleInputChange}
                    />
                    <Button type="primary" onClick={this.handleBtnClick}>提交</Button>
                </div>
                <List
                    style={{marginTop: '10px',width: '300px'}}
                    bordered
                    dataSource={this.state.list}
                    renderItem={(item,index) => (
                        <List.Item onClick={this.handleItemDelete.bind(this,index)}>
                            {item}
                        </List.Item>
                    )}
                />
            </div>

        )
    }

}
export default TodoList;
```

创建store文件夹，新建两个文件
index.js
```jsx
import { createStore } from 'redux';
import reducer from './reducer';

const store = createStore(reducer);

export default store;
```
reducer.js
```jsx
import { act } from "react-dom/test-utils";
import store from ".";

const defaultState ={
    inputValue: '123',
    list: [1,2]
}

//reducer可以接收state，但是不能修改state
export default (state = defaultState, action) => {
    
    if (action.type === 'change_input_value'){
        const newState = JSON.parse(JSON.stringify(state));
        newState.inputValue = action.value;
        return newState;
    }

    if (action.type === 'add_todo_item'){
        const newState = JSON.parse(JSON.stringify(state));
        newState.list.push(newState.inputValue);
        newState.inputValue = '';
        console.log(newState);
        return newState;
    }

    if(action.type === 'delete_todo_item'){
        const newState = JSON.parse(JSON.stringify(state));
        newState.list.splice(action.index,1);
        return newState;
    }
    return state;
}
```

