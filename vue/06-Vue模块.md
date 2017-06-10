## Vue模块



### vue-router

[http://router.vuejs.org/zh-cn/essentials/getting-started.html](http://router.vuejs.org/zh-cn/essentials/getting-started.html)

1.  安装

    ```
    npm install --save vue-router
    ```

2. 引入

    ```javascript
    import VueRouter from 'vue-router'
    ```

3. Vue安装模块

    ```javascript
    Vue.use(VueRouter)
    ```

4. 一级路由的使用

    ```html
    <router-link to="/home">Home</router-link>
    <router-link to="/cart">Cart</router-link>
    <router-link to="/mine">Mine</router-link>
    <router-view></router-view>
    ```

    引入对应的文件，并编写路由规则

    ```javascript
    import Home from './pages/home'
    import Cart from './pages/cart'
    import Mine from './pages/mine'

    // 定义路由规则
    const routes = [
        { path: '/', component: Home },
        { path: '/home', component: Home },
        { path: '/cart', component: Cart },
        { path: '/mine', component: Mine }
    ];

    // 创建路由实例
    const router = new VueRouter({
        routes
    });

    // 挂在在vue实例中
    const app = new Vue({
        el: '#app',
        template: '<App/>',
        components: { App },
        router
    })
    ```

5. 二级路由的使用

    ```html
    <router-link to="/mine/one">One</router-link>
    <router-link to="/mine/two">Two</router-link>
    <router-view></router-view>
    ```

    配置二级路由，其它区域代码不变

    ```javascript
    const routes = [
      { path: '/mine', component: Mine, children: [
          { path: '', component: One },
          { path: 'one', component: One },
          { path: 'two', component: Two },
          { path: 'three', component: Three },
      ] }
    ]
    ```

    ​

    ​



### vue-resource



1.  安装

    ```
    $ npm install --save vue-resource
    ```

2. 引入

    ```javascript
    import VueResource from 'vue-resource';
    ```

3. Vue安装模块

    ```javascript
    Vue.use(VueResource);
    ```

4. 使用

    ```javascript
    // 全局使用
    Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
    Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

    // 局部使用
    this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
    this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
    ```

    通常情况下，都会将网络请求放在`created`方法中

    ```javascript
    created () {
        this.$http.get('url').then(data => {
            // ...
        });
      
        this.$http.post('url').then(data => {
            // ...
        });
      
        this.$http.jsonp('url').then(data => {
            // ...
        });
    }
    ```

5. 其他的方法: [api文档](https://github.com/pagekit/vue-resource/blob/develop/docs/http.md)







### axios

#### 简介

在vue升级到2.0后，官方就不再更新`vue-resourece`，vue官网也不再推荐`vue-resource`作为推荐的HTTP库，转而推荐使用`axios`

>   Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。
>
>   Axios并不是Vue的一个模块！！



尤大大原话：

>   最近团队讨论了一下，Ajax 本身跟 Vue 并没有什么需要特别整合的地方，使用 fetch polyfill 或是 axios、superagent 等等都可以起到同等的效果，vue-resource 提供的价值和其维护成本相比并不划算，所以决定在不久以后取消对 vue-resource 的官方推荐。已有的用户可以继续使用，但以后不再把 vue-resource 作为官方的 ajax 方案。
>
>   这里可以去掉 vue-resource，文档也不必翻译了。



### 如何使用

1.  安装

    ```
    npm install --save axios
    ```

2. 引入

    ```javascript
    import axios from 'axios'
    ```

3. 使用

    ```javascript
    // 为给定 ID 的 user 创建请求
    axios.get('/user?ID=12345').then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });

    // 可选地，上面的请求可以这样做
    axios.get('/user', {
        params: {
          ID: 12345
        }
    })
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
    ```

4. 然后因为axios并不是vue的插件所以不能用`Vue.use`,但可以将它添加到Vue的原型中。这样我们也可以像调用vue-resourece一样在组件中调用axios的方法

    ```javascript
    //将axios添加到vue的原型中
    Vue.prototype.$http = axios
    ```

[API地址：http://www.kancloud.cn/yunye/axios/234845](http://www.kancloud.cn/yunye/axios/234845)

[Github地址：https://github.com/mzabriskie/axios](https://github.com/mzabriskie/axios)