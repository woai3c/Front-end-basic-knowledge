
* [同源策略](#同源策略)
* [跨域资源共享CORS](#跨域资源共享)
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
* [变量提升](#变量提升)
* [对象深拷贝](#对象深拷贝)
* [数组去重](#数组去重)
* [数据类型](#数据类型)
* [内置函数(原生函数)](#内置函数原生函数)
* [prototype和__proto__](#prototype和__proto__)
* [域名收敛](#域名收敛)
* [首屏时间、白屏时间](#首屏时间白屏时间)
* [当你在浏览器输入一个地址后发生了什么](#当你在浏览器输入一个地址后发生了什么)
* [自动分号](#自动分号)
* [浮点数精度](#浮点数精度)

## 同源策略
同源策略可防止 JavaScript 发起跨域请求。源被定义为 URI、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。


## 跨域资源共享
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
虽然sayHi函数已经执行完毕，但是其活动对象也不会被销毁，因为test函数仍然引用着sayHi函数中的变量name，这就是闭包。


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

## 变量提升
var会使变量提升，这意味着变量可以在声明之前使用。let和const不会使变量提升，提前使用会报错。
变量提升（hoisting）是用于解释代码中变量声明行为的术语。使用var关键字声明或初始化的变量，会将声明语句“提升”到当前作用域的顶部。 但是，只有声明才会触发提升，赋值语句（如果有的话）将保持原样。


## 对象深拷贝
```
let o1 = {a:{
    b:1
  }
}
let o2 = JSON.parse(JSON.stringify(o1))
```


## 数组去重
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

## prototype和__proto__
首先，要明确几个点：
1. 在JS里，万物皆对象。函数（Function）是对象，函数的原型(Function.prototype)是对象。因此，它们都会具有对象共有的特点。即：对象具有属性__proto__，可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。

2. 函数(Function)
函数这个特殊的对象，除了和其他对象一样有上述_proto_属性之外，还有自己特有的属性——原型属性（prototype），这个属性是一个指针，指向一个对象，这个对象的用途就是包含所有实例共享的属性和方法（我们把这个对象叫做原型对象）。原型对象也有一个属性，叫做constructor，这个属性包含了一个指针，指回原构造函数。


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
