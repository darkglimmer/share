## 推荐的CSS布局方案

> 作者：包欣雨  
> 提示：CSS的作用就是控制页面的样式，样式中最关键的就是布局。CSS中有着各种各样的属性，看似很复杂，但如果我们把这些CSS的技术，比如浮动，inline-block等等，按布局的需要来看，会发现问题很简单。CSS中我们要解决的问题无非就是实现各种水平布局，以及居中。把纷繁复杂的CSS知识按布局的需求来划分，就可以很好的来组织这些知识。在使用的时候，可以选择最合适的技术。我们关注的应该是应用的布局方案，而不是纠结于记忆具体的CSS知识。
> 这篇文章主要就是按以下的大纲来写作。具体的布局代码，在写作时可以放Codepen的demo来说明。在写作时要注意写清楚先行知识。
> 审校：

### 先行知识：BFC和IFC

**css 属性 width 和 height 作用于 div 元素所产生的盒子，而不是元素本身**

#### 了解[BFC](https://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html)——“块级格式化上下文”

什么是 BFC ？  
浮动元素和绝对定位元素，非块级盒子的块级容器（例如 inline-blocks, table-cells 和 table-captions），以及 overflow 值不为 “visiable” 的块级盒子。  

在BFC中，盒子从顶端开始垂直地一个接一个地排列，两个盒子之间的垂直的间隙是由他们的 margin 值所决定的。在一个 BFC 中，两个相邻的块级盒子的垂直外边距会产生折叠。在BFC中，每个盒子的左外边框紧挨着包含块的左边框（从右到左的格式，则为紧挨右边框）。即使存在浮动也是这样的（尽管一个盒子的边框会由于浮动而收缩），除非这个盒子的内部创建了一个新的BFC浮动，盒子本身将会变得更窄）。  

BFC 中的元素的布局是不受外界的影响（我们往往利用这个特性来消除浮动元素对其非浮动的兄弟元素和其子元素带来的影响。）并且在一个 BFC 中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。


关于BFC的使用：


1.渲染规则：
```
1) 内部的 Box 会在垂直方向，一个接一个地放置。  

2) Box 垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box 的 margin 会发生重叠  

3) 每个元素的 margin box 的左边， 与包含块 border box 的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。  

4) BFC的区域不会与 float box 重叠，常用来清除浮动和布局。 

5) BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。  

6) 计算BFC的高度时，浮动元素也参与计算
```


2.合并外边距——[折叠](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)

要注意：
**毗邻块盒子的垂直外边距折叠只有他们是在同一BFC时才会发生。如果他们属于不同的BFC，他们之间的外边距将不会折叠。**

3.包含浮动：

解决：一个容器里有浮动元素，所以容器元素没有高度，它的浮动孩子将会脱离页面的常规流的问题。  
通常用清除浮动（比如：使用一个 clearfix 的伪类元素）  
但是也可以使用 BFC ：这个容器将包含浮动的子元素，它的高度将扩展到可以包含它的子元素，在这个BFC，这些元素将会回到页面的常规文档流。

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

4.防止文字环绕: <code>overflow: hidden</code>

5.多列布局:在前一列填充完之后的后面占据所剩余的空间。
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

什么是IFC？

简单来说，每个盒子都有一个FC特性，不同的FC值代表一组盒子不同的排列方式，IFC就是盒子从左到右的水平排列方式。IFC的line box（线框）高度由其包含行内元素中最高的实际高度计算而来（不受到竖直方向的 padding / margin 影响)。

关于IFC的使用：

1.IFC渲染规则：
```
1. 框会从包含块的顶部开始，一个接一个地水平摆放。

2. 摆放这些框的时候，它们在水平方向上的外边距、边框、内边距所占用的空间都会被考虑在内。在垂直方向上，这些框可能会以不同形式来对齐：它们可能会把底部或顶部对齐，也可能把其内部的文本基线对齐。能把在一行上的框都完全包含进去的一个矩形区域，被称为该行的行框。水平的 margin、padding、border有效，垂直无效。不能指定宽高。

3. 行框的宽度是由包含块和存在的浮动来决定。
```

2.IFC <code>‘line-height’</code> 与 <code>‘vertical-align’</code> 属性

在了解这两个属性之前，要了解**行内元素**：

1. 行级盒子的 content box 的高/宽不是通过 height/width 来设置的。  
content box/area 的高由 font-size 决定的；  
content box/area 的宽等于其子行级盒子的外宽度(margin+border+padding+content width)之和。  

2.  当 inline-level box 宽度大于父容器宽度时会被拆分成多个inline-level box。
然后当属性 direction 为 ltr 时，margin/border/padding-left 将作用于第一个的inline-level box，margin/border/padding-right将作用于最后一个的 inline-level box;   
若属性direction为rtl时, margin/border/padding-right 将作用于第一个的inline-level box，margin/border/padding-left 将作用于最后一个的 inline-level box。  

3. 行级盒子的 margin/border/padding-top/bottom 均不占空间。

4. **垂直排版特性**： 
inline-level box 排版单位不是其本身，而是 line box。重点在于 line box 高度的计算。  
```
(1)位于该行上的所有 in-flow 的 inline-level box 均参与该行line box 高度的计算;(注意：是所有 inline-level box，而不仅仅是子元素所生成的 inline-level box)  

(2)replaced elements, inline-block elements, and inline-table elements 将以其对应的 opaque inline-level box的  margin box 高度参与 line box 高度的计算。而其他inline-level box 则以 line-height 的实际值参与 line box 高度的计算;  

(3)各 inline-level box 根据 vertical-align 属性值相对各自的父容器作垂直方向对齐;  

(4)最上方的 box 的上边界到最下方的下边界则是 line box 的高度。
```


<code>**‘line-height’**</code>：计算行框里的各行内级框的高度。（对于置换元素、行内块元素、行内表格元素来说，这是边界框的高度。）  

<code>**‘vertical-align’**</code>：垂直对齐。在这些框使用 ‘top’ 或 ‘bottom’ 对齐的情况下，user-agent必须以最小化行框的高为目标对齐这些框。若这些框够高，则存在多个解而 CSS 2.1 不定义行框基线的位置。
 
 总结：
1. FC(Formatting Context)，用于初始化时设置盒子自身尺寸和排版规则。

2. 对于不产生新BFC的盒子：垂直排列，盒子的 left outer edge 与所在的 containing block 的左边相接触。
 对于产生新BFC的盒子：不与 floated-box 重叠，而是 floated-box 的margin-box 与 block-level box 的 border-box 相接触。
 
3. 左边框与 containing block 的左边框接触，右边框与 containing block 的右边框接触。若存在 floated 兄弟盒子，则 line box 的宽度为 containing 


4. [**分别计算box的高宽**](https://www.cnblogs.com/fsjohnhuang/p/5259121.html)

---

### 常用的水平布局（多栏水平排布）方案

#### Float布局

<code>float</code>，顾名思义就是浮动，设置了float属性的元素会根据属性值向左或向右浮动，我们称设置了float属性的元素为浮动元素。

浮动元素会从普通文档流中脱离，但浮动元素影响的不仅是自己，它会影响周围的元素对齐进行环绕。

[简单float用法的讲解](http://www.cnblogs.com/yiyezhai/p/3203490.html)

一些细节：

**不管一个元素是行内元素还是块级元素，如果被设置了浮动，那浮动元素会生成一个块级框，可以设置它的width和height。**

（因此float常常用于制作横向配列的菜单，可以设置大小并且横向排列。）

1. 浮动元素在浮动的时候，其 margin 不会超过包含块的 padding（包含块：浮动元素的包含块就是离浮动元素最近的块级祖先元素。）

2. 如果有多个浮动元素，后面的浮动元素的 margin 不会超过前面浮动元素的 margin。简单说就是如果有多个浮动元素，浮动元素会按顺序排下来而不会发生重叠的现象。

3. 如果两个元素一个向左浮动，一个向右浮动，左浮动元素的 marginRight 不会和右浮动元素的 marginLeft 相邻。
```   
（1）包含块的宽度大于两个浮动元素的宽度总和.  

（2）包含块的宽度小于两个浮动元素的宽度总和。这时如果包含块宽度不够高，后面的浮动元素将会向下浮动，其顶端是前面浮动元素的底端。
```

4. 浮动元素顶端不会超过包含块的内边界底端，如果有多个浮动元素，下一个浮动元素的顶端不会超过上一个浮动元素的底端

5. 如果有非浮动元素和浮动元素同时存在，并且非浮动元素在前，则浮动元素不会不会高于非浮动元素

6. 在满足其他规则下，浮动元素会尽量向顶端对齐、向左或向右对齐



[**具有包裹性——文字环绕效果**](https://www.jianshu.com/p/07eb19957991)
```
block元素不指定width的话，默认是100%，一旦让该div浮动起来，立刻会像inline元素一样产生包裹性，宽度会跟随内容自适应。
```


[**高度欺骗**](https://www.jianshu.com/p/07eb19957991)


**浮动元素的延伸性**
```
当浮动元素高度高于父元素，即浮动元素超出了父元素的底端。只要将父元素也设置成浮动，浮动元素就会被包含到父元素里面。（该元素会进行延伸进而包含其所有浮动的子元素）。
```


**浮动元素存在超出父级元素的padding**
```
1）负 margin  
2）浮动元素宽度大于父级元素宽度
```

**清除浮动**
```
1. clear 属性:在 clear 时要注意 clear 只对元素本身的布局起作用，仅作用于当前元素。
2. 增加额外的 div:在其父级元素的内容中增加一个（作为最后一个子元素）。  
3. 父级元素添加 overflow:hidden  
4. 将父元素设置 auto  
5. 父元素设置 display:table  
6. 设置 after 伪元素：子元素的后面，通过它可以设置一个具有 clear 的元素，然后将其隐藏。
```  

#### inline-block 布局

<code>inline-block</code>：

集合 block 以及 inline 的优点实现，每个都会新起一行，可以设置宽高，行高，上下边距，而且可以和其他行内元素同一行。  

<code>display:inline-block</code>没有父元素的匿名包裹特性，使用非常自由，可与文字，图片混排，可内嵌 block 属性元素，可以置身于 inline 水平的元素中。  

同时，每一行所有的 inline 元素和 inline-block 元素会共同形成一个 line boxes，这个 line box 的高度由里面最高的元素决定。所以，即使 inline-block 属性的列表元素高度异常，撑开的是整个 line boxes 的高度，因而，不会与下一行的列表元素发生错位。


**置换元素**
```
<img>|<input>|<button>|<select>|<textarea>|<label>

他们区别一般 inline 元素是：这些元素拥有内在尺寸,他们可以设置width/height 属性。他们的性质同设置了 display:inline-block 的元素一致。上述六个标签在现代浏览器中即为天生的 inline-block 元素。
```


**包裹性**
```
默认情况下一个div的宽度是以100%显示的，而一旦给这个div添加了 postion:absolute 属性，则100%的默认宽度会变成自适应的内部元素宽度。  

overflow | position:absolute | float:left/right 等都可以让元素 inline-block 化产生包裹性。
```


**水平间隙问题**
```
在列表 item 元素之间输入了回车换行以方便阅读，而这间隙正是这个回车换行产生的空白符。
```

**消除方法：**

1）设置 Font-Size
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
2）<code>letter-spacing</code> 属性:

控制文字间的水平距离的，支持负值，可以让文字水平方向上重叠（line-height是让文字垂直方向上重叠）。


**垂直间隙问题**

由于 <code>inline-block</code> 垂直对齐使用的是 vertical-align 属性，而该属性默认的对齐方式为 baseline。 
``` 
解决：设置 img 的 vertical-align 的值为 middle，text-top，text-bottom。
```


**inline-block列表布局**

使用 <code>white-space:nowrap</code> 属性可以让列表不换行  
使用 <code>text-align:justify</code> 可以实现自动等宽水平排列的列表布局，而且是两端对齐的.


#### flex布局

任何一个容器都可以指定为 Flex 布局。Webkit 内核的浏览器，必须加上<code>-webkit</code> 前缀。  

**注意:设为 Flex 布局以后，子元素的 float、clear和vertical-align属性将失效。**

[flex容器和项目的属性](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[单项目到多项目的flex应用](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)


#### inline-block和float的区别
```
1.文档流（Document flow）: 浮动元素会脱离文档流，并使得周围元素环绕这个元素。而 inline-block 元素仍在文档流内。因此设置 inline-block 不需要清除浮动。当然，周围元素不会环绕这个元素，你也不可能通过清除 inline-block 就让一个元素跑到下面去。  

2.水平位置（Horizontal position）：很明显你不能通过给父元素设置 text-align:center 让浮动元素居中。事实上定位类属性设置到父元素上，均不会影响父元素内浮动的元素。但是父元素内元素如果设置了 display：inline-block，则对父元素设置一些定位属性会影响到子元素。（这还是因为浮动元素脱离文档流的关系）。  

3.垂直对齐（Vertical alignment）：inline-block 元素沿着默认的基线对齐。浮动元素紧贴顶部。你可以通过 vertical 属性设置这个默认基线，但对浮动元素这种方法就不行了。这也是我倾向于 inline-block 的主要原因。  

4.空白（Whitespace）：inline-block 包含html空白节点。如果你的html 中一系列元素每个元素之间都换行了，当你对这些元素设置inline-block 时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴
```

---

### 常用的居中布局方案

#### [水平居中](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html)

[**六种实现元素水平居中**](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html)

**实现多个块级元素**：

如果要让多个块级元素在同一水平线上居中，那么可以修改它们的 display 值。  
```x
1）使用 inline-block 的显示方式  
2）使用了 flexbox 的显示方式
```

多个垂直堆栈的块元素，那么仍然可以通过设置 margin-left 和 margin-right 为 auto

#### 垂直居中

[**CSS实现垂直居中的5种方法**](https://www.qianduan.net/css-to-achieve-the-vertical-center-of-the-five-kinds-of-methods/)

+ 使用伪元素： 
添加伪元素:before 或 :after，让它的高度与父元素相同，这样子元素垂直对齐时就能居中了。

+ transform：  
用 translateY(-50%) 来达到 - height/2——**不需要知道居中元素的高度。**

+ flexbox:
```
.container {
display: flex;
flex-direction: column;
justify-content: center;
}
```

**实现多行**：

```
1）使用等值 padding-top 和 padding-bottom 的方式实现垂直居中。CSS 为文本设置一个类似 table-cell 的父级容器，然后使用 vertical-align 属性实现垂直居中。  

2）使用 flexbox 实现垂直居中，对于父级容器为 display: flex 的元素来说，它的每一个子元素都是垂直居中的。  

3）幽灵元素（ghost element）的非常规解决方式：在垂直居中的元素上添加伪元素，设置伪元素的高等于父级容器的高，然后为文本添加 vertical-align: middle; 样式，即可实现垂直居中。
```

**块级元素**

1）已知元素的高度：
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

2）未知元素的高度:  
那么就需要先将元素定位到容器的中心位置，然后使用 transform 的 translate 属性，将元素的中心和父容器的中心重合，从而实现垂直居中。

---

### 绝对定位
所谓绝对定位，其实也要找个东西来相对来绝对的，而这个东西就是元素的第一个有position，且positon不能为static的祖先元素。

对于不同祖先元素的[实例](https://www.cnblogs.com/tim-li/archive/2012/07/09/2582618.html)


**包裹性**

一旦给元素加上 absolute 或 float 就相当于给元素加上了 display:block。 内联元素span 默认宽度是自适应的，你给其加上 width 是不起作用的。要想 width 定宽，你需要将span 设成 display:block。


**高度欺骗**

float 是欺骗父元素，让其父元素误以为其高度塌陷了，但 floa t元素本身仍处于文档流中，文字会环绕着 float 元素，不会被遮蔽。

但 absolute 其实已经不能算是欺骗父元素了，而是出现了层级关系。从父元素的视点看，设成absolute 的图片已经完全消失不见了，因此从最左边开始显示文字。而 absolute 的层级高，所以图片遮盖了文字。

**如何确定定位点**

1.  absolute 分层后，第一个出现的问题就是让浏览器在何处显示该元素?  
```
1）未指定left/top/right/bottom的absolute元素，其在所处层级中的定位点就是正常文档流中该元素的定位点。  

2）让absolute元素没有position:static以外的父元素。此时absolute所处的层是铺满全屏的，即铺满body。会根据用户指定位置的在body上进行定位。  

3）只指定left时，元素的左上角定位点的left值会变成用户指定值。但top值仍旧是该元素在正常文档流中的top值。
```

2. <code>relative</code> ：

如果 absolute 元素没有 position:static 以外的父元素，那将相对body 定位，没有极限。而一旦父元素被设为 relative，那 absolute 子元素将相对于其父元素定位。  
```
1）相对定位时，不必拘泥于 relative+absolute，试试去掉 relative，充分利用 absolute 自身定位的特性，将 relative 和absolute 解耦。

2）拉伸平铺时，用 relative 可以有效限制子 absolute 元素的拉伸平铺范围（注意是拉伸，不是缩小。要缩小请再加上 width/height:100%;）
```

3. <code>z-index</code> ：  
```
1）让 absolute 元素覆盖正常文档流内元素（不用设z-index，自然覆盖）  
2）然后一个 absolute 元素覆盖前一个 absolute 元素（不用设z-index，只要在 HTML 端正确设置元素顺序即可）  

3）当 absolute 元素覆盖另一个 absolute 元素，且 HTML 端不方便调整 DOM 的先后顺序时，需要设置 z-index: 1。非常少见的情况下多个 absolute 交错覆盖，或者需要显示最高层次的模态对话框时，可以设置 z-index > 1。
```

4. 减少重绘和回流的开销：
可以将动画效果放到 absolute 元素中，避免浏览器将 render tree 回流。


---

### 不定宽布局方案

所谓“宽度分离”，就是CSS中的width属性不与影响宽度的border/padding(有时候包括margin)属性共存。


#### 两边定宽，中间自适应

[常见实现方式](https://juejin.im/entry/5a6306966fb9a01c9e460c22)

+ BFC 三栏布局
BFC 规则有这样的描述：BFC 区域，不会与浮动元素重叠。因此我们可以利用这一点来实现 3 列布局。
```
.left {
float: left;
height: 200px;
width: 100px;
margin-right: 20px;
background-color: red;
}
.right {
width: 200px;
height: 200px;
float: right;
margin-left: 20px;
background-color: blue;
}    
.main {
height: 200px;
overflow: hidden;
background-color: green;
}
```

#### 两边自适应，中间定宽

[常见实现方式](https://segmentfault.com/a/1190000008705541)




参考资料：  
[深入理解BFC和Margin Collapse](https://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)  
[理解CSS中BFC](https://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html)  
[CSS魔法堂：重新认识Box Model、IFC、BFC和Collapsing margins](https://www.cnblogs.com/fsjohnhuang/p/5259121.html) 
[css布局-BFC和IFC](http://www.myronliu.com/2016/03/04/css/css%E5%B8%83%E5%B1%80-BFC%E5%92%8CIFC/)  
[详解CSS float属性](http://luopq.com/2015/11/08/CSS-float/)  
[CSS浮动float详解](https://www.jianshu.com/p/07eb19957991)  
[拜拜了,浮动布局-基于display:inline-block的列表布局](http://www.zhangxinxu.com/wordpress/2010/11/%E6%8B%9C%E6%8B%9C%E4%BA%86%E6%B5%AE%E5%8A%A8%E5%B8%83%E5%B1%80-%E5%9F%BA%E4%BA%8Edisplayinline-block%E7%9A%84%E5%88%97%E8%A1%A8%E5%B8%83%E5%B1%80/)  
[深入了解 Inline-Block](http://layout.imweb.io/article/inline-block.html)  
[浅析inline-block--使用inline-block创建布局](http://www.cnblogs.com/coco1s/p/4024445.html)  
[Flex 布局教程：语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  
[六种实现元素水平居中](https://www.w3cplus.com/css/elements-horizontally-center-with-css.html)  
[CSS居中完整指南](https://www.w3cplus.com/css/centering-css-complete-guide.html)  
[CSS实现垂直居中的5种方法](https://www.qianduan.net/css-to-achieve-the-vertical-center-of-the-five-kinds-of-methods/)  
[CSS 垂直居中](http://lotabout.me/2016/CSS-vertical-center/)  
[css绝对定位、相对定位和文档流的那些事](https://www.cnblogs.com/tim-li/archive/2012/07/09/2582618.html)  
[CSS绝对定位absolute详解](https://www.jianshu.com/p/a3da5e27d22b)  
[CSS || 三栏布局，两边固定，中间自适应](https://segmentfault.com/a/1190000008705541)  
[css两边固定中间自适应布局](https://juejin.im/entry/5a6306966fb9a01c9e460c22)  
[CSS流体(自适应)布局下宽度分离原则](http://www.zhangxinxu.com/wordpress/2011/02/css%E6%B5%81%E4%BD%93%E8%87%AA%E9%80%82%E5%BA%94%E5%B8%83%E5%B1%80%E4%B8%8B%E5%AE%BD%E5%BA%A6%E5%88%86%E7%A6%BB%E5%8E%9F%E5%88%99/)  
