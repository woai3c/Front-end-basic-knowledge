# Webpack
* [webpack 和 rollup 的区别](#webpack-和-rollup-的区别)
* [webpack 的 runtime 和 manifest 代码有什么用](#webpack-的-runtime-和-manifest-代码有什么用)
* [怎么写一个 plugin 和 loader](#怎么写一个-plugin-和-loader)
* [webpack 能做哪些性能优化](#webpack-能做哪些性能优化)
* [webpack 热更新原理](#webpack-热更新原理)
* [webpack 模块加载原理](#webpack-模块加载原理)

## webpack 和 rollup 的区别
webpack 优点：
1. 通过loader处理各种各样的资源依赖
2. HMR模块热替换
3. 按需加载
4. 提取公共模块

rollup 优点：
1. 编译出来的代码`可读性好`
2. rollup打包后生成的bundle内容十分`干净`，没有什么多余的代码。相比webpack(webpack打包后会生成__webpack_require__等runtime代码)，rollup拥有无可比拟的性能优势，这是由依赖处理方式决定的，`编译时依赖处理（rollup）自然比运行时依赖处理（webpack）性能更好`
3. 对于ES模块依赖库，rollup会静态分析代码中的 import，并将排除任何未实际使用的代码
4. 支持程序流分析，能更加正确的判断项目本身的代码是否有副作用(配合tree-shaking)
5. 支持导出`es`模块文件（webpack不支持导出es模块）

参考资料：
* [【第九期】Rollup：下一代ES模块打包工具](https://zhuanlan.zhihu.com/p/75717476)
#### [回到顶部](#webpack)

## webpack 的 runtime 和 manifest 代码有什么用
runtime：根据 manifest 数据来管理模块代码。主要是指模块交互时，连接模块所需的加载和解析逻辑。
包括：已经加载到浏览器中的连接模块逻辑，以及尚未加载模块的延迟加载逻辑。

manifest：记录了在打包过程中，各个模块之间的信息及关联关系。 

参考资料：
* [manifest](https://webpack.docschina.org/concepts/manifest/)

#### [回到顶部](#webpack)

## 怎么写一个 plugin 和 loader
[实现一个 webpack loader 和 webpack plugin](https://github.com/woai3c/Front-end-articles/issues/6)

#### [回到顶部](#webpack)

## webpack 能做哪些性能优化
1. 压缩代码
2. tree-shaking
3. 根据文件内容生成 hash 当作文件名，配合 CDN 做文件缓存
4. 分割代码，按需加载
5. 将第三方插件或公共代码单独提取出来打包

#### [回到顶部](#webpack)

## webpack 热更新原理
HMR 即 Hot Module Replacement是指当你对代码修改并保存后，webpack将会对代码进行重新打包，并将改动的模块发送到浏览器端。

浏览器用新的模块替换掉旧的模块，去实现局部更新页面而非整体刷新页面。

![](https://user-gold-cdn.xitu.io/2019/9/2/16cf203824359397?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

如上图所示，右侧Server端使用webpack-dev-server去启动本地服务，内部实现主要使用了webpack、express、websocket。

- 使用express启动本地服务，当浏览器访问资源时对此做响应。
- 服务端和客户端使用websocket实现长连接
- webpack监听源文件的变化，即当开发者保存文件时触发webpack的重新编译。
  - 每次编译都会生成hash值、已改动模块的json文件、已改动模块代码的js文件
  - 编译完成后通过socket向客户端推送当前编译的hash戳
- 客户端的websocket监听到有文件改动推送过来的hash戳，会和上一次对比
  - 一致则走缓存
  - 不一致则通过ajax和jsonp向服务端获取最新资源
- 使用内存文件系统去替换有修改的内容实现局部刷新


参考资料：
* [搞懂webpack热更新原理](https://juejin.im/post/6844903933157048333)

#### [回到顶部](#webpack)

## webpack 模块加载原理


[深入了解 webpack 模块加载原理](https://github.com/woai3c/Front-end-articles/issues/7)
#### [回到顶部](#webpack)

## 其他参考资料

* [code-splitting 代码切割](https://github.com/youngwind/blog/issues/100)
* [loader 机制](https://github.com/youngwind/blog/issues/101)
* [Webpack 源码解析](https://github.com/lihongxun945/diving-into-webpack)
