- [层叠与继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)
- [CSS选择器](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Selectors)
- [盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/The_box_model)
- [背景与边框](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Backgrounds_and_borders)
- [处理不同的文本方向](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Handling_different_text_directions)
- [溢出的内容](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Overflowing_content)



# 层叠与继承

#### 层叠

生效也是依照优先级顺序，css规则的顺序很重要；当应用两条同级别的规则到一个元素的时候，写在**后面**的就是实际使用的规则。

#### 优先级

理解优先级很重要，浏览器将优先级分为2部分：`HTML行内样式` 和 `选择器样式` 

##### 优先级计算

| 选择器                                    | 千位 | 百位 | 十位 | 个位 | 优先级 |
| :---------------------------------------- | :--- | :--- | :--- | :--- | :----- |
| `h1`                                      | 0    | 0    | 0    | 1    | 0001   |
| `h1 + p::first-letter`                    | 0    | 0    | 0    | 3    | 0003   |
| `li > a[href*="en-US"] > .inline-warning` | 0    | 0    | 2    | 2    | 0022   |
| `#identifier`                             | 0    | 1    | 0    | 0    | 0100   |
| 内联样式                                  | 1    | 0    | 0    | 0    | 1000   |

> **注**: 通用选择器 (`*`)，组合符 (`+`, `>`, `~`, ' ')，和否定伪类 (`:not`) 不会影响优先级。 0000
>
> **注**: 在进行计算时不允许进行进位，例如，20 个类选择器仅仅意味着 20 个十位，而不能视为 两个百位，也就是说，无论多少个类选择器的权重叠加，都不会超过一个 ID 选择器。

##### !important

标记为重要的声明，用于修改特定属性的值， 能够覆盖普通规则的层叠。

```css
.better {
    background-color: gray;
    border: none !important;
}
```

> 注： 覆盖 !important 唯一的办法就是另一个 !important 具有 相同优先级 而且顺序靠后，或者更高优先级。
>
> 建议不要使用 !important   它比ID更难覆盖，一旦使用，想要覆盖原来的声明，就需要再加上一个 !important。 最后很容易演变成 important 军备演习，而且优先级的问题依然需要处理。调试 CSS 问题非常困难。

#### 继承

继承是顺着DOM树向下传递的。

![image-20210418152447024](..\Assets\image-20210418152447024.png)

继承也需要在上下文中去理解 —— 一些设置在父元素上的css属性是可以被子元素继承的，有些则不能。

- ​	如果设置一个元素的 color 和 font-family ，每个在里面的元素也都会有相同的属性，除非你直接在元素上设置属性。
- ​	一些属性是不能继承的 — 举个例子如果你在一个元素上设置 width 50% ，所有的后代不会是父元素的宽度的50% 。如果这个也可以继承的话，CSS就会很难使用了!

哪些属性属于默认继承很大程度上是由常识决定的。（文本相关的属性，列表属性，表格边框属性）

CSS 为控制继承提供了通用属性值。**每个css属性**都接收这些值。

- [`inherit`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inherit)

  设置该属性会使子元素属性和父元素相同。实际上，就是 "开启继承".

- [`initial`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/initial)

  设置属性值和`浏览器默认样式` 相同。如果浏览器默认样式中未设置且该属性是自然继承的，那么会设置为 `inherit` 。

- [`unset`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/unset)

  将属性重置为`自然值`，也就是如果属性是自然继承那么就是 `inherit`，否则和 `initial`一样



# CSS 选择器

猫头鹰选择器： 使用猫头鹰选择器全局设置堆叠元素之间的外边距。 因为它长这样： * + * 该选择器开头是一个通用选择器（*），它可以选中所有元素，后面是一个相邻兄弟组合器（+），最后是另一个通用选择器。它因形似一只眼神空洞的猫头鹰而得名。

猫头鹰选择器不会选中直接跟在其他元素，而是会选中直接跟在其他元素后面的任何元素。也就是说，它会选中页面上有着相同父级的非第一个子元素。

```
body * + * {
	margin-top: 1.5em;
}
```



| 选择器                                                       | 示例                | 学习CSS的教程                                                |
| :----------------------------------------------------------- | :------------------ | :----------------------------------------------------------- |
| [类型选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Type_selectors) | `h1 { }`            | [类型选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Type_selectors) |
| [通配选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Universal_selectors) | `* { }`             | [通配选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#The_universal_selector) |
| [类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Class_selectors) | `.box { }`          | [类选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#Class_selectors) |
| [ID选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/ID_selectors) | `#unique { }`       | [ID选择器](https://developer.mozilla.org/zh-CN/docs/user:chrisdavidmills/CSS_Learn/CSS_Selectors/Type_Class_and_ID_Selectors#ID_Selectors) |
| [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Attribute_selectors) | `a[title] { }`      | [标签属性选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Attribute_selectors) |
| [伪类选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes) | `p:first-child { }` | [伪类](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-class) |
| [伪元素选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements) | `p::first-line { }` | [伪元素](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Pseuso-classes_and_Pseudo-elements#What_is_a_pseudo-element) |
| [后代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator) | `article p`         | [后代运算符](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Descendant_Selector) |
| [子代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator) | `article > p`       | [子代选择器](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Child_combinator) |
| [相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator) | `h1 + p`            | [相邻兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#Adjacent_sibling) |
| [通用兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator) | `h1 ~ p`            | [通用兄弟](https://developer.mozilla.org/zh-CN/docs/User:chrisdavidmills/CSS_Learn/CSS_Selectors/Combinators#General_sibling) |

# 盒模型

![image-20210418140652770](..\Assets\image-20210418140652770.png)

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



# 背景与边框 

## CSS的背景样式

#### CSS background属性 

是我们将在本课中学习的许多普通背景属性的简写

```css
.box {
  background: linear-gradient(105deg, rgba(255,255,255,.2) 39%, rgba(51,56,57,1) 96%) center center / 400px 200px no-repeat, url(big-star.png) center no-repeat, rebeccapurple;
} 
```

- background-color 属性**[Color](https://wiki.developer.mozilla.org/en-US/docs/Web/CSS/color_value)值。** 背景颜色 
- background-image属性 背景图片 允许在元素的背景中显示图像。
- background-repeat属性 控制背景平铺 用于控制图像的平铺行为 比如 `no-repeat` — 不重复。`repeat-x` —水平重复。`repeat-y` —垂直重复。`repeat` — 在两个方向重复。
- background-size属性 调整背景图像的大小  `cover` - 完全覆盖了盒子区，同时仍然保持其高宽比  `contain` - 使图像的大小适合盒子内。在这种情况下，如果图像的长宽比与盒子的长宽比不同，则可能在图像的任何一边或顶部和底部出现间隙
- background-position属性 允许选择背景图像显示在其应用到的盒子中的位置。它使用的坐标系中，**框的左上角是(0,0)**，框沿着水平(x)和垂直(y)轴定位。是background-position-x和background-position-y的简写，它们允许您分别设置不同的坐标轴的值
- <gradient> 渐变背景

#### 多个背景图像

```css
background-image: url(image1.png), url(image2.png), url(image3.png), url(image1.png);
background-repeat: no-repeat, repeat-x, repeat;
background-position: 10px 20px,  top right;
```

不同属性的每个值，将与其他属性中相同位置的值匹配。例如，上面的image1的background-repeat值将是no-repeat。

#### 背景的可访问性考虑

当你把文字放在背景图片或颜色上面时，你应该注意你有足够的对比度让文字对你的访客来说是**清晰易读**的。 

屏幕阅读者不能解析背景图像，因此背景图片应该只是纯粹的装饰；**任何重要的内容都应该是HTML页面的一部分，而不是包含在背景中。**

## 边框

同样存在简写形式，也可以分别按上右下左来设置单独一面的边框 

```CSS
.box {
   border: 1px solid black;
   /* 下面等价 */
   border-width: 1px;
   border-style: solid;
   border-color: black;
}

.box_top {
    border-top: 1px solid black;
    /* 下面等价 */
    border-top-width: 1px;
    border-top-style: solid;
    border-top-color: black;
}
```

#### 圆角

通过使用[`border-radius`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-radius)属性和与方框的每个角相关的长边来实现方框的圆角。可以使用两个长度或百分比作为值，第一个值定义水平半径，第二个值定义垂直半径。

```css
.box {
  border-radius: 10px; /* 盒子的四个角都有10px的圆角半径 */
  border-top-right-radius: 1em 10%; /* 使右上角的水平半径为1em，垂直半径为10％ */
} 
```

#### 小练习

![image-20210418164749685](..\Assets\image-20210418164749685.png)

```
.box {
  
}

h2 {
  
}
```



# 处理不同方向的文本 

## 书写模式

书写模式是指文本的排列方向是横向还是纵向的。

使用`writing-mode: vertical-rl`对一个标题的显示进行设置

`writing-mode`的三个值分别是：

- `horizontal-tb`: 块流向从上至下。对应的文本方向是横向的。
- `vertical-rl`: 块流向从右向左。对应的文本方向是纵向的。
- `vertical-lr`: 块流向从左向右。对应的文本方向是纵向的。

![image-20210418170511548](..\Assets\image-20210418170511548.png)

## 文本方向

考虑到其他国家文字的方向习惯，比如（如阿拉伯语）是横向书写的，但是是从右向左。



## 逻辑属性和逻辑值

目前为止我们已经了解了很多与屏幕的物理显示密切相关的很多属性，而书写模式和方向在水平书写模式下会很有意义。

横向书写模式下，映射到`width`的属性被称作内联尺寸（[`inline-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inline-size)）——内联维度的尺寸。而映射`height`的属性被称为块级尺寸（[`block-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/block-size)），这是块级维度的尺寸。

#### 物理和逻辑属性的映射和对比

```css
.logical {
  margin-block-start: 20px;
  padding-inline-end: 2em;
  padding-block-start: 2px;
  border-block-start: 5px solid pink;
  border-inline-end: 10px dotted rebeccapurple;
  border-block-end: 1em double orange;
  border-inline-start: 1px solid black;
}

.physical {
  margin-top: 20px;
  padding-right: 2em;
  padding-top: 2px;
  border-top: 5px solid pink;
  border-right: 10px dotted rebeccapurple;
  border-bottom: 1em double orange;
  border-left: 1px solid black;
}
```



>  应该使用物理属性还是逻辑属性呢？

逻辑属性是在物理属性之后出现的，因而最近才开始在浏览器中应用。你可以通过查看MDN的属性页面来了解浏览器对逻辑属性的支持情况。

如果你并没有应用多种书写模式，那么现在你可能更倾向于使用**物理属性**，因为这些在你使用弹性布局和网格布局时非常有用。



# 溢出的内容

CSS中万物皆盒，因此我们可以通过给[`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width)和[`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)（或者 [`inline-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/inline-size) 和 [`block-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/block-size)）赋值的方式来约束盒子的尺寸。

溢出是在你往盒子里面塞太多东西的时候发生的，所以盒子里面的东西也不会老老实实待着。CSS给了你好几种工具来控制溢出，在学习的早期理解这些概念是很有用的。在你写CSS的时候你经常会遇到溢出的情形

只要有可能，CSS就不会隐藏你的内容，隐藏引起的数据损失通常会造成困扰。在CSS的术语里面，这会导致一些内容消失，你的访客可能不会注意到这一点，如果消失的是表格上的提交按钮，没有人能填完这个表格，这是很麻烦的事情。

各种不同的控制尺寸的方式，以减少溢出的影响。但是，如果你需要固定的尺寸，你也可以控制溢出表现的形式。

## Overflow 属性 

`overflow`的默认值为`visible`，这就是我们的内容溢出的时候，我们在默认情况下看到它们的原因。

在你的盒子上设置`overflow: hidden`。这就会像它表面上所显示的那样作用——隐藏掉溢出。这可能会很自然地让东西消失掉

用了`overflow: scroll`，那么你的浏览器总会显示滚动条，即使没有足够多引起溢出的内容。你可能会需要这样的样式，它避免了滚动条在内容变化的时候出现和消失。

#### 溢出建立了块级排版上下文

使用诸如`scroll`或者`auto`的时候，你就建立了一个块级排版上下文。

#### BFC 

![image-20210418180817107](..\Assets\image-20210418180817107.png)

**块级格式化上下文**（block formatting context，BFC）。 BFC 是网页的一块区域，元素基于这块区域布局。虽然 BFC 本身是环绕文档流的一部分，但它将内部的内容与外部的上下文隔离开。这种隔离为创建 BFC 的元素做出了以下 3 件事情。

(1) 包含了内部所有元素的上下外边距。它们不会跟 BFC 外面的元素产生外边距折叠。
(2) 包含了内部所有的浮动元素。
(3) 不会跟 BFC 外面的浮动元素重叠。

BFC 里的内容不会跟外部的元素重叠或者相互影响。如果给元素增加 clear 属性，它只会清除自身所在 BFC 内的浮动。如果强制给一个元素生成一个新的 BFC，它不会跟其他 BFC 重叠。给元素添加以下的任意属性值都会创建 BFC。

- float： left 或 right，不为 none 即可。
- overflow： hidden、 auto 或 scroll，不为 visible 即可。  
- display： inline-block、 table-cell、 table-caption、 flex、 inline-flex、grid 或 inline-grid。拥有这些属性的元素称为块级容器（block container）。
- position： absolute 或 position: fixed。  



![image-20210418181319455](..\Assets\image-20210418181319455.png)



```css
.node-text-word-wrap {
    overflow: hidden; // 溢出隐藏
    overflow-wrap: break-word;  // 用来说明当一个不能被分开的字符串太长而不能填充其包裹盒时，为防止其溢出，浏览器是否允许这样的单词中断换行。break-word 表示如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。
    text-overflow: ellipsis; // 确定如何向用户发出未显示的溢出内容信号。它可以被剪切，显示一个省略号 ...
    word-break: normal; // 指定了怎样在单词内断行。默认 
    display: -moz-box;                /* Mozilla */
    display: -webkit-box;             /* WebKit */
    display: box;                     /* As specified */
    -moz-box-orient: horizontal;      /* Mozilla */
    -webkit-box-orient: horizontal;   /* WebKit */
    box-orient: horizontal;           /* As specified */
    -webkit-line-clamp: 2; // 可以把块容器中的内容限制为指定的行数. 它只有在 display 属性设置成 -webkit-box 或者 -webkit-inline-box 并且 -webkit-box-orient (en-US) 属性设置成 vertical时才有效果
}
```

https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow-wrap



