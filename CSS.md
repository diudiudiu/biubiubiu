# css篇

#### CSS-reset

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

#### 清除浮动

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



​               

​                

#### max-height max-width 当图片的过长或者过宽时，按比例缩放。                  