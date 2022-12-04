# 第一部分 作用域和闭包

​	

## 第一章 作用域是什么

### 引擎

负责整个js程序的编译与运行过程

### 编译器

进行语法分析与代码生成

### 作用域

收集并维护所有声明的标识符变量，并提供一套存储与访问的规则。

### var a = 2;运行时发生了什么？

```
var a = 2;会被拆解成var a; 和 a = 2;
```

1.处理var a

编译器询问作用域是否有该名称的变量存在于同一个作用域的集合中。

**是** 编译器则忽略该声明

**否** 作用域在当前作用域集合中声明一个新的变量，并命名为a

2.处理a=2;

引擎运行时首先询问作用域，在当前作用域集合中是否存在一个叫作a的变量。

**是** 引擎使用这个变量

**否** 引擎继续查找该变量

**总结**：声明变量（如果之前没有声明过），查找变量赋值

### LHS查询和RHS查询

当变量出现在赋值操作

**左侧**时进行LHS查询

**右侧**时进行RHS查询

```
1、LHS:Left左，注重a = 2的左边，也就是已知有个2,要进行赋值操作时，就执行LHS查询，不会去在意a的值，只在乎要有没有a
2、RHS:Right右，注重a = 2的右边，也就是已知有个a，要知道a的值查询，就执行RHS查询，比如console.log(a)，去找console对象是否有log()方法，a的值是多少
```



```javascript
function foo(a) {
  console.log(a);
}
foo(2);
// 1、RHS查询foo变量的值，有它是一个函数
// 2、LHS查询是否有a变量，有声明再函数foo()作用域中
// 3、RHS查询console对象是否有log()方法,执行log()
// 4、RHS查询a的值是多少，输出a
```

### 代码验证第1点

```javascript
function fn(a) {
  // 进行RHS查询，查询不到,引擎报错ReferenceError: b is not definedat fn
  console.log(a + b);
}
fn(2);
```

### 代码验证第2点

```javascript
{
    //非严格模式下，在当前作用域中以及全局作用域中进行LHS查询，未查询到变量a，会隐式的在全局作用域中声明变量a，并赋值
    a = 2; {
        // 找到了全局作用域中隐式添加的var a = 2;
        console.log(a); //2
    }
}
console.log(a); //2
```



```javascript
"use strict";

{
    //严格模式下，在当前作用域中以及全局作用域中进行LHS查询，未查询到变量a，报错ReferenceError: a is not defined
    a = 2; {
        console.log(a);
    }
}
console.log(a);
```

### 代码验证第3点

```javascript
var a = 1;
a();//TypeError: a is not a function
```



### 异常

```
1、如果RHS查询在所有嵌套的作用域中遍寻不到所需的变量，引擎就会抛出 ReferenceError异常
2、LHS查询时，如果在顶层（全局作用域）中也无法找到目标变量，全局作用域中就会创建一个具有该名称的变量，并将其返还给引擎，前提是程序运行在非“严格模式”下。ES5 中引入了“严格模式”严格模式禁止自动或隐式地创建全局变量。因此，在严格模式中 LHS 查询失败时，并不会创建并返回一个全局变量，引擎会抛出同 RHS 查询失败时类似的 ReferenceError 异常
3、如果RHS查询找到了一个变量，但是你尝试对这个变量的值进行不合理的操作，比如试图对一个非函数类型的值进行函数调用，或着引用 null 或 undefined 类型的值中的属性，那么引擎会抛出另外一种类型的异常，叫作 TypeError，TypeError 代表作用域判别成功了，但是对结果的操作是非法或不合理
```



### 作用域的嵌套

```
函数和{}都是块级作用域，在作用域中查找变量的过程

当前作用域——【没找到】——>上一级作用域—【一直未没找到】——>全局作用域

找到全局作用域时，不管找没有找到都会结束查找，其中没找到会报错
```

 

## 第二章 词法作用域

### 2.1词法阶段

大部分标准语言编译器的第一个工作阶段叫作词法化（也叫单词化）。词法作用域就是定义在词法阶段的作用域，词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变（大部分情况下是这样的）。部分情况存在欺骗词法作用域。



```
下面代码包含三个词法作用域
全局作用域{
	fn函数作用域{
		a
		b
		fn1函数作用域{
			c
		}
	}
}
我们来分析console.log(a,b,c);
先来找a，从fn1开始进行RHS查找，没有找到，根据作用域嵌套去查找fn函数作用域，查找到fn函数的参数a，之前说过，函数的参数变量处于函数作用域中，参数a的值为fn(2);传入得知为2。
再来找b，进行RHS查找，fn1作用域未找到，在fn作用域中找到了。值为4
再来找c，进行RHS查找，fn1作用域中找到了，值为fn1(b*2)决定为8
再来分析console.log(c)；进行RHS查找，在fn作用域中未找到，再去全局作用域中也没有找到。因此报错c is not defined
```



```javascript
function fn(a) {
    var b = a * 2;
    function fn1(c){
        console.log(a,b,c); //2,4,8
    }
    fn1(b*2);
    console.log(c);//c is not defined
}

fn(2);
```

​	

函数的参数属于该函数的作用域，而不属于该函数所在的作用域



### 查找

作用域查找会在找到第一个匹配的标识符时停止。在多层的嵌套作用域中可以定义同名的标识符，这叫作“遮蔽效应”（内部的标识符“遮蔽”了外部的标识符）。抛开遮蔽效应，作用域查找始终从运行时所处的最内部作用域开始，逐级向外或者说向上进行，直到遇见第一个匹配的标识符为止。



全局变量会自动成为全局对象（比如浏览器中的 window 对象）的属性，因此可以不直接通过全局对象的词法名称，而是间接地通过对全局对象属性的引用来对其进行访问。通过这种技术可以访问那些被同名变量所遮蔽的全局变量。



```javascript
var a = 1;
function fn(){
  var a = 2;
  console.log(window.a);
}
fn();//1
```



但非全局的变量如果被遮蔽了，无论如何都无法被访问到。一个解决办法手动将值赋值给全局变量

```javascript
function foo(a) {
  obj.a = a;
}

let obj = {}

foo(2);

console.log(obj.a);//2
```



无论函数在哪里被调用，也无论它如何被调用，它的词法作用域都只由函数被声明时所处的位置决定。



词法作用域查找只会查找一级标识符，比如 a、b 和 c。如果代码中引用了 foo.bar.baz，词法作用域查找只会试图查找 foo 标识符，找到这个变量后，“对象属性访问规则”会分别接管对 bar 和 baz 属性的访问。



### 欺骗词法

（词法作用域完全由写代码期间函数所声明的位置来定义）但是JavaScript有两种机制可以欺骗词法作用域。欺骗词法作用域是在运行时改变词法作用域，但是要注意的是欺骗词法作用域会导致性能下降。



### eval

JavaScript 中的 eval(..) 函数可以接受一个字符串为参数，并将其中的内容视为好像在书写时就存在于程序中这个位置的代码。在执行 eval(..) 之后的代码时，引擎并不“知道”或“在意”前面的代码是以动态形式插入进来，并对词法作用域的环境进行修改的。引擎只会如往常地进行词法作用域查找。



```javascript

function foo(str,a) {
  eval(str);
  console.log(a,b);
}
var b = 2;
fn('var b = 3',1);//1,3
```



当 console.log(..) 被执行时，会在 foo(..) 的内部同时找到 a 和 b，但是永远也无法找到外部的 b。因此会输出“1, 3”而不是正常情况下会输出的“1,2”。在这个例子中，为了展示的方便和简洁，我们传递进去的“代码”字符串是固定不变的。而在实际情况中，可以非常容易地根据程序逻辑动态地将字符拼接在一起之后再传递进去。eval(..) 通常被用来执行动态创建的代码，因为像例子中这样动态地执行一段固定字符所组成的代码，并没有比直接将代码写在那里更有好处。



如果 eval(..) 中所执行的代码包含有一个或多个声明（无论是变量还是函数），就会对 eval(..) 所处的词法作用域进行修改
在严格模式的程序中，eval(..) 在运行时有其自己的词法作用域，意味着其中的声明无法修改所在的作用域。
在程序中动态生成代码的使用场景非常罕见，因为它所带来的好处无法抵消性能上的损失。 



```javascript
var b = 2;
function fn(str,a) {
  "use strict";
  // 严格模式下eval()不会影响其声明的词法作用域，它有自己的词法作用域，因此在函数词法作用域中不会遮盖全局作用域的b = 2
  eval(str);
  console.log(a,b);
}
fn('var b = 3',1);//1,2
```



### 书中提到的与eval()相似的功能

1. setTimeout（）与setInterval（）第一个参数可以是字符串，这个字符串可以被解释为一段动态生成的函数代码，**注意书中的内容已经废除了**

   ```javascript
   // setTimeout("console.log('哈哈哈')", 1000);//Callback must be a function. Received "console.log('哈哈哈')"
   
   setInterval("console.log('哈哈哈')",1000);// Callback must be a function. Received "console.log('哈哈哈')"
   
   ```

   

2. new Function（）最后一个参数可以接受代码字符串，并将其转换为动态生成的函数

   ```
   1, Function中的参数全部是字符串。
   
   2, 构造函数的作用是将参数连接起来构成函数。
   
          * 如果参数只有一个即是表示函数体。
   
          * 如果参数多个，最后一个为函数体，前面的全是表示函数参数。
   
          * 如果没有参数，即创建空函数。
   ```

   

### with

通常被当作重复引用同一个对象中的多个属性的快捷方式，可以不需要重复引用对象本身。

```javascript
var obj = {
    a:1,
    b:2,
    c:3
};
//单调乏味的重复"obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;
//简单的快捷方式
with(obj){
    a=3;
    b=4;
    c=5;
}
```

```javascript
function foo(obj){
	with(obj){
		a=2;
	}
}
var o1 = {
	a:3
};
var o2 = {
	b:3
};
foo(o1);
console.log(o1.a);//2
foo(o2);
console.log(o2.a);//undefined
console.log(a);//2 ----a被泄露到全局作用域上了
```



with 可以将一个没有或有多个属性的对象处理为一个完全隔离的词法作用域，因此这个对象的属性也会被处理为定义在这个作用域中的词法标识符。



```javascript
function foo(obj){
	with(obj){
		var d = 4;
	}
	console.log(d);
}
var o1 = {
	a: 3
};
var o2 = {
	b: 3
};
foo(o1);//4
console.log(o1.a);//3
foo(o2);//4
console.log(o2.a);//undefined
console.log(d);//d is not defined
```



```
总结：不推荐使用 eval(..) 和 with 的原因是会被严格模式所影响（限制）。with 被完全禁止，而在保留核心功能的前提下，间接或非安全地使用eval(..) 也被禁止了。
```



### 性能

```
eval(...)和with会在运行时修改或创建新的作用域，以此来欺骗其他在书写时定义的词法作用域。
JavaScript引擎会在编译阶段进行数项的性能优化。其中有些优化依赖于能够所有变量和函数的定义位置，才能在执行过程中快速找到标识符
如果引擎在代码中发现了eval()或with，它只能简单地假设关于标识符位置的判断都是无效的，因为无法在词法分析阶段明确知道eval(...)会接收到什么代码，这些代码会如何对作用域进行修改，也无法知道传递给with用来创建新词法作用域的对象的内容是什么。
```



# 第三章 函数作用域和块作用域

1、函数作用域的含义是指，属于这个函数的全部变量都可以在整个函数的范围内使用及复用（事实上在嵌套的下一级作用域中也可以使用）
2、不能从上一级（外部）访问内部的变量和函数

```javascript
(function(){
	var a = 1;
	(function(){
		var b = 2;
		console.log(a,b);//1 2
	})();
})();
```



```javascript
(function(){
	let a =1;
	(function(){
		let b = 2;
		console.log(a,b);//1 2
	})();
	console.log(b);//ReferenceError:b is not defined
})();
```

## 隐藏内部实现

**什么是隐藏内部实现**

将一段代码中的任何声明（变量或函数）都将绑定在这个新创建的包装函数的作用域中，而不是先前所在的作用域中；换句话说，可以把变量和函数包裹在一个函数的作用域中，然后用这个作用域来“隐藏”它们。

**隐藏内部实现的作用（好处）**

遵循软件开发的最小暴露（最小授权、最小特权）原则，将一些变量和函数藏起来，成为私有的

**具体实现**

遵循一个原则在代码编写时，将要隐藏的内容放在需要调用它的最高一级别的块级作用域中。



**不恰当示范**

```javascript
function fn1(){
	console.log(fn2(2) + 2);
}
function fn2(a){
	return a * 2;
}

//虽然这样运行没有错误，但fn2()函数暴露在全局作用域中，实际上只被fn1()函数调用，不符合最小暴露原则
```



隐藏内部实现写法

```javascript
function fn1(){
	console.log(fn2(2) + 2);
	function fn2(a){
		return a * 2;
	}
}
fn1();//6
```



## 规避冲突

“隐藏”作用域中的变量和函数所带来的另一个好处，是可以避免同名标识符之间的冲突，两个标识符可能具有相同的名字但用途却不一样，无意间可能造成命名冲突。冲突会导致变量的值被意外覆盖。



```javascript
function foo(){
	function bar(a){
		i = 3; //未写var变量提升至全局变量可以被for循环LHS查询到，从而修改for循环中i的值
		console.log(a + i);
	}
	for(var i = 0; i < 10; i++){
		bar(i * 2);//无限循环
	}
}
foo();
```



**解决方法**

```javascript
function foo(){
	function bar(a){
		var i = 3;
		console.log(a + i);
	}
	for(var i = 0; i < 10; i++){
		var(i * 2);
	}
}
foo();
```



全局命名空间（外部js文件避免冲突的办法）

```
变量冲突的一个典型例子存在于全局作用域中。当程序中加载了多个第三方库时，如果它们没有妥善地将内部私有的函数或变量隐藏起来，就会很容易引发冲突。
这些库通常会在全局作用域中声明一个名字足够独特的变量，通常是一个对象。这个对象被用作库的命名空间，所有需要暴露给外界的功能都会成为这个对象（命名空间）的属性，而不是将自己的标识符暴漏在顶级的词法作用域中。
```



比如：一个MyReallyCoolLibrary第三方库



```javascript
var MyReallyCoolLibrary = {
	awesome: "stuff",
	doSomething: fuction(){
		//...
	},
	doAnotherThing:function(){}
}
```



## 模块管理

AMD,es6模块化,CMD等



## 函数作用域

在任意代码片段外部添加包装函数，可以将内部的变量和函数定义“隐藏”起来

```javascript
var a = 2;
function foo(){
	var a =3;
	console.log(a);//3
}//以及这一行
foo();//以及这一行
console.log(a);//2
```

虽然这种技术可以解决一些问题，但是它并不理想，因为会导致一些额外的问题。首先，必须声明一个具名函数 foo()，意味着 foo 这个名称本身“污染”了所在作用域。其次，必须显式地通过函数名（foo()）调用这个函数才能运行其中的代码。

```javascript
var a = 2;
```



```javascript
(function foo(){//添加这一行
	var a = 3;
	console.log(a);//3
})();//以及这一行
console.log(a);//2
```



包装函数的声明以(function.. . 而不仅是以function.. . 开始。尽管看上去这并不是一个很显眼的细节，但实际上却是非常重要的区别。函数会被当作函数表达式而不是一个标准的函数声明来处理



```javascript
function fn(){
    console.log('hhhhh');
}
console.log(fn);//fn(){}
fn = 1;
console.log(fn);//1
```



如果函数不需要函数名（或者至少函数名可以不污染所在作用域），并且能够自动运行，这将会更加理想。javascript为我们提供了函数表达式的形式来解决，用一对小括号把函数包裹起来

```javascript
(function fn() {
	console.log('hhhhh');
})
```

(function foo(){ .. }) 作为函数表达式意味着 foo 只能在 .. 所代表的位置中被访问，外部作用域则不行。foo 变量名被隐藏在自身中意味着不会非必要地污染外部作用域。



## 匿名和具名函数表达式

对于函数表达式你最熟悉的场景可能就是回调参数了,比如定时器中的匿名函数表达式



```
setInterval(function(){
	console.log('1秒过去了');
},1000)
```



```
缺点
1、匿名函数在栈追踪中不会显示出有意义的函数名，使得调试很困难。
2、如果没有函数名，当函数需要引用自身时只能使用已经过期的argument.callee 引用，比如在递归中。另一个函数需要引用自身的例子，是在事件触发后事件监听器需要解绑自身。
3、匿名函数省略了对于代码可读性/可理解性很重要的函数名。一个描述性的名称可以让代码不言自明。
```



```
然而具名函数表达式就没有这些烦恼，因此无论何时我们都应该有给匿名函数表达式赋予一个标识符的习惯
```



## 立即执行函数表达式

当作函数调用并传递参数进去。

```
两种方式
(function(){})();
(function(){}())
```

```javascript
var a = 1;
(function fn(object){
	console.log(object.a);//1
})(window)
```



## 块作用域

块作用域的作用

我们在 for 循环的头部直接定义了变量 i，通常是因为只想在 for 循环内部的上下文中使用 i，而忽略了 i 会被绑定在外部作用域（函数或全局）中的事实。这就是块作用域的用处。变量的声明应该距离使用的地方越近越好，并最大限度地本地化。

## 一个问题

```javascript
//i定义在浏览器主对象window上，也就是定义在全局作用域上
for(var i = 0; i < 2; i++){
  console.log(i);
}
console.log(window.i);//2
```

定义块级作用域的方法：
1、with
2、try/catch
3、let
4、const



## with

用with 从对象中创建出的作用域仅在with声明中而非外部作用域中有效



## try/catch

```javascript
try{
	undefined();//执行一个非法操作来强制制造一个异常
}catch(err){
	console.log(err);//能够正常执行！
}
	console.log(err);//ReferenceError:err not found
```

`err仅存在catch分句内部，当试图从别处引用它时会抛出异常`

try/catch 的 catch 分句会创建一个块级作用域，其中声明的变量仅在 catch 内部有效。

当同一个作用域中的两个或多个 catch 分句用同样的标识符名称声明错误变量时，很多静态检查工具还是会发出警告。

实际上这并不是重复定义，因为所有变量都被安全地限制在块作用域内部，但是静态检查工具还是会很烦人地发出警告。为了避免这个不必要的警告，很多开发者会将 catch 的参数命名为 err1、err2 等。也有开发者干脆关闭了静态检查工具对重复变量名的检查。

## let

let关键字可以将变量绑定到所在的任意作用域中（通常是 { .. } 内部）。换句话说，let为其声明的变量隐式地了所在的块作用域。

```javascript
{
	let a = 1;
}
console.log(a);//a is not defined
```

用 let 将变量附加在一个已经存在的块作用域上的行为是隐式的。在开发和修改代码的过程中，如果没有密切关注哪些块作用域中有绑定的变量，并且习惯性地移动这些块或者将其包含在其他的块中，就会导致代码变得混乱。为块作用域显式地创建块可以部分解决这个问题，使变量的附属关系变得更加清晰。通常来讲，显式的代码优于隐式或一些精巧但不清晰的代码。显式的块作用域风格非常容易书写,推荐使用显示的方式编写代码,只要声明是有效的，在声明中的任意位置都可以使用 { .. } 括号来为 let 创建一个用于绑定的块

```javascript
if(true){
  {
    let a = 1;
    console.log(a);//1
  }
} 

```



## let声明不会被提升

```javascript
{
	console.log(bar);//ReferenceError!
	let bar = 2;
}
```

## 垃圾收集

**let的作用，let声明的变量不使用后会被垃圾回收**

捕获阶段（Capturing phase） – 事件从Window向下走近元素

```javascript
function process(data){
	//	在这里做点有趣的事情
}
var someReallyBigData = {...};
process(someReallyBigData);
var btn = document.getElementById("mybutton");
btn.addEventListener("click",function click(evt){
	console.log("button clicked");
},/*capturingPhase*/false);
```

click 函数的点击回调并不需要 someReallyBigData 变量。

理论上这意味着当 process(..) 执行后，在内存中占用大量空间的数据结构就可以被垃圾回收了。

但是，由于 click 函数形成了一个覆盖整个作用域的闭包，JavaScript 引擎极有可能依然保存着这个结构（取决于具体实现）。

块作用域可以打消这种顾虑，可以让引擎清楚地知道没有必要继续保存 someReallyBigData了,为变量显式声明块作用域，并对变量进行本地绑定是非常有用的工具   

```javascript
function process(data){
	//	在这里做点有趣的事情
}
{
    let someReallyBigData = {...};
	process(someReallyBigData);
}
var btn = document.getElementById("mybutton");
btn.addEventListener("click",function click(evt){
	console.log("button clicked");
},/*capturingPhase*/false);
```

## let循环

```javascript
for(let i = 0; i < 2; i++){
  console.log(i);
}
//进行LHS查询，查询不到浏览器会隐式声明并赋值undefined
console.log(window.i);//undefined
```

## const

除了 let 以外，ES6 还引入了 const，同样可以用来创建块作用域变量，但其值是固定的（常量）。之后任何试图修改值的操作都会引起错误。

```javascript
function fn() {
  const a = 1;
}
const b = 2;
b = 3;//Assignment to constant variable.
console.log(a); //a is not defined
```

# 第四章 提升

作用域同其中的变量声明出现的位置有某种微妙的联系，这种联系就是提升，两种常见的特殊情况引出提升

```javascript
a = 2;
var a;
console.log(a);//2
```

很多人会认为是 undefined，因为 var a 声明在 a = 2 之后。但是却是正常的。

```javascript
console.log(a);
var a = 2;//undefined
```

因为JavaScript引擎编译的特殊机制，代码执行的过程应该是：
1、包括变量和函数在内的所有声明都会在任何代码被执行前首先被处理                                             
2、当你看到 var a = 2; 时，可能会认为这是一个声明。但 JavaScript 实际上会将其看成两个声明：var a; 和 a = 2;。第一个定义声明是在编译阶段进行的。第二个赋值声明会被留在原地等待执行阶段。因此上方两个代码的执行顺序是

```javascript
var a;//提升
a = 2;
console.log(a);//2
```



```javascript
var a;//提升
 console.log(a);//undefined
 a = 2;
```

每个作用域都会进行提升操作。尽管前面大部分的代码片段已经简化了（因为它们只包含全局作用域）

```javascript
console.log(b);//undefined
var b = 1;

function fn() {
  console.log(a);//undefined
  var a = 1;
}
fn();
```



## 函数提升

函数也可以被提升，可以先调用再声明 

```javascript
fn();

function fn() {
    console.log('函数');//函数
}
```

但是函数里面的代码是另外一个作用域，**不能在当前作用域中被提升**

函数可以被提升，但是函数表达式不能被提升

```javascript
fn();//浏览器报错TypeError：fn is not a function
var fn = (function() {
  console.log('函数');//函数
})//一般书写的时候我们会省略()
```

即使是具名函数表达式，名称标识符在赋值之前也无法在所在作用域中使用

```javascript
foo();//RHS 右查询，查询到了undefined，但是进行不正确的操作，浏览器报错TypeError: foo is not a function 
bar();//RHS 右查询，没查询到，浏览器报ReferenceError: bar is not defined
// function 不是在第一位，因此是函数表达式，函数表达式的变量名存储在函数表达式中
var foo = function bar() {
  console.log(1);
}
```

实际执行的顺序,把具名函数变量名提升了

```javascript
var foo;
foo();
bar();

foo = function() {
  var bar = ...self...
}
```



## 函数优先

函数声明和变量声明都会被提升。但是一个值得注意的细节（这个细节可以出现在有多个“重复”声明的代码中）是“函数会首先被提升，然后才是变量”。

```javascript
fn();//1
var fn;//函数声明提升在变量声明提升前面，会被浏览器认为是重复声明，忽略
function fn(){
  console.log(1);
}

fn = function(){
  console.log(2);
}
```

浏览器编译后实际代码执行过程

```javascript
// 函数声明提升
fn = function(){
	console.log(1);
}
fn();//1

fn = function(){
  console.log(2);
}
```

尽管重复的 var 声明会被忽略掉，但出现在后面的函数声明还是可以覆盖前面的。

```javascript
fn();//3
 
function fn(){
  console.log(1);
}

fn = function(){
  console.log(2);
}

fn();//2
```



浏览器编译后实际代码执行过程

```javascript
// 函数声明提升
fn = function(){
	console.log(1);
}
fn();//1

fn = function(){
  console.log(2);
}
```



尽管重复的 var 声明会被忽略掉，但出现在后面的函数声明还是可以覆盖前面的。

```javascript
fn();//3
 
function fn(){
  console.log(1);
}

fn = function(){
  console.log(2);
}

fn();//2
```



```javascript
fn();//3

function fn(){
  console.log(1);
}

fn = function(){
  console.log(2);
}

function fn(){
  console.log(3);
}
```



```javascript
fn(); //3

function fn() {
    console.log(1);
}

fn = function() {
    console.log(2);
}

function fn() {
    console.log(3);
}

fn(); //2
```

面代码实际编译执行的顺序

```javascript
// -------------
// 函数声明提升
function fn() {
    console.log(1);
}

function fn() {
    console.log(3);
}

// -------------
fn(); //3


fn = function() {
    console.log(2);
}


fn(); //2
```



## 已过时内容

一个普通块内部的函数声明通常会被提升到这个块所在作用域的顶部，这个过程不会像下面的代码暗示的那样可以被条件判断所控制,而是按照声明先后覆盖********（现在的JavaScript版本已经不会这样了，会直接报错，因为不会被提升了，可以放心在普通块内部声明函数了）,了解即可，已过时的知识。

**书中的内容**

```javascript
foo(); // b
if (true) {
    function foo() {
        console.log("a");
    }
} else {
    function foo() {
        console.log("b");
    }
}
```

**现在的情况**

```javascript
fn();//fn is not a function
if(true){
  function fn(){
    console.log(1);
  }
}else {
  function fn(){
    console.log(2);
  }
}
```

**额外补充**

```
为了我们编写的代码的可控性，我们应该尽可能不使用提升
es6新增的let和const都不具备提升
因此我们声明函数时，也尽可能的选择函数表达式的形式(var fn = function(){})
而不是(function fn(){})
```

# 第五章 作用域闭包

## 案例

```javascript
function fn1(){
  var a = 1;
  function fn2(){
    console.log(a);
  }
  return fn2;
}

var fn = fn1();
// 按理来说fn1()执行完之后引擎会垃圾回收掉fn1()函数，但是fn();也就是fn2()执行的时候依然可以访问到a的值，fn2依然有对fn1函数内部作用域的引用，这个引用就是闭包。
fn();//1
```

## 闭包的作用

```
在 foo() 执行后，通常会期待 foo() 的整个内部作用域都被销毁，因为我们知道引擎有垃圾回收器用来释放不再使用的内存空间。
由于看上去 foo() 的内容不会再被使用，所以很自然地会考虑对其进行回收。而闭包的“神奇”之处正是可以阻止这件事情的发生。事实上内部作用域依然存在，因此没有被回收（var a = 2还在）。谁在使用这个内部作用域？原来是 bar() 本身在使用。拜 bar() 所声明的位置所赐，它拥有涵盖 foo() 内部作用域的闭包，使得该作用域能够一直存活，以供 bar() 在之后任何时间进行引用。bar() 依然持有对该作用域的引用，而这个引用就叫作闭包。
总结：在其它作用域保留了对函数原有作用域的调用，阻止了垃圾回收机制，这就是闭包的作用
```



```
无论使用何种方式对函数类型的值进行传递，当函数在别处被调用时都可以观察到闭包。
三种方式：
 1、return fn(); 在外部用变量接受，调用变量调用这个函数所在的作用域（详情见闭包的案例） 
 2、在原有的作用域，将函数作为参数传递给外部的函数并调用（详情见下方代码片段1） 
 3、两种方式的结合体，传递函数当然也可以是间接的。用外部变量接收函数，再将外部变量作为参数传递给函数（详情见下方代码片段2） 
```



```javascript
// 将fn2函数作为参数传递给fn3，fn3调用传递进来的函数，所以就是在fn3作用域中调用了fn1作用域中的函数，在当前作用域中调用了其它作用域中的函数
function fn1(){
  var a = 1;
  function fn2(){
    console.log(a);
  }
  fn3(fn2);
}

function fn3(fn){
  fn();//1
}

fn1();
```



```javascript
var fn;
function fn1(){
  var a = 1;
  function fn2(){
    console.log(a);
  }
  fn = fn2;
}

function fn3(fn){
  fn();//1
}

fn1();

fn3(fn);
```

无论通过何种手段将内部函数传递到所在的词法作用域以外，它都会持有对原始定义作用域的引用，无论在何处执行这个函数都会使用闭包。

## 闭包应用场景



