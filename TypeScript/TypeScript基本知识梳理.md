# TypeScript vs JavaScript
TypeScript是“强类型”版的JS(JS是弱类型语言)，当我们在代码中定义变量(包括普通变量、函数、组件、hook等)的时候，TS允许我们在定义的同时指定其类型，这样使用者在使用不当的时候就会被及时报错提醒
<hr />

``` 
温馨提示： 由于TS是微软开发的，使用同样是微软开发的vscode编译器，能够更好的支持TS的特性🥳
```
使用了一段时间的TS之后，给了我完全不同于JS的开发体验，bug大大减少，编译器在我还没运行代码时就能给我及时的提示，以及代码更加易读，开发效率更高，总之，上手就回不去啦！！这对于曾经学过强类型语言例如，java，c++的同学来说
一定有一种似曾相识的感受，如果，对本篇文章感兴趣，就接着往下看吧！☕️

## TypeScript的类型
接下来我先梳理一下TS中常用的8种类型：number、string、boolean、函数、array、any、void、object
```
注意： 1.相较于JS的类型而言，增加了any，void类型
      2.在变量名后面的 : 写类型
```
下面，我们来挨个梳理一遍：
### 1.number
数字类型，包含小数、其他进制的数字：

```
/* number */
let decimal: number = 6;       // 小数
let hex: number = 0xf00d;      // 十六进制
let binary: number = 0b1010;   // 二进制
let octal: number = 0o744;     // 八进制
/* bigint */
let big: bigint = 100n;        
```

### 2.string
字符串
```
let color: string = 'blue';
```

### 3.array
在TS中，array一般指所有元素类型相同的值的集合，比如：
```
let list: Array<number> = [1, 2, 3];    // 使用的泛型 <-> 学过c++的同学应该有似曾相识的感觉

// or

interface User {     // 接口，可以看成类型的说明书
  name: string
}
const john = {name: 'john'}
const jack = {name: 'jack'}
let personList = [john, jack] // 这里 john 和 jack 都是User类型
```

注意： 在JS中混合类型的元素放在一起也叫数组，但是在TS中有更严格的限定，以`tuple`(元组)
来作为"混合类型的数组"

```
let l = ['jack', 10]
```

### 4.boolean
布尔值：

```
let isDone: boolean = false;
```

### 5.函数

两种方式：
1. 在我们熟悉的“JS函数”上直接声明参数和返回值：

```
const isFalsy = (value: any): boolean => {
  return value === 0 ? true : !!value; 
}
```

2.直接声明你想要的函数类型(对的，你没有看错！是函数类型)

```
useMount = (fn: () => void) => {
  useEffect(() => {
    fn();
  }, []);
}

const isFalsy: (value: any) => boolean = (value) => {  
// (value: any) => boolean 就是一个函数类型
  return value === 0 ? true : !!value  // !! + 变量名 -> 让变量类型变为对应的boolean类型
}
```

### 6.any
any表示这个值可以是任何值，被定义为any就意味着不做任何类型检查(注意： 少用any)

```
let looselyTyped: any = 4;
// looselyTyped 的值明明是个4，哪里来的ifItExists方法呢？
// 由于声明为any，我们没法在静态检查阶段发现这个错误
looselyTyped.ifItExists();
```

```
建议少用any，any让ts在一定情况下失去了其强类型的特点，这样做会失去原本TS的保护！
```

### 7.void
绝大部分情况下，只会用在这一个地方：表示函数不返回值或者返回undefined(因为函数不返回任何值的时候 === 返回 undefined)
```
const useMount = (fn: () => void) => {
  useEffect(() => {
      fn();
  }, []);
};
```

### 8.object
除了number、string、boolean、bigint、symbol、null、undefined, 其他都是object

### 9.tuple

```
const [users, setUsers] = useState([])   // 说白了，tuple就是“数量固定，类型可以各异”版的数组
```
### 10.enum
```
enum Color {
  Red,
  Green,
  Blue
}
let c: Color = Color.Green;

```

### 11.null和undefined
null和undefined在TypeScript中既是一个值，也是一个类型
```
let u: undefined = undefined;
let n: null = null;
```

### 12.unknown
unknow 表示这个值可以是任何值，和any很相似，但比any更安全！更多限制！
```
const isFalsy = (value: unknown) => {
  // 大家不用考虑这段console有啥意义，把它打在你的代码里对应的位置，观察编辑器会不会报错
  // 再思考应不应该报错
  console.log(value.mayNotExist)
  return value === 0 ? true : !!value
}
```
```
注意：我们可以给unknown类型的变量赋任何值，但不能把unknown类型赋给其他类型
// 做法正确
let value: unknown;
value = undefined;
value = [];


```

### 13.never

并不是太常用
```
const func = () => {
  throw new Error();
}
```

### 14.interface
interface不是一种类型，应该被翻译成接口，或者说是一个类型说明书，创建一个我们自己的类型
```
interface User {
  id: number;
}
const u: User = {id: 1}
```

### .d.ts
JS文件 + .d.ts文件 === ts文件
.d.ts文件可以让JS文件继续维持自己JS文件的身份，而拥有TS的类型保护
```
.d.ts文件一般我们写业务代码不会用到，但是点击类型跳转一般会跳转到.d.ts文件
```


