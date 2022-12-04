# 数据类型

## 【1】JavaScript有哪些数据类型，它们的区别？

JavaScript共有八种数据类型，分别是Undefined、Null、Boolean、Number、String、Object、Symbol、BigInt。

其中 Symbol 和 BigInt 是ES6 中新增的数据类型：

● Symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。

● BigInt 是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 Number 能够表示的安全整数范围。

这些数据可以分为原始数据类型和引用数据类型：

● 栈：原始数据类型（Undefined、Null、Boolean、Number、String）

● 堆：引用数据类型（对象、数组和函数）

两种类型的区别在于存储位置的不同：

● 原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；

● 引用数据类型存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

堆和栈的概念存在于数据结构和操作系统内存中，在数据结构中：

● 在数据结构中，栈中数据的存取方式为先进后出。

● 堆是一个优先队列，是按优先级来进行排序的，优先级可以按照大小来规定。

在操作系统中，内存被分为栈区和堆区：

● 栈区内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

● 堆区内存一般由开发着分配释放，若开发者不释放，程序结束时可能由垃圾回收机制回收。

### symbol的应用场景

**应用场景1：使用Symbol来作为对象属性名(key)**

Symbol类型的key是不能通过Object.keys()或者for…in来枚举的，它未被包含在对象自身的属性名集合(property names)之中。所以，利用该特性，我们可以把一些不需要对外操作和访问的属性使用Symbol来定义。也正因为这样一个特性，当使用JSON.stringify()将对象转换成JSON字符串的时候，Symbol属性也会被排除在输出内容之外，我们可以利用这一特点来更好的设计我们的数据对象，让“对内操作”和“对外选择性输出”变得更加优雅。

**应用场景2：使用Symbol来替代常量**

我们经常定义一组常量来代表一种业务逻辑下的几个不同类型，我们通常希望这几个常量之间是唯一的关系，为了保证这一点，我们需要为常量赋一个唯一的值，有了Symbol，直接就保证了常量的值是唯一的了！

```js
防止对象属性名称冲突
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"

```

**应用场景3：使用Symbol定义类的私有属性/方法**

使用它来定义的类属性是没有办法被模块外访问到的，可以达到了一个私有化的效果。

```js
//消除魔术字符串 在代码中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。
const shapeType = {
  triangle: Symbol()
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });

```

**总结**

- 用于创建独一无二的值，可做唯一key用于缓存等场景
- 防止变量名起冲突
- 可实现 Symbol.iterator迭代器，symbol作为键名时，不被常规方法遍历出来，因此可以给对象定义非私有，但只用于内部使用的方法和属性

## Number和BigInt

![](img/JS22.webp)

符号位(Sign bit): 1 bit(0 表示正数， 1 表示负数)

指数位(Exponent): 11 bits

有效数字(Significand precision): 53 bits (52 explicitly stored)在二进制中，计算机内部保存有效数字时，第一个有效数字必定是1，因此这个1并不会存储。所以52位有效数字可以存储53位。

JavaScript 的数字格式也就决定了 JavaScript 能够安全表示的整数范围是 -2^53+1 ~ 2^53-1。

安全”的概念:

1. 可以准确地表示为一个 IEEE-754 双精度数字
2. 其 IEEE-754 表示不能是舍入任何其他整数以适应 IEEE-754 表示的结果
    比如说：比如，`2^53 - 1` 是一个安全整数，它能被精确表示，在任何 IEEE-754 舍入模式（rounding mode）下，没有其他整数舍入结果为该整数。作为对比，`2^53` 就不是一个安全整数，它能够使用 IEEE-754 表示，但是 `2^53 + 1` 不能使用 IEEE-754 直接表示，在就近舍入（round-to-nearest）和向零舍入中，会被舍入为 `2^53`。

 BigInt 类型为了保证精度。它是在内存中分配一个对象。我们让它足够大，以一系列块的形式容纳所有 BigInt 的位，我们称之为“数字”

BigInt 类型虽然和 Number 很像，可以做各种数学运算，但是在运算过程中要注意两点:

- BigInt 类型不能用 Math 对象中的方法。
- 不能和 Number 示例混合运算。因为 JavaScript 在处理不同类型的运算时，会把他们先转换为同一类型，而 BigInt 类型变量在被隐式转换为 Number 类型时，可能会丢失精度，或者直接报错。



## 【1】数据类型检测的方式有哪些

### （1）typeof

```javascript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object    
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
```

其中数组、对象、null都会被判断为object，其他判断都正确。

对于未声明变量，typeof 照样返回 undefined。这里因为 typeof 有一个特殊的安全防范机制。

### （2）instanceof

instanceof可以正确判断对象的类型，其内部运行机制是判断在其原型链中能否找到该类型的原型。

```javascript
onsole.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

可以看到，instanceof只能正确判断引用数据类型，而不能判断基本数据类型。instanceof 运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

### （3） constructor

```javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true

```

constructor有两个作用，一是判断数据的类型，二是对象实例通过 constrcutor 对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，constructor就不能用来判断数据类型了：

```javascript
function Fn(){};
Fn.prototype = new Array();
var f = new Fn();
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

### （4）Object.prototype.toString.call()

Object.prototype.toString.call() 使用 Object 对象的原型方法 toString 来判断数据类型：

```javascript
var a = Object.prototype.toString;
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```

同样是检测对象obj调用toString方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？

这是因为toString是Object的原型方法，而Array、function等类型作为Object的实例，都重写了toString方法。不同的对象类型调用toString方法时，根据原型链的知识，调用的是对应的重写之后的toString方法（function类型返回内容为函数体的字符串，Array类型返回元素组成的字符串…），而不会去调用Object上原型toString方法（返回对象的具体类型），所以采用obj.toString()不能得到其对象类型，只能将obj转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用Object原型上的toString方法。

![JS1](\img\JS1.png)

## 【1】判断数组的方式有哪些

通过Object.prototype.toString.call()做判断                      

```javascript
Object.prototype.toString.call(obj).slice(8,-1) === 'Array';
```

通过原型链做判断

```javascript
obj.__proto__ === Array.prototype;
```

通过ES6的Array.isArray()做判断

```javascript
Array.isArrray(obj);
```

通过instanceof做判断

```javascript
obj instanceof Array
```

通过Array.prototype.isPrototypeOf

```javascript
Array.prototype.isPrototypeOf(obj)
```

isPrototypeOf 回答的问题是：在 obj的整条 [[Prototype]] 链中是否出现过 Array.prototype

## 【1】null和undefined区别

二者都属于基本数据类型，且只有一个值

null：空对象

undefined：声明未定义

undefined 在 JavaScript 中不是一个保留字，这意味着可以使用 undefined 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 undefined 值的判断。我们可以通过一些方法获得安全的 undefined 值，比如说 void 0。

当对这两种类型使用 typeof 进行判断时，Null 类型化会返回 “object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

```javascript
console.log(null  == undefined); //true
console.log(null  === undefined); //false
```

## 【2】typeof null 的结果是什么，为什么？

typeof null 的结果是Object。

在 JavaScript 第一个版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签(1-3 bits) 以及当前要存储值的真实数据。类型标签存储在每个单元的低位中，共有五种数据类型：

```
000: object   - 当前存储的数据指向一个对象。
  1: int      - 当前存储的数据是一个 31 位的有符号整数。
010: double   - 当前存储的数据指向一个双精度的浮点数。
100: string   - 当前存储的数据指向一个字符串。
110: boolean  - 当前存储的数据是布尔值。
```

如果最低位是 1，则类型标签标志位的长度只有一位；如果最低位是 0，则类型标签标志位的长度占三位，为存储其他四种数据类型提供了额外两个 bit 的长度。

有两种特殊数据类型：

● undefined的值是 (-2)30(一个超出整数范围的数字)；

● null 的值是机器码 NULL 指针(null 指针的值全是 0)

那也就是说，null的类型标签也是000，和Object的类型标签一样，所以会被判定为Object。

## 【1】intanceof 操作符的实现原理及实现

instanceof 运算符用于判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置

```javascript
function myInstanceof(left, right) {
  // 获取对象的原型
  let proto = Object.getPrototypeOf(left)
  // 获取构造函数的 prototype 对象
  let prototype = right.prototype; 
 
  // 判断构造函数的 prototype 对象是否在对象的原型链上
  while (true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    // 如果没有找到，就继续从其原型上找，Object.getPrototypeOf方法用来获取指定对象的原型
    proto = Object.getPrototypeOf(proto);
  }
}

myInstanceof(obj, Array);

```

## 【1】为什么0.1+0.2 ! == 0.3，如何让其相等 

```javascript
let n1 = 0.1, n2 = 0.2
console.log(n1 + n2)  // 0.30000000000000004
```

### 原因

计算机是通过二进制的方式存储数据的，所以计算机计算0.1+0.2的时候，实际上是计算的两个数的二进制的和。0.1的二进制是0.0001100110011001100...（1100循环），0.2的二进制是：0.00110011001100...（1100循环），这两个数的二进制都是无限循环的数。那JavaScript是如何处理无限循环的二进制小数呢？

0.5（十进制） = 0.1（二进制），因为2^-1是0.5。

0.25（十进制） = 0.01（二进制），因为2^-2是0.25。

那0.2呢，怎么表示呢，表示不出来啊。那就只能找个最接近的近似值来代表。

所以0.2的二进制小数是0.0011001100110011.......，也就是(2^-3)+(2^-4)+(2^-7)+(2^-8)+.....

一般我们认为数字包括整数和小数，但是在 JavaScript 中只有一种数字类型：Number，它的实现遵循IEEE 754标准，使用64位固定长度来表示，也就是标准的double双精度浮点数。在二进制科学表示法中，双精度浮点数的小数部分最多只能保留52位，再加上前面的1，其实就是保留53位有效数字，剩余的需要舍去，遵从“0舍1入”的原则。

根据这个原则，0.1和0.2的二进制数相加，再转化为十进制数就是：0.30000000000000004。

下面看一下双精度数是如何保存的：

![JS2](img\JS2.png)

● 第一部分（蓝色）：用来存储符号位（sign），用来区分正负数，0表示正数，占用1位

● 第二部分（绿色）：用来存储指数（exponent），占用11位

● 第三部分（红色）：用来存储小数（fraction），占用52位



### 解决方法

法一

这里得到的不是想要的结果，要想等于0.3，就要把它进行转化：

```javascript
(n1 + n2).toFixed(2) // 注意，toFixed为四舍五入
```

法二

一个直接的解决方法就是设置一个误差范围，通常称为“机器精度”。对JavaScript来说，这个值通常为2-52，在ES6中，提供了Number.EPSILON属性，而它的值就是2-52，只要判断0.1+0.2-0.3是否小于Number.EPSILON，如果小于，就可以判断为0.1+0.2 ===0.3。

```javascript
function numberepsilon(arg1,arg2){                   
  return Math.abs(arg1 - arg2) < Number.EPSILON;        
}        
console.log(numberepsilon(0.1 + 0.2, 0.3)); // true
```

## 如何获取安全的 undefined 值？

因为 undefined 是一个标识符，所以可以被当作变量来使用和赋值，但是这样会影响 undefined 的正常判断。表达式 void ___ 没有返回值，因此返回结果是 undefined。void 并不改变表达式的结果，只是让表达式不返回值。因此可以用 void 0 来获得 undefined。

## typeof NaN 的结果是什么？

NaN 指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。

```javascript
typeof NaN; // "number"
```

NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。而 NaN !== NaN 为 true。

```javascript
var a = "str";
var b = 2;
var c = a/b;
function isNaN(n) {
    if(typeof(n) === "number" && isNaN(n)) {
   	 	return true;
    } else {
    	return false;
    }
}
console.log(Number.isNaN(a)); // false
console.log(Number.isNaN(b)); // false
console.log(Number.isNaN(c)); // true
```

## 【2】isNaN 和 Number.isNaN 函数的区别？

● 函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。

● 函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。(即上文代码的方式)

## 【1】== 操作符的强制类型转换规则？

对于 == 来说，如果对比双方的类型不一样，就会进行类型转换。假如对比 x 和 y 是否相同，就会进行如下判断流程：

- NaN 与任何值不相等，包括NaN自己
- null 除了 null 和 undefined，与任何值不相等
- undefined 除了 null 和 undefined，与任何值不相等
- 操作数两边有数字或者是布尔类型的，就都转换成 数字进行比较
- 操作数两边如果有字符串，都转成字符串进行比较
- 如果操作数两边都是引用类型，就比较地址

![JS3](img\JS3.png)

## 【2】其他值到字符串的转换规则？

● Null 和 Undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，

● Boolean 类型，true 转换为 "true"，false 转换为 "false"。

● Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。

● Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。

● 对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值

## 【2】其他值到数字值的转换规则？

● Undefined 类型的值转换为 NaN。

● Null 类型的值转换为 0。

● Boolean 类型的值，true 转换为 1，false 转换为 0。

● String 类型的值转换如同使用 Number() 函数进行转换，如果包含非数字值则转换为 NaN，空字符串为 0。

● Symbol 类型的值不能转换为数字，会报错。

● 对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf()方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。

如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

## 【2】其他值到布尔类型的值的转换规则？

以下这些是假值：

• undefined

• null

• false

• +0、-0 和 NaN

• ""

假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。

## 【1】Object.is() 与比较操作符 “===”、“==” 的区别？

● 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。

● 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。

● 使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 是相等的。

```javascript
Object.is = function(x, y) {
    if (x === y) {
      // 1/+0 = +Infinity， 1/-0 = -Infinity, +Infinity不等于-Infinity
      // Infinity 属性用于存放表示正无穷大的数值。负无穷大是表示负无穷大一个数字值。
      return x !== 0 || 1 / x === 1 / y;
    } 
      // 一个变量不等于自身变量,那么它一定是 NaN
      // 两个都是NaN的时候返回true
      return x !== x && y !== y;
  };

```

## 【2】JavaScript 中如何进行隐式类型转换？

由操作符决定目标类型

[题目][https://juejin.cn/post/6844903907584376839#heading-0]

### 转换原理

首先要介绍ToPrimitive方法，这是 JavaScript 中每个值隐含的自带的方法，用来将值 （无论是基本类型值还是对象）转换为基本类型值。如果值为基本类型，则直接返回值本身；如果值为对象，其看起来大概是这样：

```javascript
/**
* @obj 需要转换的对象
* @type 期望的结果类型
*/
ToPrimitive(obj,type)
```

type的值为number或者string。

（1）当type为number时规则如下：

● 调用obj的valueOf方法，如果为原始值，则返回，否则下一步；

● 调用obj的toString方法，后续同上；

● 抛出TypeError 异常。

（2）当type为string时规则如下：

● 调用obj的toString方法，如果为原始值，则返回，否则下一步；

● 调用obj的valueOf方法，后续同上；

● 抛出TypeError 异常。

可以看出两者的主要区别在于调用toString和valueOf的先后顺序。默认情况下：

● 如果对象为 Date 对象，则type默认为string；

● 其他情况下，type默认为number。

总结上面的规则，对于 Date 以外的对象，转换为基本类型的大概规则可以概括为一个函数：

```javascript
var objToNumber = value => Number(value.valueOf().toString())
objToNumber([]) === 0
objToNumber({}) === NaN
```

### 不同操作符的情况下隐式转换的规则

而 JavaScript 中的隐式类型转换主要发生在+、-、*、/以及==、>、<这些运算符之间。而这些运算符只能操作基本类型值，所以在进行这些运算前的第一步就是将两边的值用ToPrimitive转换成基本类型，再进行操作。

以下是基本类型的值在不同操作符的情况下隐式转换的规则 （对于对象，其会被ToPrimitive转换成基本类型，所以最终还是要应用基本类型转换规则）：

1. +操作符+操作符的两边有至少一个string类型变量时，两边的变量都会被隐式转换为字符串；其他情况下两边的变量都会被转换为数字。

   ```js
   1 + '23' // '123'
   1 + false // 1 
   1 + Symbol() // Uncaught TypeError: Cannot convert a Symbol value to a number
   '1' + false // '1false'
   false + true // 1
   
   ```

2. -、*、\操作符NaN也是一个数字

   ```js
   1 * '23' // 23
   1 * false // 0
   1 / 'aa' // NaN
   
   ```

3. 对于==操作符

   操作符两边的值都尽量转成number，具体看前面的章节：

   ```js
   3 == true // false, 3 转为number为3，true转为number为1
   '0' == false //true, '0'转为number为0，false转为number为0
   '0' == 0 // '0'转为number为0
   ```

4. 对于<和>比较符

   如果两边都是字符串，则比较字母表顺序：

   ```js
   'ca' < 'bd' // false
   'a' < 'b' // true
   ```

   其他情况下，转换为数字再比较：

   ```js
   '12' < 13 // true
   false > -1 // true
   ```

   以上说的是基本类型的隐式转换，而对象会被ToPrimitive转换为基本类型再进行转换：

   ```js
   var a = {}
   a > 2 // false
   ```

   其对比过程如下：

   ```js
   a.valueOf() // {}, 上面提到过，ToPrimitive默认type为number，所以先valueOf，结果还是个对象，下一步
   a.toString() // "[object Object]"，现在是一个字符串了
   Number(a.toString()) // NaN，根据上面 < 和 > 操作符的规则，要转换成数字
   NaN > 2 //false，得出比较结果
   ```

![](/img/JS26.png)

# ES6

symbol set map Proxy Reflect Promise 

Async

- 定义：使异步函数以同步函数的形式书写(Generator函数语法糖)
- 原理：将`Generator函数`和自动执行器`spawn`包装在一个函数里
- 形式：将`Generator函数`的`*`替换成`async`，将`yield`替换成`await`

https://juejin.cn/post/6844903959283367950#heading-4

![](img/js27.png)

## 【1】let、const、var的区别

（1）块级作用域：块作用域由 { }包括，let和const具有块级作用域，var不存在块级作用域。块级作用域解决了ES5中的两个问题：

● 内层变量可能覆盖外层变量

● 用来计数的循环变量泄露为全局变量

（2）变量提升：var存在变量提升，let和const不存在变量提升，即在变量只能在声明之后使用，否在会报错。

（3）给全局添加属性：浏览器的全局对象是window，Node的全局对象是global。var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。

（4）重复声明：var声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的遍历。const和let不允许重复声明变量。

**为什么let和const不能重复声明？**

在ES6规范有一个词叫做`Global Enviroment Records`(也就是全局环境变量记录)，它里面包含两个内容，一个是`Object Enviroment Record`(它不等同于window对象)，另一个是`Declarative Enviroment Record`。

1. 函数声明和使用`var`声明的变量会添加进入`Object Enviroment Record`中。
2. 使用`let`声明和使用`const`声明的变量会添加入`Declarative Enviroment Record`中。下面是ECMAscript规范中对`var`,`let`,`const`的一些约束。

使用`var`声明时，V8引擎只会检查`Declarative Enviroment Record`中是否有该变量，如果有，就会报错，否则将该变量添加入`Object Enviroment Record`中。

使用`let`和`const`声明时，引擎会同时检查`Object Enviroment Record`和`Declarative Enviroment Record`是否有该变量，如果有，则报错，否则将将变量添加入`Declarative Enviroment Record`中。 这就解释了为什么使用`var`声明的变量可以重复声明，而是用`let`和`const`声明的变量不可以重复声明。

（5）暂时性死区：在使用let、const命令声明变量之前，该变量都是不可用的。这在语法上，称为暂时性死区。使用var声明的变量不存在暂时性死区。

（6）初始值设置：在变量声明时，var 和 let 可以不用设置初始值。而const声明变量必须设置初始值。

（7）指针指向：let和const都是ES6新增的用于创建变量的语法。 let创建的变量是可以更改指针指向（可以重新赋值）。但const声明的变量是不允许改变指针的指向。

| **区别**           | **var** | **let** | **const** |
| ------------------ | ------- | ------- | --------- |
| 是否有块级作用域   | ×       | ✔️       | ✔️         |
| 是否存在变量提升   | ✔️       | ×       | ×         |
| 是否添加全局属性   | ✔️       | ×       | ×         |
| 能否重复声明变量   | ✔️       | ×       | ×         |
| 是否存在暂时性死区 | ×       | ✔️       | ✔️         |
| 是否必须设置初始值 | ×       | ×       | ✔️         |
| 能否改变指针指向   | ✔️       | ✔️       | ×         |

###  变量提升

函数的声明提升会和定义一起

[非常难的题目！](https://www.cnblogs.com/xxcanghai/p/5189353.html)

```js
a = 2;
var a;
console.log(a);

//实际运行会变成
var a;
a = 2;
console.log(a);

```

js会将变量的声明提升到顶部，可是赋值语句并不会提升。对于js来说，其实var a = 2是分为两步的

而js只会将第一步提升到顶部，所以下面的例子：

```js
console.log(a);
var a = 2;

//实际运行会变成
var a;
console.log(a);
a = 2;
```

js和其他语言一样，都要经历编译和执行阶段。而js在编译阶段的时候，会搜集所有的变量声明并且提前声明变量，而其他的语句都不会改变他们的顺序，因此，在编译阶段的时候，第一步就已经执行了，而第二步则是在执行阶段执行到该语句的时候才执行。

## 【2】const对象的属性可以修改吗

与C++中的const差不多

const保证的并不是变量的值不能改动，而是变量指向的那个内存地址不能改动。对于基本类型的数据（数值、字符串、布尔值），其值就保存在变量指向的那个内存地址，因此等同于常量。

但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const只能保证这个指针是固定不变的，至于它指向的数据结构是不是可变的，就完全不能控制了。

## 【2】如果new一个箭头函数的会怎么样

箭头函数是ES6中的提出来的，它没有prototype，也没有自己的this指向，它只会从自己的作用域链的上一层继承this。更**不可以使用arguments参数**，所以不能New一个箭头函数。

```js
var arr = ["wei","ze","yang"];
arr.map(item=>"Mr."+item);   // ["Mr.wei", "Mr.ze", "Mr.yang"]
//等于
var arr = ["wei","ze","yang"];
arr.map(function(item){
     return "Mr."+item;
});
```

new操作符的实现步骤如下：

1. 创建一个对象

2. 将构造函数的作用域赋给新对象（也就是将对象的__proto__属性指向构造函数的prototype属性）

3. 指向构造函数中的代码，构造函数中的this指向该对象（也就是为这个对象添加属性和方法）

4. 返回新的对象

   所以，上面的第二、三步，箭头函数都是没有办法执行的。

## 【1】箭头函数与普通函数的区别

[深入理解](https://mp.weixin.qq.com/s/I4MZ49EwVkVTHPOhYPTJJQ)

（1）箭头函数比普通函数更加简洁

● 如果没有参数，就直接写一个空括号即可

● 如果只有一个参数，可以省去参数的括号

● 如果有多个参数，用逗号分割

● 如果函数体的返回值只有一句，可以省略大括号

● 如果函数体不需要返回值，且只有一句话，可以给这个语句前面加一个void关键字。最常见的就是调用一个函数：

```js
let fn = () => void doesNotReturn();

var fn1 = (a, b) => {
    return a + b
} 

// 无参
var fn1 = function() {}
var fn1 = () => {}
 
// 单个参数
var fn2 = function(a) {}
var fn2 = a => {}
 
// 多个参数
var fn3 = function(a, b) {}
var fn3 = (a, b) => {}
 
// 可变参数
var fn4 = function(a, b, ...args) {}
var fn4 = (a, b, ...args) => {}

( ) => return 'hello'
(a, b) => a + b

(a) => {
  a = a + 1
  return a
}
// 结构体不返回
var func = () => void doesNotReturn();
```

（2）箭头函数没有自己的this

箭头函数不会创建自己的this， 所以它没有自己的this，它只会在自己作用域的上一层继承this。所以箭头函数中this的指向在它在定义时已经确定了，之后不会改变。

（3）箭头函数继承来的this指向永远不会改变

```js
var id = 'GLOBAL';
var obj = {
  id: 'OBJ',
  a: function(){
    console.log(this.id);
  },
  b: () => {
    console.log(this.id);
  }
};
obj.a();    // 'OBJ'
obj.b();    // 'GLOBAL'
new obj.a()  // undefined
new obj.b()  // Uncaught TypeError: obj.b is not a constructor
```

（4）call()、apply()、bind()等方法不能改变箭头函数中this的指向

```js
var id = 'Global';
let fun1 = () => {
    console.log(this.id)
};
fun1();                     // 'Global'
fun1.call({id: 'Obj'});     // 'Global'
fun1.apply({id: 'Obj'});    // 'Global'
fun1.bind({id: 'Obj'})();   // 'Global'
```

（5）箭头函数不能作为构造函数使用

构造函数在new的步骤在上面已经说过了，实际上第二步就是将函数中的this指向该对象。 但是由于箭头函数时没有自己的this的，且this指向外层的执行环境，且不能改变指向，所以不能当做构造函数使用。

（6）箭头函数没有自己的arguments

箭头函数没有自己的arguments对象。在箭头函数中访问arguments实际上获得的是它外层函数的arguments值。

（7）箭头函数没有prototype

（8）箭头函数不能用作Generator函数(生成器，异步编程)，不能使用yeild关键字

## 【2】箭头函数的this指向哪⾥？

答案上文有

## 【2】对 rest 参数的理解

rest参数

把逗号隔开的值合成一个数组 function( ...args )

扩展运算符

 把数组或者类数组展开成用逗号隔开的值  ...[1, 2, 3]

扩展运算符被用在函数形参上时，它还可以把一个分离的参数序列整合成一个数组：

```js
function mutiple(...args) {
  let result = 1;
  for (var val of args) {
    result *= val;
  }
  return result;
}
mutiple(1, 2, 3, 4) // 24
```

这里，传入 mutiple 的是四个分离的参数，但是如果在 mutiple 函数里尝试输出 args 的值，会发现它是一个数组：

```js
function mutiple(...args) {
  console.log(args)
}
mutiple(1, 2, 3, 4) // [1, 2, 3, 4]
```

这就是 … rest运算符的又一层威力了，它可以把函数的多个入参收敛进一个数组里。这一点经常用于获取函数的多余参数，或者像上面这样处理函数参数个数不确定的情况。

## 【2】ES6中模板语法与字符串处理

ES6 提出了“模板语法”的概念。在 ES6 以前，拼接字符串是很麻烦的事情：

```js
var name = 'css'   
var career = 'coder' 
var hobby = ['coding', 'writing']
var finalString = 'my name is ' + name + ', I work as a ' + career + ', I love ' + hobby[0] + ' and ' + hobby[1]
```

仅仅几个变量，写了这么多加号，还要时刻小心里面的空格和标点符号有没有跟错地方。但是有了模板字符串，拼接难度直线下降：

```js
var name = 'css'   
var career = 'coder' 
var hobby = ['coding', 'writing']
var finalString = `my name is ${name}, I work as a ${career} I love ${hobby[0]} and ${hobby[1]}`
```

字符串不仅更容易拼了，也更易读了，代码整体的质量都变高了。这就是模板字符串的第一个优势——允许用${}的方式嵌入变量。但这还不是问题的关键，模板字符串的关键优势有两个：

● 在模板字符串中，空格、缩进、换行都会被保留

● 模板字符串完全支持“运算”式的表达式，可以在${}里完成一些计算

基于第一点，可以在模板字符串里无障碍地直接写 html 代码：

```js
let list = `
	<ul>
		<li>列表项1</li>
		<li>列表项2</li>
	</ul>
`;
console.log(message); // 正确输出，不存在报错
```

基于第二点，可以把一些简单的计算和调用丢进 ${} 来做：

```js
function add(a, b) {
  const finalString = `${a} + ${b} = ${a+b}`
  console.log(finalString)
}
add(1, 2) // 输出 '1 + 2 = 3'
```

除了模板语法外， ES6中还新增了一系列的字符串方法用于提升开发效率：

● 存在性判定：在过去，当判断一个字符/字符串是否在某字符串中时，只能用 indexOf > -1 来做。现在 ES6 提供了三个方法：includes、startsWith、endsWith，它们都会返回一个布尔值来告诉你是否存在。

 ○ includes：判断字符串与子串的包含关系：

```js
const son = 'haha' 
const father = 'xixi haha hehe'
father.includes(son) // true
```

  ○ startsWith：判断字符串是否以某个/某串字符开头：

```js
const father = 'xixi haha hehe'
father.startsWith('haha') // false
father.startsWith('xixi') // true
```

○ endsWith：判断字符串是否以某个/某串字符结尾：

```js
const father = 'xixi haha hehe'
father.endsWith('hehe') // true
```

○自动重复：可以使用 repeat 方法来使同一个字符串输出多次（被连续复制多次）：

```js
const sourceCode = 'repeat for 3 times;'
const repeated = sourceCode.repeat(3) 
console.log(repeated) // repeat for 3 times;repeat for 3 times;repeat for 3 times;
```

#  JavaScript基础

## 【1】new操作符的实现原理

new操作符的执行过程：

（1）首先创建了一个新的空对象

（2）设置原型，将对象的原型设置为函数的 prototype 对象。

（3）让函数的 this 指向这个对象，执行构造函数的代码（为这个新对象添加属性）

（4）判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。

```js
function objectFactory() {
  let newObject = null;
  let constructor = Array.prototype.shift.call(arguments);//shift() 方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
  let result = null;
  // 判断参数是否是一个函数
  if (typeof constructor !== "function") {
    console.error("type error");
    return;
  }
  // 新建一个空对象，对象的原型为构造函数的 prototype 对象
  newObject = Object.create(constructor.prototype);
  // 将 this 指向新建对象，并执行函数，将_constructor的__proto__成员指向了newObject函数对象的prototype成员对象
  result = constructor.apply(newObject, arguments);
  // 判断返回对象， 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。
  let flag = result && (typeof result === "object" || typeof result === "function");
  // 判断返回结果
  return flag ? result : newObject;
}
// 使用方法
objectFactory(构造函数, 初始化参数);
```

在手撕代码中有详细解释

## 【2】map和Object的区别

|          |                             Map                              |                            Object                            |
| :------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 意外的键 |        Map默认情况不包含任何键，只包含显式插入的键。         | Object 有一个原型, 原型链上的键名有可能和自己在对象上的设置的键名产生冲突。 |
| 键的类型 |     Map的键可以是任意值，包括函数、对象或任意基本类型。      |            Object 的键必须是 String 或是Symbol。             |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候， Map 对象以插入的顺序返回键值。 |                     Object 的键是无序的                      |
|   Size   |         Map 的键值对个数可以轻易地通过size 属性获取          |               Object 的键值对个数只能手动计算                |
|   迭代   |           Map 是 iterable 的，所以可以直接被迭代。           |       迭代Object需要以某种方式获取它的键然后才能迭代。       |
|   性能   |              在频繁增删键值对的场景下表现更好。              |          在频繁添加和删除键值对的场景下未作出优化。          |

## 【2】JavaScript脚本延迟加载的方式有哪些？

### 延迟加载就是等页面加载完成之后再加载 JavaScript 文件。 js 延迟加载有助于提高页面加载速度。

● defer 属性：给 js 脚本添加 defer 属性，这个属性会让脚本的加载与文档的解析同步解析，然后在文档解析完成后再执行这个脚本文件，这样的话就能使页面的渲染不被阻塞。多个设置了 defer 属性的脚本按规范来说最后是顺序执行的，但是在一些浏览器中可能不是这样。

● async 属性：给 js 脚本添加 async 属性，这个属性会使脚本异步加载，不会阻塞页面的解析过程，但是当脚本加载完成后**立即执行 **js 脚本，这个时候如果文档没有解析完成的话同样会阻塞。多个 async 属性的脚本的执行顺序是不可预测的，一般不会按照代码的顺序依次执行。

● 动态创建 DOM 方式：动态创建 DOM 标签的方式，可以对文档的加载事件进行监听，当文档加载完成后再动态的创建 script 标签来引入 js 脚本。

● 使用 setTimeout 延迟方法：设置一个定时器来延迟加载js脚本文件

● 让 JS 最后加载：将 js 脚本放在文档的底部，来使 js 脚本尽可能的在最后来加载执行。![JS4](img\JS4.png)

async和defer一样，不会阻塞当前文档的解析，它会异步地下载脚本，但和defer不同的是，async会在脚本下载完成后立即执行，如果项目中脚本之间存在依赖关系，不推荐使用async。

## 【2】JavaScript 类数组对象的定义？

### 定义

一个拥有 length 属性和若干索引属性的对象就可以被称为类数组对象，类数组对象和数组类似，但是不能调用数组的方法。常见的类数组对象有 arguments 和 DOM 方法的返回结果，还有一个函数也可以被看作是类数组对象，因为它含有 length 属性值，代表可接收的参数个数。

```js
var o = {0:42, 1:52, 2:63, length:3}
console.log(0);
```

与普通对象不同的是，类数组对象拥有一个特性：可以在类数组对象上应用数组的操作方法。

不要把类型化数组与正常数组混淆，因为在类型数组上调用 Array.isArray() 会返回false。此外，并不是所有可用于正常数组的方法都能被类型化数组所支持（如 push 和 pop）。

### 类数组转数组

常见的类数组转换为数组的方法有这样几种：

（1）通过 call 调用数组的 slice 方法来实现转换

```js
Array.prototype.slice.call(arrayLike);
```

（2）通过 call 调用数组的 splice 方法来实现转换

```js
Array.prototype.splice.call(arrayLike, 0);
```

（3）通过 apply 调用数组的 concat 方法来实现转换

```js
Array.prototype.concat.apply([], arrayLike);
```

（4）通过 Array.from 方法来实现转换

```js
Array.from(arrayLike);
```

## 【1】数组有哪些原生方法？

● 数组和字符串的转换方法：toString()、toLocalString()、join() 其中 join() 方法可以指定转换为字符串时的分隔符。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var x = fruits.toString(); // Banana,Orange,Apple,Mango
```

1. toLocaleString调用每个数组的oLocaleString() 方法，然后使用地区特定的分隔符把生成的字符串连接起来，形成一个字符串。
2. 如果你开发的脚本在世界范围都有人使用，那么将对象转换成字符串时请使用toString()方法来完成
3. LocaleString()会根据你机器的本地环境来返回字符串，它和toString()返回的值在不同的本地环境下使用的符号会有微妙的变化。
4. toString()是保险的，返回唯一值的方法,它不会因为本地环境的改变而发生变化。如果是为了返回时间类型的数据，推荐使用toLocaleString()。若是在后台处理字符串，请务必使用toString()。

● 数组尾部操作的方法 pop() 和 push()，push 方法可以传入多个参数。

● 数组首部操作的方法 shift() 删除并弹出开头和 unshift()方法将新项添加到数组的开头，并返回新的长。， 重排序的方法 reverse() 和 sort()，sort() 方法可以传入一个函数来进行比较，传入前后两个值，如果返回值为正数，则交换两个参数的位置。

● 数组连接的方法 concat() ，返回的是拼接好的数组，不影响原数组。

● 数组截取办法 slice()，用于截取数组中的一部分返回，不影响原数组。

slice（参数一，参数二）

参数一：开始截取的下标位置

参数二：结束截取下标位置，但是不会截取到该位置上的值

● 数组插入方法 splice()，影响原数组查找特定项的索引的方法，indexOf() 和 lastIndexOf() 迭代方法 every()、some()、filter()、map() 和 forEach() 方法



splice(参数1，参数2，参数3，...)：splice实现插入操作

参数1：要插入的下标位置

参数2：要删除的个数

参数3，...：是要插入的值（值得类型和个数没有限制）

在位置 2，添加新项目，并删除 1 个项目：

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 1, "Lemon", "Kiwi");
```

● 数组归并方法 reduce() 和 reduceRight() 方法，这两个方法都会迭代数组的所有项。其中，reduce()方法从数组的第一项开始，逐个遍历到最后。而reduceRight()则从数组的最后一项开始，向前遍历到第一项。

### 影响原数组的方法

fill()  array.fill(value, start, end)

pop() 用于删除数组的最后一个元素并返回删除的元素。 push()

reverse()  sort() splice() unshift() shift()

## 【2】 为什么函数的 arguments 参数是类数组而不是数组？如何遍历类数组?

arguments是一个对象，它的属性是从 0 开始依次递增的数字，还有callee(arguments.callee指向当前arguments指向的函数。)和length等属性，与数组相似；但是它却没有数组常见的方法属性，如forEach, reduce等，所以叫它们类数组。

要遍历类数组，有三个方法：

（1）将数组的方法应用到类数组上，这时候就可以使用call和apply方法，如：

```js
function foo(){ 
  Array.prototype.forEach.call(arguments, a => console.log(a))
}//callee指向foo()
```

（2）使用Array.from方法将类数组转化成数组：‌

```js
function foo(){ 
  const arrArgs = Array.from(arguments) 
  arrArgs.forEach(a => console.log(a))
}
```

（3）使用展开运算符将类数组转化成数组

```js
function foo(){ 
    const arrArgs = [...arguments] 
    arrArgs.forEach(a => console.log(a)) 
}
```

## 【1】什么是 DOM 和 BOM？

● DOM 指的是文档对象模型，它指的是把文档当做一个对象，这个对象主要定义了处理网页内容的方法和接口。

● BOM (browser指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。BOM的核心是 window，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global（全局）对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。window 对象含有 location 对象、navigator 对象、screen 对象等子对象，并且 DOM 的最根本的对象 document 对象也是 BOM 的 window 对象的子对象。

## 【1】对AJAX的理解，实现一个AJAX请求

AJAX是 Asynchronous JavaScript and XML 的缩写，指的是通过 JavaScript 的 异步通信，从服务器获取 XML 文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页。

### 创建AJAX请求的步骤

● 创建一个 XMLHttpRequest 对象。

● 在这个对象上使用 open 方法创建一个 HTTP 请求，open 方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。

● 在发起请求前，可以为这个对象添加一些信息和监听函数。比如说可以通过 setRequestHeader 方法来为请求添加头信息。还可以为这个对象添加一个状态监听函数。一个 XMLHttpRequest 对象一共有 5 个状态，当它的状态变化时会触发onreadystatechange 事件，可以通过设置监听函数，来处理请求成功后的结果。当对象的 readyState 变为 4 的时候，代表服务器返回的数据接收完成，这个时候可以通过判断请求的状态，如果状态是 2xx 或者 304 的话则代表返回正常。这个时候就可以通过 response 中的数据来对页面进行更新了。

● 当对象的属性和监听函数设置完成后，最后调用 send 方法来向服务器发起请求，可以传入参数作为发送的数据体。

```js
const SERVER_URL = "/server";
let xhr = new XMLHttpRequest();
// 创建 Http 请求
xhr.open("GET", url, true);
// 设置状态监听函数
xhr.onreadystatechange = function() {
 /*   0 － （未初始化）还没有调用send()方法
1 － （载入）已调用send()方法，正在发送请求
2 － （载入完成）send()方法执行完成，已经接收到全部响应内容
3 － （交互）正在解析响应内容
4 － （完成）响应内容解析完成，可以在客户端调用了*/
  if (this.readyState !== 4) return;
  // 当请求成功时
  if (this.status === 200) {
    handle(this.response);
  } else {
    console.error(this.statusText);
  }
};
// 设置请求失败时的监听函数
xhr.onerror = function() {
  console.error(this.statusText);
};
// 设置请求头信息
xhr.responseType = "json";
xhr.setRequestHeader("Accept", "application/json");
// 发送 Http 请求
xhr.send(null);
```

### 使用Promise封装AJAX

```js
// promise 封装实现：
function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise(function(resolve, reject) {
    let xhr = new XMLHttpRequest();
    // 新建一个 http 请求
    xhr.open("GET", url, true);
    // 设置状态的监听函数
    xhr.onreadystatechange = function() {
      if (this.readyState !== 4) return;
      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };
    // 设置响应的数据类型
    xhr.responseType = "json";
    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");
    // 发送 http 请求
    xhr.send(null);
  });
  return promise;
}
```

## 【1】JavaScript为什么要进行变量提升，它导致了什么问题？

首先要知道，JS在拿到一个变量或者一个函数的时候，会有两步操作，即解析和执行。

● 在解析阶段，JS会检查语法，并对函数进行预编译。解析的时候会先创建一个全局执行上下文环境，**先把代码中即将执行的变量、函数声明都拿出来，变量先赋值为undefined，函数先声明好可使用。**在一个函数执行之前，也会创建一个函数执行上下文环境，跟全局执行上下文类似，不过函数执行上下文会多出this、arguments和函数的参数。

 ○ 全局上下文：变量定义，函数声明

 ○ 函数上下文：变量定义，函数声明，this，arguments

● 在执行阶段，就是按照代码的顺序依次执行。

### 优点

那为什么会进行变量提升呢？主要有以下两个原因：

● 提高性能

在JS代码执行之前，会进行语法检查和预编译，并且这一操作只进行一次。这么做就是为了提高性能，如果没有这一步，**那么每次执行代码前都必须重新解析一遍该变量（函数），而这是没有必要的，因为变量（函数）的代码并不会改变，解析一遍就够了。**

在解析的过程中，还会为函数生成预编译代码。在预编译时，会统计声明了哪些变量、创建了哪些函数，并对函数的代码进行压缩，去除注释、不必要的空白等。这样做的好处就是**每次执行函数时都可以直接为该函数分配栈空间**（**不需要再解析一遍去获取代码中声明了哪些变量，创建了哪些函数**），并且因为代码压缩的原因，代码执行也更快了。

● 容错性更好

### 问题

变量提升虽然有一些优点，但是他也会造成一定的问题，在ES6中提出了let、const来定义变量，它们就没有变量提升的机制。下面看一下变量提升可能会导致的问题：

```js
var tmp = new Date();
function fn(){
	console.log(tmp);
	if(false){
		var tmp = 'hello world';
	}
}
fn();  // undefined
```

在这个函数中，原本是要打印出外层的tmp变量，但是因为变量提升的问题，内层定义的tmp被提到函数内部的最顶部，相当于覆盖了外层的tmp，所以打印结果为undefined。

```js
var tmp = 'hello world';
for (var i = 0; i < tmp.length; i++) {
	console.log(tmp[i]);
}
console.log(i); // 11
```

由于遍历时定义的i会变量提升成为一个全局变量，在函数结束之后不会被销毁，所以打印出来11。

## 什么是尾调用，使用尾调用有什么好处？

尾调用一个函数的返回值是对另外一个函数的调用，这种情况就叫做尾调用。

代码执行是基于执行栈的，所以当在一个函数里调用另一个函数时，会保留当前的执行上下文，然后再新建另外一个执行上下文加入栈中。使用尾调用的话，因为已经是函数的最后一步，所以这时可以不必再保留当前的执行上下文，从而节省了内存，这就是尾调用优化。**但是 ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。**



## 【2】ES6模块与CommonJS模块有什么异同？ 

https://www.cnblogs.com/unclekeith/p/7679503.html 有例子

ES6 Module和CommonJS模块的区别： 

-  CommonJS  module.exports， require  ES6  export和import 

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用(它只是个可读引用，不过却可以改写属性)。

  common

  对于基本数据类型，属于复制。即会被模块缓存。同时，在另一个模块可以对该模块输出的变量重新赋值。

  对于复杂数据类型，属于浅拷贝。由于两个模块引用的对象指向同一个内存空间，因此对该模块的值做修改时会影响另一个模块。

  ES6

  全部都是引用

- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。

- CommonJs 是单个值导出，ES6 Module可以导出多个

- CommonJs 是动态语法可以写在判断里，ES6 Module 静态语法只能写在顶层

- CommonJs 的 this 是当前模块，ES6 Module的 this 是 undefined

CommonJS

- 当使用require命令加载某个模块时，就会运行整个模块的代码。
- 当使用require命令加载同一个模块时，不会再执行该模块，而是取到缓存之中的值。也就是说，CommonJS模块无论加载多少次，都只会在第一次加载时运行一次，以后再加载，就返回第一次运行的结果，除非手动清除系统缓存。

ES6 Module和CommonJS模块的共同点： 

● CommonJS和ES6 Module都可以对引⼊的对象进⾏赋值，即对对象内部属性的值进⾏改变。

ES6模块有两个功能export和import

node早先只支持CommonJS的模块化方案，所以ES6的模块化特性用不了

```js
// a.js
var sex="boy";
var echo=function(value){
　　console.log(value)
}
export {sex,echo}
//通过向大括号中添加sex，echo变量并且export输出，就可以将对应变量值以sex、echo变量标识符形式暴露给其他文件而被读取到
//不能写成export sex这样的方式，如果这样就相当于export "boy",外部文件就获取不到该文件的内部变量sex的值，因为没有对外输出变量接口,只是输出的字符串。
// b.js通过import获取a.js文件的内部变量，{}括号内的变量来自于a.js文件export出的变量标识符。
import {sex,echo} from "./a.js"
console.log(sex) // boy
echo(sex) // boy
```

CommonJS 中的 module.exports， require

```js
// 导出 a.js
let obj = {
    name: 'hello',
    getName: function (){
        return this.name
    }
module.exports = obj
// 引入 b.js
let obj = require('./a.js')
```

## 【2】如何判断一个对象是否属于某个类？

● 第一种方式，使用 instanceof 运算符来判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
const auto = new Car('Honda', 'Accord', 1998);
console.log(auto instanceof Car);
// expected output: true  
console.log(auto instanceof Object);
// expected output: true
```

● 第二种方式，通过对象的 constructor 属性来判断，对象的 constructor 属性指向该对象的构造函数，但是这种方式不是很安全，因为 constructor 属性可以被改写。

```js
var arr123 = [1, 2, 3];
console.log(arr123.constructor);
//expected output: [Function: Array]
//
console.log(Array.isArray(arr123));
//true
```

![JS5](img\JS5.png)

● 第三种方式，如果需要判断的是某个内置的引用类型的话，可以使用 Object.prototype.toString() 方法来打印对象的[[Class]] 属性来进行判断。

```js
var arr123 = [1, 2, 3];
console.log(Object.prototype.toString.call(arr123));
///[object Array]

console.log(Array.prototype.toString.call(arr123));
// 1,2,3
function num() {
    return 1;
}
console.log(Object.prototype.toString.call(num));
//[object Function]
```

## 强类型语言和弱类型语言的区别

● 强类型语言：强类型语言也称为强类型定义语言，是一种总是强制类型定义的语言，要求变量的使用要严格符合定义，所有变量都必须先定义后使用。Java和C++等语言都是强制类型定义的，也就是说，**一旦一个变量被指定了某个数据类型，如果不经过强制转换，那么它就永远是这个数据类型了**。例如你有一个整数，如果不显式地进行转换，你不能将其视为一个字符串。

● 弱类型语言：弱类型语言也称为弱类型定义语言，与强类型定义相反。JavaScript语言就属于弱类型语言。简单理解就是一种变量类型可以被忽略的语言。比如JavaScript是弱类型定义的，在JavaScript中就可以将字符串'12'和整数3进行连接得到字符串'123'，在相加的时候会进行强制类型转换。

两者对比：强类型语言在速度上可能略逊色于弱类型语言，但是强类型语言带来的严谨性可以有效地帮助避免许多错误。

## 解释性语言和编译型语言的区别

（1）解释型语言

使用专门的解释器对源程序逐行解释成特定平台的机器码并立即执行。是**代码在执行时才被解释器一行行动态翻译和执行，而不是在执行之前就完成翻译**。解释型语言不需要事先编译，其**直接将源代码解释成机器码并立即执行**，所以只要某一平台提供了相应的解释器即可运行该程序。其特点总结如下

● 解释型语言每次运行都需要将源代码解释称机器码并执行，效率较低；

● 只要平台提供相应的解释器，就可以运行源代码，所以可以方便源程序移植；

● JavaScript、Python等属于解释型语言。

（2）编译型语言

使用专门的编译器，针对特定的平台，将高级语言源代码一次性的编译成可被该平台硬件执行的机器码，并包装成该平台所能识别的可执行性程序的格式。在编译型语言写的程序执行之前，需要一个专门的编译过程，把源代码编译成机器语言的文件，如exe格式的文件，以后要再运行时，直接使用编译结果即可，如直接运行exe文件。因为只需编译一次，以后运行时不需要编译，所以编译型语言执行效率高。其特点总结如下：

● 一次性的编译成平台相关的机器语言文件，运行时脱离开发环境，运行效率高；

● 与特定平台相关，一般无法移植到其他平台；

● C、C++等属于编译型语言。

两者主要区别在于：前者源程序编译后即可在该平台运行，后者是在运行期间才编译。所以前者运行速度快，后者跨平台性好。

## 【2】for...in和for...of，Object.keys()的区别 

for…of 是ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构（数组、对象等）并且返回各项的值，和ES3中的for…in的区别如下

● for…of 遍历获取的是对象的值，for…in 获取的是对象的键名；

● for… in **会遍历对象的整个原型链**，性能非常差不推荐使用，而 for … of 只遍历**当前对象不会遍历原型链**；

● 对于数组的遍历，for…in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；

总结：for...in 循环主要是为了遍历对象而生，不适用于遍历数组；for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象。

for in可遍历原型链上扩展的属性，Object.keys() 只遍历自身属性

```js
var obj = {
    0:'one',
    1:'two',
    length: 2
};
obj = Array.from(obj);
for(var k of obj){
    console.log(k)
}
```

## 【2】数组的遍历方法有哪些

| **方法**                  | **是否改变原数组** | **特点**                                                     |
| ------------------------- | ------------------ | ------------------------------------------------------------ |
| forEach()                 | 否                 | 数组方法，不改变原数组，没有返回值                           |
| map()                     | 否                 | 数组方法，不改变原数组，有返回值，可链式调用                 |
| filter()                  | 否                 | 数组方法，过滤数组，返回包含符合条件的元素的数组，可链式调用 |
| for...of                  | 否                 | for...of遍历具有Iterator迭代器的对象的属性，返回的是数组的元素、对象的属性值，不能遍历普通的obj对象，将异步循环变成同步循环 |
| every() 和 some()         | 否                 | 数组方法，some()只要有一个是true，便返回true；而every()只要有一个是false，便返回false. |
| find() 和 findIndex()     | 否                 | 数组方法，find()返回的是第一个符合条件的值；findIndex()返回的是第一个返回条件的值的索引值 |
| reduce() 和 reduceRight() | 否                 | 数组方法，reduce()对数组正序操作；reduceRight()对数组逆序操作 |

```js
let arr = [1,2,3,4,5]
//第一项必选，其余可选
arr.forEach((item, index, arr) => {
  console.log(index+":"+item)
})
/*function(currentValue,index,arr)
第一个参数为一个回调函数，必传，数组中的每一项都会遍历执行该函数。
currentValue：必传，当前项的值
index：选传，当前项的索引值
arr：选传，当前项所属的数组对象
第二个参数thisValue为可选参数，回调函数中的this会指向该参数对象。
下同
*/
let arr = [1, 2, 3];
arr.map(item => {
    return item+1;
})
 // 返回值： [2, 3, 4]

let arr = [1, 2, 3, 4, 5]
arr.filter(item => item > 2) 
// 结果：[3, 4, 5]

let arr = [1, 2, 3, 4, 5]
arr.every(item => item > 0) 
// 结果： true

let arr = [1, 2, 3, 4, 5]
arr.some(item => item > 4) 
// 结果： true
```

## 了解v8引擎吗，一段js代码如何执行的（B_Cornelius）

[详细版解释](https://juejin.cn/post/6971586506011967519)

在执行一段代码时，JS 引擎会首先创建一个执行栈

然后JS引擎会创建一个全局执行上下文，并push到执行栈中, 这个过程JS引擎会为这段代码中所有变量分配内存并赋一个初始值（undefined），在创建完成后，JS引擎会进入执行阶段，这个过程JS引擎会逐行的执行代码，即为之前分配好内存的变量逐个赋值(真实值)。

如果这段代码中存在function的声明和调用，那么JS引擎会创建一个函数执行上下文，并push到执行栈中，其创建和执行过程跟全局执行上下文一样。但有特殊情况，即当函数中存在对其它函数的调用时，JS引擎会在父函数执行的过程中，将子函数的执行上下文push到执行栈，这也是为什么子函数能够访问到父函数内所声明的变量。

还有一种特殊情况是，在子函数执行的过程中，父函数已经return了，这种情况下，JS引擎会将父函数的上下文从执行栈中移除，与此同时，JS引擎会为还在执行的子函数上下文创建一个闭包，这个闭包里保存了父函数内声明的变量及其赋值，子函数仍然能够在其上下文中访问并使用这边变量/常量。当子函数执行完毕，JS引擎才会将子函数的上下文及闭包一并从执行栈中移除。

最后，JS引擎是单线程的，那么它是如何处理高并发的呢？即当代码中存在异步调用时JS是如何执行的。比如setTimeout或fetch请求都是non-blocking的，当异步调用代码触发时，JS引擎会将需要异步执行的代码移出调用栈，直到等待到返回结果，JS引擎会立即将与之对应的回调函数push进任务队列中等待被调用，当调用(执行)栈中已经没有需要被执行的代码时，JS引擎会立刻将任务队列中的回调函数逐个push进调用栈并执行。这个过程我们也称之为事件循环。

全局执行上下文

- 在执行全局代码前将window确定为全局执行上下文
- 对全局数据进行预处理
- var定义的全局变量==>undefined, 添加为window的属性
- function声明的全局函数==>赋值(fun), 添加为window的方法
- this==>赋值(window)
- 开始执行全局代码

函数执行上下文

- 在调用函数, 准备执行函数体之前, 创建对应的函数执行上下文对象(虚拟的, 存在于栈中)
- 对局部数据进行预处理
- 形参变量=>赋值(实参)=>添加为执行上下文的属性
- arguments==>赋值(实参列表), 添加为执行上下文的属性
- var定义的局部变量==>undefined, 添加为执行上下文的属性
- function声明的函数==>赋值(fun), 添加为执行上下文的方法
- this==>赋值(调用函数的对象)
- 开始执行函数体代码

#  原型

## 【1】对原型、原型链的理解

- 显式原型的作用prototype： 用来实现基于原型得继承与属性的共享。
- 隐式原型的作用\__proto__：构成原型链，同样用于实现基于原型的继承。当我们访问obj这个对象中的属性时，如果在obj中找不到，就沿着_proto_依次查找。 

### prototype原型

在JavaScript中，每个函数都有一个prototype属性，这个属性指向函数的原型对象。

原型的概念：每一个javascript对象(除null外)创建的时候，就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型中“继承”属性。

让我们用一张图表示构造函数和实例原型之间的关系：

![JS6](img\JS6.png)

原型对象就**相当于一个公共的区域**，**所有同一个类的实例都可以访问到这个原型对象**，我们可以将对象中共有的内容，统一设置到原型对象中。

Prototype与谁共享属性和方法就指向谁，同类型 ，返回的是类型的原型

### \__proto__

这是每个对象(除null外)都会有的属性，叫做\__proto__，这个属性会指向该对象的原型。

![JS7](img\JS7.png)

### constructor

每个原型都有一个constructor属性，指向该关联的构造函数。

![JS7](img\JS8.png)

constructor 返回的是对象（类型的实例）的**构造函数**，通过prototype 添加的属性和方法不会返回。

### 原型链

在JavaScript中万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条，专业术语称之为原型链。

举例说明:person → Person → Object ，普通人继承人类，人类继承对象类

当我们访问对象的一个属性或方法时，如果该对象内部不存在这个属性，那么就会去它的\__proto\__属性所指向的那个对象（父对象）里找，一直找，直\__proto\__属性的终点null，再往上找就相当于在null上取值，会报错。通过\__proto\__属性将对象连接起来的这条链路即我们所谓的原型链。

**原型链的尽头一般来说都是 Object.prototype** **所以这就是新建的对象为什么能够使用 toString()** **等方法的原因。**

特点：JavaScript 对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。**当修改原型时，与之相关的对象也会继承这一改变**。

### 区分

- \__proto__和constructor属性是对象所独有的；
-  prototype属性是函数所独有的，因为函数也是一种对象，所以函数也拥有\_\_proto\_\_和constructor属性。
- prototype属性的作用就是让该函数所实例化的对象们都可以找到公用的属性和方法，即person.\__proto\__ === Foo.prototype。
- 原型链终点是Object.prototype，再往后就是null

![JS9](img\JS9.png)

![JS9](img\JS10.png)

## 【2】原型修改、重写

```js
function Person(name) {
    this.name = name
}
// 修改原型
Person.prototype.getName = function() {}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype) // true
console.log(p.__proto__ === p.constructor.prototype) // true
// 重写原型
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // false
```

![JS9](img\JS26.webp)

可以看到修改原型的时候p的构造函数不是指向Person了，因为直接给Person的原型对象直接用对象赋值时，它的构造函数指向的了根构造函数Object，所以这时候**p.constructor === Object** ，而不是p.constructor === Person。要想成立，就要用constructor指回来： 

```js
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
p.constructor = Person
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // true
```

##  原型链的终点是什么？如何打印出原型链的终点？

由于Object是构造函数，原型链终点是Object.prototype.\__proto\__，而Object.prototype.\__proto\__=== null // true，所以，原型链的终点是null。原型链上的所有原型都是对象，所有的对象最终都是由Object构造的，而Object.prototype的下一级是Object.prototype.\__proto\__

![JS11](img\JS11.jpg)

# 执行上下文/作用域链/闭包

## 【1】对闭包的理解

闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以访问到当前函数的局部变量。

### 用途

闭包有两个常用的用途；

● 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。

● 闭包的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。

比如，函数 A 内部有一个函数 B，函数 B 可以访问到函数 A 中的变量，那么函数 B 就是闭包。

```js
function A() {
  let a = 1
  window.B = function () {
      console.log(a)
  }
}
A()
B() // 1
```

### 经典使用场景

1. `return` 回一个函数

2. 函数作为参数

3. IIFE（自执行函数）

   ```js
   var n = '林一一';
   (function p(){
       console.log(n)
   })()
   /* 输出
   *   林一一
   / 
   ```

4. 循环赋值

### 经典问题

在 JS 中，闭包存在的意义就是让我们可以间接访问函数内部的变量。经典面试题：循环中使用闭包解决 var 定义函数的问题

```js
for (var i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}
```

● 第一种是使用闭包的方式

```js
for (var i = 1; i <= 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j)
    }, j * 1000)
  })(i)
}
```

在上述代码中，首先使用了立即执行函数将 i 传入函数内部，这个时候值就被固定在了参数 j 上面不会改变，当下次执行 timer 这个闭包的时候，就可以使用外部函数的变量 j，从而达到目的。

● 第二种就是使用 setTimeout 的第三个参数，这个参数会被当成 timer 函数的参数传入。

```js
for (var i = 1; i <= 5; i++) {
  setTimeout(
    function timer(j) {
      console.log(j)
    }, i * 1000, i)
}
```

● 第三种就是使用 let 定义 i 了来解决问题了。如果函数内引用的变量是let定义的局部变量，那就会形成闭包；这个也是最为推荐的方式

```js
for (let i = 1; i <= 5; i++) {
  setTimeout(function timer() {
    console.log(i)
  }, i * 1000)
}
```

对于var来说，形成的闭包始终获取到的都是循环完成后被改变的最终的i

而对于let，每一个i都是独立在当前块级作用域的，**当前的i只在本轮循环有效**，所以每一次循环的i其实都是一个新的变量。而JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。

## 【2】对作用域、作用域链的理解

### 作用域分类

1）全局作用域和函数作用域

（1）全局作用域

● 最外层函数和最外层函数外面定义的变量拥有全局作用域

● 所有未定义直接赋值的变量自动声明为全局作用域

● 所有window对象的属性拥有全局作用域

● 全局作用域有很大的弊端，过多的全局作用域变量会污染全局命名空间，容易引起命名冲突。

（2）函数作用域

● 函数作用域声明在函数内部，一般只有固定的代码片段可以访问到

● 作用域是分层的，内层作用域可以访问外层作用域，反之不行

2）块级作用域

● 使用ES6中新增的let和const指令可以声明块级作用域，块级作用域可以在函数中创建也可以在一个代码块中的创建（**由{ }包裹的代码片段**）

● let和const声明的变量**不会有变量提升，也不可以重复声明**

● 在循环中比较适合绑定块级作用域，这样就可以把声明的计数器变量限制在循环内部。

### 作用域链

在当前作用域中查找所需变量，但是该作用域没有这个变量，那这个变量就是自由变量。如果在自己作用域找不到该变量就去父级作用域查找，依次向上级作用域查找，直到访问到window对象就被终止，这一层层的关系就是作用域链。

作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，可以访问到外层环境的变量和函数。

作用域链的本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。

当查找一个变量时，如果当前执行环境中没有找到，可以沿着作用域链向后查找。

## 【2】对执行上下文的理解

### 执行上下文类型

（1）全局执行上下文

任何不在函数内部的都是全局执行上下文，它首先会创建一个全局的window对象，并且设置this的值等于这个全局对象，一个程序中只有一个全局执行上下文。

（2）函数执行上下文

当一个函数被调用时，就会为该函数创建一个新的执行上下文，函数的上下文可以有任意多个。

（3）eval函数执行上下文

执行在eval函数中的代码会有属于他自己的执行上下文，不过eval函数不常使用，不做介绍。

### 执行上下文栈

● JavaScript引擎使用执行上下文栈来管理执行上下文

● 当JavaScript执行代码时，首先遇到全局代码，会创建一个全局执行上下文并且压入执行栈中，每当遇到一个函数调用，就会为该函数创建一个新的执行上下文并压入栈顶，引擎会执行位于执行上下文栈顶的函数，当函数执行完成之后，执行上下文从栈中弹出，继续执行下一个上下文。当所有的代码都执行完毕之后，从栈中弹出全局执行上下文。

```js
let a = 'Hello World!';
function first() {
  console.log('Inside first function');
  second();
  console.log('Again inside first function');
}
function second() {
  console.log('Inside second function');
}
first();
//执行顺序
//先执行second(),在执行first()
```



### 创建执行上下文

创建执行上下文有两个阶段：创建阶段和执行阶段

**在执行一点JS代码之前，需要先解析代码。解析的时候会先创建一个全局执行上下文环境，先把代码中即将执行的变量、函数声明都拿出来，变量先赋值为undefined，函数先声明好可使用。**这一步执行完了，才开始正式的执行程序。

在一个函数执行之前，也会创建一个函数执行上下文环境，跟全局执行上下文类似，不过函数执行上下文会多出this、arguments和函数的参数。

● 全局上下文：变量定义，函数声明

● 函数上下文：变量定义，函数声明，this，arguments

## 执行上下文与作用域的区别

作用域是基于函数的，而上下文是基于对象的。 换句话说，作用域涉及到所被调用函数中的变量访问，并且不同的调用场景是不一样的。上下文始终是this关键字的值， 它是拥有（控制）当前所执行代码的对象的引用。

作用域相当于部门，提前就分配好的。去到那个部门就能找到你（找变量

执行上下文，相当于委托人派给你的任务，委托人是任意的，有问题就去找委托人。

```c++
<!--
1. 区别1
  * 全局作用域之外，每个函数都会创建自己的作用域，作用域在函数定义时就已经确定了。而不是在函数调用时
  * 全局执行上下文环境是在全局作用域确定之后, js代码马上执行之前创建
  * 函数执行上下文是在调用函数时, 函数体代码执行之前创建
2. 区别2
  * 作用域是静态的, 只要函数定义好了就一直存在, 且不会再变化
  * 执行上下文是动态的, 调用函数时创建, 函数调用结束时就会自动释放
3. 联系
  * 执行上下文(对象)是从属于所在的作用域
  * 全局上下文环境==>全局作用域
  * 函数上下文环境==>对应的函数使用域
-->
```



# this/call/apply/bind

## 【1】对this对象的理解

this 是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，this 的指向可以通过四种调用模式来判断。

● 第一种是函数调用模式，当一个函数不是一个对象的属性时，直接作为函数来调用时，this 指向全局对象。

```js
var name = 'Jenny';
function person() {
    return this.name;
}
console.log(person());  //Jenny
```

上面这个例子在全局作用域中调用person()，此时的调用对象为window，因此this指向window，在window中定义了name变量，因此this.name相当于window.name，为Jenny。

● 第二种是方法调用模式，如果一个函数作为一个对象的方法来调用时，this 指向这个对象。

```js
var name = 'Jenny';
var obj = {
    name: 'Danny',
    person: function() {
        return this.name;
    }
};
console.log(obj.person());  //Danny
```

在这个例子中，person()函数在obj对象中定义，调用时是作为obj对象的方法进行调用，因此此时的this指向obj，obj里面定义了name属性，值为Danny，因此this.name = "Danny"。

● 第三种是构造器调用模式，如果一个函数用 new 调用时，函数执行前会新创建一个对象，this 指向这个新创建的对象。

```js
function person() {
    return new person.prototype.init();
}
person.prototype = {
    init: function() {
        return this.name;
    },
    name: 'Brain'
};
console.log(person().name); //undefined
```

首先，创建构造函数person，为函数重新定义原型，在原型上定义了两个方法init和name，其中init返回this.name。

调用person函数的name属性，发现返回的是undefined，为什么不是Brain呢？

我们看，调用person，返回person.prototype.init()的一个**实例**，假设返回的这个实例名为a，那么此时的this指向的就是a，a作为person.prototype.init()的一个实例，那么所有定义在person.prototype.init()中的方法等都可以被a调用，但是name属性定义在person的原型中，而非init函数中，因此返回undefined。

● 第四种是 apply 、 call 和 bind 调用模式，这三个方法都可以显示的指定调用函数的 this 指向。

bind返回一个方法，用于后面调用，apply和call会直接执行

apply 方法接收两个参数：一个是 this 绑定的对象，一个是参数数组。

call 方法接收的参数，第一个是 this 绑定的对象，后面的其余参数是传入函数执行的参数。也就是说，在使用 call() 方法时，传递给函数的参数必须逐个列举出来。

bind 方法通过传入一个对象，返回一个 this 绑定了传入对象的**新函数**。这个函数的 this 指向除了使用 new 时会被改变，其他情况下都不会改变。

```js
function person() {
    return this.name;
}
var obj = {
    name: 'Jenny',
    age: 18
};
console.log(person.apply(obj));  //Jenny
```

使用apply函数改变了调用person的对象，是在obj对象下面调用person，不再是在window对象下调用了，因此this指向obj，this.name = "Jenny";

这四种方式，使用构造器调用模式的优先级最高，然后是 apply、call 和 bind 调用模式，然后是方法调用模式，然后是函数调用模式。

## 【1】 call() 和 apply() 的区别？

它们的作用一模一样，区别仅在于传入参数的形式的不同。

## 【1】实现call、apply 及 bind 函数

## call 函数的实现步骤：

● 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。

● 判断传入上下文对象是否存在，如果不存在，则设置为 window 。

● 处理传入的参数，截取第一个参数后的所有参数。

● 将函数作为上下文对象的一个属性。

● 使用上下文对象来调用这个方法，并保存返回结果。

● 删除刚才新增的属性。

● 返回结果。

```js
Function.prototype.myCall = function(context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }
  // 获取参数
  let args = [...arguments].slice(1),
    result = null;
  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;
  // 将调用函数设为对象的方法
  context.fn = this;
  // 调用函数
  result = context.fn(...args);
  // 将属性删除
  delete context.fn;
  return result;
};
```

![](img/JS23.png)

## apply函数的实现步骤：

与call类似，仅在取参数的时候不一样

```js
Function.prototype.myApply = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  let result = null;
  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;
  // 将函数设为对象的方法
  context.fn = this;
  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }
  // 将属性删除
  delete context.fn;
  return result;
};
```

## bind 函数的实现步骤：

查看手撕代码文件，有解析

前面是将函数绑定给对象，这里是将对象绑定给新函数

需要返回一个新函数

● 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。

● 保存当前函数的引用，获取其余传入参数值。

● 创建一个函数返回

● 函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。

```js
Function.prototype.myBind = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }
  // 获取参数
  var args = [...arguments].slice(1),
  fn = this;
  return function Fn() {
    // 根据调用方式，传入不同绑定值,在函数调用方式下，this是window
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
```

```js
//为构造函数时的情况
var value = 2;

var foo = {
    value: 1
};

function bar(name, age) {
    this.habit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
}

bar.prototype.friend = 'kevin';

var bindFoo = bar.bind(foo, 'daisy');

var obj = new bindFoo('18');
// undefined  this指向了obj
// daisy
// 18
console.log(obj.habit);
console.log(obj.friend);
// shopping
// kevin
```



# 异步编程

promise解决地域回调，async/await解决then链式调用

## 【2】异步编程的实现方式？

![JS12](img\JS12.png)

JavaScript中的异步机制可以分为以下几种：

### 回调函数的方式

使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成**回调函数地狱**，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。

```js
    function fn1() {
      console.log('Function 1')
    }
 
    function fn2(callback) {
      setTimeout(() => {
        console.log('Function 2')
        callback()
      }, 500)
    }
 
    function fn3() {
      console.log('Function 3')
    }
    fn1();
    fn2(fn3);
 
    // 结果：
    // Function 1
    // Function 2
    // Function 3
```

### Promise 的方式

使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成**多个 then 的链式调用**，可能会造成代码的语义不够明确。

```js
 // 买包就是一个Promise,Promise的意思就是承诺
  // 这时候老公给老婆一个承诺
  // 在未来的一个月，不管挣没挣到钱，都会给老婆一个答复
    let buyBag = new Promise((resolve, reject) => {
    // Promise 接受两个参数
    // resolve: 异步事件成功时调用（挣到钱）
    // reject: 异步事件失败时调用（没挣到钱）
     // 模拟挣钱概率事件
    let result = function makeMoney() {
      return Math.random() > 0.5 ? '挣到钱' : '没挣到钱'
    }
     // 下面老公给出承诺，不管挣没挣到钱，都会给老婆一个答复
    if (result == '挣到钱')
      resolve('我买包了')
    else
      reject('不好意思，我这个月没挣到钱')
  })
   buyBag().then(res => {
    // 返回 "我买包了"
    console.log(res)
  }).catch(err => {
    // 返回 "不好意思，我这个月没挣到钱"
    console.log(err)
  })
```

### generator （生成器）的方式

它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。当遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕时再将执行权给转移回来。因此在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。

```js
function* showWords() {
      yield 'one';
      yield 'two';
      return 'three';
}

let show = showWords();
console.log(show.next()) // {done: false, value: "one"}
console.log(show.next()) // {done: false, value: "two"}
console.log(show.next()) // {done: true, value: "three"}
console.log(show.next()) // {done: true, value: undefined}
```

### async 函数 的方式

async 函数是 generator 和 promise 实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个 await 语句的时候，如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。

```js
 async function test() {
      return 'Hello World';
 }
test().then(res=>{
      console.log(res) // Hello World
})
console.log('我在后面，但是我先执行')

```

![JS13](img\JS13.jpg)

```js
 function testAwait() {
      return new Promise((resolve) => {
        setTimeout(function () {
          console.log("Test Await");
          resolve();
        }, 1000);
      });
}
 
async function test() {
      await testAwait();
      console.log("Hello World");
}

test();
// Test Await
// Hello World

```

![JS14](img\JS14.jpg)

## 【2】setTimeout、Promise、Async/Await 的区别

[代码输出题目](https://juejin.cn/post/6844903796754104334)

[阮一峰对事件循环的解释](https://www.ruanyifeng.com/blog/2014/10/event-loop.html)

### 了解任务执行顺序

执行顺序：同步任务——>微观任务——>宏观任务

宏观任务的方法有：script(整体代码)、setTimeout、setInterval、I/O、UI交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)

微观任务的方法有：Promise.then、MutaionObserver、process.nextTick(Node.js 环境)，async/await实际上是promise+generator的语法糖，也就是promise，也就是微观任务

setTimeout属性宏任务，Promise里面的then方法属于微任务，Async/Await中await语法后面紧跟的表达式是同步的。异步的属于微任务。

微任务会在本次宏任务末尾去执行

### 顺序

-  settimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行；
-  promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；
- async函数表示函数里面可能会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。

```js
console.log(1);

let p = new Promise(resolve => {
  console.log(2);
  resolve();
  console.log(3);
});

console.log(4);

p.then(() => {
  console.log(5);
});

console.log(6);

//123465
```



## 【1】对Promise的理解

Promise是异步编程的一种解决方案，它是一个对象，可以获取异步操作的消息，他的出现大大改善了异步编程的困境，避免了地狱回调，它比传统的解决方案回调函数和事件更合理和更强大。

所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

灶蒸饭，不会自己停，需要自己盯着，同步

电饭煲煮饭，煮好了会通知，不需要盯着，能去干别的，异步

### 三个状态

● Pending（进行中）

● Resolved（已完成）

● Rejected（已拒绝）

当把一件事情交给promise时，它的状态就是Pending，任务完成了状态就变成了Resolved、没有完成失败了就变成了Rejected。

### 特点

● 对象的状态不受外界影响。promise对象代表一个异步操作，有三种状态，pending（进行中）、fulfilled（已成功）、rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是promise这个名字的由来——“承诺”；

● **一旦状态改变就不会再变**，任何时候都可以得到这个结果。promise对象的状态改变，只有两种可能：从pending变为fulfilled，从pending变为rejected。这时就称为resolved（已定型）。如果改变已经发生了，你再对promise对象添加回调函数，也会立即得到这个结果。这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

### 缺点

● 无法取消Promise，一旦新建它就会立即执行，**无法中途取消**。

● 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。

● 当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

### 总结

- Promise 对象是异步编程的一种解决方案，最早由社区提出。**Promise 是一个构造函数**，接收一个函数作为参数，返回一个 Promise 实例。一个 Promise 实例有三种状态，分别是pending、resolved 和 rejected，分别代表了进行中、已成功和已失败。实例的状态只能由 pending 转变 resolved 或者rejected 状态，并且状态一经改变，就凝固了，无法再被改变了。
- 状态的改变是通过 resolve() 和 reject() 函数来实现的，可以在异步操作结束后调用这两个函数改变 Promise 实例的状态，它的原型上定义了一个 then 方法，使用这个 then 方法可以为两个状态的改变注册回调函数。**这个回调函数属于微任务，会在本轮事件循环的末尾执行**。
- 注意：在构造 Promise 的时候，构造函数内部的代码是立即执行的

## 【1】Promise的基本用法

### 创建Promise对象

Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。

```js
const promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

一般情况下都会使用new Promise()来创建promise对象，但是也可以使用promise.resolve和 promise.reject这两个方法，他们的返回值也是promise：

```js
Promise.resolve(11).then(function(value){
  console.log(value); // 打印出11
});

Promise.reject(new Error(“我错了，请原谅俺！！”));
```

### Promise方法

Promise有五个常用的方法：then()、catch()、all()、race()、finally。下面就来看一下这些方法。

#### then()

then方法返回的是一个**新的Promise实例（不是原来那个Promise实例）**。因此可以采用链式写法，即then方法后面再调用另一个then方法。

```js
let promise = new Promise((resolve,reject)=>{
    ajax('first').success(function(res){
        resolve(res);
    })
})
promise.then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    
})
```

#### catch()

Promise对象除了有then方法，还有一个catch方法，该方法相当于then方法的第二个参数，指向reject的回调函数。不过catch方法还有一个作用，就是在执行resolve回调函数时，如果出现错误，抛出异常，不会停止运行，而是进入catch方法中。

```js
p.then((data) => {
     console.log('resolved',data);
},(err) => {
     console.log('rejected',err);
     }
); 
p.then((data) => {
    console.log('resolved',data);
}).catch((err) => {
    console.log('rejected',err);
});
```

#### all()

all方法可以完成并行任务， 它接收一个数组，数组的每一项都是一个promise对象。当数组中所有的promise的状态都达到resolved的时候，all方法的状态就会变成resolved，如果有一个状态变成了rejected，那么all方法的状态就会变成rejected。

```js
javascript
let promise1 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(1);
	},2000)
});
let promise2 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(2);
	},1000)
});
let promise3 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(3);
	},3000)
});
Promise.all([promise1,promise2,promise3]).then(res=>{
    console.log(res);
    //结果为：[1,2,3] 
})
```

#### race()

race方法和all一样，接受的参数是一个每项都是promise的数组，但是与all不同的是，当最先执行完的事件执行完之后，就直接返回该promise对象的值。自身状态又第一个promise对象决定。

如果第一个promise对象状态变成resolved，那自身的状态变成了resolved；反之第一个promise变成rejected，那自身状态就会变成rejected。

那么race方法有什么实际作用呢？当要做一件事，超过多长时间就不做了，可以用这个方法来解决

```js
Promise.race([promise1,timeOutPromise(5000)]).then(res=>{})
```

#### finally()

finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

## 【2】Promise解决了什么问题

在工作中经常会碰到这样一个需求，比如我使用ajax发一个A请求后，成功后拿到数据，需要把数据传给B请求；那么需要如下编写代码：

```js
let fs = require('fs')
fs.readFile('./a.txt','utf8',function(err,data){
  fs.readFile(data,'utf8',function(err,data){
    fs.readFile(data,'utf8',function(err,data){
      console.log(data)
    })
  })
})
```

上面的代码有如下缺点：

● 后一个请求需要依赖于前一个请求成功后，将数据往下传递，会导致多个ajax请求嵌套的情况，代码不够直观。

● 如果前后两个请求不需要传递参数的情况下，那么后一个请求也需要前一个请求成功后再执行下一步操作，这种情况下，那么也需要如上编写代码，导致代码不够直观。

使用promise

```js
let fs = require('fs')
function read(url){
  return new Promise((resolve,reject)=>{
    fs.readFile(url,'utf8',function(error,data){
      error && reject(error)
      resolve(data)
    })
  })
}
read('./a.txt').then(data=>{
  return read(data) 
}).then(data=>{
  return read(data)  
}).then(data=>{
  console.log(data)
})
```

这样代码看起了就简洁了很多，解决了地狱回调的问题。

## 【2】Promise.all和Promise.race的区别的使用场景

**（1）Promise.all**

Promise.all可以将多个Promise实例包装成一个新的Promise实例。同时，成功和失败的返回值是不同的，成功的时候返回的是一个结果数组，而失败的时候则返回最先被reject失败状态的值。

Promise.all中传入的是数组，返回的也是是数组，并且会将进行映射，传入的promise对象返回的值是按照顺序在数组中排列的，但是注意的是他们执行的顺序并不是按照顺序的，除非可迭代对象为空。

需要注意，Promise.all获得的成功结果的数组里面的数据顺序和Promise.all接收到的数组顺序是一致的，这样当遇到**发送多个请求并根据请求顺序获取和使用数据的场景**，就可以使用Promise.all来解决。

**（2）Promise.race**

顾名思义，Promse.race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面**哪个结果获得的快，就返回那个结果**，不管结果本身是成功状态还是失败状态。

## 【1】对async/await 的理解

async/await其实是Generator 的语法糖，它能实现的效果都能用then链来实现，它是为优化then链而开发出来的。从字面上来看，async是“异步”的简写，await则为等待，所以很好理解async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成。当然语法上强制规定await只能出现在asnyc函数中，先来看看async函数返回了什么：

```js
async function testAsy(){
   return 'hello world';
}
let result = testAsy(); 
console.log(result)
```

![JS15](img\JS15.png)

所以，**async 函数返回的是一个 Promise 对象**。async 函数（包含函数语句、函数表达式、Lambda表达式）会返回一个 Promise 对象，如果在函数中 return 一个直接量，async 会把这个直接量通过 Promise.resolve() 封装成 Promise 对象。

所以在最外层不能用 await 获取其返回值的情况下，当然应该用原来的方式：then() 链来处理这个 Promise 对象，就像这样：

```js
async function testAsy(){
   return 'hello world'
}
let result = testAsy() 
console.log(result)
result.then(v=>{
    console.log(v)   // hello world
})
```

那如果 async 函数没有返回值，又该如何？很容易想到，它会返回 Promise.resolve(undefined)。

联想一下 Promise 的特点——无等待，所以**在没有 await 的情况下执行 async 函数**，它会立即执行，返回一个 Promise 对象，并且，绝不会阻塞后面的语句。这**和普通返回 Promise 对象的函数并无二致**。

## 易错点

[题目][https://juejin.cn/post/7108751200262029319]

1. promise本身是一个同步的代码(只是容器)，只有它后面调用的then()方法里面的回调才是微任务

   ```js
   new Promise( resolve => {
   	console.log(7777);
       resolve()
   }).then(function () {
       console.log(8888);
   })
   console.log(9999);
   // 7777 9999 8888
   ```

2. await右边的表达式还是会立即执行,表达式之后的代码才是微任务, await微任务可以转换成等价的promise微任务分析

   ```js
   console.log(1);
   async function async1 () {
   	await async();
   	console.log(2);
   }
   async function async2 () {
   	console.log(3);
   }
   async1();
   // 1 3 2
   ```

3. script标签本身是一个`宏任务`， 当页面出现多个script标签的时候，浏览器会把script标签作为宏任务来解析

   ```html
   <script>
       console.log(1);
       setTimeout( () => {
           console.log(2);
       })
   </script>
   <script>
       console.log(3);
   </script>
   1  3  2
   ```

   

## await 到底在等啥？

await 不仅仅用于等 Promise 对象，它可以等任意表达式的结果，所以，await 后面实际是可以接普通函数调用或者直接量的。

```js
function getSomething() {
    return "something";
}
async function testAsync() {
    return Promise.resolve("hello async");
}
async function test() {
    const v1 = await getSomething();
    const v2 = await testAsync();
    console.log(v1, v2);
}
test();
```

await 表达式的运算结果取决于它等的是什么。

● 如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的东西。

● 如果它等到的是一个 Promise 对象，await 就忙起来了，它**会阻塞后面的代码**，等着 Promise 对象 resolve，然后得到 resolve 的值，作为 await 表达式的运算结果。

```js
function testAsy(x){
   return new Promise(resolve=>{setTimeout(() => {
       resolve(x);
     }, 3000)
    }
   )
}
async function testAwt(){    
  let result =  await testAsy('hello world');
  console.log(result);    // 3秒钟之后出现hello world
  console.log('cuger')   // 3秒钟之后出现cug
}
testAwt();
console.log('cug')  //立即输出cug
```

这就是 await 必须用在 async 函数中的原因。async 函数调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。await暂停当前async的执行，所以'cug''最先输出，hello world'和‘cuger’是3秒钟后同时出现的。

根据 TC39 最近决议，await将直接使用Promise.resolve()相同语义。

```js
async function f() {
  await p
  console.log('ok')
}

//等于
function f() {
  return RESOLVE(p).then(() => {
    console.log('ok')
  })
}
```



## async/await的优势

单一的Promise链并不能发现async/await的优势，但是，如果需要处理由多个 Promise 组成的 then 链的时候，优势就能体现出来了（很有意思，**Promise通过then链来解决多层回调的问题，现在又用 async/await** **来进一步优化它**）。

假设一个业务，分多个步骤完成，每个步骤都是异步的，而且**依赖于上一个步骤**的结果。仍然用 setTimeout 来模拟异步操作：

```js
/**
 * 传入参数 n，表示这个函数执行的时间（毫秒）
 * 执行的结果是 n + 200，这个值将用于下一步骤
 */
function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}
function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}
function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}
function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
```

 Promise 方式来实现这三个步骤的处理：

```js
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}
doIt();
// c:\var\test>node --harmony_async_await .
// step1 with 300
// step2 with 500
// step3 with 700
// result is 900
// doIt: 1507.251ms
```

输出结果 result 是 step3() 的参数 700 + 200 = 900。doIt() 顺序执行了三个步骤，一共用了 300 + 500 + 700 = 1500 毫秒，和 console.time()/console.timeEnd() 计算的结果一致。

如果用 async/await 来实现呢，会是这样

```js
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}
doIt();
```

结果和之前的 Promise 实现是一样的，但是这个代码看起来是不是清晰得多，几乎跟同步代码一样

## async/await对比Promise的优势

● 代码读起来更加同步，Promise虽然摆脱了回调地狱，但是then的链式调⽤也会带来额外的阅读负担 

● Promise传递中间值⾮常麻烦，⽽async/await⼏乎是同步的写法，⾮常优雅 

● 错误处理友好，async/await可以⽤成熟的try/catch，Promise的错误捕获⾮常冗余（异步不能用try catch） 

● 调试友好，Promise的调试很差，由于没有代码块，你不能在⼀个返回表达式的箭头函数中设置断点，如果你在⼀个.then代码块中使⽤调试器的步进(step-over)功能，调试器并不会进⼊后续的.then代码块，因为调试器只能跟踪同步代码的每⼀步。

## 模拟async/await机制实现

```js
const getData = () => new Promise(res => setTimeout(() => res(1), 1000))
//首先先写一个简单的generate函数
function * demo() {
	const data1 = yield getData()
	console.log(data1);
	const data2 = yield getData()
	console.log(data2);
	return 'success'
}

//创建一个foo的async函数，由asyncDemo代替*，接受demo函数作为参数
const foo = asyncDemo(demo)
//需要实现,foo是执行完后return一个promise对象，可执行then方法
foo().then(res => {
	console.log(res)
})

//实现asyncDemo
const asyncDemo = (demo) => {
	//上面代码执行asyncDemo(demo)需要返回一个函数
	return function(){
		const demoFoo = demo.apply(this, arguments)
		//既然要调用then方法，即return promise
		return new Promise((res, rej) => {
			//声明进一步的函数
			function step(behavior, arg){
				let result
				try{
					result = demoFoo[behavior](arg)
				}
				catch(error){
					return rej(error)
				}
				const {value, done} = result;
				if(done){
					return res(value);
				}
				else{
					return Promise.resolve(value).then(
						function onResolve(val){
							step('next', val)
						},
						function onReject(err){
							step('throw', err)
						}
					)
				}
			}
			step('next')
		})
	}
}

```

# 面向对象

## 【1】对象创建的方式有哪些？

一般使用字面量的形式直接创建对象，但是这种创建方式对于创建大量相似对象的时候，会产生大量的重复代码。但 js和一般的面向对象的语言不同，在 ES6 之前它没有类的概念。但是可以使用函数来进行模拟，从而产生出可复用的对象创建方式，常见的有以下几种：

### 工厂模式

工厂模式的主要工作原理是用函数来封装创建对象的细节，从而通过调用函数来达到复用的目的。但是它有一个很大的问题就是创建出来的对象无法和某个类型联系起来，它只是简单的封装了复用代码，而**没有建立起对象和类型间的关系**。

```js
function createPerson(name){
   //1、原料
    var obj=new Object();
   //2、加工
    obj.name=name;
    obj.showName=function(){
       alert(this.name);
    }     
    //3、出场
     return obj; 
} 
var p1=createPerson('小米');
p1.showName();
```

### 构造函数模式

js 中每一个函数都可以作为构造函数，**只要一个函数是通过** **new** **来调用的，那么就可以把它称为构造函数**。执行构造函数首先会创建一个对象，然后将对象的**原型指向构造函数的** **prototype** **属性**，然后将执行上下文中的 this 指向这个对象，最后再执行整个函数，如果返回值不是对象，则返回新建的对象。因为 this 的值指向了新建的对象，因此可以使用 this 给对象赋值。

构造函数模式相对于工厂模式的优点是，**所创建的对象和构造函数建立起了联系，因此可以通过原型来识别对象的类型**。但是构造函数存在一个**缺点就是，造成了不必要的函数对象的创建，因为在** **js** **中函数也是一个对象，因此如果对象属性中如果包含函数的话，那么每次都会新建一个函数对象，浪费了不必要的内存空间**，因为函数是所有的实例都可以通用的。

```js
function CreatePerson(name){
  this.name=name;
  this.showName=function(){
    alert(this.name);
  }
}
 var p1=new CreatePerson('小米');
```

### 原型模式

因为每一个函数都有一个 prototype 属性，这个属性是一个对象，它包含了通过构造函数创建的所有实例都能共享的属性和方法。因此可以使用原型对象来添加公用属性和方法，从而实现代码的复用。这种方式相对于构造函数模式来说，解决了函数对象的复用问题。但是这种模式也存在一些问题，一个是**没有办法通过传入参数来初始化值**，另一个是**如果存在一个引用类型如** **Array 这样的值，那么所有的实例将共享一个对象**，一个实例对引用类型值的改变会影响所有的实例。所以一般很少单独使用原型模式。

```js
function Person(){}
Person.prototype.name="小米";
Person.prototype.showName=function(){
alert(this.name);
}
var p1=new Person();
p1.showName();
```

### 组合使用构造函数模式和原型模式

这是创建自定义类型的最**常见方式**。因为构造函数模式和原型模式分开使用都存在一些问题，因此可以组合使用这两种模式，通过构造函数来初始化对象的属性，通过原型对象来实现函数方法的复用。这种方法很好的解决了两种模式单独使用时的缺点，但是有一点不足的就是，因为使用了两种不同的模式，所以对于代码的封装性不够好。

```jav
　function 构造函数(){
　　　　this.属性;
　　}
　　构造函数.原型.方法=function(){};
　　var 对象1=new 构造函数();
　对象1.方法();
```

没有共享name的内存空间，但是函数内存共享

```js
function CreatePerson(name){
        this.name=name;
    }
    CreatePerson.prototype.showName=function(){
        console.log(this.name)
    }
var p1=new CreatePerson('小米');
// p1.showName();
var p2=new CreatePerson('小米2');
p2.showName();
p1.showName();
console.log(p1.showName==p2.showName);
```

![JS16](img/JS16.png)

### 动态原型模式

这一种模式将原型方法赋值的创建过程移动到了构造函数的内部，通过对属性是否存在的判断，可以实现仅在第一次调用函数时对原型对象赋值一次的效果。这一种方式很好地对上面的混合模式进行了封装。

```js
function Person(name, age, job) {
    // 属性
    this.name = name;
    this.age = age;
    this.job = job;
    // 方法
    if (typeof this.sayName != "function") {
        Person.prototype.sayName = function() {
            console.log(this.name);
        }
    }   
}

var friend = new Person("Mary", 18, "Software Engineer");
friend.sayName();
var friend2 = new Person("Mxxxx", 18, "Software Engineer");
friend2.sayName();
console.log(friend.sayName === friend2.sayName)
```

![JS17](img/JS17.png)

### 寄生构造函数模式

这一种模式和工厂模式的实现基本相同。

寄生构造函数模式与工厂模式极为相似，区别在于：

- 寄生构造函数模式将工厂模式中的那个通用函数`createPerson()`改名为`Person()`，并将其看作为对象的构造函数。
- 创建对象实例时，寄生构造函数模式采用`new`操作符

那么两者有什么功能上的差别呢？事实上，两者本质上的差别仅在于`new`操作符（因为函数取什么名字无关紧要），工厂模式创建对象时将`createPerson`看作是普通的函数，而寄生构造函数模式创建对象时将`Person`看作是构造函数，**不过这对于创建出的对象来说，没有任何差别**。

```js
function Person(name, age, job) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    }
    return o;
}

var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");
```

## 【1】对象继承的方式有哪些？

### 原型链

这种实现方式存在的缺点是，在包含有**引用类型的数据时，会被所有的实例对象所共享**，容易造成修改的混乱。还有就是在创建子类型的时候不能向超类型传递参数。

```js
function Parent(name) {
    this.name = name;
    this.showName = function () {
        console.log(this.name);
    }
}

function Children(name) {
    this.name = name;
}

Children.prototype = new Parent();

var c = new Children("dd");
c.showName()
```

![JS18](img\JS18.png)

### 构造函数

重点：使用.call()和.apply()将父类构造函数引入子类函数，使用父类的构造函数来增强子类实例，等同于复制父类的实例给子类。

```js
function SuperType(name){
    this.name = name;
    this.colors = ["red", "blue", "green"];
    this.sayName = function(){
    console.log(this.name);}
};

function SubType(name, age){
    //继承属性
    SuperType.call(this, name);
    this.age = age;
}

var example1 = new SubType("Mike", 23);
example1.colors.push("black");
console.log(example1.colors);//"red,blue,green,black"

var example2 = new SubType();
console.log(example2.colors);//"red,blue,green"

console.log(example1.name); // "Mike"
console.log(example1.age); // 23
console.log(example2.sayName === example1.sayName)

```

借用构造函数继承的重点就在于SuperType.call(this, name)，调用了SuperType构造函数，这样，SubType的每个实例都会将SuperType中的属性复制一份。

缺点：

- 只能继承父类的实例属性和方法，不能继承原型属性/方法；
- 无法实现构造函数的复用，**每个子类都有父类实例函数的副本**，影响性能，代码会臃肿。

### 组合继承

组合继承是将原型链和借用构造函数组合起来使用的一种方式。其背后的思路是使用原型链实现对原型方法的继承，而通过借用构造函数来实现对实例属性的继承，这样，既通过在原型上定义方法实现了函数复用，又能保证每个实例都有它自己的属性。

```js
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};

function SubType(name, age){
  //继承属性
  SuperType.call(this, name);
  this.age = age;
}

// 继承方法
SubType.prototype = new SuperType();//此时 SubType prototype 中的 constructor 被重写了，会导致 example1.constructor === SuperType
SubType.prototype.constructor = SubType; //将SubType 原型对象的 constructor 指针重新指向 SubType 本身
SubType.prototype.sayAge = function(){
    alert(this.age);
};

var example1 = new SubType("Mike", 23);
example1.colors.push("black");
alert(example1.colors); //"red,blue,green,black"
example1.sayName(); //"Mike";
example1.sayAge(); //23

var example2 = new SubType("Jack", 22);
alert(example2.colors); //"red,blue,green"
example2.sayName(); //"Jack";
example2.sayAge(); //22
```

SayName共用

### 原型式继承

原型式继承的主要思路就是基于已有的对象来创建新的对象，实现的原理是，向函数中传入一个对象，然后返回一个以这个对象为原型的对象。这种继承的思路主要不是为了实现创造一种新的类型，只是对某个对象实现一种简单继承，ES5 中定义的 Object.create() 方法就是原型式继承的实现。缺点与原型链方式相同。

```js
const cat = {
  heart: '❤️',
  colors: ['white', 'black']
}

const guaiguai = Object.create(cat)
const huaihuai = Object.create(cat)
guaiguai.colors.push('test')
console.log(guaiguai)
console.log(huaihuai)

console.log(guaiguai.colors)
console.log(huaihuai.colors)
```

### 寄生式继承

寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后对象进行扩展，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种继承的优点就是对一个简单对象实现继承，如果这个对象不是自定义类型时。缺点是没有办法实现函数的复用。

```js
function createAnother(original){
var clone = Object.create(original); //通过调用函数创建一个新对象
 clone.sayHi = function(){    //以某种方式来增强这个对象
  alert("Hi");
 };
 
 return clone;      //返回这个对象
}

var person = {
 name: "Bob",
 friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi();
```

新对象不仅具有person的所有属性和方法，还有自己的sayHi()方法。

### 寄生式组合继承-最常用

组合继承最大的问题就是无论在什么情况下，都会调用两次构造函数：一次是在创建子类型原型时，另一次是在子类型构造函数内部。

```js
function SuperType(name){
 this.name = name;
 this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
 alert(this.name);
}

function SubType(name, age){
 SuperType.call(this, name);　　//第二次调用SuperType()
 
 this.age = age;
}
SubType.prototype = new SuperType();　　//第一次调用SuperType()
SubType.prototype.sayAge = function(){
 alert(this.age);}
let sub = new SubType();
```

![JS20](img\JS20.webp)

在第一次调用SuperType构造函数时，SubType.prototype会得到两个属性： name和colors； 他们都是SuperType的实例属性，只不过现在位于SubType的原型中。

当调用SubType构造函数时，又会调用一次SuperType构造函数，这一次又在新对象上创建了实例属性name和colors。

于是这两个属性就屏蔽了原型中的两个同名属性。

寄生组合式继承就是为了解决这一问题。

- 通过借用构造函数来继承属性；
- 通过原型链来继承方法。

```js
   // 实现继承的核心函数
   function inheritPrototype(subType,superType) {
      function F() {};//没有属性
      //F()的原型指向的是superType
      F.prototype = superType.prototype; 
      //subType的原型指向的是F()
      subType.prototype = new F(); 
      // 重新将构造函数指向自己，修正构造函数
      subType.prototype.constructor = subType; 
   }
   // 设置父类
   function SuperType(name) {
       this.name = name;
       this.colors = ["red", "blue", "green"];
       SuperType.prototype.sayName = function () {
         console.log(this.name)
       }
   }
   // 设置子类
   function SubType(name, age) {
       //构造函数式继承--子类构造函数中执行父类构造函数
       SuperType.call(this, name);
       this.age = age;
   }
   // 核心：因为是对父类原型的复制，所以不包含父类的构造函数，也就不会调用两次父类的构造函数造成浪费
   inheritPrototype(SubType, SuperType)
   // 添加子类私有方法
   SubType.prototype.sayAge = function () {
      console.log(this.age);
   }
   var instance = new SubType("Taec",18)
   console.dir(instance)
```

![JS21](img\JS21.webp)

这个例子的高效率体现在它只调用了一次SuperType构造函数,并且因此避免了在SubType.prototype上创建不必要的 多余的属性.与此同时,原型链还能保持不变.因此,还能够正常使用instanceof 和isPrototypeOf确定继承关系.

### ES6中 Class ...  extends 关键字实现继承

```js
class Colorpoint extends Point {
    constructor(x,y,color){
        super(x,y); //调用父类的constructor(x,y)
        this.color = color
    }
    toString(){
        //调用父类的方法
        return this.color + ' ' + super.toString(); 
    }
}
```

　子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工，如果不调用super方法，子类就得不到this对象。因此，只有调用super之后，才可以使用this关键字。

prototype 和__proto__：

一个继承语句同时存在两条继承链：一条实现属性继承，一条实现方法的继承

（1）`子类的__proto__属性，表示构造函数的继承，总是指向父类。`

（2）`子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。`

```js
class A extends B{}
A.__proto__ === B;  //继承属性
A.prototype.__proto__ == B.prototype;//继承方法
```

源码

```js
function _inherits(subClass, superClass) { 
    if (typeof superClass !== "function" && superClass !== null) { 
        throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); 
    } 
    subClass.prototype = Object.create(superClass && superClass.prototype, { 
        constructor: { 
            value: subClass, 
            enumerable: false, 
            writable: true, 
            configurable: true 
        } 
    }); 
    if (superClass) 
        Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass; 
}
// setPrototypeOf _proto_
```



## class

```
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```

# DOM API

## oninput & onchange

**oninput和onchange：** 这两个都是事件对象，当输入框的值发生变化时会触发该事件。区别在于，**当输入框的值发生改变时会立即触发oninput事件，而onchange是在值改变后并且失焦才会触发**，并且还可以用在非输入框中，如：select等

# 垃圾回收与内存泄漏

## 内存机制

当声明一个变量时，如果声明的是**基本数据类型**就存储在内存的栈（stack）中，如果是**引用数据类型**则存储在堆（heap）中。

![](img/JS24.png)

![](img/JS25.png)



## 【1】浏览器的垃圾回收机制

### 概念

垃圾回收：JavaScript代码运行时，需要分配内存空间来储存变量和值。当变量不在参与运行时，就需要系统收回被占用的内存空间，这就是垃圾回收。

回收机制：

● Javascript 具有自动垃圾回收机制，会定期对那些不再使用的变量、对象所占用的内存进行释放，原理就是找到不再使用的变量，然后释放掉其占用的内存。

● JavaScript中存在两种变量：局部变量和全局变量。全局变量的生命周期会持续要页面卸载；而局部变量声明在函数中，它的生命周期从函数执行开始，直到函数执行结束，在这个过程中，局部变量会在堆或栈中存储它们的值，当函数执行结束后，这些局部变量不再被使用，它们所占有的空间就会被释放。

● 不过，当局部变量被外部函数使用时，其中一种情况就是闭包，在函数执行结束后，函数外部的变量依然指向函数内部的局部变量，此时局部变量依然在被使用，所以不会回收。

### 垃圾回收的方式

详细可看JS基础，里面有一些例子和优化方式

浏览器通常使用的垃圾回收方法有两种：标记清除，引用计数。

1）标记清除

● 标记清除是浏览器常见的垃圾回收方式，当变量进入执行环境时，就标记这个变量“进入环境”，被标记为“进入环境”的变量是不能被回收的，因为他们正在被使用。当变量离开环境时，就会被标记为“离开环境”，被标记为“离开环境”的变量会被内存释放。

● 垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记。然后，它会去掉环境中的变量以及被环境中的变量引用的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后。垃圾收集器完成内存清除工作，销毁那些带标记的值，并回收他们所占用的内存空间。

2）引用计数

● 另外一种垃圾回收机制就是引用计数，这个用的相对较少。引用计数就是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型赋值给该变量时，则这个值的引用次数就是1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数就减1。当这个引用次数变为0时，说明这个变量已经没有价值，因此，在在机回收期下次再运行时，这个变量所占有的内存空间就会被释放出来。

● 这种方法会引起循环引用的问题：例如： obj1和obj2通过属性进行相互引用，两个对象的引用次数都是2。当使用循环计数时，由于函数执行完后，两个对象都离开作用域，函数执行结束，obj1和obj2还将会继续存在，因此它们的引用次数永远不会是0，就会引起循环引用。

### 减少垃圾回收

虽然浏览器可以进行垃圾自动回收，但是当代码比较复杂时，垃圾回收所带来的代价比较大，所以应该尽量减少垃圾回收。

● 对数组进行优化：在清空一个数组时，最简单的方法就是给其赋值为[ ]，但是与此同时会创建一个新的空对象，**可以将数组的长度设置为****0**，以此来达到清空数组的目的。

● 对object进行优化：对象尽量复用，对于不再使用的对象，就将其设置为null，尽快被回收。

● 对函数进行优化：在循环中的函数表达式，如果可以复用，尽量放在函数的外面。

## 【2】哪些情况会导致内存泄漏

以下四种情况会造成内存的泄漏：

● 意外的全局变量：由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直留在内存中无法被回收。

● 被遗忘的计时器或回调函数：设置了 setInterval 定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。

● 脱离 DOM 的引用：获取一个 DOM 元素的引用，而后面这个元素被删除，由于一直保留了对这个元素的引用，所以它也无法被回收。

● 闭包：不合理的使用闭包，从而导致某些变量一直被留在内存当中。

[https://juejin.cn/post/6844903907584376839#heading-0]: 