# JavaScript

* [同源策略](#同源策略)
* [跨域](#跨域)
* [JSONP](#JSONP)
* [事件绑定的方式](#事件绑定的方式)
* [事件委托](#事件委托)
* [事件循环](#事件循环)
* [事件模型](#事件模型)
* [target和currentTarget区别](#target和currentTarget区别)
* [prototype和__proto__的关系是什么](#prototype和__proto__的关系是什么)
* [原型继承](#原型继承)
* [继承](#继承)
* [闭包](#闭包)
* [Ajax](#Ajax)
* [使用Ajax的优缺点分别是什么](#使用Ajax的优缺点分别是什么)
* [Ajax和Fetch区别](#Ajax和Fetch区别)
* [变量提升](#变量提升)
* [使用let、var和const创建变量有什么区别](#使用letvar和const创建变量有什么区别)
* [对象深拷贝](#对象深拷贝)
* [数组去重](#数组去重)
* [数据类型](#数据类型)
* [内置函数(原生函数)](#内置函数原生函数)
* [域名收敛](#域名收敛)
* [首屏时间、白屏时间](#首屏时间白屏时间)
* [当你在浏览器输入一个地址后发生了什么](#当你在浏览器输入一个地址后发生了什么)
* [自动分号](#自动分号)
* [浮点数精度](#浮点数精度)
* [cookie、localStorage、sessionStorage区别](#cookielocalStoragesessionStorage区别)
* [自执行函数?用于什么场景？好处?](#自执行函数用于什么场景好处)
* [实现add函数,让add(a)(b)和add(a,b)两种调用结果相同](#实现add函数让addab和addab两种调用结果相同)
* [js中万物皆对象你认为对么](#js中万物皆对象你认为对么)
* [多个页面之间如何进行通信](#多个页面之间如何进行通信)
* [css动画和js动画的差异](#css动画和js动画的差异)
* [请用js去除字符串空格](#请用js去除字符串空格)
* [new一个对象经历了什么](#new一个对象经历了什么)
* [如何实现文件断点续传](#如何实现文件断点续传)
* [bind、call、apply的区别](#bindcallapply的区别)
* [请简述JavaScript中的this](#请简述JavaScript中的this)
* [==和===的区别是什么](#和的区别是什么)

## 同源策略
同源策略可防止 JavaScript 发起跨域请求。源被定义为 URI、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。


## 跨域
* 原因 <br>
浏览器的同源策略导致了跨域
* 作用 <br>
用于隔离潜在恶意文件的重要安全机制
* 解决
1. jsonp ，允许 script 加载第三方资源
2. 反向代理（nginx 服务内部配置 Access-Control-Allow-Origin *）
3. cors 前后端协作设置请求头部，Access-Control-Allow-Origin 等头部信息
4. iframe 嵌套通讯，postmessage

https://zhuanlan.zhihu.com/p/41479807 <br>
[跨域资源共享 CORS 阮一峰](http://www.ruanyifeng.com/blog/2016/04/cors.html)


## JSONP
这是我认为写得比较通俗易懂的一篇文章 直接转载过来<br>
https://blog.csdn.net/hansexploration/article/details/80314948

## 事件绑定的方式
* 嵌入dom
```
<button onclick="func()">按钮</button>
```

* 直接绑定
```
btn.onclick = function(){}
```

* 事件监听
```
btn.addEventListener('click',function(){})
```


## 事件委托
事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。所有用到按钮的事件（多数鼠标事件和键盘事件）都适合采用事件委托技术，
使用事件委托可以节省内存。
```
<ul>
  <li>苹果</li>
  <li>香蕉</li>
  <li>凤梨</li>
</ul>

// good
document.querySelector('ul').onclick = (event) => {
  let target = event.target
  if (target.nodeName === 'LI') {
    console.log(target.innerHTML)
  }
}

// bad
document.querySelectorAll('li').forEach((e) => {
  e.onclick = function() {
    console.log(this.innerHTML)
  }
}) 
```


## 事件循环
事件循环是一个单线程循环，用于监视调用堆栈并检查是否有工作即将在任务队列中完成。如果调用堆栈为空并且任务队列中有回调函数，则将回调函数出队并推送到调用堆栈中执行。



## 事件模型
* DOM0<br>
直接绑定
```
<input onclick="sayHi()"/>

btn.onclick = function() {}
btn.onclick = null
```

* DOM2<br>
DOM2级事件可以冒泡和捕获
通过addEventListener绑定
通过removeEventListener解绑
```
// 绑定
btn.addEventListener('click', sayHi)
// 解绑
btn.removeEventListener('click', sayHi)
```

* DOM3<br>
DOM3具有更多事件类型
DOM3级事件在DOM2级事件的基础上添加了更多的事件类型，全部类型如下：
```
UI事件，当用户与页面上的元素交互时触发，如：load、scroll
焦点事件，当元素获得或失去焦点时触发，如：blur、focus
鼠标事件，当用户通过鼠标在页面执行操作时触发如：dbclick、mouseup
滚轮事件，当使用鼠标滚轮或类似设备时触发，如：mousewheel
文本事件，当在文档中输入文本时触发，如：textInput
键盘事件，当用户通过键盘在页面上执行操作时触发，如：keydown、keypress
合成事件，当为IME（输入法编辑器）输入字符时触发，如：compositionstart
变动事件，当底层DOM结构发生变化时触发，如：DOMsubtreeModified
```

https://www.jianshu.com/p/3acdf5f71d5b

## target和currentTarget区别
* event.target<br>
返回触发事件的元素
* event.currentTarget<br>
返回绑定事件的元素


## prototype和__proto__的关系是什么
所有的对象都拥有__proto__属性，它指向Object.prototype（Object是一个原生函数，所有的对象都是Object的实例）
```
let obj = {}
obj.__proto__ === Object.prototype // true
```

所有的函数都同时拥有__proto__和protytpe属性
函数的__proto__指向自己的函数实现 函数的protytpe是一个对象 所以函数的prototype也有__proto__属性 指向Object.prototype
```
function func() {}
func.prototype.__proto__ === Object.prototype // true
```

Object.prototype.__proto__指向null
```
Object.prototype.__proto__ // null
```


## 原型继承
所有的JS对象都有一个prototype属性，指向它的原型对象。当试图访问一个对象的属性时，如果没有在该对象上找到，它还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。


## 继承
JS高程第3版 第6章 继承
寄生组合式继承
```
function SuperType(name) {
    this.name = name
    this.colors = ['red']
}

SuperType.prototype.sayName = function() {
    console.log(this.name)
}
// 继承实例属性
function SubType(name, age) {
    SuperType.call(this, name)
    this.age = age
}

function inheritPrototype(subType, superType) {
    let prototype = Object.create(superType.prototype)
    prototype.constructor = subType
    subType.prototype = prototype
}
// 继承原型方法
inheritPrototype(SubType, SuperType)

// 定义自己的原型方法
SubType.prototype.sayAge = function() {
    console.log(this.age)
}
```


## 闭包
闭包是指有权访问另一个函数作用域中的变量的函数。
```
function sayHi(name) {
    return () => {
       console.log(`Hi! ${name}`)
    }
}
const test = sayHi('xiaoming')
test() // Hi! xiaoming
```
虽然sayHi函数已经执行完毕，但是其活动对象也不会被销毁，因为test函数仍然引用着sayHi函数中的变量name，这就是闭包。<br>
但也因为闭包引用着另一个函数的变量，导致另一个函数已经不使用了也无法销毁，所以闭包使用过多，会占用较多的内存，这也是一个副作用。


## Ajax
Ajax(asynchronous JavaScript and XML)是使用客户端上的许多 Web 技术，创建异步 Web 应用的一种 Web 开发技术。借助 Ajax，Web 应用可以异步（在后台）向服务器发送数据和从服务器检索数据，而不会干扰现有页面的显示和行为。通过将数据交换层与表示层分离，Ajax 允许网页和扩展 Web 应用程序动态更改内容，而无需重新加载整个页面。实际上，现在通常将 JSON 替换为 XML，因为 JavaScript 对 JSON 有原生支持优势。<br>
XMLHttpRequest API 经常用于异步通信。此外还有最近流行的fetch API。
```
let xmlhttp
if (window.XMLHttpRequest) {
	//  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
	xmlhttp = new XMLHttpRequest()
} else {
	// IE6, IE5 浏览器执行代码
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP")
}
xmlhttp.onreadystatechange = () => {
	if (xmlhttp.readyState === 4 && xmlhttp.status === 200) {
		document.getElementById("myDiv").innerHTML = xmlhttp.responseText
	}
}
xmlhttp.open("GET", "/ajax/test.txt", true)
xmlhttp.send()
```
#### [回到顶部](#JavaScript)

## 使用Ajax的优缺点分别是什么

**优点**

* 交互性更好。来自服务器的新内容可以动态更改，无需重新加载整个页面。
* 减少与服务器的连接，因为脚本和样式只需要被请求一次。
* 状态可以维护在一个页面上。JavaScript 变量和 DOM 状态将得到保持，因为主容器页面未被重新加载。
* 基本上包括大部分 SPA 的优点。

**缺点**

* 动态网页很难收藏。
* 如果 JavaScript 已在浏览器中被禁用，则不起作用。
* 有些网络爬虫不执行 JavaScript，也不会看到 JavaScript 加载的内容。
* 基本上包括大部分 SPA 的缺点。

https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md

#### [回到顶部](#JavaScript)

## Ajax和Fetch区别
* ajax是使用XMLHttpRequest对象发起的，但是用起来很麻烦，所以ES6新规范就有了fetch，fetch发一个请求不用像ajax那样写一大堆代码。
* 使用fetch无法取消一个请求，这是因为fetch基于Promise，而Promise无法做到这一点。
* 在默认情况下，fetch不会接受或者发送cookies
* fetch没有办法原生监测请求的进度，而XMLHttpRequest可以
* fetch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
* fetch由于是ES6规范，兼容性上比不上XMLHttpRequest

#### [回到顶部](#JavaScript)

## 变量提升
var会使变量提升，这意味着变量可以在声明之前使用。let和const不会使变量提升，提前使用会报错。
变量提升（hoisting）是用于解释代码中变量声明行为的术语。使用var关键字声明或初始化的变量，会将声明语句“提升”到当前作用域的顶部。 但是，只有声明才会触发提升，赋值语句（如果有的话）将保持原样。

#### [回到顶部](#JavaScript)

## 使用let、var和const创建变量有什么区别
用var声明的变量的作用域是它当前的执行上下文，它可以是嵌套的函数，也可以是声明在任何函数外的变量。let和const是块级作用域，意味着它们只能在最近的一组花括号（function、if-else 代码块或 for 循环中）中访问。
```js
function foo() {
  // 所有变量在函数中都可访问
  var bar = 'bar';
  let baz = 'baz';
  const qux = 'qux';

  console.log(bar); // bar
  console.log(baz); // baz
  console.log(qux); // qux
}

console.log(bar); // ReferenceError: bar is not defined
console.log(baz); // ReferenceError: baz is not defined
console.log(qux); // ReferenceError: qux is not defined
```

```js
if (true) {
  var bar = 'bar';
  let baz = 'baz';
  const qux = 'qux';
}

// 用 var 声明的变量在函数作用域上都可访问
console.log(bar); // bar
// let 和 const 定义的变量在它们被定义的语句块之外不可访问
console.log(baz); // ReferenceError: baz is not defined
console.log(qux); // ReferenceError: qux is not defined
```

var会使变量提升，这意味着变量可以在声明之前使用。let和const不会使变量提升，提前使用会报错。

```js
console.log(foo); // undefined

var foo = 'foo';

console.log(baz); // ReferenceError: can't access lexical declaration 'baz' before initialization

let baz = 'baz';

console.log(bar); // ReferenceError: can't access lexical declaration 'bar' before initialization

const bar = 'bar';
```

用var重复声明不会报错，但let和const会。

```js
var foo = 'foo';
var foo = 'bar';
console.log(foo); // "bar"

let baz = 'baz';
let baz = 'qux'; // Uncaught SyntaxError: Identifier 'baz' has already been declared
```

let和const的区别在于：let允许多次赋值，而const只允许一次。

```js
// 这样不会报错。
let foo = 'foo';
foo = 'bar';

// 这样会报错。
const baz = 'baz';
baz = 'qux';
```

https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Chinese/questions/javascript-questions.md#%E4%BD%BF%E7%94%A8letvar%E5%92%8Cconst%E5%88%9B%E5%BB%BA%E5%8F%98%E9%87%8F%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB

#### [回到顶部](#JavaScript)

## 对象深拷贝
```
let o1 = {a:{
    b:1
  }
}
let o2 = JSON.parse(JSON.stringify(o1))
```
另一种方法
```
function deepCopy(s) {
    const d = {}
    for (let k in s) {
        if (typeof s[k] == 'object') {
            d[k] = deepCopy(s[k])
        } else {
            d[k] = s[k]
        }
    }

    return d
}
```
#### [回到顶部](#JavaScript)

## 数组去重
ES5
```
function unique(arry) {
    const temp = []
    arry.forEach(e => {
        if (temp.indexOf(e) == -1) {
            temp.push(e)
        }
    })
    
    return temp
}
```
ES6
```
function unique (arr) {
   return Array.from(new Set(arr))
}
```


## 数据类型
1. Undefined 
2. Null 
3. Boolean 
4. Number
5. String 
6. Object
7. symbol(ES6新增)


## 内置函数(原生函数)
* String
* Number
* Boolean
* Object
* Function
* Array
* Date
* RegExp
* Error
* Symbol

原始值 "I am a string" 并不是一个对象，它只是一个字面量，并且是一个不可变的值。
如果要在这个字面量上执行一些操作，比如获取长度、访问其中某个字符等，那需要将其
转换为 String 对象。
幸好，在必要时语言会自动把字符串字面量转换成一个 String 对象，也就是说你并不需要
显式创建一个对象。

## 域名收敛
PC 时代为了突破浏览器的域名并发限制。有了域名发散。<br>
浏览器有并发限制，是为了防止DDOS攻击。<br>
域名收敛：就是将静态资源放在一个域名下。减少DNS解析的开销。<br>
域名发散：是将静态资源放在多个子域名下，就可以多线程下载，提高并行度，使客户端加载静态资源更加迅速。<br>
域名发散是pc端为了利用浏览器的多线程并行下载能力。而域名收敛多用与移动端，提高性能，因为dns解析是是从后向前迭代解析，如果域名过多性能会下降，增加DNS的解析开销。


## 首屏时间、白屏时间
Performance 接口可以获取到当前页面中与性能相关的信息。<br>
该类型的对象可以通过调用只读属性 Window.performance 来获得。<br>
白屏时间：
```
performance.timing.responseStart - performance.timing.navigationStart
```
首屏时间
```
window.onload = () => {
    new Date() - performance.timing.responseStart
}
```
https://developer.mozilla.org/zh-CN/docs/Web/API/Performance

## 当你在浏览器输入一个地址后发生了什么
https://github.com/skyline75489/what-happens-when-zh_CN/blob/master/README.rst?utm_medium=social&utm_source=wechat_session&from=timeline&isappinstalled=0


## 自动分号
有时 JavaScript 会自动为代码行补上缺失的分号，即自动分号插入（Automatic SemicolonInsertion，ASI）。<br>
因为如果缺失了必要的 ; ，代码将无法运行，语言的容错性也会降低。ASI 能让我们忽略那些不必要的 ; 。<br>
请注意，ASI 只在换行符处起作用，而不会在代码行的中间插入分号。<br>
如果 JavaScript 解析器发现代码行可能因为缺失分号而导致错误，那么它就会自动补上分
号。并且，只有在代码行末尾与换行符之间除了空格和注释之外没有别的内容时，它才会
这样做。

## 浮点数精度
https://www.css88.com/archives/7340

## cookie、localStorage、sessionStorage区别
|特性 | cookie | localStorage | sessionStorage|
|-|-|-|-|
|数据的生命周期|一般由服务器生成，可设置失效时间，如果在浏览器生成，默认是关闭浏览器之后失效  |永久保存，可清除  | 仅在当前会话有效，关闭页面后清除|
|存放数据大小|4KB|5MB|5MB|
|与服务器通信|每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题|仅在客户端保存|仅在客户端保存|
|用途|一般由服务器生成，用于标识用户身份|用于浏览器缓存数据|用于浏览器缓存数据|

## 自执行函数?用于什么场景？好处?
自执行函数:1、声明一个匿名函数2、马上调用这个匿名函数。<br>
作用：创建一个独立的作用域。<br>


好处：防止变量弥散到全局，以免各种js库冲突。隔离作用域避免污染，或者截断作用域链，避免闭包造成引用变量无法释放。利用立即执行特性，返回需要的业务函数或对象，避免每次通过条件判断来处理<br>


场景：一般用于框架、插件等场景

## 实现add函数,让add(a)(b)和add(a,b)两种调用结果相同
```
function add(a, b) {
    if (b === undefined) {
        return function(x) {
            return a + x
        }
    }

    return a + b
}
```
#### [回到顶部](#JavaScript)

## js中万物皆对象你认为对么
我认为是对的
在JS中有原生函数、基本数据类型，它们的原型最终还是对象。
可以看看本文的原生函数


#### [回到顶部](#JavaScript)

## 多个页面之间如何进行通信
有如下几个方式：
* cookie
* web worker
* localeStorage和sessionStorage

#### [回到顶部](#JavaScript)

## css动画和js动画的差异
1. 代码复杂度，js 动画代码相对复杂一些
2. 动画运行时，对动画的控制程度上，js 能够让动画，暂停，取消，终止，css动画不能添加事件
3. 动画性能看，js 动画多了一个js 解析的过程，性能不如 css 动画好

https://zhuanlan.zhihu.com/p/41479807

#### [回到顶部](#JavaScript)

## 请用js去除字符串空格
### 去除所有空格
```
str.replace(/\s/g, '')
```
### 去除两边空格
```
str.replace(/^\s+|\s+$/g, '')
// 原生方法
str.trim()
```
#### [回到顶部](#JavaScript)

## new一个对象经历了什么
```
function Test(){}
const test = new Test()
```

1. 创建一个新对象：
```
const obj = {}
```
2. 设置新对象的constructor属性为构造函数的名称，设置新对象的__proto__属性指向构造函数的prototype对象
```
obj.constructor = Test
obj.__proto__ = Test.prototype
```
3. 使用新对象调用函数，函数中的this被指向新实例对象
```
Test.call(obj)
```
4. 将初始化完毕的新对象地址，保存到等号左边的变量中

#### [回到顶部](#JavaScript)

## 如何实现文件断点续传
断点续传最核心的内容就是把文件“切片”然后再一片一片的传给服务器，但是这看似简单的上传过程却有着无数的坑。

首先是文件的识别，一个文件被分成了若干份之后如何告诉服务器你切了多少块，以及最终服务器应该如何把你上传上去的文件进行合并，这都是要考虑的。

因此在文件开始上传之前，我们和服务器要有一个“握手”的过程，告诉服务器文件信息，然后和服务器约定切片的大小，当和服务器达成共识之后就可以开始后续的文件传输了。

前台要把每一块的文件传给后台，成功之后前端和后端都要标识一下，以便后续的断点。

当文件传输中断之后用户再次选择文件就可以通过标识来判断文件是否已经上传了一部分，如果是的话，那么我们可以接着上次的进度继续传文件，以达到续传的功能。
有了HTML5 的 File api之后切割文件比想想的要简单的多的多。

只要用slice 方法就可以了
```
var packet = file.slice(start, end);
```
参数start是开始切片的位置，end是切片结束的位置 单位都是字节。通过控制start和end 就可以是实现文件的分块

如
```
file.slice(0,1000);
file.slice(1000,2000);
file.slice(2000,3000);
// ......
```
在把文件切成片之后，接下来要做的事情就是把这些碎片传到服务器上。
如果中间掉线了，下次再传的时候就得先从服务器获取上一次上传文件的位置，然后以这个位置开始上传接下来的文件内容。

https://www.cnblogs.com/zhwl/p/3580776.html

#### [回到顶部](#JavaScript)

## bind、call、apply的区别
call和apply其实是一样的，区别就在于传参时参数是一个一个传或者是以一个数组的方式来传。<br>
call和apply都是在调用时生效，改变调用者的this指向。<br>
```
let name = 'Jack'
const obj = {name: 'Tom'}
function sayHi() {console.log('Hi! ' + this.name)}

sayHi() // Hi! Jack
sayHi.call(obj) // Hi! Tom

```
bind也是改变this指向，不过不是在调用时生效，而是返回一个新函数。
```
const newFunc = sayHi.bind(obj)
newFunc() // Hi! Tom
```
#### [回到顶部](#JavaScript)

## 请简述`JavaScript`中的`this`。

JS 中的`this`是一个相对复杂的概念，不是简单几句能解释清楚的。粗略地讲，函数的调用方式决定了`this`的值。我阅读了网上很多关于`this`的文章，[Arnav Aggrawal](https://medium.com/@arnav_aggarwal) 写的比较清楚。`this`取值符合以下规则：

1. 在调用函数时使用`new`关键字，函数内的`this`是一个全新的对象。
1. 如果`apply`、`call`或`bind`方法用于调用、创建一个函数，函数内的 this 就是作为参数传入这些方法的对象。
1. 当函数作为对象里的方法被调用时，函数内的`this`是调用该函数的对象。比如当`obj.method()`被调用时，函数内的 this 将绑定到`obj`对象。
1. 如果调用函数不符合上述规则，那么`this`的值指向全局对象（global object）。浏览器环境下`this`的值指向`window`对象，但是在严格模式下(`'use strict'`)，`this`的值为`undefined`。
1. 如果符合上述多个规则，则较高的规则（1 号最高，4 号最低）将决定`this`的值。
1. 如果该函数是 ES2015 中的箭头函数，将忽略上面的所有规则，`this`被设置为它被创建时的上下文。

想获得更深入的解释，请查看[他在 Medium 上的文章](https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3)。

https://github.com/yangshun/front-end-interview-handbook/blob/master/Translations/Chinese/questions/javascript-questions.md#%E8%AF%B7%E7%AE%80%E8%BF%B0javascript%E4%B8%AD%E7%9A%84this

#### [回到顶部](#JavaScript)

## ==和===的区别是什么
`==`是抽象相等运算符，而`===`是严格相等运算符。`==`运算符是在进行必要的类型转换后，再比较。`===`运算符不会进行类型转换，所以如果两个值不是相同的类型，会直接返回`false`。使用`==`时，可能发生一些特别的事情，例如：
```js
1 == '1'; // true
1 == [1]; // true
1 == true; // true
0 == ''; // true
0 == '0'; // true
0 == false; // true
```
如果你对`==`和`===`的概念不是特别了解，建议大多数情况下使用`===`

#### [回到顶部](#JavaScript)
