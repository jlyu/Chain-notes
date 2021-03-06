## 布局

#### 浮动

#### Flexbox 弹性盒子布局

给元素添加 `display: flex` 就变成一个弹性容器。它的直接子元素就变成弹性子元素，默认在同一行从左到右顺序并排排列。弹性容器像块状元素一样填满可用宽度，但弹性子元素不一定填满弹性容器的宽度。

#### Grid 网格布局



## Position

#### static 

默认值，静态定位，如果 position 使用其他值，就可以说元素被定位了，但使用static，就是说元素未被定位。

#### inherit

从父元素继承 position 属性的值。

#### fixed 

固定定位，元素可以放在浏览器窗口的任意位置进行定位。配合 top、bottom、left、right 属性一起使用。特点：

- `fixed`元素脱离文档流
- `fixed`元素不占位
- `fixed`相对于浏览器窗口来定位，不管是否有`static`定位以外的父元素
- `absolute`元素会随着页面的滚动而滚动，而`fixed`不会

#### absolute

绝对定位，相对于`static`定位以外的第一个父元素进行定位。如果父元素未被定位，那么浏览器会沿着DOM树往上找它的祖父，曾祖父，直到找到一个定位元素。 

- `absolute`元素同样脱离文档流

#### relative

相对定位。跟固定定位和绝对定位不同，不能用 top、bottom、left、right 属性改变相对定位元素大小，这些值只能让元素在上下左右方向移动，（但top或bottom不可一起使用，用了bottom会被忽略，left 和 right 同理）

- 保持原有文档流，但相对本身的原始位置发生位移，且占空间
- 元素也遵循从上到下，从左到右的排版布局
- 相对于其正常位置进行定位，在这里设置了`relative`的元素相对其原本位置（`position: static`）进行位移
- 元素占有原本位置，因此下一个元素会排到该元素后方
- 元素占位不会随着定位的改变而改变。也就是说`relative`在文档流中占有的位置与其原本位置（`position: static`）相同



## 行内元素与块状元素

#### 内联元素、行内元素

行内元素表示位于行内的元素。
行内元素只能容纳文本或者其他行内元素，它允许其他行内元素与其位于同一行。
行内元素的宽度高度不起作用。

常见的内联元素：`<a>`/`<span>`/`<i>`/`<strong>`等。

#### 块状元素

块状元素一般是其他元素的容器，可容纳行内元素和其他块状元素。
块状元素排斥其他元素与其位于同一行。
块状元素的宽度高度起作用。

常见的块状元素有：

- `<div>`/`<p>`/`<h1>`/`<h2>…<h6>`/`<ul>`/`<ol>`
- HTML5 新元素: `<section>`/`<article>`/`<header>`/`<footer>`等

### 





