```
温馨提示： 本篇文章内容较多，可分多次浏览
```

在前面所写的文章[《Hooks的优势》](Hooks/Hooks的优势.md)中，我提到过，函数组件是没有生命周期函数的，因为函数组件是一个纯函数，执行完即销毁，自己无法实现生命周期。但是生命周期函数的功能却又非常强大，它能在恰当好处的时候让组件使用或释放资源，从而提高性能和增加页面渲染的灵活度，但是生命周期函数会让相同的业务逻辑，分散到各个方法中，不便于测试和复用。而`useEffect`这个react自带的钩子能够取长补短，从而使`函数组件`变得异常强大...

`useEffect`能够让函数组件模拟生命周期！具体如何操作，请看下面代码
### 模拟DidUpdate & DidMount生命周期

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
<img src='https://github.com/Bruce-shuai/Books/blob/main/images/Hooks/Hooks%20-1.png' height='200px'>

从图中我们可以看出(黄色背景的提醒与此代码无关，请忽略)，`useEffect`模拟的是生命周期中的`DidMount` 和 `DidUpdate`, 有兴趣的朋友，可以跟着敲一敲代码，自己看看效果。
<br><br/>
生命周期函数有很多，而useEffect其实也可以模拟出很多非常重要的生命周期函数，感兴趣的话，就接着往下看吧！！

```
// 模拟class组件的DidMount
useEffect(() => {
  console.log('在此发送一个ajax请求')
}, [])                  // 用空数组作为第二个参数


// 模拟class组件的DidUpdate
useEffect(() => {
  console.log(‘加载完了’)
}, [count, name])       // 第二个参数不是空数组
```
模拟`DidMount`的打印结果
<img src='https://github.com/Bruce-shuai/Books/blob/main/images/Hooks/Hooks%20-2.png' height='150px'>

模拟`DidUpdate`的打印结果
<img src='https://github.com/Bruce-shuai/Books/blob/main/images/Hooks/Hooks%20-3.png' height='150px'>

我们先来分析一下`useEffect`如何分别模拟的`DidMount`和`DidUpdate`，和前面`useEffect`同时模拟`DidMount`和`DidUpdate`不同，这里的`useEffect`有第二个参数，不过第一个`useEffect`钩子的第二参数是空数组，第二个`useEffect`钩子的第二参数是有元素的数组。
<br><br/>
那[]和[count, name]为何能模拟不同生命周期呢？ 答案就是数组里的元素是作为`useEffect`变化的依赖！`count`或者`name`任意一个元素发生变化，`useEffect`就会被重新执行，所以就模拟的`DidUpdate`这个生命周期函数，甚至它比`DidUpdate`更加灵活！而空数组为何能模拟`DidMount`呢? 原因就是该数组内没任何元素，所以不存在数组内元素变化，`useEffect`也就不会因此而被重新执行，所以自始至终就只会执行一次`useEffect`,这一次执行就是在函数最初被渲染的时候所执行的！

### 模拟WillUnMount生命周期
话不多说，上代码！
```
import React, { useState, useEffect } from 'react';

```


```
看到这儿了就先休息一会儿吧~ ☕️
```
好了，上面我讲了`useEffect`模拟`DidMount`和`DidUpdate`这两个生命周期函数。但是

```
// 模拟class组件的DidMount 和 WillUnMount
useEffect(() => {
  let timerId = window.setInterval(() => {
      console.log(Date.now())
  }, 1000)
  
  // 返回一个函数
  // 模拟WillUnMount
  return () => {
      window.clearInterval(timerId)
  }
}, [])
```

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


