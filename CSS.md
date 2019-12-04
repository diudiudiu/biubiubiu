# css篇

### CSS-reset

在 HTML标签在浏览器里有默认的样式，不同浏览器的默认样式之间也会有差别。为了更好的布局和开发，需要清除HTML标签的的默认样式。这就是CSS reset。实际上就是自己用CSS来重新定义各个HTML标签。各个网站论坛有着各种各样的CSS reset，当然CSS reset也可以按照个人习惯去设置。

1. 最基础的reset：

   ```css
   * { padding: 0; margin: 0; border: 0; }
   ```

2. 个人使用的感觉比较全的：

   ```css
   body, div, dl, dt, dd, ul, ol, h1, h2, h3, h4, h5, h6, pre, form, fieldset, input, textarea, p,blockquote, th, td {padding:0; margin:0;} 
   fieldset, img{border:0;} 
   table {border-collapse:collapse; border-spacing:0;} 
   ol, ul {list-style:none;} 
   address, caption, cite, code, dfn, em, strong, th,var { font-weight:normal; font-style:normal;}
   caption, th{text-align:left;} 
   h1, h2, h3, h4, h5, h6{font-weight:normal; font-size:100%;} 
   q:before, q:after {content: '';} 
   abbr,acronym{border:0;}
   ```

### img底部空白间隙

原因：img的默认的垂直对齐方式vertical-align的默认值是baseline，以X字母的下方为基准。

解决方法：

1. 修改img行内元素的垂直居中方式    

   ```css
   img {
   	vertical-align: bottom;
   	/* vertical-align: top;*/
   }
   ```

2. 修改父元素行高，使行高变小，这样基线下方的位置基本可以忽略。

   1. ```css
       line-height: 0px;
      ```

3. 让img变成块级元素，不在受行内基线的影响。浮动也可以让元素变成块级。

### 清除浮动

1. 额外标签

   在浮动的块元素后加个空标签`<div style="clear: both;"></div>` 或`<br />`等标签。

   缺点：添加许多无意义的标签，结构化差。

2. 给父级元素加overflow属性

   可以通过BFC的方式，给父级元素加：overflow为 hidden(主要) | auto | scroll。

   缺点：内容增多时候容易造成不会自动换行导致内容被隐藏，无法显示需要溢出的元素。

3. 使用afert伪元素

   ```css
   .clearfix:after{
   	content: "";
   	display: block;
   	height: 0;
   	clear: both;
   	visibility: hidden;
   }
   .clearfix{
   	*zoom: 1; //IE6-7专有
   }
   ```

   缺点：由于IE6-7不支持：after，使用zoom：1触发hasLayout。

4. 在父元素上使用使用 before 和 afert 双伪元素

   ```css
   .clearfix:before，.clearfix:after{
   	content:"";
   	display:table; //此句触发BFC
   }
   .clearfix:after{
   	clear:both;
   }
   .clearfix{
   	*zoom:1; //IE6-7专有
   }
   ```

   缺点：由于IE6-7不支持：after，使用zoom：1触发hasLayout。



### flexbox 弹性盒子布局

flexbox 是一维布局，只能在一条直线上放置内容区块

flex 容器 css 设置 `display: flex;`

flex 项目 ==> 父容器内的子级

##### 父容器属性

1. flex-direction **主轴**的方向
   - row（默认值）：主轴为水平方向，起点在左端。
   - row-reverse：主轴为水平方向，起点在右端。
   - column：主轴为垂直方向，起点在上沿。
   - column-reverse：主轴为垂直方向，起点在下沿。
2. flex-wrap 如果一条轴线排不下，如何**换行**
   - nowrap（默认）：不换行。
   - wrap：换行，第一行在上方。
   - wrap-reverse：换行，第一行在下方。
3. justify-content **主轴**上的对齐方式。
   - flex-start（默认值）：左对齐
   -  flex-end：右对齐
   -  center： 居中
   -  space-between：两端对齐，项目之间的间隔都相等。
   -  space-around：项目之间的间隔比项目与边框的间隔大一倍。
4.  align-items **交叉轴**上对齐方式
   - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
   - flex-start：交叉轴的起点对齐。
   - flex-end：交叉轴的终点对齐。
   - center：交叉轴的中点对齐。
   - baseline: 项目的第一行文字的基线对齐。
5. align-content **多根**轴线的对齐方式。(项目只有一根轴线，该属性不起作用)
   - stretch（默认值）：轴线占满整个交叉轴。
   - flex-start：与交叉轴的起点对齐。
   - flex-end：与交叉轴的终点对齐。
   - center：与交叉轴的中点对齐。
   - space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
   - space-around：每根轴线两侧的间隔都相等。轴线之间的间隔比轴线与边框的间隔大一倍。
6. flex-flow: 是 flex-direction 和 flex-wrap 的简写

##### flex 项目的属性

1. order 项目的排列顺序。数值越小，排列越靠前，默认为0
2. flex-grow:项目的放大比例，默认为0，不放大
3. flex-shrink：项目的缩小比例，默认为1，空间不够就缩小
4. flex-basis：在分配多余空间之前，项目占据的主轴空间的大小 默认为 auto
5. flex: flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
6. align-self：允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。
   - auto（ 默认值）：继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。
   - stretch：占满整个容器的高度。
   - flex-start：交叉轴的起点对齐。
   - flex-end：交叉轴的终点对齐。
   - center：交叉轴的中点对齐。
   - baseline: 项目的第一行文字的基线对齐。

### Grid 网格布局

Grid 是一个二维布局，将网页划分成一个个网格，可以任意组合不同的网格。

引入一个新的单位：fr:剩余空间分配数。

用于在分配剩余空间，如果多个指定了多个部分，则剩下的空间根据各自的数字按比例分配。

同时引入一个css函数

网格容器  ==>设置display:grid 父容器

网格项     ==> 父容器内的子级 是HTML中能够找到的DOM元素

网格线    ==> 组成网格项的分界线

网格轨道  ==> 两个相邻的网格线之间

网格单元 ==> 两个相邻的列网格线和两个相邻的行网格线组成的是网格单元

网格区域 ==> 四个网格线包围的总空间（含一个或多个网格单元）

#### max-height max-width 当图片的过长或者过宽时，按比例缩放。      

#### transform

##### 工作中遇到的问题：

- 一个位置高度元素 father 需要进行居中，那么就用到了 transform 。

- ```css
  .father{
  	position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }
  ```

- 该元素下有一个固定定位的元素son position: fixed ，这时会发现 son 并没有依赖页面左上角定位，而是依赖的是父元素 father 的位置。

会影响子元素的 position：fixed的定位 ，其子元素的相对的不再是页面的左上角，而是父元素的左上角。

#### iframe

##### 工作中遇到的问题：

- iframe 如果想自己增加个 header 类似一个浏览器的上边框的UI样式。

- ```css
  header{
  	height: 40px;
  }
  iframe{
  	width: 100%;
  	height: 100%;
  }
  ```

此时iframe 的内部，如果出现了滚动条，那么它将会隐藏 40px 的高度。所以需要在iframe的属性上，使用 clac 函数进行精确计算。

