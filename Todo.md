# Todo

- [ ] 手写Redux
- [ ] 复习DP背包问题







# Books

- [x] 大前端架构
- [x] 整洁代码之道
- [ ] 重构（第2版）
- [ ] 算法小抄（抄袭拼凑，欺世盗名，非原创） 
- [ ] 设计模式 https://refactoringguru.cn/design-patterns/catalog



# SVG

**SVG 分层概念**  svg中一半用 <g> 标签来表示图层，分组的概念。svg中不可以用 z-index 属性调节层叠顺序，只能通过JavaScript来动态改变SVG的元素顺序

**SVG 阴影** [SVG Shadows Example](https://codepen.io/pnowell/pen/eJbaeNthe)  feOffset element moves the shadow down 4px, the feGaussianBlur blurs it, the feColorMatrix makes it black and lowers the opacity, and feMergeNode combines the shadow with the element that the shadow belongs to.

**SVG编辑器的点击事件**兼容pc端和移动端[方案](https://www.jianshu.com/p/2e46a55f00dad3)  [touchstart mousedown test](https://codepen.io/toneworm/pen/Gontm) 

**SVG监听键盘事件** Because SVG is not an input-type, listen for the event on the window instead可以选择直接绑定在 svg 元素上，但需要设置foucusable属性，已经手动更新 focus 状态但focus被触发后，event.target.nodeName 才能拿到 svg ，不然直接落在 document 或 body 无法区别

**SVG支持多语言文字** （英文环境已经调整完毕，但中文字体的话内容溢出） 主要还是处理不同字体引起的行高变化问题，加了padding在textblockHeight内部解决

**SVG页面自动滚动**

**SVG Text 换行问题**

- [x] [\<textPath>](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/textPath) 
- [x] [彻底搞懂word-break、word-wrap、white-space](https://juejin.im/post/6844903667863126030) 
- [x] [CSSで改行ルールを簡単設定](https://www.sejuku.net/blog/75317) 
- [x] [最全的文本溢出截断省略方案合集](https://www.zoo.team/article/text-overflow) 

1. 嵌入foreignObject标签实现（破坏pure svg，IE11不兼容）[[1]](https://qastack.cn/programming/4991171/auto-line-wrapping-in-svg-text)[[2]](https://segmentfault.com/q/1010000008426252/a-1020000008428672)[[3]](https://codepen.io/maxzz/pen/NzBGVE)
2. 自动换行算法自行计算tspan占用宽度（但不支持单词分行，只能做到字符级别）[[1]](https://juejin.im/entry/6844903582072832008)[[2]](http://zaaack.github.io/2018/08/16/svg-auto-wrapped-text-component-for-react/)
3. 用 d3plus.textWrapping   [[1]](https://bl.ocks.org/davelandry/a39f0c3fc52804ee859a)[[2]](https://github.com/alexandersimoes/d3plus/wiki/Text-Wrapping)





​	

# CSS

- [ ] 垂直居中的12种实现方式 https://juejin.im/post/6844903550909153287CSS
- [ ] 垂直居中的7个方法 https://juejin.im/post/6844903839187877895
- [x] CSS 内部分享第2讲主讲 https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks

# JS

- [x] JavaScript 如何实现继承？https://kaiwu.lagou.com/course/courseInfo.htm?courseId=822#/detail/pc?id=7199
- [x] JS继承实现：探究 JS 常见的 6 种继承方式 https://kaiwu.lagou.com/course/courseInfo.htm?courseId=601#/detail/pc?id=6176

# TS

- [x] TypeScript 高级用法 https://zhuanlan.zhihu.com/p/350033675



# React

- [x] React 异步请求https://www.valentinog.com/blog/await-react/  https://blog.logrocket.com/patterns-for-data-fetching-in-react-981ced7e5c56/
- [x] 在REACT中将异步请求抽象为高阶组件(TYPESCRIPT) https://chennima.github.io/with-async-data
- [x] 🚧removeChild not workingreact key-binding throttle key event，即使加了节流函数or防抖函数，依然不能阻止 removeChild 不 work
- [ ] [**React.js 小书**](http://huziketang.mangojuice.top/books/react)
- [ ] [**React-typescript-cheatsheet**](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet  )
- [x] **React 动态设置Component中的 props style属性？**[React inline sytle best practices](https://stackoverflow.com/questions/26882177/react-js-inline-style-best-practices)
- [x] **React Event**  SVG mouseLeave not trigger when mouse move too fast mouseenter和mouseleave各会触发一个动画，当你快速移动鼠标离开的时候，两个动画效果会产生一个执行动画的序列。
  - [x] [React event onMouseLeave not triggered when moving cursor fast](https://stackoverflow.com/questions/31775182/react-event-onmouseleave-not-triggered-when-moving-cursor-fast) 
  - [x] [Moving the mouse: mouseover/out, mouseenter/leave](https://javascript.info/mousemove-mouseover-mouseout-mouseenter-mouseleave) 
- [ ] [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/context/)

## **React** Hooks

- [ ] 30分钟精通React Hooks https://juejin.im/post/6844903709927800846



# Redux 

[Redux 源码专精(17集完整版)](https://www.bilibili.com/video/BV1254y1L7UP) 90 分钟手写一个 Redux & ReactRedux

# Jest

- [x] [**Jest** **react项目Jest+Enzyme单元测试集成至gitlab**](https://juejin.im/post/6844904161494958087) 
- [x] [**使用Jest进行React单元测试**](https://juejin.im/post/6844903654294716423) 
- [x] [**Enzyme** Testing React Component using Enzyme + Jest Part 3: Browser Globals](https://ttfb.test.traveloka.com/testing-react-component-using-enzyme-jest-part-3/) 
- [x] [Coverage in HTML view is broken #9388](https://github.com/facebook/jest/issues/9388) 
- [x] 集成测试：所谓集成测试（Integration Testing），是指对软件中的所有模块按照设计要求进行组装为完整系统后，进行检查和验证。通俗的讲，在前端，集成测试可以理解为对多个模块实现的一个交互完整的交互流程进行测试。jest的运行环境是node.js，这里jest使用jsdom来让我们可以书写dom操作相关的测试逻辑* 单元测试（Unit Test）有 Mocha, Ava, Karma, Jest, Jasmine 等。
- [x] 集成测试（Integration Test）和 UI 测试（UI Test）有 ReactTestUtils, Test Render, Enzyme, React-Testing-Library, Vue-Test-Utils 等。



# Git

- [x] [Git 在工作中是如何使用 Git 的](https://zhuanlan.zhihu.com/p/250493093)
- [x] [Git 分支管理策略总结](https://juejin.im/post/6844904203115036685)
- [x] [Git 分支开发规范](https://juejin.im/post/6844903635533594632) 
- [x] [Git 用 rebase 合并分支](https://backlog.com/git-tutorial/cn/stepup/stepup2_8.html)