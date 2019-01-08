# HTML

* [HTML5语义化](#html5语义化)
* [`cookie`、`sessionStorage`和`localStorage`的区别](#cookiesessionstorage和localstorage的区别)
* [为什么最好把 CSS 的`<link>`标签放在`<head></head>`之间？为什么最好把 JS 的`<script>`标签恰好放在`</body>`之前，有例外情况吗？](#为什么最好把css的link标签放在headhead之间为什么最好把js的script标签恰好放在body之前有例外情况吗)
* [什么是渐进式渲染（progressive rendering）](#什么是渐进式渲染progressive-rendering)
* [viewport](#viewport)
* [Reflow和Repaint](#Reflow和Repaint)

## HTML5语义化
什么是语义化？就是用合理、正确的标签来展示内容，比如h1~h6定义标题。
#### 好处
* 易于用户阅读，样式丢失的时候能让页面呈现清晰的结构。
* 有利于SEO，搜索引擎根据标签来确定上下文和各个关键字的权重。
* 方便其他设备解析，如盲人阅读器根据语义渲染网页
* 有利于开发和维护，语义化更具可读性，代码更好维护，与CSS3关系更和谐。
http://www.daqianduan.com/6549.html


## `cookie`、`sessionStorage`和`localStorage`的区别。
上面提到的技术名词，都是在客户端以键值对存储的存储机制，并且只能将值存储为字符串。

|                                                    | `cookie`                                           | `localStorage` | `sessionStorage` |
| -------------------------------------------------- | -------------------------------------------------- | -------------- | ---------------- |
| 由谁初始化                                         | 客户端或服务器，服务器可以使用`Set-Cookie`请求头。 | 客户端         | 客户端           |
| 过期时间                                           | 手动设置                                           | 永不过期       | 当前页面关闭时   |
| 在当前浏览器会话（browser sessions）中是否保持不变 | 取决于是否设置了过期时间                           | 是             | 否               |
| 是否随着每个 HTTP 请求发送给服务器                 | 是，Cookies 会通过`Cookie`请求头，自动发送给服务器 | 否             | 否               |
| 容量（每个域名）                                   | 4kb                                                | 5MB            | 5MB              |
| 访问权限                                           | 任意窗口                                           | 任意窗口       | 当前页面窗口     |


## 为什么最好把 CSS 的`<link>`标签放在`<head></head>`之间？为什么最好把 JS 的`<script>`标签恰好放在`</body>`之前，有例外情况吗？

**把`<link>`放在`<head>`中**

把`<link>`标签放在`<head></head>`之间是规范要求的内容。此外，这种做法可以让页面逐步呈现，提高了用户体验。将样式表放在文档底部附近，会使许多浏览器（包括 Internet Explorer）不能逐步呈现页面。一些浏览器会阻止渲染，以避免在页面样式发生变化时，重新绘制页面中的元素。这种做法可以防止呈现给用户空白的页面或没有样式的内容。

**把`<script>`标签恰好放在`</body>`之前**

脚本在下载和执行期间会阻止 HTML 解析。把`<script>`标签放在底部，保证 HTML 首先完成解析，将页面尽早呈现给用户。

例外情况是当你的脚本里包含`document.write()`时。但是现在，`document.write()`不推荐使用。同时，将`<script>`标签放在底部，意味着浏览器不能开始下载脚本，直到整个文档（document）被解析。也许，对此比较好的做法是，`<script>`使用`defer`属性，放在`<head>`中。


## 什么是渐进式渲染（progressive rendering）？
渐进式渲染是用于提高网页性能（尤其是提高用户感知的加载速度），以尽快呈现页面的技术。

在以前互联网带宽较小的时期，这种技术更为普遍。如今，移动终端的盛行，而移动网络往往不稳定，渐进式渲染在现代前端开发中仍然有用武之地。

一些举例：

* 图片懒加载——页面上的图片不会一次性全部加载。当用户滚动页面到图片部分时，JavaScript 将加载并显示图像。
* 确定显示内容的优先级（分层次渲染）——为了尽快将页面呈现给用户，页面只包含基本的最少量的 CSS、脚本和内容，然后可以使用延迟加载脚本或监听`DOMContentLoaded`/`load`事件加载其他资源和内容。
* 异步加载 HTML 片段——当页面通过后台渲染时，把 HTML 拆分，通过异步请求，分块发送给浏览器。更多相关细节可以在[这里](http://www.ebaytechblog.com/2014/12/08/async-fragments-rediscovering-progressive-html-rendering-with-marko/)找到。


## viewport
Viewport ：字面意思为视图窗口，在移动web开发中使用。表示将设备浏览器宽度虚拟成一个特定的值（或计算得出），这样利于移动web站点跨设备显示效果基本一致。移动版的 Safari 浏览器最新引进了 viewport 这个 meta tag，让网页开发者来控制 viewport 的大小和缩放，其他手机浏览器也基本支持。

在移动端浏览器当中，存在着两种视口，一种是可见视口（也就是我们说的设备大小），另一种是视窗视口（网页的宽度是多少）。
举个例子：如果我们的屏幕是320像素 * 480像素的大小（iPhone4），假设在浏览器中，320像素的屏幕宽度能够展示980像素宽度的内容。那么320像素的宽度就是可见视口的宽度，而能够显示的980像素的宽度就是视窗视口的宽度。

为了显示更多的内容，大多数的浏览器会把自己的视窗视口扩大，简易的理解，就是让原本320像素的屏幕宽度能够容下980像素甚至更宽的内容（将网页等比例缩小）。

Viewport属性值

width	设置layout viewport 的宽度，为一个正整数，或字符串"width-device"

initial-scale	设置页面的初始缩放值，为一个数字，可以带小数

minimum-scale	允许用户的最小缩放值，为一个数字，可以带小数

maximum-scale	允许用户的最大缩放值，为一个数字，可以带小数

height	设置layout viewport 的高度，这个属性对我们并不重要，很少使用

user-scalable	是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

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
[回到顶部](#HTML)

#### Repaint和Reflow是不可避免的，只能说对性能的影响减到最小，给出下面几条建议：
* 避免逐条更改样式。建议集中修改样式，例如操作className。
* 避免频繁操作DOM。创建一个documentFragment或div，在它上面应用所有DOM操作，最后添加到文档里。设置display:none的元素上操作，最后显示出来。
* 避免频繁读取元素几何属性（例如scrollTop）。绝对定位具有复杂动画的元素。
* 绝对定位使它脱离文档流，避免引起父元素及后续元素大量的回流

https://harttle.land/2015/08/11/reflow-repaint.html<br>
http://www.blueidea.com/tech/web/2011/8365.asp
[回到顶部](#HTML)
