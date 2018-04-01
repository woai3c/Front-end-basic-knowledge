
* [同源策略](#同源策略)
* [跨域资源共享CORS](#跨域资源共享)
* [事件委托](#事件委托)
* [原型继承](#原型继承)
* [闭包](#闭包)
* [Ajax](#Ajax)

## 同源策略
同源策略可防止 JavaScript 发起跨域请求。源被定义为 URI、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。


## 跨域资源共享
[跨域资源共享 CORS 阮一峰](http://www.ruanyifeng.com/blog/2016/04/cors.html)


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


## 原型继承
所有的JS对象都有一个prototype属性，指向它的原型对象。当试图访问一个对象的属性时，如果没有在该对象上找到，它还会搜寻该对象的原型，以及该对象的原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或到达原型链的末尾。


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
Ajax（asynchronous JavaScript and XML）是使用客户端上的许多 Web 技术，创建异步 Web 应用的一种 Web 开发技术。借助 Ajax，Web 应用可以异步（在后台）向服务器发送数据和从服务器检索数据，而不会干扰现有页面的显示和行为。通过将数据交换层与表示层分离，Ajax 允许网页和扩展 Web 应用程序动态更改内容，而无需重新加载整个页面。实际上，现在通常将 JSON 替换为 XML，因为 JavaScript 对 JSON 有原生支持优势。<br>
XMLHttpRequest API 经常用于异步通信。此外还有最近流行的fetch API。
