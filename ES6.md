# ES6兼容性

http://kangax.github.io/compat-table/es6/可以查看兼容性



变量不能重复声明

块儿级作用域 全局，函数，eval



## let

不存在变量提升



不影响作用域链



## const 

赋初始值

一般常量使用大写

常量的值不能修改

块儿级作用域

对于数组和对象的元素修改，不算做对常量的修改，不会报错



ES6允许按照一定模式从数组和对象中提取值，对变量进行赋值

这被称为解构赋值

1.数值的结构

```javascript
const F4 =['小沈阳','刘能','赵四','宋小宝'];
let [xiao,liu,zhao,song] = F4;
console.log(xiao);
console.log(liu);
console.log(zhao);
console.log(song);
```



2.对象的结构

```
const zhao = {
	name:'赵本山',
	age:'不详',
	xiaopin:function(){
		console.log("我可以演小品");
	}
};

//let {name,age,xiaopin} = zhao;
//console.log(name);
//console.log(age);
//console.log(xiaopin);
//xiaopin();

let {xiaopin} = zhao;
xiaopin();
```



## 模板字符串

`` '' ""



ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。

可以省略function

```javascript
const school = {
	name,
	change,
	improve(){
		console.log("111");
	}
}
```



ES6 允许使用[箭头] （=>）定义函数。

声明一个函数

```javascript
let fn = function(){

}

let fun = (a,b) => {
	return a + b;
}
//调用函数
let result = fn(1,2);
console.log(result);
```



```javascript
//1.this是静态的，this 始终指向函数声明时所在作用域下的this的值
function getName(){
	console.log(this.name);
}
let getName2 = () => {
	console.log(this.name);
}
```



```javascript
//设置 window 对象 的name属性
window.name = '尚硅谷';
const school = {
	name:"ATGUIGU"
}

//直接调用
getName();//尚硅谷
getName2();//尚硅谷

//call 方法调用
getName.call(school);//ATGUIGU
getName2.call(school);//尚硅谷

//2.不能作为构造实例化对象
let Person = (name,age) => {
    this.name = name;
    this.age = age;
}
let me = new Person('xiao',30);
console.log(me);

//3.不能使用 arguments 变量
let fn = () => {
    console.log(arguments);
}
fn(1,2,3);
```



箭头函数简写

1. 省略小括号，当形参有且只有一个的时候

```javascript
let add = n => {
	return n + n;
}
console.log(add(9));
```

2. 省略花括号，当代码体只有一条语句的时候，此时 return 必须省略

```
//原
let pow = (n) => {
return n*n;
}
console.log(pow(9));
//现
let pow = (n) => n*n;
console.log(pow(9));
```



从数组中返回偶数的元素

```javascript
//原
const result = arr.filter(function(item){
	if(item % 2 === 0){
		return true;
	}else{
		return false;
	}
});
//现
const result = arr.filter(item => item % 2 === 0);

console.log(result);
```



箭头函数适合与this无关的回调，定时器，数组的方法回调

箭头函数不适合与this有关的回调，事件回调，对象的方法



ES6允许给函数参数赋值初始值

形参初始值 具有默认值的参数，一般位置要靠后(潜规则)

```javascript
function add(a,c=10,b){
	return a + b + c;
}
let result = add(1,2);
console.log(result);
```



与解构赋值结合

```
function connect(host="127.0.0.1",username,password,port){
	console.log(host);
}
connect({
	host:'atguigu.com',
	username:'root',
	password:'root',
	port:3306
})
```



rest参数(形式为`...变量名`)

ES6 引用 rest 参数，用于获取函数的实参，用来代替 arguments

ES5 获取实参的方式



rest 参数必须要放到**参数最后**



...扩展运算符能将[数组]转换为逗号分隔的[参数序列]



## Symbol

基本使用

 	独一无二的值，JavaScript语言的第七种数据类型

特点

 	1. Symbol值不能与其他数据进行运算
 	2. Symbol值不能与其他数据进行运算
 	3. Symbol定义的对象属性不能使用 for...in 循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

## 解构赋值

 字符串解构：const [a, b, c, d, e] = "hello"
 数值解构：const { toString: s } = 123
 布尔解构：const { toString: b } = true
 **对象解构**
形式：const { x, y } = { x: 1, y: 2 }
默认：const { x, y = 2 } = { x: 1 }
改名：const { x, y: z } = { x: 1, y: 2 }
 **数组解构**
规则：数据结构具有Iterator接口可采用数组形式的解构赋值
形式：const [x, y] = [1, 2]
默认：const [x, y = 2] = [1]
 **函数参数解构**
数组解构：function Func([x = 0, y = 1]) {}
对象解构：function Func({ x = 0, y = 1 } = {}) {}

## 扩展运算符

 把数组或者类数组展开成用逗号隔开的值  ...[1, 2, 3]





```javascript
function * gen(arg){
	console.log(arg);
    let one = yield 111;
    console.log(one);
    let two = yield 222;
    console.log(two);
    let three = yield 333;
    console.log(three);
}
//获取迭代器对象
let iterator = gen("AAA");
console.log(iterator.next());
//next方法可以传入实参
console.log(iterator.next("BBB"));
console.log(iterator.next("CCC"));
console.log(iterator.next("DDD"));
```



## promise对象

**定义**

包含异步操作结果的对象

**状态**

**进行中**：`pending`

**已成功**：`resolved`

**已失败**：`rejected`

**特点**

对象的状态不受外界影响

一旦状态改变就不会再变，任何时候都可得到这个结果

声明：`new Promise((resolve, reject) => {})`

- 异步操作未完成（pending）
- 异步操作成功（fulfilled）
- 异步操作失败（rejected）

- 异步操作成功，Promise 实例传回一个值（value），状态变为`fulfilled`。
- 异步操作失败，Promise 实例抛出一个错误（error），状态变为`rejected`。

```javascript
// 引入fs 模块
const fs = require('fs');

fs.readFile('./观书.md',(err,data)=>{
    //如果失败,则抛出错误
    if(err) throw err;
    //如果没有出错，则输出内容
    console.log(data.toString());
});
```



使用 Promise 封装

```javascript
const p = new Promise(function(resolve,reject){
    fs.readFile("./观书.md",(err,data)=>{
        //如果失败,则抛出错误
        if(err) reject(err);
        //如果没有出错，则输出内容
        resolve(data);
    });
});

p.then(function(value){
    console.log(value.toString());
},function(reason){
    console.log("读取失败！！！");
});
```



Promise封装ajax请求

```javascript
    const p =new Promise((resolve,reject)=>{
        //创建对象
        const xhr = new XMLHttpRequest();

        //初始化
        xhr.open("GET","https://route.showapi.com/213-1?keyword=%E6%B5%B7%E9%98%94%E5%A4%A9%E7%A9%BA&page=1&showapi_appid=38151&showapi_timestamp=20181026165947&showapi_sign=28aada70b1e1f31b0496290a233fd46");

        //发送
        xhr.send();

        //绑定事件，处理响应结果
        xhr.onreadystatechange = function(){
            //判断
            if(xhr.readyState === 4){
                //判断响应状态码 200-299
                if(xhr.status >= 200 && xhr.status < 300){
                    //表示成功
                    resolve(xhr.response);
                }else{
                    //如果失败
                    reject(xhr.status);
                }
            }
        }
    })

    //指定回调
    p.then(function(value){
        console.log(value);
    },function(reason){
        console.error(reason);
    })
```



Promise then方法

```javascript
    //调用 then 方法 then方法的返回结果是Promise对象，对象状态由回调函数的执行结果决定
    //1. 如果回调函数中返回的结果是 非promise类型的属性，状态为成功，返回值为对象的成功的值
    const p = new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve("用户数据");
            // reject("出错了");
        },1000)
    });

    //调用 then 方法 then方法的返回结果是Promise 对象，对象状态由回调函数的执行结果决定
    //1. 如果回调函数中返回的结果是 非promise 类型的属性，状态为成功，返回值为对象的成功的值

    // const result = p.then(value => {
    //     console.log(value);
    //     // //1.非 promise 类型的属性
    //     // // return 'iloveyou';
    //     // //2.是 promise 对象
    //     // return new Promise((resolve,reject)=>{
    //     //     // resolve('ok');
    //     //     reject('error');
    //     // })
    //     //3.抛出错误
    //     // throw new Error('出错啦！');
    //     throw '出错啦！';
    // }, reason=>{
    //     console.warn(reason);
    // });

    //链式调用
    //p.then(value=>{},reason=>{}).then(value=>{},reason=>{})
    p.then(value=>{

    }).then(value=>{

    })
    console.log(result);
```

多个文件读取

```javascript
//引入 fs 模块
// const fs = require("fs");
// const { resolve } = require("path");

// fs.readFile('学.md',(err,data1)=>{
//     fs.readFile('诗.md',(err,data2)=>{
//         fs.readFile('观书.md',(err,data3)=>{
//             let result = data1 + '\r\n' + data2 + '\r\n' + data3;
//             console.log(result);
//         });
//     });
// });

//使用 promise 实现
const p = new Promise((resolve,reject)=>{
    fs.readFile('./学.md',(err,data)=>{
        resolve(data);
    });
});
p.then(value=>{
    return new Promise((resolve,reject)=>{
    fs.readFile('./诗.md',(err,data)=>{
        resolve([value,data]);
        });
    })
}).then(value =>{
    return new Promise((resolve,reject)=>{
        fs.readFile('./观书.md',(err,data)=>{
            //压入
            value.push(data);
            resolve(value);
        });
    })
}).then(value=>{
    console.log(value.join('\r\n'));
})

```



```javascript
const p = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        //设置p对象的状态为失败，并设置失败的值
        reject("出错啦！");
    },1000)
});

// p.then(function(value){},function(reason){
//     console.error(reason);
// });

p.catch(function(reason){
    console.warn(reason);
})

```



```javascript
function equal(a,b){
	if(Math.abs(a-b) < Number.EPSILON){
		return true;
	}else{
		return false;
	}
}
console.log(0.1 + 0.2 === 0.3);
console.log(equal(0.1 + 0.2, 0.3));
```



## 数值扩展

Number.EPSILON 是JavaScript表示的最小精度

Number.isFinite 检测一个数值是否为有限数

Number.isNaN检测数值是否为NaN

Number.parseInt Number.parseFloat字符串转整数

Math.trunc 将数字的小数部分抹掉

Math.sign 判断一个数到底为正数 负数 还是零



对象方法扩展

Object.is 判断两个值是否完全相等

Object.assign 对象的合并

```javascript
const config1 = {
	host: 'localhost',
	port:3306,
	name:'root',
	test:'test'
};

const config2 = {
	host: 'http://atguigu.com',
	post: 33060,
	name: 'atguigu.com',
	pass: 'iloveyou',
	test2: 'test2'
}

console.log(Object.assign(config1,config2));
```

Object.setPrototypeOf 设置原型对象

Object.getPrototypeof

```javascript
const school = {
    name: '尚硅谷'
}
const cities = {
    xiaoqu:['北京','上海','深圳']
}

Object.setPrototypeOf(school,cities);
console.log(Object.getPrototypeOf(school));
console.log(school);
```



引入 m1.js 模块内容

```javascript
import * as m1 from "./src/js/m1.js"
```

## 模块暴露数据

```
1. 通用方式     
    import * as m1 from './m1.js';
2. 结构赋值 √   当目标模块使用『分别暴露』时
    import {add, minus} from './m1.js';
3. 简便方式 √   当目标模块使用『默认暴露』时
    import test from './m3.js';
```



### 默认暴露

当只有一个数据需要暴露时, 选择默认暴露

export

```javascript
export default {
	school: 'ATGUIGU';
	change:function(){
		console.log("我们可以改变你！！！");
	}
}
```



### 分别暴露

```javascript
export let school = '尚硅谷';
export function teach(){
	console.log('我们可以教给你开发技能');
}
```



### 统一暴露

```javascript
let school = '尚硅谷';
function findJob(){
	console.log("我们可以帮助你找工作！！");
}
export {school,findJob};
```

### 引入模块数据

通用导入方式

```javascript
引入 m3.js 模块内容
import * as m1 from "./src/js/m3.js";
m3.default.change();
```



解构赋值形式

```javascript
import {school,teach} from "./src/js/m1.js";
import {school as guigu,findJob} from "./src/js/m2.js"; //使用别名
console.log(guigu,findJob);
```



### 简便形式

只针对**默认暴露**

```javascript
import m3 from "./src/js/m3.js";
console.log(m3);
```



```
npm --yes
npm init --yes
npm i babel-cli babel-preset-env browserify -D
npx babel src/js -d dist/js --presets-babel-preset-env
```



```
        1. 安装工具 babel-cli babel-preset-env browserify(webpack)
        2.npx babel src/js -d dist/js
        3.打包 npx browserify dist/js/app.js -o dist/bundle.js
```



```javascript
async function fn(){
    return new Promise((resolve,reject)=>{
        reject("失败的错误");
    })
}
const result = fn();

//调用 then方法
result.then(value => {
    console.log(value);
}, reason => {
    console.log(reason);
})
```



async与await结合读取文件内容

```javascript
const p =new Promise((resolve,reject) =>{
reject("失败啦！");
})

async function main(){
try{
let result = await p;

console.log(result);
}catch(e){
console.log(e);
}
}
main();
```



```
//1.引入 fs 模块
const fs = require("fs");
```

## proxy

- 定义：修改某些操作的默认行为
- 声明：`const proxy = new Proxy(target, handler)`
- 入参
  - **target**：拦截的目标对象
  - **handler**：定制拦截行为
- 方法
  - **Proxy.revocable()**：返回可取消的Proxy实例(返回`{ proxy, revoke }`，通过revoke()取消代理)

```javascript
const target = {
    foo: 'bar',
    baz: 'qux'
};
const handler = {
    //捕获器 对应一些对象的基本操作，并在代理对象调用这些对应操作时，可以在这些操作传播到目标对象之前自动触发，从而进行拦截或者修改对应行为
    get(trapTarget,property,receiver){
        let decoration = '';
        if(property === 'foo'){
            decoration = '!!!';
        }
        return trapTarget[property]+decoration;
    }
};
//handler目标对象代理具体哪些事件配置对象包含一些需要对目标拦截的事件名的特定函数
const proxy = new Proxy(target,handler);
const myTarget = {};
const proxy2 = new Proxy(myTarget,{
    //set例子通过代理设置源对象的值，set捕获器接收多一个参数
    set(target,property,value,receiver){
        console.log('set()');
        return target[property] = value;
    }
});
//输入proxy.test = "hahaha"
//输出set()
//myTarget
//{test: 'hahaha'}
```

- 要使`Proxy`起作用，必须针对`实例`进行操作，而不是针对`目标对象`进行操作
- 没有设置任何拦截时，等同于`直接通向原对象`
- 属性被定义为`不可读写/扩展/配置/枚举`时，使用拦截方法会报错
- 代理下的目标对象，内部`this`指向`Proxy代理`

## Reflect 反射器

```javascript
// Reflect 反射器
const target = {
    foo:'bar',
    baz:'qux'
};
const handler = {
    get(trapTarget,property,receiver){
        let decoration = '';
        if(property === 'foo'){
            decoration = '!!!';
        }
        return Reflect.get(...arguments) + decoration;//加工属性操作可以写成这样
        //Reflect.get就是get的默认行为，get的默认行为是什么
        //get:Reflect.get
    }
};
const proxy = new Proxy(target,handler);
```



```javascript
const myTarget ={};

const proxy = new Proxy(myTarget,{
    set(target,property,value,receiver){
        console.log('set()');
        return Reflect.set(...arguments);
        //使用Reflect反射API封装好的默认的set行为
    }
});
//>>>>>>proxy.test = 'gagaga'
//set()
//<<<<<'gagaga'
//>>>>>myTarget
//<<<<<{test:'gagaga'}

//多数情况推荐优先使用反射器封装
```



Proxy.revocable 创建可撤销代理和摧毁方法

```javascript
const target = {
    foo: 'bar'
};
const handler = {
    get(){
        return 'intercepted';
    }
};
const {proxy,revoke} =  Proxy.revocable(target,handler);
console.log(proxy.foo);//intercepted
console.log(target.foo);//bar
revoke();
```



## **模块**

### 概念

模块是一段JavaScript代码, 这段代码会**自动运行**、而且是在**严格模式下**、并且**无法退出**运行。

### 特点

模块内定义的变量不会被自动添加到全局作用域

由于上面的原因, 模块要向外面暴露一些自己的数据, 这些数据可以被外界访问到

一个模块可以从另外一个模块中导入数据(即可以使用其他模块的数据)

模块顶层的 `this` 是 `undefined`, 并不是 `window` 或 `global`





