

## 浏览器支持

-moz /* Firefox 4 */

-webkit/* Safari 和 Chrome */

-o/* Opera */

## 选择器

###  id 选择器

#id

### 类选择器

.class

### *通用选择器

### 标签选择器

p、h1......

### 属性选择器

*[属性]把包含（属性）的所有元素选择

a[href]只对a标签含有href属性的元素选择

### 相邻兄弟选择器

+相邻兄弟选择

### 选择器大小排序

用户的重要声明(最大)：!important，

内联式（即行内样式）

id选择器>类选择器 伪类选择器 属性选择器

标签选择器>伪元素选择器

通用选择器、相邻兄弟选择器（+）、兄弟选择器（~）、子选择器（>）

| 属性                             | 特性值 |
| -------------------------------- | ------ |
| 行内样式表                       | 1000   |
| id选择器                         | 100    |
| 类选择器、属性选择器、伪类选择器 | 10     |
| 标签选择器                       | 1      |
| 层级选择器（空格、>、+、~、*）   | 0      |

## line-height行高

两行文字基线之间的距离!

![image-20221010140854769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221010140854769.png)

| 值      | 描述                                                 |
| ------- | ---------------------------------------------------- |
| normal  | 默认。设置合理的行间距                               |
| number  | 设置数字，此数字会与当前的字体尺寸相乘来设置行间距。 |
| length  | 设置固定的行间距。                                   |
| %       | 基于当前字体尺寸的百分比行间距。                     |
| inherit | 规定应该从父元素继承 line-height 属性的值。          |

## text-transform控制文本大写

| 值         |                                    |
| ---------- | ---------------------------------- |
| none       | 默认                               |
| capitalize | 每个单词以大写字母开头             |
| lowercase  | 定义无大写字母，仅有小写字母       |
| uppercase  | 定义无小写字母，仅有大写字母       |
| inherit    | 从父元素继承text-transform属性的值 |

## cursor光标

| 值        | 描述                                                         |
| :-------- | :----------------------------------------------------------- |
| *url*     | 需使用的自定义光标的 URL。**注释：**请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| default   | 默认光标（通常是一个箭头）                                   |
| auto      | 默认。浏览器设置的光标。                                     |
| crosshair | 光标呈现为十字线。                                           |
| pointer   | 光标呈现为指示链接的指针（一只手）                           |
| move      | 此光标指示某对象可被移动                                     |
| ...       | ...                                                          |

## 文本

### 文本装饰

text-decoration 属性规定添加到文本的修饰，下划线、上划线、删除线等 

| 值           | 描述                                            |
| :----------- | :---------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。                            |
| overline     | 定义文本上的一条线。                            |
| line-through | 定义穿过文本下的一条线。                        |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

虚线与波浪线

```css
h1 {  text-decoration: underline overline dotted red; }  
h2 {  text-decoration: underline overline wavy blue; }
```

### *font-family*设置字体

属性是用来设置元素的字体系列

### font-weight字体浓淡

| 可用值  | 说明           |
| ------- | -------------- |
| normal  | 字体正常显示。 |
| bold    | 粗体           |
| bolder  | 比粗体更加粗   |
| lighter | 比正常字体淡   |
| 100~900 |                |

### 字体大小

#### px

px 是绝对单位

因此只要设定多少px，就会精确的呈现

#### em

em是相对单位

为每个子元素通过「倍数」乘以父元素的px值

```text
每一层div都使用1.2em，最内层就会是16px x 1.2 x 1.2 x 1.2 x 1.2 x 1.2 = 39.8px。
(浏览器预设字体大小为16px，若无特别指定则会直接继承父元素字体大小)
```

#### %

%百分比是相对单位

```
m就是百分比除以一百，如果我们每一层div都使用120%，就等同于1.2em，最内层就会是16px x 1.2 x 1.2 x 1.2 x 1.2 x 1.2 = 39.8px。
```

#### rem

rem是相对单位

为每个元素通过「倍数」乘以根元素的px值

```
每一层div都使用1.2rem，最内层就会是16px x 1.2 = 19.2px。(根元素指的是html的font-size，预设为16px )。
```

### 阴影

#### 文本

text-shadow

#### div元素

box-shadow: 10px 10px 5px #888888;

| 值         | 描述                             |
| :--------- | :------------------------------- |
| *h-shadow* | 必需。水平阴影的位置。允许负值。 |
| *v-shadow* | 必需。垂直阴影的位置。允许负值。 |
| *blur*     | 可选。模糊的距离。               |
| *color*    | 可选。阴影的颜色。               |

## display属性

| 值                 | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| none               | 此元素不会被显示。                                           |
| block              | 此元素将显示为块级元素，此元素前后会带有换行符。             |
| inline             | 默认。此元素会被显示为内联元素，元素前后没有换行符。         |
| inline-block       | 行内块元素。                                                 |
| list-item          | 此元素会作为列表显示。                                       |
| run-in             | 此元素会根据上下文作为块级元素或内联元素显示。               |
| table              | 此元素会作为块级表格来显示（类似 <table>），表格前后带有换行符。 |
| inline-table       | 此元素会作为内联表格来显示（类似 <table>），表格前后没有换行符。 |
| table-row-group    | 此元素会作为一个或多个行的分组来显示（类似 <tbody>）。       |
| table-header-group | 此元素会作为一个或多个行的分组来显示（类似 <thead>）。       |
| table-footer-group | 此元素会作为一个或多个行的分组来显示（类似 <tfoot>）。       |
| table-row          | 此元素会作为一个表格行显示（类似 <tr>）。                    |
| table-column-group | 此元素会作为一个或多个列的分组来显示（类似 <colgroup>）。    |
| table-column       | 此元素会作为一个单元格列显示（类似 <col>）                   |
| table-cell         | 此元素会作为一个表格单元格显示（类似 <td> 和 <th>）          |
| table-caption      | 此元素会作为一个表格标题显示（类似 <caption>）               |
| inherit            | 规定应该从父元素继承 display 属性的值。                      |

## position定位

position有5个值，分别为[static](https://so.csdn.net/so/search?q=static&spm=1001.2101.3001.7020)，relative，absolute，fixed，sticky。

| 值       |                                                              |
| -------- | ------------------------------------------------------------ |
| static   | 默认属性                                                     |
| relative | 相对定位的元素                                               |
| absolute | 绝对定位                                                     |
| fixed    | 一直相对于浏览器窗口定位，无论页面如何滚动，此元素总在屏幕的同一个位置。 |
| sticky   | sticky粘性定位需要指定 top，right，bottom，left 四个阈值其中之一才会生效。 |

## z-index堆叠

## 外边框边缘

outline-offset属性在元素的轮廓与边框之间添加空间。元素及其轮廓之间的空间是透明的。

outline:2px solid red;
outline-offset:15px;



## 边距

### 外边距

margin

margin-top

margin-right

margin-bottom

margin-left

### 内边距

padding

padding-top

padding-right

padding-bottom

padding-left

## box-sizing

content-box 

```css
#example1 {
  box-sizing: content-box;  
  width: 300px;
  height: 100px;
  padding: 30px;  
  border: 10px solid blue;
}
```

高度和宽度只应用于元素的内容

![image-20221010135910583](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221010135910583.png)

border-box

```css
#example2 {
  box-sizing: border-box;
  width: 300px;
  height: 100px;
  padding: 30px;  
  border: 10px solid blue;
}
```

高度和宽度应用于元素的所有部分: 内容、内边距和边框

![image-20221010140009603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221010140009603.png)

inherit

指定 box-sizing 属性的值，应该从父元素继承

## 图片滤镜

filter

| 值                  |                                                              |
| ------------------- | ------------------------------------------------------------ |
| none                | 默认值                                                       |
| blur(*px*)          | 高斯模糊                                                     |
| brightness(*%*)     | 给图片应用一种线性乘法，使其看起来更亮或更暗。（值是0%，图像会全黑。值是100%，则图像无变化。其他的值对应线性乘数效果。值超过100%也是可以的，图像会比原来更亮。） |
| contrast(*%*)       | 调整图像的对比度。（值是0%的话，图像会全黑。值是100%，图像不变。值可以超过100%，意味着会运用更低的对比。若没有设置值，默认是1。） |
| drop-shadow         | 设置一个阴影效果                                             |
| Grayscale           | 图像转换为灰度图像                                           |
| hue-rotate()   函数 | 给图像应用色相旋转  例如：hue-rotate(90deg);                 |



## 背景

background:*transparent*,意思就代表背景透明

## 背景图片

### 不重复

background-repeat:no-repeat;

### 尺寸

background-size 

第一个值设置宽度，第二个值设置高度。

| 值      |                                                              |
| ------- | ------------------------------------------------------------ |
| cover   | 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。（背景图像的某些部分也许无法显示在背景定位区域中。） |
| contain | 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。 |

### 设置是否延伸

background-clip

设置元素的背景(背景图片或背景颜色)是否延伸至 `border-box` 下, `padding-box` 下, 或者 `content-box` 下

| 属性        |                                                              |
| ----------- | ------------------------------------------------------------ |
| border-box  | `默认值`. 背景延申至 `border` 的外沿, 但是在 `z` 方向上仍在 `border` 下层 |
| padding-box | 背景延申至 `padding` 的外沿, 不会绘制在边框下面              |
| content-box | 背景被裁剪至 `content` 内容区                                |
| text        | 背景被裁减成文字的形状                                       |



### 线性渐变

background-image: linear-gradient();

```css
/*background-image
					linear-gradient
					旋转度数	deg 
					repeating-linear-gradient	重复线性渐变*/
background-image: linear-gradient(0deg, red 0%, blue 100%);

/* 使用之前得设置宽高，-webkit-linear-gradient(参数1, 参数2, 参数3);*/
/*参数1表示渐变开始方向：可以写上下左右和角度（to left | to top |to right|to bottom|50deg|-50deg）等  */
/* 参数2可以设置两个值，一个代表渐变的起始颜色；一个为渐变的起始位置，可以填 像素或者百分比（正负数都可以） 代表从此位置开始渐变 */
/* 参数3可以设置两个值，一个代表渐的终止颜色；一个为渐变的结束位置，可以填 像素或者百分比（正负数都可以） 代表从此位置结束渐变 */
```

### 径向渐变

background-image: radial-gradient();

```css
/*两个颜色*/
div {
    width: 200px;
    height: 300px;
    border-radius: 50%;
    background-image: radial-gradient(yellow, pink);
}
/*圆形*/
background-image: radial-gradient(circle, red, blue);
/*椭圆形*/
background-image: radial-gradient(ellipse, red, blue);
/*重复渐变：记得加repeating在前面*/
background-image: repeating-radial-gradient(ellipse, pink 10%, blue 15%);
/*调整中心点的位置*/
background-image: repeating-radial-gradient(ellipse at 30% 40%, pink 10%, blue 15%);
```

### 背景层混合模式

background-blend-mode 属性定义了属性定义了背景层的混合模式（图片与颜色）

| 值          | 描述                         |
| :---------- | :--------------------------- |
| normal      | 默认值。设置正常的混合模式。 |
| multiply    | 正片叠底模式。               |
| screen      | 滤色模式。                   |
| overlay     | 叠加模式。                   |
| darken      | 变暗模式。                   |
| lighten     | 变亮模式。                   |
| color-dodge | 颜色减淡模式。               |
| saturation  | 饱和度模式。                 |
| color       | 颜色模式。                   |
| luminosity  | 亮度模式。                   |

## 是否可见

visibility在网页中该占的位置还是占着

```css
visibility:hidden;/*不可见*/
```

## 剪切形状

clip-path

### circle圆

circle()可以传入2个可选参数；

```
1. 圆的半径，默认元素宽高中短的那个为直径

   支持百分比

2. 圆心位置，默认为元素中心点
```

**示例 clip-path: circle(30% at 150px 120px);**

### ellipse椭圆

ellipse()可以传人3个可选参数；

```
1. 椭圆的X轴半径，默认是宽度的一半，支持百分比
2. 椭圆的Y轴半径，默认是高度的一半，支持百分比
3. 椭圆中心位置，默认是元素的中心点
```

**示例 clip-path: ellipse(45% 30% at 50% 50%);**

### polygon多边形

polygon() 

可选，表示填充规则用来确定该多边形的内部。可能的值有nonzero和evenodd,默认值是nonzero 后面的每对参数表示多边形的顶点坐标（X,Y），也就是连接点

**示例 clip-path: polygon(50% 0,100% 50%,0 100%);**   

## object-fit适应容器

| 值         |                                                              |
| ---------- | ------------------------------------------------------------ |
| fill       | 默认值                                                       |
| contain    | 缩放替换后的内容以保持其纵横比，同时将其放入元素的内容框     |
| cover      | 调整替换内容的大小在填充元素的整个内容框时保持其长宽比。该对象将被裁剪以适应。 |
| none       | 不对替换的内容调整大小                                       |
| scale-down | 调整内容大小就像没有指定内容或包含内容一样（将导致较小的具体对象尺寸） |



## opacity不透明度

```css
opacity:0.5;
```

## 列表

list-style

| 值                   | 描述                                  |
| -------------------- | ------------------------------------- |
| none                 | 无标记。                              |
| disc                 | 默认。标记是实心圆。                  |
| circle               | 空心圆                                |
| square               | 实心方块                              |
| decimal              | 数字                                  |
| 值                   | 描述                                  |
| none                 | 无标记。                              |
| disc                 | 默认。标记是实心圆。                  |
| circle               | 标记是空心圆。                        |
| square               | 标记是实心方块。                      |
| decimal              | 标记是数字。                          |
| decimal-leading-zero | 0开头的数字标记。(01, 02, 03, 等。)   |
| lower-roman          | 小写罗马数字(i, ii, iii, iv, v, 等。) |
| upper-roman          | 大写罗马数字(I, II, III, IV, V, 等。) |
| lower-alpha          | 小写英文字母                          |
| upper-alpha          | 大写英文字母                          |


​	


## 动画

### animation

使用简写属性，将动画与 div 元素绑定

```css
animation:mymove 5s infinite;
-webkit-animation:mymove 5s infinite; /* Safari 和 Chrome */
animation 属性是一个简写属性，用于设置六个动画属性：

/*用来设置
animation-name需要绑定到选择器的 keyframe 名称
animation-duration完成动画所花费的时间，以秒或毫秒计
animation-timing-function动画的速度曲线。
animation-delay在动画开始之前的延迟。
animation-iteration-count动画应该播放的次数。
animation-direction是否应该轮流反向播放动画。*/
```

### animation-name 

定义动画名字

### animation-duration

定义动画所需时间，以秒或毫秒计

### animation-timing-function

指定动画将如何完成一个周期

animation-timing-function使用的数学函数，称为三次贝塞尔曲线，速度曲线。

| 值                            |                                                              |
| ----------------------------- | ------------------------------------------------------------ |
| linear                        | 动画从头到尾的速度是相同的。                                 |
| ease                          | 默认。动画以低速开始，然后加快，在结束前变慢。               |
| ease-in                       | 动画以低速开始。                                             |
| ease-out                      | 动画以低速结束。                                             |
| ease-in-out                   | 动画以低速开始和结束。                                       |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。 |

```css
animation - timing -function: linear;
- webkit - animation - timing -function: linear;
/* Safari and Chrome */
```



### 旋转

| 值                                                           | 描述                                    |
| :----------------------------------------------------------- | :-------------------------------------- |
| none                                                         | 定义不进行转换。                        |
| matrix(*n*,*n*,*n*,*n*,*n*,*n*)                              | 定义 2D 转换，使用六个值的矩阵。        |
| matrix3d(*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。 |
| translate(*x*,*y*)                                           | 定义 2D 转换。                          |
| translate3d(*x*,*y*,*z*)                                     | 定义 3D 转换。                          |
| translateX(*x*)                                              | 定义转换，只是用 X 轴的值。             |
| translateY(*y*)                                              | 定义转换，只是用 Y 轴的值。             |
| translateZ(*z*)                                              | 定义 3D 转换，只是用 Z 轴的值。         |
| scale(*x*,*y*)                                               | 定义 2D 缩放转换。                      |
| scale3d(*x*,*y*,*z*)                                         | 定义 3D 缩放转换。                      |
| scaleX(*x*)                                                  | 通过设置 X 轴的值来定义缩放转换。       |
| scaleY(*y*)                                                  | 通过设置 Y 轴的值来定义缩放转换。       |
| scaleZ(*z*)                                                  | 通过设置 Z 轴的值来定义 3D 缩放转换。   |
| rotate(*angle*)                                              | 定义 2D 旋转，在参数中规定角度。        |
| rotate3d(*x*,*y*,*z*,*angle*)                                | 定义 3D 旋转。                          |
| rotateX(*angle*)                                             | 定义沿着 X 轴的 3D 旋转。               |
| rotateY(*angle*)                                             | 定义沿着 Y 轴的 3D 旋转。               |
| rotateZ(*angle*)                                             | 定义沿着 Z 轴的 3D 旋转。               |
| skew(*x-angle*,*y-angle*)                                    | 定义沿着 X 和 Y 轴的 2D 倾斜转换。      |
| skewX(*angle*)                                               | 定义沿着 X 轴的 2D 倾斜转换。           |
| skewY(*angle*)                                               | 定义沿着 Y 轴的 2D 倾斜转换。           |
| perspective(*n*)                                             | 为 3D 转换元素定义透视视图。            |

## backface-visibilit隐藏旋转

隐藏被旋转的 div 元素的背面

```css
div
{
backface-visibility:hidden;
-webkit-backface-visibility:hidden;	/* Chrome 和 Safari */
-moz-backface-visibility:hidden; 	/* Firefox */
-ms-backface-visibility:hidden; 	/* Internet Explorer */
}
```

## 元素变大

transition

把鼠标指针放到 div 元素上，其宽度会从 100px 逐渐变为 300px：

```css
div
{
width:100px;
transition: width 2s;
-moz-transition: width 2s; /* Firefox 4 */
-webkit-transition: width 2s; /* Safari 和 Chrome */
-o-transition: width 2s; /* Opera */
}
```



## 滑动条

Overflow

| 值      | 描述                                                     |
| :------ | :------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。                 |

## 字符距离

letter-spacing

设置字符之间的距离（可以为负值）

normal：默认 

```css
p{        
	letter-spacing:1em;//注意：1em=16px
}
p{        
	letter-spacing:-0.5em;
}

```

## 列

column-count 分列数

column-gap 指定列之间的的间隙 column-gap: length|normal;

column-rule 设置列之间的宽度，样式和颜色

## 布局

### flex

水平主轴(main axis) 和垂直的交叉轴(cross axis)

每个单元块被称之为 flex item，每个项目占据的主轴空间为 (main size), 占据的交叉轴的空间为 (cross size)

![30 分钟学会 Flex 布局](https://pica.zhimg.com/v2-54a0fc96ef4f455aefb8ee4bc133291b_720w.jpg?source=172ae18b)

```css
.container {
    display: flex | inline-flex;       //可以有两种取值
    //使用块元素 flex 使用行内元素 inline-flex
}
```

**需要注意的是：当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效。**

#### 项目排列方向

flex-direction: 决定主轴的方向(即项目的排列方向)

| 值             | 描述                           |
| -------------- | ------------------------------ |
| row            | 主轴为水平方向，起点在左端     |
| row-reverse    | 主轴为水平方向，起点在右端     |
| column         | 主轴为**垂直**方向，起点在上沿 |
| column-reverse | 主轴为**垂直**方向，起点在下沿 |

#### 是否可换行

flex-wrap :可实现项目的换行

| 值           |                                    |
| ------------ | ---------------------------------- |
| nowrap       | 不换行                             |
| wrap         | 超出容器时换行（第一行在**上方**） |
| wrap-reverse | 换行（第一行在**下方**）           |

#### 主轴的对齐方式

 justify-content：定义了项目在主轴的对齐方式。

| 值            |                                                              |
| ------------- | ------------------------------------------------------------ |
| flex-start    | 左对齐![img](https://pic1.zhimg.com/80/v2-1bafab80044a7ab2a6198d5937172eb0_720w.webp) |
| flex-end      | 右对齐![img](https://pic3.zhimg.com/80/v2-8b163809a4c944486a127a7c22eee7b2_720w.webp) |
| center        | 居中![img](https://pic4.zhimg.com/80/v2-dea82c75d35f532d35a52d1f9c1c762b_720w.webp) |
| space-between | 两端对齐，项目之间的间隔相等，即剩余空间等分成间隙。![img](https://pic1.zhimg.com/80/v2-ea4061e0f64dd8d7a1fcb5b0ad6f96a8_720w.webp) |
| space-around  | 每个项目两侧的间隔相等，所以项目之间的间隔比项目与边缘的间隔大一倍。![img](https://pic1.zhimg.com/80/v2-42a358111a221ff52768bdd55238eb0c_720w.webp) |

#### 交叉轴上的对齐方式

align-items: 定义了项目在交叉轴上的对齐方式

| 值         |                                                              |                                                              |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| stretch    | 如果项目未设置高度或者设为 auto，将占满整个容器的高度。（假设容器高度设置为 100px，而项目都没有设置高度的情况下，则项目的高度也为 100px。） | ![img](https://pic2.zhimg.com/80/v2-0cced8789b0d73edf0844aaa3a08926d_720w.webp) |
| flex-start | 交叉轴的起点对齐                                             | ![img](https://pic3.zhimg.com/80/v2-26d9e85039beedd78e412459bd436e8a_720w.webp) |
| flex-end   | 交叉轴的终点对齐                                             | ![img](https://pic4.zhimg.com/80/v2-8b65ee47605a48ad2947b9ef4e4b01b3_720w.webp) |
| center     | 交叉轴的中点对齐                                             | ![img](https://pic3.zhimg.com/80/v2-7bb9d8385273d8ad469605480f40f8f2_720w.webp) |
| baseline   | 项目的第一行文字的基线对齐                                   | ![img](https://pic3.zhimg.com/80/v2-abf7ac4776302ad078986f7cd0dddaee_720w.webp) |

#### 多根轴线对齐方式

align-content: 定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用

| 值            |                                                              |                                                              |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| flex-start    | 轴线全部在交叉轴上的**起点**对齐                             | ![img](https://pic2.zhimg.com/80/v2-61d92d7dc68e3d7d415a16830050fd11_720w.webp) |
| flex-end      | 轴线全部在交叉轴上的**终点**对齐                             | ![img](https://pic2.zhimg.com/80/v2-0a0a7f10c50596aade787ae11b7b0a75_720w.webp) |
| center        | 轴线全部在交叉轴上的**中间**对齐                             | ![img](https://pic1.zhimg.com/80/v2-dcf53fce8dbcde7da9c677dd1a033860_720w.webp) |
| space-between | 轴线两端对齐，之间的间隔相等，即剩余空间等分成间隙。         | ![img](https://pic2.zhimg.com/80/v2-d80940f71e1e08d45d3d6df4c5401d0d_720w.webp) |
| space-around  | 每个轴线两侧的间隔相等，所以轴线之间的间隔比轴线与边缘的间隔大一倍。 | ![img](https://pic2.zhimg.com/80/v2-7c4d5c01f3851a3cec7f8487c6edb21d_720w.webp) |
| stretch       | 默认值 三条轴线平分容器的垂直方向上的空间。（不设置高度会撑开整个容器） | ![img](https://pic2.zhimg.com/80/v2-c284017b4b8b731bc213cd1caab514e5_720w.webp) |

### item

##### 排列顺序

 **order: 定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0**

##### 基础尺寸

 flex-basis: 定义了在分配多余空间之前，项目占据的主轴空间

flex-basis浏览器根据这个属性，计算主轴是否有多余空间

```javascript
.item {
    flex-basis: <length> | auto;
}
```

**当主轴为水平方向的时候，当设置了 flex-basis，项目的宽度设置值会失效，flex-basis 需要跟 flex-grow 和 flex-shrink 配合使用才能发挥效果。**

当 flex-basis 值为 0 % 时

该项目视为零尺寸的，故即使声明该尺寸为 140px，也并没有什么用。

当 flex-basis 值为 auto 时

则跟根据尺寸的设定值(假如为 100px)，则这 100px 不会纳入剩余空间。

##### **放大比例**

```css
.item {
    flex-grow: <number>;
}
```

如果所有项目的 flex-grow 属性都为 1

​	则它们将**等分剩余空间**。(如果有的话)

如果一个项目的 flex-grow 属性为 2

​	其他项目都为 1，则前者占据的剩余空间将比其他项**多一倍**。

##### **缩小比例**

```css
.item {
    flex-shrink: <number>;
}
```

如果所有项目的 flex-shrink 属性都为 1

当空间不足时，都将**等比例缩小**。

如果一个项目的 flex-shrink 属性为 0，其他项目都为 1

则空间不足时，前者**不缩小**。

**flex: flex-grow, flex-shrink 和 flex-basis的简写**

##### **对齐方式**

align-self: 允许**单个项目**有与其他项目不一样的对齐方式

```css
.item {
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```



### Grid

容器里面的水平区域称为"行"（row），垂直区域称为"列"（column）。

默认情况下，容器元素都是块级元素(grid)，但也可以设成行内元素。(inline-grid)

```css
div {
  display: inline-grid;
}
```

#### 列宽

grid-template-columns属性定义每一列的列宽

#### 行高

grid-template-rows属性定义每一行的行高。

#### 重复值

repeat()

第一个参数是重复的次数，第二个参数是所要重复的值。

```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 33.33%);
  grid-template-rows: repeat(3, 33.33%);
}
```

（重复某种模式也是可以的）

```css
grid-template-columns: repeat(2, 100px 20px 80px);
```

#### 关键字

##### 自动填充

auto-fill

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
}
```

[上面代码](https://jsbin.com/himoku/edit?css,output)表示每列宽度`100px`，然后自动填充，直到容器不能放置更多的列。

##### 倍数

**fr**如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。

> ```css
> .container {
>   display: grid;
>   grid-template-columns: 150px 1fr 2fr;
> }
> //第一列的宽度为150像素，第二列的宽度是第三列的一半。
> ```

##### 浏览器决定长度

auto关键字表示由浏览器自己决定长度

```css
grid-template-columns: 100px auto 100px;
```

#### 长度范围

`minmax()`函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

> ```css
> grid-template-columns: 1fr 1fr minmax(100px, 1fr);
> ```

上面代码中，`minmax(100px, 1fr)`表示列宽不小于`100px`，不大于`1fr`。

#### 网格线的名称

[]

`grid-template-columns`属性和`grid-template-rows`属性

还可以使用方括号[]，指定每一根网格线的名字，方便以后的引用。

```css
.container {
  display: grid;
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

注：**网格布局允许同一根线有多个名字**

#### 两栏式布局

grid-template-columns

```css
.wrapper {
  display: grid;
  grid-template-columns: 70% 30%;
}
//上面代码将左边栏设为70%，右边栏设为30%。
```

#### 行间距

`grid-row-gap`属性设置行与行的间隔（行间距）

#### 列间距

`grid-column-gap`属性设置列与列的间隔（列间距）。

合并写法

`grid-gap`属性是`grid-column-gap`和`grid-row-gap`的合并简写形式

`grid-template-areas`属性用于定义区域。

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
}
```





## 连字符

```
 hyphens:none|manual|auto|initial|inherit;
```

![image-20221010162530250](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20221010162530250.png)

## 检测

@supports检测当前浏览器是否支持某个CSS属性并加载具体样式

支持逻辑运算符；not, and, or

```css
@supports not(display: grid){...}
@supports (display: grid) and (position: sticky){...}
@supports (display: grid) or (display: flex){...}
```

括号内不一定都要是“关键字”，只要是CSS语法都可以

```css
@supports (border-radius: 4px) or (--btn-color: red){...}
```

是否有级联 是否有继承属性

字体系列，字体大小、颜色等属性

如果写个固定的10px，在浏览器设置里面去设置字体大小就变得无效了！！

组件驱动设计

BEM系统 修饰符

## CSS变量

`currentColor`顾名思意就是“当前颜色”

 `transparent`属性用来指定全透明色彩
