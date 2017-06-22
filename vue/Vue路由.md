#### vue路由

1.首先用vue-cli 生成开发环境 具体步骤如下:

```javascript
1.全局安裝Vue-cli  npm install-g vue-cli
2.初始化項目 vue init webpack 項目名
3.进入项目 cd 项目名
4.安装依赖的包 npm install
5.开发环境运行 npm run dev
6.进行编译打包 npm run build
```

2.  在新生成的项目中 新建一个文件夹 在src中新建文件 pages 里面home.vue

   ```
   <template>
   	<div class="home">
   		首页
   	</div>
   </template>

   <script>
   	export default{
   		name:"Home"
   	}
   </script>

   <style>
   	
   </style>
   ```

   3.在新生成的文件夹中 新建一个router的文件 里面写index.js的路由规则

   ```javascript
   //引入模块
   import Vue from "vue"

   //引入router
   import VueRouter from "vue-router"

   Vue.use(VueRouter)

   //导入组件
   import Home from '../pages/Home'
   import Market from '../pages/Market'
   import Mine from '../pages/Mine'
   import Cart from '../pages/Cart'

   const routes = [
   	{path:"", redirect:"/home"},
   	{path:"/home", component:Home},
   	{path:"/market", component:Market},
   	{path:"/cart", component:Cart},
   	{path:"/mine", component:Mine},
   ]

   //导出创建好的路由
   export default new VueRouter({
   	routes
   })
   ```

   4.在app.Vue中写入路由

   ```javascript
   <template>
     <div id="app">
       <footer>
         <router-link to="/home" tag="li">首页</router-link>
         <router-link to="/market" tag="li">超市</router-link>
         <router-link to="/cart" tag="li">购物</router-link>
         <router-link to="/mine" tag="li">我的</router-link>
       </footer>
       <router-view></router-view>
     </div>
   </template>

   <script>

   export default {
     name: 'app',
     
   }
   </script>

   <style>
   #app footer{
     display: flex;
     justify-content: space-around;
     align-items: center;
     position: fixed;
     left: 0;
     right: 0;
     bottom: 0;
     height: 70px;
     line-height: 70px;
     background: #ccc;
   }
   #app footer{
     text-decoration: none;
     color:green;
   }
   #app footer li{
     list-style: none;
   }
   #app footer .router-link-active{
     color:red;
   }
   </style>
   ```

   5.在main.js中引入router 再挂载在实例中

   ```
   //引入路由
   import router from './router'
   //挂载
   new Vue({
     el: '#app',
     template: '<App/>',
     components: { App },
     router
   })

   ```