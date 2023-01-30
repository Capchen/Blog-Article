## 原文地址：[https://zhuanlan.zhihu.com/p/40736286](https://zhuanlan.zhihu.com/p/40736286)
## 1. display: contents
> The element itself does not generate any boxes, but its children and pseudo-elements still generate boxes as normal. For the purposes of box generation and layout, the element must be treated as if it had been replaced with its children and pseudo-elements in the document tree.

元素本身不产生任何边界框，而元素的子元素与伪元素仍然生成边界框，元素文字照常显示。为了同时照顾边界框和布局，处理这个元素时要想象这个元素不在元素树形结构里，而只有内容留下。包括子元素和伪元素

比如在一个父元素声明为flex时，子元素会依照flex规则进行显示排列，但是子元素的子元素并不会受flex规则影响，这时设置子元素的display为contents，那么子元素的子元素就会老老实实的受它爷爷辈元素的影响了。

## 2. @supports 查询特性
可以理解为css的能力检测

```css
@support (display: contents) {
	.hasContents {
  	display: contents
  }
}
@support not(display: contents) {
  .hasContents: {
  	display: flex
  }
}
```

如上，可以判断是否支持所列的css属性，并且支持 `not ` `and` `or` 关键字

## 3. overflow-behavior
在针对滚动链接的现象时，使用这个属性，可以控制页面滚动的影响。
这个属性有三个值
> - `auto`：其默认值。元素（容器）的滚动会传播给其祖先元素。有点类似JavaScript中的冒泡行为一样
> - `contain`：阻止滚动链接。滚动行为不会传播给其祖先元素，但会影响节点内的局部显示。例如，Android上的光辉效果或iOS上的回弹效果。当用户触摸滚动边界时会通知用户。注意，`overscroll-behavior:contain`在`html`元素上使用，可以阻止导航滚动操作
> - `none`：和`contain`一样，但它也可以防止节点本身的滚动效果


值得注意的是，这个属性还不是W3C标准，而是Web孵化器WICG的一个建议。不过，说不定哪一天，这个特性就进入到W3C工作组中，成为W3C的一个标准。

## 4. :focus-within        :placeholder-shown
:focus-within 有点类似于伪元素选择器 `:before` `:focus` 
但是它的生效条件是 **只要任意后代元素获得焦点，那么它就会被匹配** .

:placeholder-shown 
![](https://cdn.nlark.com/yuque/0/2019/jpeg/297368/1561098417624-04ff5ed0-294e-40d4-b8f2-e14487cf5d75.jpeg#align=left&display=inline&height=275&originHeight=379&originWidth=720&size=0&status=done&width=522)
首先要先分清楚，`:placeholder-shown`和`::placeholder`是不同的两个东东。神奇的是`:placholder-shown`是W3C标准规范的一个属性，而`::placeholder`却不是。`::placeholder-shown`仍然会影响占位符文本的样式。

## 5. Containment
使用CSS Containment，我们可以把页面的部分圈起来，然后说“这里发生了什么，只在这里做相应的事”。这也是另一种方法，可以保护元素不受外部的变化而受影响。

CSS的Containment是一个新属性，使用关键字`contain`，它支持四个属性值：

- `layout`：这个值打开该元素的布局控制。这确保所包含元素对布局目的完全不透明；外部不能影响其内部布局，反之亦然
- `paint`：这个值打开该元素的绘制控制。这确保包含元素的后代节点不显示在其边界外，因此，如果一个元素在屏幕外或是不可见的，它的后代节点同样也被保证是不可见的
- `size`：这个值打开该元素的尺寸控制。这确保包含元素可以无需检查其后代节点进行布局。
- `style`：这个值打开该元素的样式控制。这确保了，对于性能这会不仅仅作用于一个元素及其后代，这些效果也不忽视包含的元素
