# CSS

1. 垂直居中的多种方式
2. 弹性盒子布局考验
3. bfc和ikc
4. 高度塌陷形成原因及解决方式
5. 常用伪元素和伪类及写法
6. position的属性值，absolute的盒子设置left，right值是相对于哪个盒子来计算的
7. css选择器的权重计算规则



# svg 

- [x] SVG 的use 用法
- [x] #shadow-root (closed) 
- [x] style="transform: matrix
- [x] viewBox
- [x] scss 和 less 的区别



React ref 是什么？ 

React.createRef();



Safari 兼容性

https://stackoverflow.com/questions/48109988/svg-filter-makes-element-invisible-in-safari-and-mobile-chrome

https://stackoverflow.com/questions/49822876/css-svg-filter-safari-not-working

现在要兼容 Safari 好像遇到一个问题没法解决，在给button加阴影的时候，由于是SVG元素，所以不能走CSS给HTML的那套，SVG 的阴影是通过 defs 中定义 filter 来实现的，Safari 不支持 filter， chrome 支持的。

SVG 中的 g 元素不能自动计算width 和 height，要自己主动写入 style 指定

