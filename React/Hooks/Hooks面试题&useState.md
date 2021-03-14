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
不知你看带有
