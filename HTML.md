# HTML

* [HTML5语义化](#html5语义化)
* [`cookie`、`sessionStorage`和`localStorage`的区别](#cookiesessionstorage和localstorage的区别)
* [为什么最好把 CSS 的`<link>`标签放在`<head></head>`之间？为什么最好把 JS 的`<script>`标签恰好放在`</body>`之前，有例外情况吗？](#为什么最好把css的link标签放在headhead之间为什么最好把js的script标签恰好放在body之前有例外情况吗)
* [什么是渐进式渲染（progressive rendering）](#什么是渐进式渲染progressive-rendering)
* [viewport](#viewport)
* [Reflow和Repaint](#Reflow和Repaint)
* [img中的alt和元素的title属性作用](#img中的alt和元素的title属性作用)
* [href和src区别](#href和src区别)
* [浏览器的渲染过程](#浏览器的渲染过程)
* [为何会出现浏览器兼容问题](#为何会出现浏览器兼容问题)
* [doctype有什么用](#doctype有什么用)

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

### Viewport属性值

* width	设置layout viewport 的宽度，为一个正整数，或字符串"width-device"
* initial-scale	设置页面的初始缩放值，为一个数字，可以带小数
* minimum-scale	允许用户的最小缩放值，为一个数字，可以带小数
* maximum-scale	允许用户的最大缩放值，为一个数字，可以带小数
* height	设置layout viewport 的高度，这个属性对我们并不重要，很少使用
* user-scalable	是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许这些属性可以同时使用，也可以单独使用或混合使用，多个属性同时使用时用逗号隔开就行了。

[回到顶部](#HTML)



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

https://harttle.land/2015/08/11/reflow-repaint.html<br>
http://www.blueidea.com/tech/web/2011/8365.asp

[回到顶部](#HTML)

## img中的alt和元素的title属性作用
* img的alt属性<br>
如果无法显示图像，浏览器将显示alt指定的内容
* 元素title属性<br>
在鼠标移到元素上时显示title的内容

[回到顶部](#HTML)

## href和src区别
* href <br>
href标识超文本引用，用在link和a等元素上，href是引用和页面关联，是在当前元素和引用资源之间建立联系<br>
若在文档中添加href ，浏览器会识别该文档为 CSS 文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用 link 方式加载 CSS，而不是使用 @import 方式。
* src <br>
src表示引用资源，替换当前元素，用在img，script，iframe上，src是页面内容不可缺少的一部分。<br>
当浏览器解析到src ，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等也如此，类似于将所指向资源应用到当前内容。这也是为什么建议把 js 脚本放在底部而不是头部的原因。

https://blog.csdn.net/lhjuejiang/article/details/80795081

[回到顶部](#HTML)

## 浏览器的渲染过程
1. 解析HTML生成DOM树。
2. 解析CSS生成CSSOM规则树。
3. 将DOM树与CSSOM规则树合并在一起生成渲染树。
4. 遍历渲染树开始布局，计算每个节点的位置大小信息。
5. 将渲染树每个节点绘制到屏幕。

https://baijiahao.baidu.com/s?id=1593097105869520145&wfr=spider&for=pc

[回到顶部](#HTML)

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

https://github.com/jirengu/frontend-interview/issues/35

[回到顶部](#HTML)

## doctype有什么用
doctype是一种标准通用标记语言的文档类型声明，目的是告诉标准通用标记语言解析器要使用什么样的文档类型定义（DTD）来解析文档。

<!DOCTYPE>声明是用来指示web浏览器关于页面使用哪个HTML版本进行编写的指令。

<!DOCTYPE>声明必须是HTML文档的第一行，位于html标签之前。


浏览器本身分为两种模式，一种是标准模式，一种是怪异模式，浏览器通过doctype来区分这两种模式，doctype在html中的作用就是触发浏览器的标准模式，如果html中省略了doctype，浏览器就会进入到Quirks模式的怪异状态，在这种模式下，有些样式会和标准模式存在差异，而html标准和dom标准值规定了标准模式下的行为，没有对怪异模式做出规定，因此不同浏览器在怪异模式下的处理也是不同的，所以一定要在html开头使用doctype。


[回到顶部](#HTML)
