## $refs

尽管有props和events，但是仍然需要在JavaScript中直接访问子组件。

为此可以使用`ref`为子组件指定一个索引ID。

>   注意，ref并不是动态更新的，也就是说不应该绑定变量，而是写一个固定值即可。



在`home.vue`中添加list组件

```html
<div class="home">
    <list ref="childList"></list>
    <button @click="getChild()">获取子组件</button>
</div>
```

`home.vue`中的的方法中，可以直接访问到子组件对象

```javascript
import List from '../components/list'
export default {
    components: { List },
    methods: {
        getChild() {
            // 获取子组件中的数据
            console.log(this.$refs.childList.$data.name);

            // 调用子组件中的方法
            this.$refs.childList.test1();
        }
    }
}
```



`list.vue`组件中有数据

```javascript
export default {
    data() {
        return {
            name: 'list',
            version: '1.0.0'
        }
    },
    methods: {
        test1() {
            console.log('test1');
        },
        test2() {
            console.log('test2');
        }
    }
}
```



>   注意：子组件同样也可以调用父组件中的方法，通过props传值即可
>
>   通过此种方法，子组件也可以传值给父组件

父组件中

```html
<template lang="html">
    <div class="home">
        <list :func="test3"></list>
    </div>
</template>

<script>
import List from '../components/list'
export default {
    components: { List },
    methods: {
        test3(value) { // 参数value就是子组件传递给父组件的值
            console.log('test3', value);
        }
    }
}
</script>

<style lang="css">
</style>
```

子组件中

```html
<template lang="html">
    <div class="list">
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
        </ul>
        <button @click="userParentFunc()">调用父组件中的方法</button>
    </div>
</template>

<script>
export default {
    props: ['func'],
    methods: {
        userParentFunc() {
            this.func('我是子组件中的数据');
        }
    }
}
</script>

<style lang="css" scoped>
</style>
```

