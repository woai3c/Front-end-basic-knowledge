## 关键渲染路径
关键渲染路径是浏览器将 HTML，CSS 和 JavaScript 转换为屏幕上的像素所经历的步骤序列。优化关键渲染路径可提高渲染性能。关键渲染路径包含了 文档对象模型 (DOM)，CSS 对象模型 (CSSOM)，渲染树和布局。

在解析 HTML 时会创建文档对象模型。HTML 可以请求 JavaScript，而 JavaScript  反过来，又可以更改 DOM。HTML 包含或请求样式，依次来构建 CSS 对象模型。浏览器引擎将两者结合起来以创建渲染树。布局确定页面上所有内容的大小和位置。确定布局后，将像素绘制到屏幕上。

优化关键渲染路径可以缩短首次渲染的时间。了解和优化关键渲染路径对于确保重排和重绘可以每秒 60 帧的速度进行，以确保高效的用户交互并避免讨厌是很重要的。

Web 性能包含了服务器请求和响应、加载、执行脚本、渲染、布局和绘制每个像素到屏幕上。

网页请求从 HTML 文件请求开始。服务器返回 HTML -- 响应头和数据。然后浏览器开始解析 HTML，转换收到的数据为 DOM 树。浏览器每次发现外部资源就初始化请求，无论是样式、脚本或者嵌入的图片引用。有时请求会阻塞，这意味着解析剩下的 HTML 会被终止直到重要的资源被处理。浏览器接着解析 HTML，发请求和构造 DOM 直到文件结尾 ，这时开始构造 CSS对象模型。等到 DOM 和 CSSOM 完成之后，浏览器构造渲染树，计算所有可见内容的样式。一旦渲染树完成布局开始，定义所有渲染树元素的位置和大小。完成之后，页面被渲染完成，或者说是绘制到屏幕上。

* [关键渲染路径](https://developer.mozilla.org/zh-CN/docs/Web/Performance/Critical_rendering_path)
* [css 性能优化](https://developer.mozilla.org/zh-CN/docs/Learn/Performance/CSS)

## 浏览器渲染原理
渲染过程
1. 解析HTML生成DOM树。
2. 解析CSS生成CSSOM规则树。
3. 解析JS，操作 DOM 树和 CSSOM 规则树。
4. 将DOM树与CSSOM规则树合并在一起生成渲染树。
5. 遍历渲染树开始布局，计算每个节点的位置大小信息。
6. 浏览器将所有图层的数据发送给GPU，GPU将图层合成并显示在屏幕上。

* [浏览器渲染原理](https://developer.mozilla.org/zh-CN/docs/Web/Performance/How_browsers_work)

## DOM 树和渲染树区别
DOM 树包含了 HTML 元素，渲染树则是要绘制到页面上的元素。渲染树可以不包含 DOM 树中的元素，像 `<head>` 和它的子节点以及任何具有 `display: none` 样式的结点

## 浏览器缓存机制
当浏览器向服务器发起请求时，服务器会将缓存规则放入HTTP响应报文的HTTP头中和请求结果一起返回给浏览器，控制强制缓存的字段分别是Expires和Cache-Control，其中Cache-Control优先级比Expires高。

[彻底理解浏览器的缓存机制](https://juejin.cn/post/6844903593275817998)

## Reflow和Repaint
#### Reflow
当涉及到DOM节点的布局属性发生变化时，就会重新计算该属性，浏览器会重新描绘相应的元素，此过程叫Reflow（回流或重排）。
#### Repaint
当影响DOM元素可见性的属性发生变化 (如 color) 时, 浏览器会重新描绘相应的元素, 此过程称为Repaint（重绘）。因此重排必然会引起重绘。
#### 引起Repaint和Reflow的一些操作
* 调整窗口大小
* 字体大小
* 样式表变动
* 元素内容变化，尤其是输入控件
* CSS伪类激活，在用户交互过程中发生
* DOM操作，DOM元素增删、修改
* width, clientWidth, scrollTop等布局宽高的计算

#### Repaint和Reflow是不可避免的，只能说对性能的影响减到最小，给出下面几条建议：
* 避免逐条更改样式。建议集中修改样式，例如操作className。
* 避免频繁操作DOM。创建一个documentFragment或div，在它上面应用所有DOM操作，最后添加到文档里。设置display:none的元素上操作，最后显示出来。
* 避免频繁读取元素几何属性（例如scrollTop）。绝对定位具有复杂动画的元素。
* 绝对定位使它脱离文档流，避免引起父元素及后续元素大量的回流

参考资料：
* [减少页面重排与重绘（Reflow & Repaint）](https://harttle.land/2015/08/11/reflow-repaint.html)
* [页面重构应注意的repaint和reflow](http://www.blueidea.com/tech/web/2011/8365.asp)


## 同源策略
同源策略可防止 JavaScript 发起跨域请求。源被定义为协议、主机名和端口号的组合。此策略可防止页面上的恶意脚本通过该页面的文档对象模型，访问另一个网页上的敏感数据。

下表给出了与 URL http://store.company.com/dir/page.html 的源进行对比的示例:
|URL|	结果|	原因|
|-|-|-|
|http://store.company.com/dir2/other.html|	同源	|只有路径不同|
|http://store.company.com/dir/inner/another.html	|同源	|只有路径不同|
|https://store.company.com/secure.html|	失败	|协议不同|
|http://store.company.com:81/dir/etc.html|	失败	|端口不同 ( http:// 默认端口是80)|
|http://news.company.com/dir/other.html|	失败	|主机不同|

参考资料：
* [浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)



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

参考资料：
* [新鲜出炉的8月前端面试题](https://zhuanlan.zhihu.com/p/41479807)
* [跨域资源共享 CORS 阮一峰](http://www.ruanyifeng.com/blog/2016/04/cors.html)



## JSONP
这是我认为写得比较通俗易懂的一篇文章[jsonp原理详解——终于搞清楚jsonp是啥了](https://blog.csdn.net/hansexploration/article/details/80314948)。



## 域名收敛
PC 时代为了突破浏览器的域名并发限制，有了域名发散。浏览器有并发限制，是为了防止DDOS攻击。

**域名收敛**：就是将静态资源放在一个域名下。减少DNS解析的开销。

**域名发散**：是将静态资源放在多个子域名下，就可以多线程下载，提高并行度，使客户端加载静态资源更加迅速。

域名发散是pc端为了利用浏览器的多线程并行下载能力。而域名收敛多用与移动端，提高性能，因为dns解析是是从后向前迭代解析，如果域名过多性能会下降，增加DNS的解析开销。

## 为何会出现浏览器兼容问题
* 同一产品，版本越老 bug 越多
* 同一产品，版本越新，功能越多
* 不同产品，不同标准，不同实现方式
### 处理兼容问题的思路
1. 要不要做
* 产品的角度（产品的受众、受众的浏览器比例、效果优先还是基本功能优先）
* 成本的角度 (有无必要做某件事)

2.做到什么程度
* 让哪些浏览器支持哪些效果

3..如何做
* 根据兼容需求选择技术框架/库(jquery)
* 根据兼容需求选择兼容工具(html5shiv.js、respond.js、css reset、normalize.css、Modernizr)
* 条件注释、CSS Hack、js 能力检测做一些修补

* 渐进增强(progressive enhancement): 针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验
* 优雅降级 (graceful degradation): 一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

参考资料：
* [为何会出现浏览器兼容性问题？如何解决？](https://github.com/jirengu/frontend-interview/issues/35)


## 为什么最好把 CSS 的`<link>`标签放在`<head></head>`之间？为什么最好把 JS 的`<script>`标签恰好放在`</body>`之前，有例外情况吗？

**把`<link>`放在`<head>`中**

这种做法可以让页面逐步呈现，提高了用户体验。将样式表放在文档底部附近，会使许多浏览器（包括 Internet Explorer）不能逐步呈现页面。一些浏览器会阻止渲染，以避免在页面样式发生变化时，重新绘制页面中的元素。这种做法可以防止呈现给用户空白的页面或没有样式的内容。
 
**PS**：等待加载 CSS 不会阻塞 DOM 的解析，但会阻塞 DOM 渲染。另外，它也会阻塞 JavaScript，因为 JavaScript 经常用于查询元素的 CSS 属性。

**把`<script>`标签恰好放在`</body>`之前**

脚本在下载和执行期间会阻止 HTML 解析。把`<script>`标签放在底部，保证 HTML 首先完成解析，将页面尽早呈现给用户。

如果一定要放在 `<head>` 中，可以让 `<script>` 标签使用 `defer` 属性。

参考资料：
* [使用 JavaScript 添加交互](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript)

## 什么是渐进式渲染（progressive rendering）？
渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。

在以前互联网带宽较小的时期，这种技术更为普遍。如今，移动终端的盛行，而移动网络往往不稳定，渐进式渲染在现代前端开发中仍然有用武之地。

一些举例：

* 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
* 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听`DOMContentLoaded`/`load`事件加载其他资源和内容。
* 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。更多相关细节可以在[这里](http://www.ebaytechblog.com/2014/12/08/async-fragments-rediscovering-progressive-html-rendering-with-marko/)找到。


## load、DOMContentLoaded等等事件的触发顺序
[页面生命周期](https://github.com/fi3ework/blog/issues/3)

## 前端持久化的方式、区别
* [cookie、localStorage、sessionStorage区别](https://github.com/woai3c/Front-end-basic-knowledge/blob/master/JavaSciprt.md#cookielocalStoragesessionStorage%E5%8C%BA%E5%88%AB)
* [indexDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)
