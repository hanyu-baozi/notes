# data写法

两种

```javascript
new Vue({
	el:'#root',
	//对象式
	/*	data:{
		name:'尚硅谷'
	}*/
	//函数式
	data(){
		console.log('@@@',this);//此处的this是Vue实例对象
		return{
			name:'尚硅谷'
		}
	}
})
```

# 指令语法

```kotlin
v-bind:为单向数据绑定 ,数据只能从data流向页面
```

v-bind:动态绑定样式

**修饰符**

v-model.number 使输入的数据类型转换成 Number类型

v-model.lazy 时输入的数据在失去焦点后, 才会被收集

v-model.trim 去掉前后的空格

# **事件监听器**

**`v-on:xxx`**

# 数据代理

js Object.defineProperty

```javascript
<!-- 数据代理：通过一个对象代理对另一个对象中属性的操作（读/写） -->

    let obj = { x: 100 };
    let obj2 = { y: 200 };

    Object.defineProperty(obj2, "x", {
        get() {
            return obj.x;
        },
        set(value) {
            obj.x = value;
        },
    });

<!--
obj.x
100
obj2.x
100
obj2
{y: 200}
-->
```

# 事件修饰符

@click.prevent 阻止默认事件

stop 阻止事件冒泡（常用）

once 事件只触发一次（常用）

capture 使用事件的捕获模式

self 只有event.target是当前操作的元素是才触发事件

passive 事件默认行为立即执行，无需等待事件回调执行完毕

