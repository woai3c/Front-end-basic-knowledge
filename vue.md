# Vue

* [什么是mvvm](#什么是mvvm)

## 什么是mvvm
MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离
把Model和View关联起来的就是ViewModel。<br>
ViewModel负责把Model的数据同步到View显示出来，还负责把View的修改同步回Model<br>
View 和 Model 之间的同步工作完全是自动的，无需人为干涉（由viewModel完成，在这里指VUE）<br>
因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理<br>

#### ViewModel如何编写？

需要用JavaScript编写一个通用的ViewModel，这样，就可以复用整个MVVM模型了

一个MVVM框架和jQuery操作DOM相比有什么区别？
我们先看用jQuery实现的修改两个DOM节点的例子：
```
<!-- HTML -->
<p>Hello, <span id="name">Bart</span>!</p>
<p>You are <span id="age">12</span>.</p>

Hello, Bart!

You are 12.
```
用jQuery修改name和age节点的内容：
```
var name = 'Homer';
var age = 51;

$('#name').text(name);
$('#age').text(age);
```

如果我们使用MVVM框架来实现同样的功能，我们首先并不关心DOM的结构，而是关心数据如何存储。最简单的数据存储方式是使用JavaScript对象：
```
var person = {
    name: 'Bart',
    age: 12
}
```
我们把变量person看作Model，把HTML某些DOM节点看作View，并假定它们之间被关联起来了。

要把显示的name从Bart改为Homer，把显示的age从12改为51，我们并不操作DOM，而是直接修改JavaScript对象：
```
person.name = 'Homer';
person.age = 51;
```

执行上面的代码，我们惊讶地发现，改变JavaScript对象的状态，会导致DOM结构作出对应的变化！这让我们的关注点从如何操作DOM变成了如何更新JavaScript对象的状态，而操作JavaScript对象比DOM简单多了！

这就是MVVM的设计思想：关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把开发者从操作DOM的繁琐步骤中解脱出来！
下图可以很好的解释view viewModel model之间的关系
![mvvm](https://github.com/woai3c/mini-vue/blob/master/imgs/mvvm.jpg)
