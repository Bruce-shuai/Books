在前面所写的文章[《Hooks的优势》](Hooks/Hooks的优势.md)中，我提到过，函数组件是没有生命周期函数的，因为函数组件是一个纯函数，执行完即销毁，自己无法实现生命周期。但是生命周期函数的功能却又非常强大，它能在恰当好处的时候让组件使用或释放资源，从而提高性能和增加页面渲染的灵活度，但是生命周期函数会让相同的业务逻辑，分散到各个方法中，不便于测试和复用。而`useEffect`这个react自带的钩子能够取长补短，从而使`函数组件`变得异常强大...

`useEffect`能够让函数组件模拟生命周期！具体如何操作，请看下面代码
```
import React, { useState, useEffect } from 'react';

function ClickCounter(){
  // 数组的解构
  const [count, setCount] = useState(0);
  const [name, setName] = useState('帅得乱七八糟');
  
  useEffect(() => {
    console.log('在此发送一个ajax请求')
  })
  
  function clickHandler() {
    setCount(count + 1)
    setName(name + '2020')
  }
  return <div>
    <p>你点击了{count}次</p>
    <button onClick={clickHandler}>点击</button>
  </div>
}
```
<img src='https://github.com/Bruce-shuai/Books/blob/main/images/Hooks/Hooks-1.png'>

还是和之前一样，我通过用class组件和hook实现相同的功能，进行二者代码的对比，从而让我们更深刻的理解hooks
```
import React, { Component } from 'react';

Class FriendStatus extends React.Component {
  constructor() {
    super(props);
    this.state = {
      status: false; 
    }
  }
  render() {
    return <div>
      好友{ this.props.friendId } 在线状态：{this.state.status}
    </div>
  }
  
  componentDidMount() {
    console.log(`开始监听 ${this.props.friendId}的在线状态`)
  }
  componentWillMount() {
    console.log(`结束监听 ${this.props.friendId}的在线状态`)
  }
  // friend更新的时候
  componentDidUpdate(preProps) {
    console.log(`结束监听 ${preProps.friendId} 在线状态`)
		console.log(`开始监听${this.props.friendId} 在线状态`)
  }
}

//--------------------------------------

function App() {
 const [flag, setFlag] = useState(true);
 const [id, setId] = useState(1)
 
 return (
  <div>
    <p> React Hooks 示例（双越老师）</p>
    <div>
      <button onClick={() => setFlag(false)}>flag = false</button>
      <button onClick={() => setId(id + 1)}>id++</button>
    </div>
  </div>
  
  <hr></hr>
  {flag && <FriendStatus friendId={}/>}
 )
}


