## 先行知识：BFC 和 IFC

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