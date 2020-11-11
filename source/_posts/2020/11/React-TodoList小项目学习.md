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
``

## redux 实现todolist功能

