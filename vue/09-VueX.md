## VueX

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

状态为驱动应用的数据源



### 什么是“状态管理模式”？

```javascript
Vue.component('Counter', {
    // state
    data () {
        return {
          count: 0
        }
    },
  
    // views
    template: `
        <div>{{ count }}</div>
    `,
  
    // actions
    methods: {
        increment () {
            this.count++
        }
    }
})
```

这个状态自管理应用包含以下几个部分：

-   **state**，驱动应用的数据源
-   **view**，以声明方式将**state**映射到视图
-   **actions**，响应在**view**上的用户输入导致的状态变化

以下是一个表示“单向数据流”理念的极简示意：

![](https://ws1.sinaimg.cn/large/006tKfTcly1fg5y7yam5lj30zk0o2dg0.jpg)

但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏

-   多个视图依赖于同一状态
-   来自不同视图的行为需要变更同一状态



**解决方案**

>   把组件的共享状态抽取出来，以一个全局单例模式管理

通过这种思路，vuex就诞生了

![](https://ws3.sinaimg.cn/large/006tKfTcly1fg5yb44traj30jh0fb0sp.jpg)

### 什么情况下我应该使用 Vuex？

虽然 Vuex 可以帮助我们管理共享状态，但也附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 [global event bus](http://vuejs.org/guide/components.html#Non-Parent-Child-Communication) 就足够您所需了。但是，如果您需要构建是一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。





### 开始使用Vuex

每一个 Vuex 应用的核心就是 store（仓库）

`store`基本上就是一个容器，它包含着你的应用中大部分的**状态(state)**

Vuex和单纯的全局对象有以下两点不同：

1.  Vuex 的状态存储是响应式的
    当 Vue 组件从 store 中读取状态的时候，若store中的状态发生变化，那么相应的组件也会相应地得到高效更新
2.  你不能直接改变 store 中的状态
    改变 store 中的状态的唯一途径就是显式地**提交(commit) mutations**
    这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用

**最简单的Store**

```javascript
const store = new Vuex.Store({
  	state: { // 共享的状态
    	count: 0
	},
	mutations: { // 修改状态的mutations
    	increment (state) {
      		state.count++
    	}
  	}
})
```

通过 `store.state` 来获取状态对象，以及通过 `store.commit` 方法触发状态变更

```javascript
store.commit('increment')
console.log(store.state.count) // 1
```

>   再次强调，我们通过提交 mutation 的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化

>   由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。
>   触发变化也仅仅是在组件的 methods 中提交 mutations



Vuex 通过 `store` 选项，提供了一种机制将状态从根组件『注入』到每一个子组件中

```javascript
const vm = new Vue({
    el: '#app',
    // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
    store,
    components: { Counter },
})
```

子组件中可以这样调用

```javascript
methods: {
    reduceOne () {
        // 通过$store进行mutations的触发
        this.$store.commit('REDUCE_PRICE', this.price)
    }
},
computed: {
    price () {
        // 通过$store变量获取数据
        return this.$store.state.money
    }
}
```





**state 状态**

vuex把所有的共享的数据都存放在了state中，任何地方获取的时候，都应该从state中获取



**Getters 可以认为是store的计算属性**

```javascript
const store = new Vuex.Store({
    state: {
        people: [
            { name: 'xiaoming', age: 18, gender: 'F' },
            { name: 'xiaohong', age: 19, gender: 'F' },
            { name: 'xiaohei', age: 17, gender: 'M' },
            { name: 'xiaobai', age: 20, gender: 'M' },
            { name: 'xiaofang', age: 13, gender: 'M' }
        ]
    },
    getters: {
        man: state => {
            return state.people.filter(p => p.gender == 'M')
        }
    }
})
```

Getters 会暴露为 `store.getters` 对象：

```
store.getters.man // [ { name: 'xiaohei', age: 17, gender: 'M' }, { name: 'xiaobai', age: 20, gender: 'M' }, { name: 'xiaofang', age: 13, gender: 'M' }]
```



**mutations 同步事件**

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

Vuex 中的 mutations 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**

你不能直接调用一个 mutation handler，而应该使用store的commit方法

```javascript
store.commit('increment')
```

一条重要的原则就是要记住**mutation 必须是同步函数**



**actions 异步事件**

Action 类似于 mutation，不同在于：

-   Action 提交的是 mutation，而不是直接变更状态。
-   Action 可以包含任意异步操作。

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation

Action 通过 `store.dispatch` 方法触发：

```javascript
store.dispatch('increment')

// 以载荷形式分发(传参)
store.dispatch('incrementAsync', {
    amount: 10
})

// 以对象形式分发
store.dispatch({
    type: 'incrementAsync',
    amount: 10
})
```







###  使用Vuex

完成案例，购买苹果香蕉橘子等价格相加

1.  安装vuex

    ```
    $ npm install --save vuex
    ```


2.  在`main.js`中引入并安装

    ```javascript
    import Vuex from 'vuex'
    Vue.use(Vuex)
    ```

3. 在`main.js`中创建store，并添加到实例中
    使用Vuex.Store创建一个store，可以理解为一个容器，存放所有的state

    ```javascript
    // 创建store
    const store = new Vuex.Store({
        state: { // 共享的数据
            money: 0
        },
        mutations: { // 同步执行的方法
            ADD_PRICE (state, { price }) { // 第一个参数为date，后面的参数为传递的数据，此处采用了解构
                state.money += price
                console.log(this);
            },
            REDUCE_PRICE (state, num) {
                state.money -= num
            }
        },
        actions: { // 异步执行的方法
            reducePrice (context, price) { // 第一个参数为context，里面包含commit、dispatch等方法，如果commit使用很多，可以使用{ commit }
                console.log(context);
                setTimeout(function () {
                    context.commit('REDUCE_PRICE', price)
                }, 1000);
            }
        }
    })

    // 添加vue实例中
    new Vue({
        el: '#app',
        store, // ES6简写
        template: '<App/>',
        components: { App }
    })
    ```

4. 编写组件
    **allPrice.vue**

    ```html
    <template lang="html">
        <div class="all-price">
            <h1>总金额: {{ totoalPrice }}</h1>
        </div>
    </template>

    <script>
    export default {
        computed: {
            totoalPrice () {
                return this.$store.state.money
            }
        }
    }
    </script>

    <style lang="css">
    </style>
    ```

    **Banana.vue**

    ```html
    <template lang="html">
        <div class="banana">
            <h2>{{ msg }}</h2>
            <button type="button" @click="addOne()">添加一个香蕉</button>
            <button type="button" @click="reduceOne()">减少一个香蕉</button>
            <new-banana />
        </div>
    </template>

    <script>
    import NewBanana from './newBanana'
    export default {
        name: 'banana',
        data () {
            return {
                msg: '我是香蕉',
                price: 5
            }
        },
        methods: {
            addOne () {
                // 触发对应的方法
                this.$store.commit({
                    type: 'ADD_PRICE',
                    price: this.price
                })
            },
            reduceOne () {
                this.$store.commit('REDUCE_PRICE', this.price)
            }
        },
        components: { NewBanana }
    }
    </script>

    <style lang="css">
    </style>
    ```

    **newBanana.vue**

    ```html
    <template lang="html">
        <div class="new-banana">
            <h2>{{ msg }}</h2>
            <button @click="buy()">买一根{{ msg }}</button>
        </div>
    </template>

    <script>
    export default {
        data () {
            return {
                msg: '进口的香蕉'
            }
        },
        methods: {
            buy () {
                this.$store.commit({
                    type: 'ADD_PRICE',
                    price: 100
                })
            }
        }
    }
    </script>

    <style lang="css">
    </style>
    ```

    **Orange.vue**

    ```html
    <template lang="html">
        <div class="banana">
            <h2>{{ msg }}</h2>
            <button type="button" @click="addOne()">添加一个橙子</button>
            <button type="button" @click="reduceOne()">减少一个橙子</button>
        </div>
    </template>

    <script>
    export default {
        name: 'banana',
        data () {
            return {
                msg: '我是橙子a',
                price: 10
            }
        },
        methods: {
            addOne () {
                // 触发mutations
                this.$store.commit({
                    type: 'ADD_PRICE',
                    price: this.price
                })
            },
            reduceOne () {
                // 触发actions
                this.$store.dispatch('reducePrice', this.price)
            }
        }
    }
    </script>

    <style lang="css">
    </style>
    ```

5. `App.vue`

    ```html
    <template>
        <div id="app">
            <all-price />
            <hr>
            <banana />
            <hr>
            <orange />
            <hr>
        </div>
    </template>

    <script>
    import AllPrice from './pages/AllPrice'
    import Banana from './pages/Banana'
    import Orange from './pages/Orange'

    export default {
        name: 'app',
        components: {
            AllPrice, Banana, Orange
        }
    }
    </script>

    <style>
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 60px;
    }
    </style>
    ```









