# 微前端

## 作用与背景

单页面应用的广泛使用带来新的问题，复杂应用需要拆分模块。复杂应用打包时间长，而无法满足上线需求，需要拆包进行。
最早的解决方案就是 iframe，功能主要模块拆分规模化应用，子应用之间使用跳转。但 iframe 方案需要重新加载页面导致白屏。
[微前端与 iframe 的优缺点](https://www.yuque.com/kuitos/gky7yw/gesexv)

## 概念与特点

微前端是一个类似于微服务的架构。由一个主应用和多个子应用组成，特点是**独立开发、独立运行、独立部署**

## 实现

- 基于 sinle-Spa 的 qiankun
- 基于 WebComponent 的 MicroAPP
- 基于 webpack5 的 ModuleFederation

### Single-SPA

single-spa 是一个很好的微前端基础框架，而**qiankun 框架就是基于 single-spa 来实现的**，在 single-spa 的基础上做了一层封装，也解决了 single-spa 的一些缺陷。
singlie-spa 中主应用需要在初始化时注册所有 App 的路由并保存各子应用的路由映射关系，这个映射关系充当路由表，当 url 变化时找到子应用对应路由并渲染。
single-spa 中子应用需要导出自己的生命周期供主应用触发

#### qiankun

基于 single-spa，封装提供系列 api 实现微前端，主要技术由样式隔离、js 隔离、HTML-ENtry 等

```js
registerMicroApps([
  {
    name: "react app", // app name registered
    entry: "//localhost:7100",
    container: "#yourContainer",
    activeRule: "/yourActiveRule",
  },
  {
    name: "vue app",
    entry: { scripts: ["//localhost:7100/main.js"] },
    container: "#yourContainer2",
    activeRule: "/yourActiveRule2",
  },
]);
```

**同 single-spa 类似**，qiankun 会注册所有子应用的 activeRule 组成 container，在路由变化时触发对应映射并执行子应用的生命周期。
**与 single-spa 不同的是**，qiankun 的子应用不需要做额外的操作，只需要在入口文件手动暴露自己的生命周期函数并配置对应打包工具即可。

优点：

- 监听路由自动的加载、卸载当前路由对应的子应用
- 完备的沙箱方案，js 沙箱做了 SnapshotSandbox、LegacySandbox、ProxySandbox 三套渐进增强方案，css 沙箱做了两套 strictStyleIsolation、experimentalStyleIsolation 两套适用不同场景的方案
- 路由保持，浏览器刷新、前进、后退，都可以作用到子应用
- 应用间通信简单，全局注入
  缺点：
- 基于路由匹配，无法同时激活多个子应用，也不支持子应用保活
- 改造成本较大，从 webpack、代码、路由等等都要做一系列的适配
- css 沙箱无法绝对的隔离，js 沙箱在某些场景下执行性能下降严重
- 无法支持 vite 等 ESM 脚本运行

### WebComponent 与 MicroAPP

#### WebComponent

MDNDefine:WebComponent 是 HTML5 提供的一套自定义元素的接口，WebComponent 是一套不同的技术，允许您创建可重用的定制元素（它们的功能封装在您的代码之外）并且在您的 web 应用中使用它们。
相关概念：

- Custom elements（自定义元素）： 一组 JavaScript API，允许您定义 custom elements 及其行为，然后可以在您的用户界面中按照需要使用它们。
- Shadow DOM（影子 DOM） ：一组 JavaScript API，用于将封装的“影子”DOM 树附加到元素（与主文档 DOM 分开呈现）并控制其关联的功能。通过这种方式，您可以保持元素的功能私有，这样它们就可以被脚本化和样式化，而不用担心与文档的其他部分发生冲突。
- HTML templates（HTML 模板）：  <template>  和  <slot>  元素使您可以编写不在呈现页面中显示的标记模板。然后它们可以作为自定义元素结构的基础被多次重用。

#### Micro-APP

所有功能都封装到一个类 WebComponent 组件中，从而实现在基座应用中嵌入一行代码即可渲染一个微前端应用。
同时 micro-app 还提供了 js 沙箱、样式隔离、元素隔离、预加载、数据通信、静态资源补全等一系列完善的功能。
