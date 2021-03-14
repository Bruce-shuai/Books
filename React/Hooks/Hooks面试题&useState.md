## React-Hooks面试题
在开始复习React-hooks知识前，先写出几个hooks相关的面试题，看看自己是否能够对此侃侃而谈
### 面试题 -1
* 为什么会有React Hooks，它解决了哪些问题？
* React Hooks如何模拟组件的生命周期？
* 如何自定义Hook？

### 面试题 -2
* React Hooks性能优化？
* 使用React Hooks遇到过哪些坑？
* Hooks相比HOC和Render Prop有哪些优点？

如果大家对这些问题有所困惑，别着急！下面我会在我的总结里来回答上面的这些问题！如果感兴趣就接着往下看吧！🥳

## useState

在[《Hooks的优势》](Hooks/Hooks的优势.md)里我说过函数组件是没有`state`和`setState`的，因为`函数组件`是一个纯函数，执行完即销毁，无法存储`state`。而`useState`这个钩子能把`state`功能钩入到纯函数中
下面的例子，我将一个简单的点击功能分别用`class组件`和带`useState`这个`hook`的函数组件来实现,这样就方便大家理解useState的使用了
```
/*Class组件*/
import React, { Component } from 'react';

class ClickCounter extends Componnet {
  constructor(props) {        // constructor为生命周期函数
    super(props);
    this.state = {            // 传统的初始化state数据的方式
      count: 0;
    };
    this.setCount = this.setCount.bind(this);     // this绑定
  }
  setCount() {
    this.setState({count: this.state.count + 1})  // 只能在setState里修改数据
  }
  
  render() {
    return <div>
      <p> 你点击了{count}次 </p>
      <button onClick={this.setCount}> 点击 </button>
     </div>
  }
}
```
上面就是传统的`Class组件`实现点击功能，`state`只能在`constructor`里赋值，只能通过`setState`对`state`的值进行修改。下面来看看`useState`如何实现这相同的功能的
```
import React, { useState } from 'react';

function ClickCounter() {
  // 数组的解构
  const [count, setCount] = useState(0);  
  
  return <div>
      <p> 你点击了{count}次 </p>
      <button onClick={() => {setCount(count + 1)}}> 点击 </button>
     </div>
}
```
不知当你看完同一个功能，使用`hooks`和使用`Class组件`两种方法实现的第一感受是什么？我第一次敲完这两个代码，第一感受就是`hooks`真TM干净！简短的代码和逻辑。输入参数，返回JSX。没有烦人的this，没有所谓的生命周期函数！ 好了，让我们来分析一下`useState`到底是怎么做到让代码如此干净的！
<br />

首先，让我们来看看核心语句`const [count, setCount] = useState(0)` ，在分析此语句前我啰嗦一下~由于`useState`是react自带的钩子，所以必须如我所写的代码一样要先引入`useState`！！[count, setCount]是数组的解构赋值，其真实面目如下
```
// const [count, setCount] = useState(0)
/*--------逻辑和下面代码一样--------*/
const arr = useState(0);
const count = arr[0];
const setCount = arr[1]
```
`count`其实就对应于`Class组件`的`count`，而`setCount`对应于`Class组件`的`setCount`，而`useState(0)`就是对`count`赋了一个初值0。这就是`useState`的基本用法，替换了原来`Class组件`烦人的`state`赋值，修改`state`值，和this绑定方法，而换上了一身的轻装！
### useState的使用总结
* useState(0)传入初始值，返回数组[state, setState]
* 通过state获取值
* 通过setState(1)修改值

### Hooks命名规范
* 规定所有的`Hooks`都use开头，如useXxx
* 自定义`Hook`也要以use开头
* 非`Hooks`的地方，尽量不要使用useXxx写法


<br></br>
注：此篇文章的内容是在我看完慕课网视频：[《前端框架及项目面试 聚焦Vue3/React/Webpack》](https://coding.imooc.com/learn/list/419.html)后总结所得

