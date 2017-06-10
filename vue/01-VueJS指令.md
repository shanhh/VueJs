## 表达式

在HTML中直接嵌入JavaScript代码，需要使用表达式

**{{  }}**

```javascript
{{ 10 + 20 }}
{{ Math.random() > 0.5 }}
{{ 'hello vue'.toUpperCase() }}
{{ 0 > 1 ? 'true' : 'false' }}
```

和AngularJS中使用规则一致

注意：放置的代码，一定要具有返回值



## 常用指令

#### v-text 

绑定内容值

```html
<h2>{{ a }}</h2>
<h2 v-text="a"></h2>
```

和AngularJS中ng-bind功能一致



#### v-model

双向绑定

```html
<input type="text" v-model="username">
<h2>{{ username }}</h2>
```

和AngularJS中ng-model功能一致

v-model是语法糖，不使用v-model也可实现（v-bind / v-on）

注意：使用变量username的时候，一定需要在控制器中进行过定义



#### v-for

渲染列表

关键字使用`of`或`in`都可以

```javascript
// 首先在数据中声明
let vm = new Vue({
    el: '#app',
    data: {
        people: [
            { name: 'xiaoming', age: 19 },
            { name: 'xiaohong', age: 20 },
            { name: 'xiaoli', age: 12 }
        ]
    }
});
```

```html
<!-- 在HTML中的使用 -->
<ul>
    <li v-for="person in people">{{ person.name + ' ' + person.age }}</li>
</ul>
<ul>
    <li v-for="person of people">{{ person.name + ' ' + person.age }}</li>
</ul>
<ul>
    <li v-for="(person, index) in people">{{ index + ' ' + person.name + ' ' + person.age }}</li>
</ul>
<ul>
    <li v-for="(person, index) of people">{{ index + ' ' + person.name + ' ' + person.age }}</li>
</ul>
```



#### v-on

事件绑定

和AngularJS不同，在Vue中，绑定任何事件都使用`v-on`这一个指令

```javascript
// 在method中声明方法
let vm = new Vue({
    el: '#app',
    methods: {
        show() {
            console.log('show');
        }
    }
});
```

```html
<!-- 在HTML中的调用 -->
<button type="button" v-on:click="show()">Show</button>
```

**简写形式**，使用`@`代替了`v-on`指令

```html
<button type="button" @click="show()">Show</button>
<h2 @mouseover="show()">mouseover</h2>
```

**修饰符**

执行事件的同时，另外去执行其它的任务

*   `stop`：调用event.stopPropagation()
*   `prevent`：调用event.preventDefault()
*   `enter`：调用指定按键上触发的回调

```html
<input type="text" @keyup.enter="show()">
<input type="text" @keyup.13="show()">
<a href="http://blog.lidaze.com" @click.prevent="show()">李大泽</a>
<div @click.stop="show()">阻止事件冒泡</div>
```

**传递参数**

```html
<h2 @click="show($event)">$event参数</h2>
```

```javascript
methods: {
    show(e) { // e代表传递进来的参数
        console.log(e);
    }
}
```



#### v-bind

绑定属性

```html
<img v-bind:src="imgUrl">
<a v-bind:href="blogUrl">{{ blogName }}</a>
<div v-bind:class="{ 'active': flag }">v-bind:class</div>
<div v-bind:style="{ fontSize: '50px' }">v-bind:style</div>
<div v-bind="{ id: 'div1', title: 'i\'m div' }">v-bind</div>
```

**简写形式**，使用`:`来代替`v-bind`指令

```html
<img :src="imgUrl">
<a :href="blogUrl">{{ blogName }}</a>
<div :class="{ 'active': flag }">v-bind:class</div>
<div :style="{ fontSize: '50px' }">v-bind:style</div>
```



#### v-if  v-else-if  v-else

是否渲染

```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```



#### v-show 

显示隐藏标签

```html
<h2 v-show="flag">v-show</h2>
```



#### v-cloak

这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

```html
<div id="app" v-cloak></div>
```



#### v-once

只渲染元素和组件**一次**。随后的重新渲染,元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

```html
<h2 @click="change()" v-once>{{ msg }}</h2>
```

```javascript
change () {
    this.msg += 'ggggggggg';
    console.log(this.msg);
}
```



