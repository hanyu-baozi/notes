## 分层

### abstracts抽象

_functions.scss 

_mixins.scss

清除浮动 居中 各设备媒体查询

_variables.scss

颜色 字体大小 层叠样式...

###  base基础

_animations.scss

动画

_base.scss

基础样式

_typograph.scss

对字体、字号、缩进、行间距、字符间距进行设计、安排

_utillties.scss

公用 比如存放字体居中 外边距最大最小

### components 部件

_bg-video.scss

视频

_button.scss

按钮

_card.scss

卡片

_composition.scss

组成（照片和字的位置）

_feature-box.scss

水波纹样式

_form.scss

表样式

_popup.scss

弹出窗口样式

_story.scsss

描述板块

###  layout布局

_footer.scss

底部导航

_grid.scss

row col-1of -2......

_header.scss

头部

_navigation.scss

导航（复选框、按钮、背景、导航项、icon）



###  pages页面

_home.scss

.section-about .setion-features....

页面主要部分（各模块）

### mains.scss

@import "路径";

导入各scss

## & 与

## $变量

## @mixin 

指令允许我们定义一个可以在**整个样式表**中重复使用的样式。

## @include 

指令可以将混入（mixin）引入到文档中。

```scss
@mixin important-text {
  color: red;
  font-size: 25px;
  font-weight: bold
  border: 1px solid blue;
}
.danger {
  @include important-text;
  background-color: green;
}
```

转换为css

```css
.danger {
  color: red;
  font-size: 25px;
  font-weight: bold;
  border: 1px solid blue;
  background-color: green;
}
```

向混入传递变量

不确定使用多少个参数可用可变参数（使用...）

```scss
/* 混入接收两个参数 */
@mixin bordered($color, $width) {
  border: $width solid $color;
}

.myArticle {
  @include bordered(blue, 1px);  // 调用混入，并传递两个参数
}

.myNotes {
  @include bordered(red, 2px); // 调用混入，并传递两个参数
}
```

转化为css

```css
.myArticle {
  border: 1px solid blue;
}

.myNotes {
  border: 2px solid red;
}
```

@function函数名
使用 %定义继承样式

## @if

判断

## @media

媒体查询

```scss
@media screen and (max-width: 300px){}
@media only screen and (min-width: 112.5em){}
```

##  @keyframes 创建动画

```scss
@keyframes pulse{
    0%{}
    80%{}
    100%{}
}
```

## #{}插值

```scss
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```

转换为css

```css
p {
  font: 12px/30px; }
```

## @for循环

## @content自定义

1. `@content`用在`mixin`里面的，当定义一个`mixin`后，并且设置了`@content`；
2. `@include`的时候可以传入相应的内容到`mixin`里面

## @each遍历列表

## @while 循环

## Partials