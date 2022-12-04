### 值和变量

常量命名时全部大写（约定）

```javascript
let PI=3.1415

typeof null //object (其实是null)
```

基本数据类型

七种

​	String 字符串

​        Number 数值 

​        Boolean 布尔值 true/false

​        Null 空值 null

​        Undifined 未定义 undifined

​        Object 对象

​        Symbol

### 脚本

！只适用于外部脚本



​	defer属性 延迟到整个页面都解析完毕再运行

​	async属性 可能第二个脚本会优先执行，不必等脚本加载和执行完后再加载页面



### 浏览器不支持JavaScript

noscript元素 

```html
<body>

<noscript></noscript>

</body>
```

### var关键字

定义变量，范围：函数作用域

var在一个函数内部定义一个变量，就意味着该变量将在函数退出时被销毁

```javascript
function test() {
    var message = "hi"; // 局部变量
}
test();
console.log(message);// 出错！
```

### var声明提升

使用var关键字声明的变量会自动提升到函数作用域顶部

```javascript
console.log(me);//undefined
console.log(job); //报错
console.log(year);//报错
var me = 'Jonas';
let job = 'teacher';
const year = 1991;

```



### let声明变量

块作用域（比如在if里面时在外面获取它会报没有被定义的错误）

区别：

！不允许冗余（重复声明同一个变量）

！let关键字不能提升

window.let //undefined

### const声明常量

不会改变

### 操作符

#### 一元操作符

只操作一个值的操作符

#### 递增/递减操作符

++  加一    /      --  减一

#### 位操作符

##### 按位非

用波浪符（~）表示

对 数值取反并减 1

##### 按位与

符用和号（&）表示

与一个各位都为零的数值相与，结果为零

运算规则：0&0=0;  0&1=0;  1&0=0;   1&1=1;

##### 按位或

用和号（|）表示

参加运算的两个对象只要有一个为1，其值为1。

运算规则：0|0=0；  0|1=1；  1|0=1；  1|1=1；

##### 左移

用两个小于号（<<）表示



### 类型转换

Number()转换数字

string()转换成字符串

### Boolean真假转换

false value：0，''，undefined，null，NaN

### 等式运算符

== 类型可以不相等

===类型要相等（更严格）

### 预编译

在执行时，函数会先在其AO{}对象内找相应的变量，如果AO{}中没有，再在全局变量GO{}内寻找。

**局部预编译的4个步骤：**

1. 创建AO对象（Activation Object）执行期上下文。
2. 找形参和变量**声明**，将变量和形参名作为AO属性名，值为undefined
3. 将实参值和形参统一。
4. 在函数体里面找函数声明，值赋予函数体。



**全局预编译的3个步骤：**

1. 创建GO对象（Global Object）全局对象。
2. 找变量声明，将变量名作为GO属性名，值为undefined
3. 查找函数声明，作为GO属性，值赋予函数体

  ![预编译](C:\Users\包子\Desktop\预编译.jpg)

### 遍历

总结：**for in 一般用来遍历对象的key**、**for of 一般用来遍历数组的value**

**1.for-in遍历**

for-in是为遍历对象而设计的，不适用于遍历数组。(遍历数组的缺点：数组的下标index值是数字，for-in遍历的index值"0","1","2"等是字符串)

```javascript
var obj = {a:1, b:2, c:3};
for (let key in obj) {
  console.log(key);
}
// a
// b
// c
```

**2.for-of遍历**

continue 退出当前循环进入下一个循环  break退出整个循环

### 闭包

当内部函数被保存到外部时，将会生成闭包。闭包会导致原有作用域不释放，造成内存泄露。    

![image-20220708140644351](C:\Users\包子\AppData\Roaming\Typora\typora-user-images\image-20220708140644351.png)

```javascript
function test(){
    var num = 100;
    function a (){
        num++;
        console.log(num);
    }
    function b (){
        num --;
        console.log(num);
    }
    return [a,b];
}
var myArr = test();
myArr[0]();//101
myArr[1]();//100
```

 

```javascript
function eater(){
    var food = "";
    var obj = {
        eat : function () {
            console.log("i am eating " + food );
            food = "";
        },
        push : function (myFood){
            food = myFood;
        } 
    }
    return obj;
}
var eater1 = eater();
eater1.push('banana');
eater1.eat();
```

### 立即执行函数

针对初始化功能的函数

只被执行一次，能立即销毁

（除了改页面颜色的不需要返回值，其他的都需要返回值）

(function (){}())

```javascript
//立即执行函数
(function abc(){
    var a = 123;
    var b = 234;
    console.log(a + b); 
}())

(function (a,b,c){
    console.log(a + b + c*2);
}{1,2,3})
// 除了改页面颜色的不需要返回值，其他的都需要返回值
var num = (function(a,b,c){
    var d = a + b + c * 2 - 2;
    return d;
}(1,2,3));
```

只有表达式才能被执行符号执行



### 数组

push()//数组后面添加

unshift()//数组开头添加

pop()//删除

slice() 方法可从已有的数组中返回选定的元素。

slice() 方法可提取字符串的某个部分，并以新的字符串返回被提取的部分

splice()在原数组里删除

fill() 方法

concat()合并数组

reverse()反转数组

filter()创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

使用固定值填充数组

```javascript
      //1
      console.log(friends.indexOf("Steven"));
      //-1
      console.log(friends.indexOf("Bob"));

      friends.push(23);
      //true
      console.log(friends.includes("Steven"));
      //false
      console.log(friends.includes("Bob"));
      //"23" 23 false
      console.log(friends.includes("23"));
```

### forEach循环数组

```
movements.forEach(function(movement){
	
})
```



### Math.max()返回最大值

 函数返回一组数中的最大值。

### 箭头函数

箭头函数只会作用其父作用域的this关键字

### arguments关键字

存在于函数，不存在于箭头函数

### Object.assign

```javascript
//复制对象
const jessica2 = {
  firstName: 'Jessica',
  lastName: 'Williams',
  age: 27,
  family: ['Alice', 'Bob'],
};
const jessicaCopy = Object.assign({}, jessica2); //合并两个对象，返回新的
```

### 展开运算符

...arr 从数组中取出所有元素

### 无效合并符

??

Null undefined(不包括0和'')

### 逻辑赋值运算符

```javascript
const rest1 = {
  name: 'Capri',
  // numGuests: 20,
  numGuests: 0,
};
const rest2 = {
  name: 'La Piazza',
  owner: 'Giovanni Rossi',
};
// rest1.numGuests = rest1.numGuests || 10;
// rest2.numGuests = rest2.numGuests || 10;

// //OR
rest1.numGuests ||= 10; //10
rest2.numGuests ||= 10; //10
//null or undefined
rest1.numGuests ??= 10; //0
rest2.numGuests ??= 10; //10

// rest1.owner = rest1.owner && '<ANONYMOUS>';
// rest2.owner = rest2.owner && '<ANONYMOUS>';
//AND
rest1.owner &&= '<ANONYMOUS>';
rest2.owner &&= '<ANONYMOUS>';
console.log(rest1); //{name: 'Capri', numGuests: 0}
console.log(rest2); //{"name": "La Piazza","owner": "<ANONYMOUS>"}
```



### 对象

#### entries()

entries() 方法返回一个数组的迭代对象，该对象包含数组的键值对 (key/value)。

```javascript
for (const [i, el] of menu.entries()) {
  console.log(`${i + 1}:${el}`); //使用数组本来的索引
}
```

#### 受保护的属性

通常以下划线 `_` 作为前缀。

#### 只读 “power”

#### 私有#



#### 循环

```javascript
//Property Values
const values = Object.values(openingHours);
console.log(values); //0: {open: 12, close: 22}

//Entir object
const entries = Object.entries(openingHours);
console.log(entries); //0: (2) ['thu', {…}]
//[key,value]
for (const [key, { open, close }] of entries) {
  console.log(`On ${key} we open at ${open} and close at ${close}`);
}
```

### Set集合

.size查大小

.has查询是否有这个值（返回true或者false）

.add添加

.delete删除

### Map地图

.get用key值获取value

### 字符串

.length长度

.indexOf()查询第一次出现

.lastIndexOf()查询最后一次出现

.slice(数字)从（数字所在位置）开始提取 输入两个数的话就是后面减去前面的

.toLowerCase()小写

.toUpperCase()大写

.trim()去空格

.replace()替换

`padStart()`用于头部补全，`padEnd()`用于尾部补全。

includes() 方法用于判断字符串是否包含指定的子字符串。

如果找到匹配的字符串则返回 true，否则返回 false。

**注意：** includes() 方法区分大小写。

startsWith("") 是否以""开头

.split('')分割

.repeat()之后会将原来的字符串复制n次

.join() 方法用于把数组中的所有元素转换一个字符串。

元素是通过指定的分隔符进行分隔的。

### bind方法

创建一个新的函数，并绑定this

### prototype原型

被用于复制现有实例来生成新实例的函数

#### _proto原型链（链接点）

从属关系

prototype -> 函数的一个属性：对象{}

__proto__ :对象Object的一个属性:对象{}

对象的__proto__保存者该对象的构造函数的prototype



```javascript
test.__proto__===Test.prototype  //true

Object.prototype.__proto__  //null  
```

test {

​	a:1,

​	__proto__:Test.prototype = {

​		b:2,

​	__proto:Object.prototype = {

​		c:3

​		x __proto__

​		}

​	}

}

```javascript
Function.__proto__ === Function.prototype //true
Object.__proto__ === Function.prototype //true
```



hasOwnProperty()



' 输入需要查找的字符串' in 对象



### **构造函数**

用new来调用，就是为了创建一个自定义类

### **实例**

是类在实例化之后一个一个具体的对象

