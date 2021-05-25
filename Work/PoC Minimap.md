# Minimap 

- 3.1 如何实现minimap. 
- 3.2 minimap的实现，是否可以是一个公共的，和内容无关的库。还是和子节点具体的实现耦合

------

没有找到可开箱即用的第三方库，结论上讲，大概率需要我们自行实现。





​	如何把SVG塞入minimap

​	如何对应动态SVG内容

​	如何处理双图层（SVG+html）

​    因为是数据全部copy以及直接操作dom，存在 性能问题

​    浏览器自身zoom in out 处理

​	minimap是做进diagram内部，还是作为外部库调用使用？

 

https://codepen.io/billdwhite/pen/qPWKOg

https://stackoverflow.com/questions/52430288/how-do-i-create-a-minimap-of-an-svg-using-d3-v5-and-show-the-current-viewport-di

https://codepen.io/ivan_bacher/pen/BObLEa

https://stackoverflow.com/questions/41062482/minimap-view-for-a-d3-diagram-in-a-layout



# layout布局

4.1 支持tree的分支布局计算。

4.2 自动布局和自由拖拽。