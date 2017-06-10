## 组件

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、自包含和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

<img src="http://cn.vuejs.org/images/components.png" >

### 简介

>   页面上的每一小块区域都可以称为组件
>
>   或者完整的一张网页，也可以称为是一个组件

**特点**

*   组件可以扩展HTML元素，封装可重用的代码
*   组件是自定义元素，Vue.js的编译器为它添加特殊功能 
*   组件也可以是原生HTML元素的形式，以is形式扩展



### 组件的分类

#### 全局组件

定义好之后，任何地方都可以使用组件称为全局组件

```javascript
Vue.component('myComponent', {
    template: `<h2>全局组件</h2>`
});
```

组件的名称在使用的时候可以使用横线连接

```html
<div id="app">
    <my-component></my-component>
</div>
```



#### 局部组件

在实例内部定义，只可以在当前实例中使用的组件称为局部组件。

```javascript
components: {
    myComponent2: {
        template: `<h2>局部组件</h2>`
    }
}
```

```html
<div id="app">
    <my-component2></my-component2>
</div>
```



### 组件内部详解

#### data 数据

组件的数据必须是函数，然后以返回值的形式返回数据。目的是每个组件，都有一份自己的数据

如果不是函数，每个组件都共享一个数据，会相互影响

```javascript
Vue.component('myComponent', {
    template: `<h2>全局组件</h2>`,
    data () { // 每个组件拥有自己的一套数据
        return {
            a: 10
        }
    }
});
```

>   实例中的数据是对象，组件中的数据是函数，以返回值形式返回数据



#### props 传递数据

父组件给子组件传递数据，应使用props进行声明接收使用

声明使用props接收的变量

```javascript
Vue.component('myComponent', {
    props: ['msg'],
    template: `<h2>全局组件 {{ msg }}</h2>`
});
```

给组件添加message属性，并赋值，该值可以传递到组件中的message变量中

```html
<my-component msg="abc"></my-component>
```

如果在Vue实例中有一个变量name，希望把这个值赋值到组件中，可以绑定

```html
<my-component v-bind:msg="name"></my-component>
```

简写如下

```html
<my-component :msg="name"></my-component>
```

>   props是单向数据流
>
>   如果prop是一个对象或数组，在子组件内部改变会影响父组件的状态（数组和对象是引用类型）



#### slot 分发内容

在使用组件的时候，经常会遇到组件嵌套的写法

```html
<app>
    <app-header></app-header>
    <app-footer></app-footer>
</app>
```

如果app标签内部没有使用slot标签，则app标签内部的内容会被丢弃，而不是显示出来

应该使用slot标签解决这个问题

```javascript
Vue.component('app', {
    template: `<div>
                <h2>App</h2>
                <slot></slot>
            </div>`
});
```

在模板中使用`slot`标签之后，标签内部的内容，就会被插入到`slot`标签的位置



**具名slot**

给slot标签添加name属性，成为具名slot，如果没有name属性，则为默认的slot

```javascript
Vue.component('app', {
    template: `<div>
                <h2>App</h2>
                <slot name="header"></slot>
                <slot></slot>
                <slot name="footer"></slot>
            </div>`
});
```

使用

```html
<app>
    <div slot="header">
        <p>该内容显示在 slot header 的位置</p>
    </div>
    <h2>该内容显示在默认 slot 的位置</h2>
    <div slot="footer">
        <p>该内容显示在 slot footer 的位置</p>
    </div>
</app>
```



#### 单文件组件

[https://vue-loader.vuejs.org/en/start/spec.html](https://vue-loader.vuejs.org/en/start/spec.html)

```vue
<template>
    <div class="example">{{ msg }}</div>
</template>

<script>
export default {
    data () {
        return {
            msg: 'Hello world!'
        }  
    }
}
</script>

<style>
.example {
    color: red;
}
</style>

<custom1>
    This could be e.g. documentation for the component.
</custom1>
```







