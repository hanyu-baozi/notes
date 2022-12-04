## 嵌套

```less
.container{
    h1{
        color:red;
    }
    p{
        color:yellow;
    }
}
```

```css
.container h1 {
 color:red;
}
.container p {
 color:yellow;
}
```

## 算术运算符

加号(+)，减号( - )，乘法(*)和除法(/)

```less
@size:10px;
.class{
    font-size:@size*2;
}
```

```css
.class{
font-size:20px;
}
```

## 转义

使用属性或变量值作为任意字符串。

```less
p{
	color:~"green";
}
```

```css
p{
	color:green;
}
```

## 函数

圆函数，floor 函数，ceil 函数，百分比函数等。

```less
@width:1.0;
.mycolor{
 width: percentage(@width);//百分比
}
```

```css
.mycolor{
	width:100%;
}
```

## 命名空间和访问器

命名空间可以避免名称冲突，并从外部封装mixin组。

```less
.class1 {
  .class2 {
    .val(@param) {
      font-size: @param;
      color:green;
    }
  }
}

.myclass {
  .class1 > .class2 > .val(20px);
}

```

```css
.myclass{
	font-size:20px;
	color:green;
}
```

## 变量范围

变量范围指定可用变量的位置。 变量将从本地作用域搜索，如果它们不可用，则编译器将从父作用域搜索。

```less
@var: @a;
@a: 15px;

.myclass {
  font-size: @var;
  @a:20px;
  color: green;
}
```

```css
.myclass {
  font-size: 20px;
  color: green;
}
```

## 注释

less：// 转换css：/**/

变量

@定义变量，声明变量的格式为“@变量名：变量值”

扩展

:extend 在一个选择器中扩展其他选择器样式

```less
h2{
    &:extend(.style);
    font-style:italic;
}
.style{
    background:green;
}
```



```css
h2 {
  font-style: italic;
}
.style,
h2 {
  background: green;
}
```

## 混合

​	

```less
.p1{
    color:red;
}
.p2{
    background:#64d9c0;
    .p1();
}
```

```css
.p1 {
  color: red;
}
.p2 {
  background: #64d9c0;
  color: red;
}
```

## 混合参数

使用一个或多个参数

```less
.border(@width;@style;@color){
    boder:@width @style ;
}
.myheader {
    .border(2px; dashed;red);
}

```

```css
.myheader {
  boder: 2px dashed red;
}

```

## 规则集传递

```less
@detached:{
    .mixin(){
    font-family:"Comic Sans MS";
    background-color:red;
    }
};
.cont{
    @detached();
    .mixin();
}
```

```css
.cont{
	font-family:"Comic Sans MS";
    background-color:red;
}
```

## Mixin 

如果你想在表达式上匹配简单的值或参数数量，那么你可以使用Guards。 它与mixin声明相关联，并包括附加到mixin的条件。 每个mixin将有一个或多个由逗号分隔的防护，并且guard必须括在括号中。 LESS使用Guards的mixins而不是if / else语句，并执行计算以指定匹配的mixin。

```less
.mixin (@a) when (lightness(@a) >= 50%) {
   font-size: 14px;
}
.mixin (@a) when (lightness(@a) < 50%) {
   font-size: 16px;
}
.mixin (@a) {
   color: @a;
}
.class1 {
   .mixin(#FF0000)
}
.class2 {
   .mixin(#555)
}
```

```css
.class1 {
  font-size: 14px;
  color: #FF0000;
}
.class2 {
  font-size: 16px;
  color: #555;
}
```

## Guards

if 与&结合

```less
@usedScope: global;
.mixin() {
  @usedScope: mixin;
  .cont when (@usedScope=global) {
    background-color: red;
    color: black;
  }
  .style when (@usedScope=mixin) {
    background-color: blue;
    color: white;
  }
  @usedScope: mixin;
}
.mixin();
```

```css
.style {
  background-color: blue;
  color: white;
}
```

## 循环

递归mixin与 **Guard表达式**和**模式匹配**组合

```less
.cont(@count)when(@count>0){
    .cont((@count - 1));
    width:(25px * @count);
}
div{
    .cont(7);
}
```

```css
div {
  width: 25px;
  width: 50px;
  width: 75px;
  width: 100px;
  width: 125px;
  width: 150px;
  width: 175px;
}
```

## 父选择器

&（和号）引用父选择器

```less
a{
    color:red;
    &:hover{
        background-color:green;
    }
}
```

```css
a {
  color: red;
}
a:hover {
  background-color: green;
}

```

## less 其他函数



| color        | 代表颜色的字符串                                             |
| ------------ | ------------------------------------------------------------ |
| image-size   | 文件检查图像的大小                                           |
| image-width  | 图像的宽度                                                   |
| image-height | 检查图像从文件的高度                                         |
| convert      | 数字从一个单位转换为另一个单位                               |
| data-uri     | 数据uri是统一资源标识符(URI)模式，其在网页中嵌入资源。       |
| default      | 默认函数仅在保护条件内可用且与任何其他混合程序不匹配时才返回true。 |
| unit         |                                                              |
| get-unit     |                                                              |
| svg-gradient |                                                              |

## default

```less
.mixin(1)                   {a: 5}
.mixin(2)                   {b: 10}
.mixin(3)                   {c: 15}
.mixin(@a) when (default()) {d: @a}

div {
  .mixin(12);
}

div.style {
  .mixin(3);
```

```css
div {
  d: 12;
}
div.special {
  c: 15;
}
```

## 字符串函数

| 类型    | 描述                |      |
| ------- | ------------------- | ---- |
| Escape  | 特殊字符使用URL编码 |      |
| e       | 字符串函数          |      |
| %format | 格式化一个字符串    |      |
| replace | 替换字符串中的文本  |      |

## 列表函数

length()

```less
p{
	@list:"a","b","c","d","e","f",
	@val:length(@list)
	font-size:@val;
}
```

```css
p{
    font-size:6;
}
```

### 列表提取函数

extract(@list, 2)



## 数学函数

| 函数       | 描述                                                         | 例子                       |
| ---------- | ------------------------------------------------------------ | -------------------------- |
| ceil       | 向上舍入为下一个最大整数                                     | ceil(0.7)→1                |
| floor      | 向下取整为下一个最小整数                                     | floor(3.3)→3               |
| percentage | 浮点数转换为百分比字符串                                     | percentage(0.2)→20%        |
| round      | 舍入浮点数                                                   | round(3.77)→4              |
| sqrt       | 平方根                                                       | sqrt(25)→5                 |
| abs        | 数字的绝对值                                                 | abs(30ft)→30ft             |
| sin        | 返回数字上的弧度                                             | sin(2)→0.90929742682       |
| asin       | 返回-pi / 2和pi / 2之间的值的数字的反正弦(反正弦)。          | asin(1)→1.5707963267948966 |
| cos        | 返回指定值的余弦值，并在没有单位的数字上确定弧度             | cos(2)→-0.4161468365471424 |
| acos       | 返回0和pi之间的值的数字的反余弦                              | acos(1)→0                  |
| tan        | 数字的正切                                                   | tan(60)→0.320040389379563  |
| atan       | 数字的反正切(反正切)                                         | atan(1)→0.7853981633974483 |
| pi         | 返回pi值                                                     | pi()→3.141592653589793     |
| pow        | 指定第一个参数的值增加到第二个参数的权力                     | pow(3,3)→27                |
| mod        | 返回相对于第二个参数的第一个参数的模数。 它还处理负点和浮点数 | mod(7,3)→1                 |
| min        | 一个或多个参数的最小值                                       | min(70,30,45,20)→20        |
| max        | 一个或多个参数的最大值                                       | max(70,30,45,20)→70        |

## 类型函数

| 函数         | 描述                                                         | 例子                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| isnumber     | 使用一个值作为参数，并返回 true ，如果它是一个数字或 false   | isnumber(1234); // trueisnumber(24px); // trueisnumber(7.8%); // trueisnumber(#fff); // falseisnumber(red); // falseisnumber(“variable"); // falseisnumber(keyword); // falseisnumber(url(...)); // false |
| isstring     | 使用一个值作为参数，并返回 true ，如果它是一个字符串或 false | isstring("variable"); // true isstring(1234);       // false isstring(24px);       // false isstring(7.8%);       // false isstring(#fff);       // false isstring(red);        // false isstring(keyword);    // false isstring(url(...));   // false |
| iscolor      | 使用一个值作为参数，并返回 true (如果值是颜色)或 false (如果不是)。 | iscolor(#fff);        // true<br/>iscolor(red);         // true<br/>iscolor(1234);        // false<br/>iscolor(24px);        // false<br/>iscolor(7.8%);        // false<br/>iscolor("variable");  // false<br/>iscolor(keyword);     // false<br/>iscolor(url(...));    // false |
| iskeyword    | 使用一个值作为参数，并返回 *true* (如果值是关键字)或 *false* (如果不是) | iskeyword(keyword);   // true<br/>iskeyword(1234);      // false<br/>iskeyword(24px);      // false<br/>iskeyword(7.8%);      // false<br/>iskeyword(#fff);      // false<br/>iskeyword(red) ;      // false<br/>iskeyword("variable");// false<br/>iskeyword(url(...));  // false |
| isurl        | 使用一个值作为参数，并返回 true (如果值为url)或 false (如果值不为) | isurl(url(...));      // true<br/>isurl(keyword);       // false<br/>isurl(1234);          // false<br/>isurl(24px);          // false<br/>isurl(7.8%);          // false<br/>isurl(#fff);          // false<br/>isurl(red) ;          // false<br/>isurl("variable");    // false |
| ispixel      | 值是以像素为单位的数字，或者 false ，则以值为参数返回 true   | ispixel(24px);          // true<br/>ispixel(1234);          // false<br/>ispixel(7.8%);          // false<br/>ispixel(keyword);       // false<br/>ispixel(#fff);          // false<br/>ispixel(red) ;          // false<br/>ispixel("variable");    // false<br/>ispixel(url(...));      // false |
| isem         | 采用一个值作为参数，并返回 true ，如果值是em值或 false (如果值不是) | isem(0.5em);            // true<br/>isem(1234);             // false<br/>isem(24px);             // false<br/>isem(keyword);          // false<br/>isem(#fff);             // false<br/>isem(red) ;             // false<br/>isem("variable");       // false<br/>isem(url(...));         // false |
| ispercentage | 采用一个值作为参数，如果值不是百分比，则返回 true ，如果值为百分比，或返回 false | ispercentage(7.5%);       // true<br/>ispercentage(url(...));   // false<br/>ispercentage(keyword);    // false<br/>ispercentage(1234);       // false<br/>ispercentage(24px);       // false<br/>ispercentage(#fff);       // false<br/>ispercentage(red) ;       // false<br/>ispercentage("variable"); // false |
| isunit       | 值是指定单位中作为参数提供的数字，则返回 true ，如果值不是指定单位中的数字，则返回 false | isunit(10px, px);       // true<br/>isunit(5rem, rem);      // true<br/>isunit(7.8%, '%');      // true<br/>isunit(2.2%, px);       // false<br/>isunit(24px, rem);      // false<br/>isunit(48px, "%");      // false<br/>isunit(1234, em);       // false<br/>isunit(#fff, pt);       // false<br/>isunit("mm", mm);       // false |
| isruleset    | 一个值作为参数，并返回 true ，如果值为规则集或 false         | @rules: {<br/>    color: green;<br/>}<br/>isruleset(@rules);      // true<br/>isruleset(1234);        // false<br/>isruleset(24px);        // false<br/>isruleset(7.8%);        // false<br/>isruleset(#fff);        // false<br/>isruleset(blue);        // false<br/>isruleset("variable");  // false<br/>isruleset(keyword);     // false<br/>isruleset(url(...));    // false |

## 颜色定义函数

| 函数 | 描述                                                         | 例子                                         |
| ---- | ------------------------------------------------------------ | -------------------------------------------- |
| rgb  | 从红色（0 - 255），绿色（0 - 255）和蓝色（0 - 255）值的颜色  | rgb(220,20,60)→#dc143c                       |
| rgba | 红色:包含介于0 - 255之间的整数或介于0 - 100%之间的百分比。<br/>绿色:它包含介于0 - 255之间的整数或介于0 - 100%之间的百分比。<br/>蓝色:包含介于0 - 255之间的整数或介于0 - 100%之间的百分比。<br/>alpha :它包含0到1之间的数字或0到100%之间的百分比。 | rgba(220,20,60,0.5)                          |
| hsl  | hue :它包含介于0 - 360之间的整数，表示度数。<br/>饱和度:它包含介于0 - 1之间的数字或介于0 - 100%之间的百分比。<br/>亮度:它包含介于0 - 1之间的数字或介于0 - 100%之间的百分比。 | hsl(120,100%，50%)→#00ff00                   |
| hsla | hue 、饱和度、亮度、alpha:它包含0到1之间的数字或0到100%之间的百分比。 | hsla(0,100%，50%，0.5)→rgba(255，0，0，0.5); |
| hsv  | hue、饱和度、value：它包含介于0 - 1之间的数字或介于0 - 100%之间的百分比。 | hsv(80,90%，70%)→#7db312                     |
| hsva | hue、饱和度、value、alpha                                    | hsva(80,90%，70%，0.6)→rgba(125,179,18,0.6)  |

# 颜色通道函数

| 函数             | 描述                     | 例子                                                        |
| ---------------- | ------------------------ | ----------------------------------------------------------- |
| hue              | 提取颜色对象的色调通道。 | background: hue(hsl(75, 90%, 30%));→background: 75;         |
| saturation       | 提取彩色对象的饱和通道。 | background: saturation(hsl(75, 90%, 30%));→background: 90%; |
| lightness        | 颜色对象提取亮度通道。   | background: lightness(hsl(75, 90%, 30%));→background: 30%;  |
| hsvhue           | 提取色彩对象的色调通道。 | background: hsvhue(hsv(75, 90%, 30%));→background: 75;      |
| hsvsaturation    | 提取彩色对象的饱和通道。 | background: hsvsaturation(hsv(75, 90%, 30%));→              |
| hsvvalue         |                          |                                                             |
| red、green、blue |                          |                                                             |
| alpha            |                          |                                                             |
| luma             |                          |                                                             |
| luminance        |                          |                                                             |

