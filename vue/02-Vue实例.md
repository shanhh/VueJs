## Vue实例

#### 简介

任何Vue程序都需要至少一个Vue实例对象，然后在内部设置各种属性和方法，以及生命周期的控制



#### 创建Vue示例

```javascript
let vm = new Vue({
    el: '#app',
    data: {},
    methods: {}
})
```



#### el 对应的标签根节点

放置一个选择器，不一定是id，可以是class或其它

```javascript
new Vue({
    el: '.app'
});
```



#### data 实例对应的数据

注意：此处的data是数据对象

```javascript
data: {
    message: 'abc'，
    people: [
        { name: 'xiaoming' },
        { name: 'xiaohong' },
        { name: 'xiaoli' }
    ],
    person: {
        name: 'xiaohong',
        age: 18,
        gender: 'M'
    }
}
```



#### methods 实例对应的方法

方法的写法不同，内部的this便不同

```javascript
methods: {
    test1: function () {
        console.log(this); // vm
    },
    test2 () { // 建议写法
        console.log(this); // vm
    },
    test3: () => {
        console.log(this); // window
    }
}
```





#### computed 计算属性

语法上看起来对应的是函数，其实代表的是`函数的返回值`

```javascript
let vm = new Vue({
    el: '#app',
    data: { a: 10 },
    methods: {
        doubleAFunc () {
            console.log('doubleAFunc'); // 使用一次，执行一次
            return this.a * 2;
        },
        change () {
            this.a += 10; // 如果值发生了改变，则方法执行N次，计算属性执行一次
        }
    },
    computed: { // 计算属性
        doubleA: function () {
            console.log('doubleA'); // 无论使用多少次，都只是执行一次
            return this.a * 2;
        }
    }
});
```

```html
<h2>{{ doubleA }}</h2>
<h2>{{ doubleAFunc() }}</h2>
<h2>{{ doubleA }}</h2>
<h2>{{ doubleAFunc() }}</h2>
<h2>{{ doubleA }}</h2>
<h2>{{ doubleAFunc() }}</h2>
```



#### watch 观察属性值

当值发生改变，第一时间获取新值和旧值

```javascript
watch: {
    a (newValue, oldValue) {
        console.log('a changed');
        console.log(newValue, oldValue);
    }
}
```



#### filter 过滤器

筛选指定的数据

```html
<li>{{ msg | upper }}</li>
<li>{{ people | students(18) }}</li>
```

```javascript
filters: {
    upper (str) {
        return str.toUpperCase();
    }, 
    students (people, age) { // 携带了参数
        const arr = [];
        people.map(item => {
            item.age >= age && arr.push(item);
        });
        return arr;
    }
}
```


