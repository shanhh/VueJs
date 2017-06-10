## Vue自定义指令



### 自定义全局指定

自定义可以让表单元素自动获取光标的元素

```javascript
import Vue from 'vue'

// 全局指令
Vue.directive('focus', {
    inserted(el) { // 被添加进去之后
        el.focus() // 获取光标
    }
})
```

使用

```html
<input type="text" v-foucs />
```





### 自定义局部指令

```javascript
// 实例或者组件接受一个directives的属性，内部可以添加自定义属性，则为局部指令
directives: {
    focus2: {
        inserted(el) {
            el.focus()
        }
    }
}
```

使用

```html
<input type="text" v-focus2 />
```





### 钩子函数

```javascript
directives: {
    focus2: {
        bind(el, binding, vnode) {
            // 只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
            console.log(el);
            console.log(binding);
            console.log(vnode);
            console.log('bind');
        },
        inserted(el) {
            // 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。
            el.focus()
            console.log('inserted');
        },
        update() {
            // 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。
            console.log('update');
        },
        componentUpdated() {
            // 被绑定元素所在模板完成一次更新周期时调用。
            console.log('componentUpdated');
        },
        unbind() {
            // 只调用一次， 指令与元素解绑时调用。
            console.log('unbind');
        }
    }
}
```





### 对象字面量

```javascript
directives: {
    demoo(el, binding) {
        console.log(el); // 获取当前元素 <div></div>
        console.log(binding.value.name); // 获取传递的值 xiaoming
        console.log(binding.value.age); // 获取传递的值 18
    },
}
```

使用

```html
<div v-demoo="{ name: 'xiaoming', age: 18 }"></div>
```

