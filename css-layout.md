## 推荐的CSS布局方案

> 作者：包欣雨  
> 提示：CSS的作用就是控制页面的样式，样式中最关键的就是布局。CSS中有着各种各样的属性，看似很复杂，但如果我们把这些CSS的技术，比如浮动，inline-block等等，按布局的需要来看，会发现问题很简单。CSS中我们要解决的问题无非就是实现各种水平布局，以及居中。把纷繁复杂的CSS知识按布局的需求来划分，就可以很好的来组织这些知识。在使用的时候，可以选择最合适的技术。我们关注的应该是应用的布局方案，而不是纠结于记忆具体的CSS知识。
> 这篇文章主要就是按以下的大纲来写作。具体的布局代码，在写作时可以放Codepen的demo来说明。在写作时要注意写清楚先行知识。
> 审校：

### 目录


### 参考
---
+ [深入理解BFC和Margin Collapse](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)  
+ [理解CSS中BFC](https://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html)  
+ [CSS魔法堂：重新认识Box Model、IFC、BFC和Collapsing margins](https://www.cnblogs.com/fsjohnhuang/p/5259121.html) 
+ [css布局-BFC和IFC](http://www.myronliu.com/2016/03/04/css/css%E5%B8%83%E5%B1%80-BFC%E5%92%8CIFC/)  
+ [详解CSS float属性](http://luopq.com/2015/11/08/CSS-float/)  
+ [CSS浮动float详解](https://www.jianshu.com/p/07eb19957991)  
+ [拜拜了,浮动布局-基于display:inline-block的列表布局](http://www.zhangxinxu.com/wordpress/2010/11/%E6%8B%9C%E6%8B%9C%E4%BA%86%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%80-%E5%9F%BA%E4%BA%8Edisplayinline-block%E7%9A%84%E5%88%97%E8%A1%A8%E5%B8%83%E5%B1%80/)  
+ [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
+ [深入了解 Inline-Block](http://layout.imweb.io/article/inline-block.html)  
+ [浅析inline-block--使用inline-block创建布局](http://www.cnblogs.com/coco1s/p/4024445.html)  
+ [Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  
+ [六种实现元素水平居中](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html)  
+ [CSS居中完整指南](https://www.w3cplus.com/css/centering-css-complete-guide.html)  
+ [CSS实现垂直居中的5种方法](https://www.qianduan.net/css-to-achieve-the-vertical-center-of-the-five-kinds-of-methods/)  
+ [CSS 垂直居中](http://lotabout.me/2016/CSS-vertical-center/)  
+ [css绝对定位、相对定位和文档流的那些事](https://www.cnblogs.com/tim-li/archive/2012/07/09/2582618.html)  
+ [CSS绝对定位absolute详解](https://www.jianshu.com/p/a3da5e27d22b)  
+ [CSS || 三栏布局，两边固定，中间自适应](https://segmentfault.com/a/1190000008705541)  
+ [css两边固定中间自适应布局](https://juejin.im/entry/5a6306966fb9a01c9e460c22)  
+ [CSS流体(自适应)布局下宽度分离原则](http://www.zhangxinxu.com/wordpress/2011/02/css%E6%B5%81%E4%BD%93%E8%87%AA%E9%80%82%E5%BA%94%E5%B8%83%E5%B1%80%E4%B8%8B%E5%AE%BD%E5%BA%A6%E5%88%86%E7%A6%BB%E5%8E%9F%E5%88%99/)  


### 先行知识：BFC 和 IFC

**CSS 属性 width 和 height 作用于 div 元素所产生的盒子，而不是元素本身**

<code>Formatting context </code> 是 <code>W3C CSS2.1</code> 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的是 <code>BFC</code> & <code>IFC</code>。


#### 了解 [BFC](https://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html)—— “块级格式化上下文”

所含元素：

+ 根元素或其它包含它的元素
+ 浮动元素 (元素的 <code>float</code>)
+ 绝对定位的元素 (元素具有 <code>position</code> 为 <code>absolute</code> 或 <code>fixed</code>)
+ 内联块 <code>inline-blocks</code> (元素具有 <code>display: inline-block</code>)
+ 表格单元格 (元素具有 <code>table-cells</code>，HTML表格单元格默认属性)
+ 表格标题 (元素具有 <code>table-captions</code>, HTML表格标题默认属性)
+ [块元素](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Block-level_elements)（元素具有 <code>overflow </code> 值不是 <code>visiable</code>）

在 <code>BFC</code> 中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的 margin 值所决定的。在一个 BFC 中，两个相邻的块级盒子的垂直外边距会产生**折叠**。在 <code>BFC</code> 中，每个盒子的左外边框紧挨着包含块的左边框，从右到左的格式，则为紧挨右边框。即使存在浮动也是这样的，尽管一个盒子的边框会由于浮动而收缩，除非这个盒子的内部创建了一个新的 <code>BFC</code> 浮动，盒子本身将会变得更窄。  

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%885.22.00.png?1524346244762">

<code>BFC</code> 是一个独立的渲染区域，只有 <code>Block-level box</code> 参与,它规定了内部的 <code>Block-level Box</code> 如何布局,并且与这个区域外部毫不相干。我们往往利用这个特性来消除浮动元素对其非浮动的兄弟元素和其子元素带来的影响。


关于 <code>BFC</code> 的使用：

1. 渲染规则：

+ 内部的 <code>Box</code> 会在垂直方向，一个接一个地放置。  
+ <code>Box</code> 垂直方向的距离由 <code>margin</code> 决定。属于同一个 <code>BFC</code> 的两个相邻 <code>Box</code> 的 <code>margin</code> 会发生重叠  
+ 每个元素的 <code>margin box</code> 的左边， 与包含块 <code>border box</code> 的左边相接触，即使存在浮动也是如此。  
+ <code>BFC</code> 的区域不会与 <code>float box</code> 重叠，常用来清除浮动和布局。 
+ <code>BFC</code> 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。  
+ 计算 <code>BFC</code> 的高度时，浮动元素也参与计算



2. 合并外边距——[折叠](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)

**外边距折叠**:

[尝试](https://codepen.io/anon/pen/LmpYjX)
效果：

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-21%20%E4%B8%8B%E5%8D%888.38.26.png?1524346230550">

代码：
```
//html
<div class="container"> 
     <p>Sibling 1 </p>
     <p>Sibling 2 </p>
</div>
//css
.container { 
  background-color: black; 
  overflow: hidden; /* creates a block formatting context */ 
}
p { 
  background-color: lightgreen;
  margin: 10px 0; 
}
body {
  text-align: center;
  width: 500px;
  margin: 20px auto;
}
```

理论上两个兄弟元素之间的边距应该是来两个元素的边距之和（20px），但它实际上为10px。这就是被称为外边距折叠。当兄弟元素的外边距不一样时，将以最大的那个外边距为准。


**浮动和绝对定位不与任何元素产生 margin 折叠**: 

因为元素会脱离当前的文档流，违反了上面所述的两个 margin 是邻接的条件同时，又因为浮动和绝对定位会使元素为它的内容创建新的 BFC。

[尝试](https://codepen.io/anon/pen/QrjKeJ)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%885.58.00.png?1524347899831">

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.05.44.png?1524348364553">

代码：
```
//html
<div class="wrapper overHid">
    <div class="big-box blue">non-float</div>
    <div class="middle-box green floatL">
        <div class="small-box red"></div>
        float left
    </div>
</div>

<div class="wrapper overHid">
    <div class="big-box">non-float</div>
    <div class="middle-box green floatL">float left</div>
    <div class="middle-box red">non-clear</div>
</div>

//css
.small-box {
  width: 50px;
  height: 50px;
  margin: 10px;
  background: #9cc;
}
.middle-box {
  width: 100px;
  height: 100px;
  margin: 20px;
  background: #99c;
} 
.big-box {
  width: 120px;
  height: 120px;
  margin: 20px;
  background: #33e;
} 
.floatL {
  float: left;
}  
.clear {
  clear: both;
} 
/* .posA {
  position: absolute;
}  */
.overHid{
  overflow: hidden;
} 
.red {
  background: #f00;
} 
.green {
  background: #0f0;
} 
.blue {
  background: #00f;
}

```

红色的块盒在没有清楚浮动的情况下，它的 margin-top 和蓝色块盒的 margin-bottom 产生了折叠


 <code>clearance</code>:

 当浮动元素之后的元素设置 clear 以闭合相关方向的浮动时，根据 w3c 规范规定，闭合浮动的元素会在其 margin-top 以上产生一定的空隙，该空隙会阻止元素 margin-top 的折叠，并作为间距存在于元素的 margin-top 的上方。

 [尝试](https://codepen.io/anon/pen/BxoQoP)

 <img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.13.15.png?1524348812310">

代码：
```
<div class="wrapper overHid">
    <div class="big-box" style="box-shadow:0 20px 0 rgba(0,0,255,0.2);">non-float</div>
    <div class="middle-box green floatL" style="opacity:0.6">float left</div>
    <div class="middle-box red clear" style="margin-top:40px;box-shadow:0 -40px 0 rgba(255,0,0,0.2);">clear</div>
</div>

.small-box {
  width: 50px;
  height: 50px;
  margin: 10px;
  background: #9cc;
}
.middle-box {
  width: 100px;
  height: 100px;
  margin: 20px;
  background: #99c;
} 
.big-box {
  width: 120px;
  height: 120px;
  margin: 20px;
  background: #33e;
} 
.floatL {
  float: left;
}  
.clear {
  clear: both;
} 
/* .posA {
  position: absolute;
}  */
.overHid{
  overflow: hidden;
} 
.red {
  background: #f00;
} 
.green {
  background: #0f0;
} 
.blue {
  background: #00f;
}
```

我们为红色块盒设置的40px的 margin-top 并没有对紫色块盒起作用，而且无论我们怎么修改这个 margin-top 值都不会影响红色块盒的位置，而只由绿色块盒的 margin-bottom 所决定。

结论：
+ 毗邻块盒子的垂直外边距折叠只有他们是在同一BFC时才会发生。如果他们属于不同的 <code>BFC</code>，他们之间的外边距将不会折叠。
+ 浮动元素不与任何元素的外边距产生折叠（包括其父元素和子元素）
+ 绝对定位元素不与任何元素的外边距产生折叠
+ <code>inline-block</code> 元素不与任何元素的外边距产生折叠
+ 一个常规文档流元素的 <code>margin-bottom</code> 与它下一个常规文档流的兄弟元素的 <code>margin-top</code> 会产生折叠，除非它们之间存在间隙。
+ 一个常规文档流元素的 <code>margin-top</code> 与其第一个常规文档流的子元素的 <code>margin-top</code> 产生折叠，条件为父元素不包含 <code>padding</code> 和 <code>border</code> ，子元素不包含 clearance。
+ 一个 'height' 为 'auto' 并且 'min-height' 为 '0'的常规文档流元素的 <code>margin-bottom</code> 会与其最后一个常规文档流子元素的 <code>margin-bottom</code> 折叠，条件为父元素不包含 <code>padding</code> 和 <code>border</code> ，子元素的 <code>margin-bottom</code> 不与包含 clearance 的 <code>margin-top</code> 折叠。
+ 一个不包含<code>border-top</code>、<code>border-bottom</code>、<code>padding-top</code>、<code>padding-bottom</code>的常规文档流元素，并且其 'height' 为 0 或 'auto'， 'min-height' 为 '0'，其里面也不包含行盒(line box)，其自身的 <code>margin-top</code> 和 <code>margin-bottom</code> 会折叠。



3. 包含浮动：

浮动：使元素脱离文档流，按照指定方向发生移动，遇到父级边界或者相邻的浮动元素停了下来。

解决：一个容器里有浮动元素，所以容器元素没有高度，它的浮动孩子将会脱离页面的常规流的问题。 

通常用清除浮动（比如：使用一个 clearfix 的伪类元素）  
但是也可以使用 BFC ：这个容器将包含浮动的子元素，它的高度将扩展到可以包含它的子元素，在这个BFC，这些元素将会回到页面的常规文档流。

[尝试](https://codepen.io/anon/pen/deYONz)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.30.02.png?1524349897352">

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.30.20.png?1524349901025">

```
//html
<div class="container"> 
     <div>Sibling</div>
     <div>Sibling</div>
</div>
//css
.container { 
    overflow: hidden; /* creates block formatting context */ 
    background-color: green;
} 
.container div { 
    float: left; 
    background-color: 
    lightgreen; 
    margin: 10px; 
}
```

加入 <code>overflow: hidden</code>, 这个容器将包含浮动的子元素，它的高度将扩展到可以包含它的子元素，在这个BFC，这些元素将会回到页面的常规文档流。


4. 防止文字环绕: <code>overflow: hidden</code>

[尝试](https://codepen.io/anon/pen/jxbVwj)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.40.46.png?1524350478541">
<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.40.36.png?1524350479680">

代码：
```
<div class="box">
    <div class="img">image</div>
    <p class="info">信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信息信</p>
</div>

.box {
  width:210px;
  border: 1px solid #000;
  float: left;
} 
.img {
  width: 100px;
  height: 100px;
  background: #696;
  float: left;
} 
.info {
  //overflow: hidden;
  background: #ccc;
  color: #fff;
}
```

5. 多列布局: 在前一列填充完之后的后面占据所剩余的空间。

[尝试](https://codepen.io/anon/pen/jxbVGj)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.44.50.png?1524350709736">

代码：
```
//html
<div class="container"> 
    <div class="column">column 1</div> 
    <div class="column">column 2</div> 
    <div class="column">column 3</div> 
</div>
//css
.column { 
    width: 31.33%; 
    background-color: green; 
    float: left; 
    margin: 0 1%; 
} 
.column:last-child { 
    float: none; 
    overflow: hidden; 
}
```

#### 了解[IFC]()——“行内格式化上下文”

简单来说，每个盒子都有一个 FC 特性，不同的 FC 值代表一组盒子不同的排列方式，IFC 就是盒子从左到右的水平排列方式。IFC 的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的 padding / margin 影响)。

关于 <code>IFC</code> 的使用：

1. <code>IFC</code> 渲染规则：
+ 框会从包含块的顶部开始，一个接一个地水平摆放。
+ 水平的 margin、padding、border 有效，垂直无效。不能指定宽高。
+ 摆放这些框的时候，它们在水平方向上的外边距、边框、内边距所占用的空间都会被考虑在内。在垂直方向上，这些框可能会以不同形式来对齐：它们可能会把底部或顶部对齐，也可能把其内部的文本基线对齐。能把在一行上的框都完全包含进去的一个矩形区域，被称为该行的行框。
+ 行框的宽度是由包含块和存在的浮动来决定。


2. IFC <code>line-height</code> 与 <code>vertical-align</code> 属性

在了解这两个属性之前，要了解**行内元素**：

1. 行级盒子的 <code>content box</code> 的高/宽不是通过 height/width 来设置的。  
content box/area 的高由 <code>font-size</code> 决定的；  
content box/area 的宽等于其子行级盒子的外宽度(margin+border+padding+content width)之和。  

2.  当 <code>inline-level box</code> 宽度大于父容器宽度时会被拆分成多个<code>inline-level box</code>。
然后当属性 <code>direction</code> 为 <code>ltr</code> 时，margin/border/padding-left 将作用于第一个的<code>inline-level box</code>，margin/border/padding-right将作用于最后一个的 <code>inline-level box</code>;   
若属性 <code>direction</code> 为 <code>rtl</code> 时, margin/border/padding-right 将作用于第一个的 <code>inline-level box</code>，margin/border/padding-left 将作用于最后一个的 <code>inline-level box</code>。  

[尝试](https://codepen.io/anon/pen/WJQoMB)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.52.00.png?1524351140218">


3. 行级盒子的 margin/border/padding-top/bottom 均不占空间。

[尝试](https://codepen.io/anon/pen/deYOeJ)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%886.58.13.png?1524351514307">

代码：
```
<div>before bababababababa</div>
<div class="existed">babababababababababa</div>
<div>after bababababababa</div>
<br>
<div>before bababababababa</div>
<span class="existed">babababababababababa</span>
<div>after bababababababa</div>

.existed{
  margin: 20px;
  padding: 20px;
  border: solid 1px red;
  background: yellow;
  background-clip: content-box;
}
```

4. **垂直排版特性**： 
<code>inline-level box</code> 排版单位不是其本身，而是 <code>line box</code>。重点在于 <code>line box</code> 高度的计算。  

+ 位于该行上的所有 <code>in-flow</code> 的 <code>inline-level box</code> 均参与该行 <code>line box</code> 高度的计算。(注意：是所有 <code>inline-level box</code>，而不仅仅是子元素所生成的 <code>inline-level box</code>)  
+ <code>replaced elements</code>, <code>inline-block elements</code>, and <code>inline-table elements</code> 将以其对应的 <code>opaque inline-level box</code> 的 <code>margin box</code> 高度参与 <code>line box</code> 高度的计算。而其他 <code>inline-level box</code> 则以 <code>line-height</code> 的实际值参与 <code>line box</code> 高度的计算;  
+ 各 <code>inline-level box</code> 根据 <code>vertical-align</code> 属性值相对各自的父容器作垂直方向对齐;  
+ 最上方的 <code>box</code> 的上边界到最下方的下边界则是 <code>line box</code> 的高度。



<code>**line-height**</code>：计算行框里的各行内级框的高度。（对于置换元素、行内块元素、行内表格元素来说，这是边界框的高度。）  

<code>**vertical-align**</code>：垂直对齐。在这些框使用 ‘top’ 或 ‘bottom’ 对齐的情况下，user-agent 必须以最小化行框的高为目标对齐这些框。若这些框够高，则存在多个解而 CSS 2.1 不定义行框基线的位置。


用 IFC 看行级元素：

[尝试](https://codepen.io/anon/pen/qYOqyP)

<img src= "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%887.07.29.png?1524352070599">

代码：
```
<span class="parent">
  <span class="child">
    <span class="inline-block">display:inline-block元素</span>
    xp子元素的文字
  </span>
  xp父元素的文字
</span>
<div class="other">其他元素</div>

.parent{
  line-height: 1;
  font-size: 14px;
 
  border: solid 1px yellow;
}
.child{
  font-size: 30px;
  vertical-align: middle;
 
  border: solid 1px blue;
}
.inline-block{
  display: inline-block;
  overflow: hidden;
 
  border: solid 1px red;
}
.other{
  border: solid 1px green;
}
```

1. 其中 <code>span.parent</code> 的"高度"为其 <code>line-height</code> 实际值，<code>span.child</code> 的"高度"为其 <code>line-height</code> 实际值，而 <code>span.inline-block</code> 的"高度"为其 <code>margin box</code> 的高度。由于设置 <code>line-height:1</code>，因此 <code>span.parent</code> 和 <code>span.child</code> 的 <code>content box</code> 高度等于 <code>line-height</code> 实际值;
2. 根据 <code>vertical-align </code>属性垂直对齐，造成各“高度”间并不以上边界或下边界对齐;
3. <code>span.inline-block </code>红色的上边框到 <code>span.child</code> 蓝色的下边框的距离再减去1px即为line box的高度。
 
 总结：
1. FC(Formatting Context)，用于初始化时设置盒子自身尺寸和排版规则。

2. 对于不产生新 <code>BFC</code> 的盒子：垂直排列，盒子的 left outer edge 与所在的 <code>containing block</code> 的左边相接触。
 对于产生新 <code>BFC</code> 的盒子：不与 <code>floated-box</code> 重叠，而是 <code>floated-box</code>  <code>margin-box </code> 与 <code>block-level box </code>的 <code>border-box </code>相接触。
 
3. 左边框与 <code>containing block </code>的左边框接触，右边框与 <code>containing block </code>的右边框接触。若存在 floated 兄弟盒子，则 <code>line box </code>的宽度为 containing 


4. [**分别计算box的高宽**](https://www.cnblogs.com/fsjohnhuang/p/5259121.html)

---









### 常用的水平布局（多栏水平排布）方案

#### Float布局

<code>float</code>，顾名思义就是浮动，设置了 float 属性的元素会根据属性值向左或向右浮动，我们称设置了 float 属性的元素为浮动元素。

浮动元素会从普通文档流中脱离，但浮动元素影响的不仅是自己，它会影响周围的元素对齐进行环绕。

[简单float用法的讲解](http://www.cnblogs.com/yiyezhai/p/3203490.html)

[尝试](https://codepen.io/anon/pen/pVjNqj)

[一些细节](http://luopq.com/2015/11/08/CSS-float/)：

**不管一个元素是行内元素还是块级元素，如果被设置了浮动，那浮动元素会生成一个块级框，可以设置它的width和height。**

（因此 float 常常用于制作横向配列的菜单，可以设置大小并且横向排列。）

1. 浮动元素在浮动的时候，其 margin 不会超过包含块的 padding（包含块：浮动元素的包含块就是离浮动元素最近的块级祖先元素。）

2. 如果有多个浮动元素，后面的浮动元素的 margin 不会超过前面浮动元素的 margin。简单说就是如果有多个浮动元素，浮动元素会按顺序排下来而不会发生重叠的现象。

3. 如果两个元素一个向左浮动，一个向右浮动，左浮动元素的 marginRight 不会和右浮动元素的 marginLeft 相邻。
+ 包含块的宽度大于两个浮动元素的宽度总和.  
+ 包含块的宽度小于两个浮动元素的宽度总和。这时如果包含块宽度不够高，后面的浮动元素将会向下浮动，其顶端是前面浮动元素的底端。

4. 浮动元素顶端不会超过包含块的内边界底端，如果有多个浮动元素，下一个浮动元素的顶端不会超过上一个浮动元素的底端

5. 如果有非浮动元素和浮动元素同时存在，并且非浮动元素在前，则浮动元素不会不会高于非浮动元素

6. 在满足其他规则下，浮动元素会尽量向顶端对齐、向左或向右对齐



[**具有包裹性——文字环绕效果**](https://www.jianshu.com/p/07eb19957991)
block 元素不指定 width 的话，默认是100%，一旦让该 div 浮动起来，立刻会像 inline 元素一样产生包裹性，宽度会跟随内容自适应。

[尝试](https://codepen.io/anon/pen/mLeRdv)

<img src ="https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-22%20%E4%B8%8A%E5%8D%887.47.30.png?1524354466901">

代码：
```
<div class="one">
    <img src="https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%A4%B4%E5%83%8F%E5%85%94%E5%AD%90.png?1511848432357" />
</div>
<div class="two">
    <img src="https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%A4%B4%E5%83%8F%E5%85%94%E5%AD%90.png?1511848432357" />
</div>
<div class="three">
    <img src="https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%A4%B4%E5%83%8F%E5%85%94%E5%AD%90.png?1511848432357" /> 
</div>

.one{
  border:4px solid blue;
}
.two{
  border:4px solid red;float:left;
}
.three{
  border:4px solid green;
}
```


[**高度欺骗**](https://www.jianshu.com/p/07eb19957991)

将外层 div 的 float 移到内层img中。  
[尝试](https://codepen.io/anon/pen/JvYEop)


[**浮动元素的延伸性**](http://luopq.com/2015/11/08/CSS-float/)
当浮动元素高度高于父元素，即浮动元素超出了父元素的底端。只要将父元素也设置成浮动，浮动元素就会被包含到父元素里面。（该元素会进行延伸进而包含其所有浮动的子元素）。


**浮动元素存在超出父级元素的padding**
+ 负 margin  
+ 浮动元素宽度大于父级元素宽度

**清除浮动**
+ clear 属性:在 clear 时要注意 clear 只对元素本身的布局起作用，仅作用于当前元素。
+ 增加额外的 div:在其父级元素的内容中增加一个（作为最后一个子元素）。  
+ 父级元素添加 overflow:hidden  
+ 将父元素设置 auto  
+ 父元素设置 display:table  
+ 设置 after 伪元素：子元素的后面，通过它可以设置一个具有 clear 的元素，然后将其隐藏。
 









#### inline-block 布局

<code>inline-block</code>：

集合 <code>block</code> 以及 <code>inline</code> 的优点实现，每个都会新起一行，可以设置宽高，行高，上下边距，而且可以和其他行内元素同一行。  

<code>display:inline-block</code>没有父元素的匿名包裹特性，使用非常自由，可与文字，图片混排，可内嵌 <code>block</code> 属性元素，可以置身于 <code>inline</code> 水平的元素中。  

同时，每一行所有的 inline 元素和 <code>inline-block</code> 元素会共同形成一个 <code>line boxes</code>，这个 <code>line box</code>的高度由里面最高的元素决定。所以，即使 <code>inline-block</code> 属性的列表元素高度异常，撑开的是整个 <code>line boxes</code> 的高度，因而，不会与下一行的列表元素发生错位。


**置换元素**
```
<img>|<input>|<button>|<select>|<textarea>|<label>
```

他们区别一般 inline 元素是：这些元素拥有内在尺寸,他们可以设置 width/height 属性。他们的性质同设置了 display:inline-block 的元素一致。上述六个标签在现代浏览器中即为天生的 <code>inline-block</code> 元素。


**包裹性**
默认情况下一个 div 的宽度是以100%显示的，而一旦给这个 div 添加了 <code>postion:absolute</code> 属性，则100%的默认宽度会变成自适应的内部元素宽度。  

overflow | position:absolute | float:left/right 等都可以让元素 <code>inline-block</code>  化产生包裹性。



[**水平间隙问题**](http://www.zhangxinxu.com/wordpress/2010/11/%E6%8B%9C%E6%8B%9C%E4%BA%86%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%80-%E5%9F%BA%E4%BA%8Edisplayinline-block%E7%9A%84%E5%88%97%E8%A1%A8%E5%B8%83%E5%B1%80/)
在列表 item 元素之间输入了回车换行以方便阅读，而这间隙正是这个回车换行产生的空白符。

消除方法：

+ 设置 Font-Size
```
.nav {
    background: #999;
    font-size: 0; /* 空白字符大小为0 */
}
.nav-item{
    display:inline-block;
    width: 100px;
    font-size: 16px; /* 重置 font-size 为16px*/
    background: #ddd;
}
```

+ <code>letter-spacing</code> 属性:

控制文字间的水平距离的，支持负值，可以让文字水平方向上重叠（<code>line-height</code> 是让文字垂直方向上重叠）。


[**垂直间隙问题**](http://layout.imweb.io/article/inline-block.html)

由于 <code>inline-block</code> 垂直对齐使用的是 vertical-align 属性，而该属性默认的对齐方式为 <code>baseline</code>。 
解决：设置 img 的 <code>vertical-align</code> 的值为 middle，text-top，text-bottom。



**inline-block列表布局**

使用 <code>white-space:nowrap</code> 属性可以让列表不换行  
使用 <code>text-align:justify</code> 可以实现自动等宽水平排列的列表布局，而且是两端对齐的.



#### flex布局

任何一个容器都可以指定为 Flex 布局。<code>Webkit</code> 内核的浏览器，必须加上<code>-webkit</code> 前缀。  

**注意:设为 Flex 布局以后，子元素的 float、clear和vertical-align属性将失效。**

[flex容器和项目的属性](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

[尝试](https://codepen.io/anon/pen/erpgor)

[单项目到多项目的flex应用](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)





#### inline-block和float的区别
+ 文档流（Document flow）: 浮动元素会脱离文档流，并使得周围元素环绕这个元素。而 <code>inline-block</code> 元素仍在文档流内。因此设置 <code>inline-block</code> 不需要清除浮动。当然，周围元素不会环绕这个元素，你也不可能通过清除 <code>inline-block</code> 就让一个元素跑到下面去。  

+ 水平位置（Horizontal position）：很明显你不能通过给父元素设置 <code>text-align:center</code> 让浮动元素居中。事实上定位类属性设置到父元素上，均不会影响父元素内浮动的元素。但是父元素内元素如果设置了 <code>display：inline-block</code>，则对父元素设置一些定位属性会影响到子元素。（这还是因为浮动元素脱离文档流的关系）。  

+ 垂直对齐（Vertical alignment）：<code>inline-block</code> 元素沿着默认的基线对齐。浮动元素紧贴顶部。你可以通过 vertical 属性设置这个默认基线，但对浮动元素这种方法就不行了。这也是我倾向于 inline-block 的主要原因。  

+ 空白（Whitespace）：<code>inline-block</code> 包含html空白节点。如果你的html 中一系列元素每个元素之间都换行了，当你对这些元素设置 <code>inline-block</code> 时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴










































---

### 常用的居中布局方案

#### 水平居中

在确定元素容器的宽度的情况下:
```
//块级元素
.center {
	width: 960px;
	margin:0 auto;
}	
//文本
.center {
    text-align:center; 
}
//图片
<div align="center">
    <img src="http://www.divcss5.com/img201305/divcss5-logo-201305.gif" />
</div> 

```

但有很多情况之下，我们是无法确定元素容器的宽度。未有明确宽度的时候，上面的方法无法让我们实现元素水平居中。这时我们可以通过
[**六种实现元素水平居中**](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html):

+ <code>margin</code> 和 <code>width</code> 实现水平居中

在分页容器上定义一个宽度，然后配合 <code>margin</code> 的左右值为 <code>auto</code> 实现效果

缺点：无法确定每个分页选项的宽度是多少，也就无法确认容器的宽度。

+ <code>inline-block</code> 实现水平居中方法

关键要在元素的父容器中设置 <code>text-align</code> 的属性为 <code>center</code>

缺点：分页项与分页项由回车符带来的空白间距，具体解决方法可以看[如何解决inline-block元素的空白间距](http://www.w3cplus.com/css/fighting-the-space-between-inline-block-elements)

+ [浮动实现水平居中的方法](http://matthewjamestaylor.com/blog/beautiful-css-centered-menus-no-hacks-full-cross-browser-support)

浮动实现居中的关键就是：如果 div 设置了浮动之后，他的内容有多宽度就会撑开有多大的容器。然后通过 <code>position:relative</code> 属性实现让分页导航居中。

+ 让导航浮动到左边，而且每个分页项也进行浮动:
<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-27%20%E4%B8%8B%E5%8D%883.48.36.png?1524815521024">

+ 首先在列表项 <code>ul</code> 上向右移动50%, <code>left:50%</code>，使整个分页向右移动了 50% 的距离：
<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-27%20%E4%B8%8B%E5%8D%883.48.43.png?1524815524687">

+ 接着我们在 <code>li</code> 上也定义 <code>position:relative</code> 属性，但其移动的方向和列表 <code>ul</code> 移动的方向刚好是反方向，而其移动的值保持一致：
<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-27%20%E4%B8%8B%E5%8D%883.48.47.png?1524815527384">

+ 绝对定位实现水平居中

可以通过设定父元素 <code> position: relative</code> ，<code>ul</code> 为 <code>position: absolute</code> 实现。

+ CSS3的flex实现水平居中方法

+ CSS3的fit-content实现水平居中方法

<code>fit-content</code> 是CSS中给 <code>width</code> 属性新加的一个属性值，配合 <code>margin</code> 可以轻松的实现水平居中的效果


以上六种方法的[实现](https://codepen.io/anon/pen/LmbGQd)以及效果图：

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-27%20%E4%B8%8B%E5%8D%883.32.50.png?1524814747077">


**实现多个块级元素**

如果要让多个块级元素在同一水平线上居中，那么可以修改它们的 <code>display</code> 值。

+ 使用 <code>inline-block</code> 的显示方式  
+ 使用了 <code>flexbox</code> 的显示方式

多个垂直堆栈的块元素，那么仍然可以通过设置 <code>margin-left</code> 和 <code>margin-right</code> 为 <code>auto</code>

#### 垂直居中

+ <code>line-height</code>: 

设置为对象的 <code>height</code> 值就可以使文本居中了。

[实现](https://codepen.io/anon/pen/zjNppB)


+ 使用伪元素： 

添加伪元素 <code>:before</code> 或 <code>:after</code>，让它的高度与父元素相同，这样子元素垂直对齐时就能居中了。

[实现](https://codepen.io/anon/pen/WJowKE)

但如果我们需要使用父元素的伪元素做一些其它的事情，同时又需要居中，那我们就无能为力了。

+ <code>transform</code>：  

用 <code>translateY: -50%</code> 来达到 - height / 2 —— **不需要知道居中元素的高度**

[实现](https://codepen.io/anon/pen/vjyGzO)

+ <code>flexbox</code>:

[实现](https://codepen.io/anon/pen/mLOPzV)
```
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
```


**实现多行**：

+ 使用等值 <code>padding-top</code> 和 <code>padding-bottom</code> 的方式实现垂直居中。CSS 为文本设置一个类似 <code>table-cell</code> 的父级容器，然后使用 <code>vertical-align</code> 属性实现垂直居中。  


+ 使用 flexbox 实现垂直居中，对于父级容器为 <code>display: flex</code> 的元素来说，它的每一个子元素都是垂直居中的。  

+ 幽灵元素（ghost element）的非常规解决方式：在垂直居中的元素上添加伪元素，设置伪元素的高等于父级容器的高，然后为文本添加 <code>vertical-align: middle</code> 样式，即可实现垂直居中。


**块级元素**

1. 已知元素的高度：
```
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    height: 100px;
    margin-top: -50px; /* account for padding and border if not using box-sizing: border-box; */
}
```

2. 未知元素的高度:  
那么就需要先将元素定位到容器的中心位置，然后使用 <code>transform</code> 的 <code>translate</code> 属性，将元素的中心和父容器的中心重合，从而实现垂直居中。

---

### 绝对定位

所谓绝对定位，其实也要找个东西来相对来绝对的，而这个东西就是元素的第一个有 <code>position</code>，且 <code>positon</code> 不能为 <code>static</code> 的祖先元素。

对于不同祖先元素的[实例](https://www.cnblogs.com/tim-li/archive/2012/07/09/2582618.html)


**包裹性**

一旦给元素加上 <code>absolute</code> 或 <code>float</code> 就相当于给元素加上了 <code>display: block</code>。 
内联元素 <code>span</code> 默认宽度是自适应的，你给其加上 width 是不起作用的。要想 width 定宽，你需要将 <code>span</code> 设成 <code>display: block</code>。

[实现](https://codepen.io/anon/pen/BxpJOM)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-29%20%E4%B8%8B%E5%8D%8810.16.11.png?1525011655218">


**高度欺骗**

<code>float</code> 是欺骗父元素，让其父元素误以为其高度塌陷了，但 <code>float</code> 元素本身仍处于文档流中，文字会环绕着 <code>float</code> 元素，不会被遮蔽。

但 <code>absolute</code> 其实已经不能算是欺骗父元素了，而是出现了层级关系。从父元素的视点看，设成 <code>absolute</code> 的图片已经完全消失不见了，因此从最左边开始显示文字。而 <code>absolute</code> 的层级高，所以图片遮盖了文字。


**如何确定定位点**

1.  <code>absolute</code> 分层后，第一个出现的问题就是让浏览器在何处显示该元素?  

+ 未指定 <code>left/top/right/bottom</code> 的 <code>absolute</code> 元素，其在所处层级中的定位点就是正常文档流中该元素的定位点。  
改变图片为第二个子元素：

[实现](https://codepen.io/anon/pen/vjgpPa)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-29%20%E4%B8%8B%E5%8D%8810.28.27.png?1525012126678">

+ 让 <code>absolute</code> 元素没有 <code>position:static</code> 以外的父元素。此时 <code>absolute</code> 所处的层是铺满全屏的，即铺满 body。会根据用户指定位置的在 body 上进行定位。 

只指定 <code>left</code> 时，元素的左上角定位点的 <code>left</code> 值会变成用户指定值。但 <code>top</code> 值仍旧是该元素在正常文档流中的 <code>top</code> 值：

[实现](https://codepen.io/anon/pen/ergywB)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-29%20%E4%B8%8B%E5%8D%8810.33.51.png?1525012464071">

同理可得只指定 <code>right</code> 时


只指定 <code>top</code> 时，元素的左上角定位点的 <code>top</code> 值会变成用户指定值。但 <code>left</code> 值仍旧是该元素在正常文档流中的 <code>left</code> 值：

[实现](https://codepen.io/anon/pen/wjgywV)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-29%20%E4%B8%8B%E5%8D%8810.40.06.png?1525012884809">

同理可得只指定 <code>bottom</code> 时


2. <code>relative</code>:

如果 <code>absolute</code> 元素没有 <code>position: static</code> 以外的父元素，那将相对 body 定位，没有极限。而一旦父元素被设为 <code>relative</code>，那 <code>absolute</code> 子元素将相对于其父元素定位。  

[实现](https://codepen.io/anon/pen/jxyZqW)

<img src = "https://cdn.glitch.com/6fab60b1-32c5-42ee-b5f3-40edd35dc042%2F%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-04-29%20%E4%B8%8B%E5%8D%8810.53.17.png?1525013733462">

+ 相对定位时，不必拘泥于 <code>relative</code> + <code>absolute</code>，试试去掉 <code>relative</code>，充分利用 <code>absolute</code> 自身定位的特性，将 <code>relative</code> 和 <code>absolute</code> 解耦。

+ 拉伸平铺时，用 <code>relative</code> 可以有效限制子 <code>absolute</code> 元素的拉伸平铺范围,注意是拉伸，不是缩小。要缩小请再加上 <code>width / height: 100%</code>。


3. <code>z-index</code> ：  

+ 让 <code>absolute</code> 元素覆盖正常文档流内元素（不用设z-index，自然覆盖)

+ 然后一个 <code>absolute</code> 元素覆盖前一个 <code>absolute</code> 元素（不用设 <code>z-index</code>，只要在 HTML 端正确设置元素顺序即可）  

+ 当 <code>absolute</code> 元素覆盖另一个 <code>absolute</code> 元素，且 <code>HTML</code> 端不方便调整 DOM 的先后顺序时，需要设置 <code>z-index: 1</code>。非常少见的情况下多个 <code>absolute</code> 交错覆盖，或者需要显示最高层次的模态对话框时，可以设置 <code>z-index > 1</code>。


4. 减少重绘和回流的开销：

可以将动画效果放到 <code>absolute</code> 元素中，避免浏览器将 <code>render tree</code> 回流。


---

### 不定宽布局方案

所谓“宽度分离”，就是CSS中的width属性不与影响宽度的 border/padding (有时候包括margin)属性共存。


#### 两边定宽，中间自适应

[常见实现方式](https://juejin.im/entry/5a6306966fb9a01c9e460c22)

#### 两边自适应，中间定宽

+ 通过浮动与 <code>margin</code> 实现

[实现](https://codepen.io/anon/pen/GdvNQx)

代码：
```
//HTML
<div class="left">
    <div class="inner">left</div>
</div>
<div class="center">
    <div class="inner">center</div>
</div>
<div class="right">
    <div class="inner">right</div>
</div>

//CSS
.center {
  width: 600px; 
  background: yellow;
  float: left; 
}
.left{
  float: left; 
  width: 50%; 
  margin-left: -300px; 
}
.right { 
  float: left; 
  width: 50%; 
  margin-left: -300px; 
}

.inner { 
  padding: 50px;
}
.left .inner,
.right .inner { 
  margin-left: 300px;
  background: red;
}
```

将其都进行 50% 的宽度设置，并加上中负的左边距，此负的左边距最理想的值是中间栏宽度的一半加上 1px，比如说此例中是 "600px/2+1", 也就是说他们都有一个 <code>margin-left: -301px; </code>。这样一来，左右边栏内容无法正常显示，那是因为对他们进行了负的左边距操作，现在只需要在左右边栏的内层 <code>div.inner</code> 将其拉回来

+ 通过定位与 <code>margin</code> 实现

[实现](https://codepen.io/anon/pen/GdvNQx)

CSS 代码：
```
* { padding: 0; margin: 0;}
.center { 
  width: 600px; 
  background: yellow;
  margin: 0 auto;
}
.inner { 
  padding: 50px;
}
.left { 
  position: absolute; 
  left: 0;
  top: 0;
  width: 50%;
}
.right { 
  position: absolute; 
  right: 0; 
  top: 0;
  width: 50%;
}
.left .inner { 
  margin-left: 300px; 
  position: relative; 
  left: -300px; 
  background: red;
}
.right .inner { 
  margin-right: 300px; 
  position: relative; 
  left: 300px; 
  background: red;}
```
+ CSS3 的 flex 布局

[实现](https://codepen.io/anon/pen/KRvNrO)

代码：
```
//HTML
<div class="grid">
  <div class="col fluid">
    left
  </div>
  <div class="col fixed">
    center
  </div>
  <div class="col fluid">
    right
  </div>
</div>

//CSS
.grid {
  display: -webkit-flex;
  display: -moz-flex;
  display: -o-flex;
  display: -ms-flex;
  display: flex;
}

.col {
  padding: 50px;
  
}
.fluid {
  flex: 1;
  background: red;
}
.fixed {
  width: 400px;
  background: yellow;
}

```
