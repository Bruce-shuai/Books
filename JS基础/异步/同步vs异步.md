## 题目
在回顾异步前，先出3个题目，看看自己能否清晰地回答出来
 * 同步和异步的区别是什么？
 * 手写用Promise加载一张图片
 * 前端使用异步的场景有哪些？

```
setTimeout 笔试题  尝试写出打印结果！
console.log(1);
setTimeout(function () {
  console.log(2);
}, 1000)
console.log(3);
setTimeout(function () {
  console.log(4);
}, 0)
console.log(5)
```
## 本节要讲解的知识点
 * 单线程和异步
 * 应用场景
 * callback hell 和 Promise

### 单线程和异步
首先，JS是单线程语言，只能在一个时间段里做一件事，浏览器和nodejs已支持JS启动`进程`，如Web Worker，JS和DOM渲染共用同一个线程，因为JS可修改DOM结构
<hr />
异步就是由单线程这个背景而来的！ 异步用来解决单线程等待的问题（防止一样东西要加载很久。比如网络请求、定时任务，用户不可能坐着，干等着页面加载)。
```
注意： 异步是基于callback函数形式来调用的
```

下面为异步代码演示, 有兴趣的伙伴，可在自己的编译器里运行此代码
```
console.log(100)
setTimeout(function() {
  console.log(200);
}, 1000)
console.log(300)        
// 100
// 300
// ’等待1s‘ 200
```

下面为同步的代码
```
console.log(100)
alert(200)
console.log(300)

// 100
// 要点击确认按钮， 不然永远阻塞在200，页面不会往后运行
// 300
```

### 应用场景
说白了，异步其实就是在等待的时候才用。而前端主要在以下两个场景使用异步
```
1. 网络请求  如ajax, 图片加载
2. 定时任务  如setTimeout
```

因为在网络请求的时候cpu是空闲的，可不能浪费资源啊！所以需要异步的机制

```
// ajax
console.log('start')
$.get('./data1.json', function(data1) {
  console.log(data1)
})
console.log('end')


// 图片加载
console.log('start')
let img = document.createElement('img');
img.onload = function() {
  console.log('loaded')
}
img.src = '/xxx.png'
console.log('end')


// 定时任务
// setTimeout
console.log(100)
setTimeout(function() {
  console.log(200)
}, 1000)
console.log(300)

// setInterval
console.log(100)
setInterval(function () {
  console.log(200)
}, 1000)
console.log(300)

```

### callback hell 和 Promise
异步是基于callback来执行，但是如果只用callback解决ajax问题，就会陷入`callback hell`这个坑里面
例如：

```
// 获取第一份数据
$.get(url1, (data1) => {
  console.log(data1)
  
  // 获取第二份数据
  $.get(url2, (data2) => {
    console.log(data2)
    
    // 获取第三份数据
    $.get(url3, (data3) => {
      console.log(data3)
      
      // 还可以获得更多的数据...
    })
  })
})
```
如果再获取到10份，100份数据，甚至更多。那么这恶心的嵌套，简直是反人类的代码！这就是臭名昭著的`callback hell`，而`callback hell`是促进Promise产生的一个很重要的原因！ 下面来看看使用Promise解决ajax问题是如何做的！

```
function getData(url) {
  return new Promise((resolve, reject) => {
    $.ajax({
      url,
      success(data) {
        resolve(data)
      },
      error(err) {
        reject(err)
      }
    })
  })
}

const url1 = '/data1.json'
const url2 = '/data2.json'
const url3 = '/data3.json'
getData(url1).then(data1 => {
  console.log(data1)
  return getData(url2)
}).then(data2 => {
  console.log(data2)
  return getData(url3)
}).then(data3 => {
  console.log(data3)
}).catch(err => console.error(err))
```
Promise中，catch是防错处理，.then都是单层的，是一个管道形式，而不是一个嵌套形式，比较符合人类的感官。

## 将前面所讲的问题进行解答
### 同步和异步的区别是什么？
  * 基于JS是单线程语言
  * 异步不会阻塞代码执行
  * 同步会阻塞代码执行

### 手写用Promise加载一张图片
new Promise里面传一个函数，这个函数有两个参数 resolve和reject，并且这两个参数也是函数
```
/* 代码一*/
function loadImg(src) {
  const p = new Promise(
    (resolve, reject) => {
      const img = document.createElement('img')
      img.onload = () => {     
        resolve(img)
      }
      img.onerror = () => {
        const err = new Error(`图片加载失败 ${src}`)
        reject(err)
      }
      img.src = src
    }
  )
  return p      // 一定要返回
}

const url = 'http://imga2.5054399.com/upload_pic/2015/9/1/4399_17562395347.gif';

loadImg(url).then(img => {
  console.log(img.width)
  return img       // 返回一个普通对象， 第一个img会return到第二个then里面的img里
}).then(img => {
  console.log(img.height)
}).catch(ex => console.error(ex))
```
```
/* 代码二 */
const url1 = 'http://imga2.5054399.com/upload_pic/2015/9/1/4399_17562395347.gif';
const url2 = 'http://imga5.5054399.com/upload_pic/2014/5/15/4399_17502294860.jpg';

loadImg(url1).then(img1 => {
  console.log(img1.width)
  return img1;  // 普通对象
}).then(
  img1 => {
    console.log(img1.height)
    return loading(url2) // Promise 实例
  }).then(img2 => {
  
  })
```
如果return普通对象，下一个参数就会接受这个对象，如果return promise实例，它的下一个参数接受的就是这个promise实例加载的结果 ---> (这里可能要纠错！)

### 异步应用场景
 * 网络请求， 如ajax图片加载
 * 定时任务，如setTimeout

### setTimeout 笔试题
```
console.log(1)
setTimeout(function() {
  console.log(2)
}, 1000)
console.log(3)
setTimeout(function() {
  console.log(4)
}, 0)
console.log(5)
```
打印结果  1 3 5 4 2

