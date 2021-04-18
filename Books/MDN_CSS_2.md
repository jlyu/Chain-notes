- [层叠与继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)
- [CSS选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors)
- [盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model)
- [背景与边框](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Backgrounds_and_borders)
- [处理不同的文本方向](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Handling_different_text_directions)
- [溢出的内容](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Overflowing_content)



# 盒模型

![image-20210418140652770](D:\SAPShare\Chainnotes\Assets\image-20210418140652770.png)

外边距，内边距，边框  对应着margin、padding 和 border属性的**简写** 

#### 外边距

比如使用margin属性一次控制一个元素的所有边距，或者每边单独使用等价的普通属性控制：

- margin-top
- margin-right
- margin-bottom
- margin-left

记忆口诀：上、右、下、左 -> TRouBLe: Top, Right, Bottom, Right

##### 负外边距 

当外边距设置为负时，是存在一些特殊用途的，比如让元素重叠，收缩空间等

##### 外边距折叠

理解外边距的一个关键是外边距折叠的概念。**如果你有两个外边距相接的元素，这些外边距将合并为一个外边距，即最大的单个外边距的大小。**

需要注意的是，并不是所有情况下都会发生外边距叠加，比如行内框、浮动框或绝对定位框之间的外边距不会叠加。

#### 边框

有四个边框，每个边框都有样式、宽度和颜色，我们可能需要分别对它们进行操作。

可以使用border属性一次设置所有四个边框的宽度、颜色和样式

分别设置每边的宽度、颜色和样式，可以使用：

- border-top
- border-right
- border-bottom
- border-left

#### 内边距

内边距位于边框和内容区域之间。与外边距不同，不能有负数量的内边距，所以值必须是0或正的值。（可以写负的，但与0等价）

应用于元素的任何背景都将显示在内边距后面，内边距通常用于将内容推离边框。

## 盒模型调整和计算

CSS3 中新增了一种盒模型计算方式：`box-sizing`属性。盒模型默认的值是`content-box`, 新增的值是`padding-box`和`border-box`，几种盒模型计算元素宽高的区别如下：

- content-box（默认） 元素宽高(`width/height`)等于：`content`  布局所占宽高等于：`width/height(content) + padding + border` 
- border-box                元素宽高(`width/height`)等于：`content + padding + border` 布局所占宽高等于：`width/height(content + padding + border)`
- padding-box             元素宽高(`width/height`)等于：`content + padding`  布局所占宽高等于：`width/height(content + padding) + border`

## 全局设置 border-box

```css
:root {
  box-sizing: border-box; /* 根元素设置为border-box */
}

*,
::before,
::after {
	box-sizing: inherit; /*  告诉其他所有元素和伪元素继承其盒模型为border-box */
}
```

这样在一个新开发的网站上的每一个元素都有一个可预测性更好的盒模型了。如果引入了第三方库，但第三方库内部没有使用 border-box 盒子模型，那也可以在必要时，选中第三方组件的根元素，将其恢复为 content-box 或其他

## 盒子模型和内联盒子

内联元素 span 可以和其他内联元素位于同一行，且宽高设置无效；外边距、内边距和边框是生效的，

块状元素 div 不可和其他元素位于同一行，且宽高设置有效。

> 当我们给某个元素设置宽高不生效，是因为该元素为内联元素。那么有没有办法解决这个问题呢？

## 使用display

我们可以通过设置display的值来对元素进行调整。

- 设置为block块状元素，此时可以设置宽度width和高度height。

- 设置为inline内联元素，此时宽度高度不起作用。

- 设置为inline-block，可以理解为块状元素和内联元素的结合，布局规则包括：
  - 位于块状元素或者其他内联元素内；
  - 可容纳其他块状元素或内联元素；
  - 宽度高度起作用。

除了内联元素和块状元素，我们还可以将元素设置为inline-block，inline-block可以很方便解决一些问题：使元素居中、给inline元素（<a>/<span>）设置宽高、将多个块状元素放在一行等。