1.页面和组件

页面即组件 

先说明一下简写 v-bind:disabled  写成 : disabled 绑定属性 

v-on:click 写成@click 绑定事件

用咱们容易网组件的时候 @click.native = "todo()"

子组件 

    <template>
      <div :class="[{'red-error': flag}, 'chilren']">
        <div>我是子组件, 这是父组件传过来的数据 {{father}}</div>
      	<div>我自己的数据{{msg}}</div>
        <button @click="save()">给父亲通信的事件</button>
      </div>
    </template>
      
    <script>
    export default {
    	name:'Children',
        props:{
          'father': {
            type: String,
            default:'baba',
            require: true
          },
          'flag': {
            type: Boolean,
            default: false
          },
          'num': [String, Number]
        },
        data () {
          return {
             msg: 'Hello world!'
          }  
        },
        methods:{
          toFather () {
            console.log('father 用ref调用我的事件了')
          },
          save () {
            this.$emit('fatherEvent', {msg: this.msg, num: this.num}
        }
    }
    </script>
    <style lang = "less" scoped>
      .chilren{
        foht-size: 12px;
      }
    </style>
      

父组件 

    <template>
      <div class="father">
        <ul v-if="flag" class="father-ul"
        	<li v-for="item in list" :key="item.id">
        </ul>
        <input :disable="canWrite" v-model="num">
        <button @click="dealChirendEvent()">给父亲通信的事件</button>
        <Chidren
        	:fater="msg"
            :flag="flag"
            num="12345"
            ref="chidren"
            @fatherEvent="fatherReceviceData($event)"
          ></Chidren>
      </div>
    </template>
      
    <script>
    import Chidren from '../components/Chidren'
    export default {
    	name:'Father',
      	components: {
          Chidren,
        },
        data () {
          return {
             msg: 'I am baba!',
             num: 0,
             flag: false,
             list: []
          }  
        },
      	creted () {
          // 组件的生命周期可以 在这个里面发请求 页面加载好 数据也该回来了 等等一些不操作dom的预处理
          // this.nextTick(() = {// 处理dom事件}
      	},
      	mounted () {
          // 组件的生命周期 页面加载完毕 可以获取dom
      	},
      	watch: {
          'list':{
            handler (newVal, oldVal) {
              console.log(newVal === oldVal) // 数组和对象深度监听 虽然变化但是newVal === oldVal
            }, 
            deep: true // 数组和对象专用
          },
          num(newVal, oldVal) { // 一般的监听
            console.log(newVal, oldVal)
          }
      	},
      	// 属性和方法的区别 
      	computed: {  // 属性一般根据数据的改变才改变 属性常用 方法是点击事件函数调用 写法上还不一样
          canWrite () {
            return this.num > 5
          }
      	},
        methods: {
          dealChirendEvent () {
            this.$refs.children.toFather()
          },
          fatherReceviceData (data) {
            console.log(data)
          }
        }
    }
    </script>
    <style lang = "less" scoped>
      .father{
        foht-size: 12px;
      }
    </style>

组件的引入

在入口 main.js 中 

    import { Confirm, Alert, Toast, Notify, Loading } from 'vue-ydui/dist/lib.rem/dialog'
    // 挂载到原型 这是我做的一个wifi项目用的ydui组件
    Vue.prototype.$dialog = {
      confirm: Confirm,
      alert: Alert,
      toast: Toast,
      notify: Notify,
      loading: Loading
    }
    // 在页面中直接 用 this.$dialog.confirm({....})

页面的引入

在router文件夹下的index.js 

    {
      name: 'management-setting',
      path: 'setting/:id?',
      redirect: {
      	name: 'setting-selectCategoty'
      },
      meta: {
      title: '新建'
      },
      component: resolve => require(['business/management/setting/setting.vue'], resolve),
      children:[] // 和上面的结构一样 子路由
    }

2.data与页面

    // vue当中的data是个函数
    data () {
      return {
        msg: 'hello world',
        list: [] //  请求得到的list
      }
    }
    // 注意 所有页面中用到的数据 都要在这个里面有 要不数据即使改变 页面也不刷新
    // 局部用法
    this.$set(this.object, 'msg', 'OK')
    // 全局用法
    Vue.set(this.object, 'msg', 'OK')
    // 这个时候的data 里面就添加了

3.数据请求

    // 我们容易网用的是vue-resourece 但是自从vue2以后 老尤不在更新了 开始维护axios
    // 安装 npm install --save axios
    
    // 使用
    // 在main.js中
    import axios from 'axios'
    // 将axios添加到vue的原型中
    Vue.prototype.axios = axios
    
    // 页面中直接用 
    this.axios.post('/html/member/api/1/reg', vm.params)
      .then(res => {
    	console.log(res)
       })
    // 个人建议建一个server.js 统一维护
    //  server.js
    function getChildren(vm, params) {
      return vm.axios.post('/goods-api/goodsCategory/getChildren', params)
    }
    export {
      getChildren
    }
    // 页面
    import  * as server from '../server'
    // 调用
    server.getChildren(this, params)

4.vuex使用 （ 3秒知道）

vuex是状态管理的一种 应用场景常见页面的传值 有些不该用的就不要用了

    // 安装 npm install --save vuex
    
    // 我说的事自己用vue-cli构建的项目 容易网的项目架构 用的事函数的 一看就知道了 可自行下载
    // 建立一个vuex文件夹 里面有个vue.js
    import Vue from 'vue'
    import Vuex from 'vuex'
    Vue.use(Vuex) // 有的vue插件可以用Vue.use
    export default new Vuex.Store({
      state: { //共享的数据
        step: 1 // 步骤条的数据
      },
      mutations: { //修改数据的唯一途径
        ADD_STEP (state, {num, init}) {
          if(init) {
            state.step = 1
          } else {
            state.step += num
          }  
        },
        REDUCE_STEP (state, num) {
          state.step -= num
        }
        },
       actions: { //异步的操作时间
    
       }, 
        getters: { //这里的getters相当于就是实例中用到的计算属性
    
       }
    })
    //  在 main.js中
    // 引入vuex文件
    import store from './vuex/vuex'
    new Vue({
      el: '#app',
      template: '<App/>',
      components: { App },
      router,
      store
    })
    
    // 各个页面的使用
    // 操控mutations 
    this.$store.commit('ADD_STEP', { num:1 , init: true })
    // 得到state中的值
    const step =  this.$store.state.step

5.vue-cli

    安装vue-cli
    npm install -g vue-cli 
    
    初始化项目
    vue init webpack project-name 
    
    进入项目文件夹
    cd project-name 
    
    安装所依赖的包
    npm install
    
    开发环境运行
    npm run dev
    
    进行编译打包
    npm run build

6.反向代理

    // 在你安装的vue-cli中 有个config文件
    // 建立一个文件proxyConfig.js
    module.exports = {
       proxy: {
         '/api':{
             target: 'http://uat.bj.wucaicheng.com.cn',
             changeOrigin: true,
             pathRewrite: {
             '^/api': ''
              }
          }
       }
    }
    // 还是在config下面有个index.js的文件
    const proxyConfig = require('./proxyConfig') // 引入
    // 在dev中 加上
    proxyTable: proxyConfig.proxy
    // 最后一步
    // 打包上线要把这段注释掉
    axios.defaults.baseURL = '/api'

7.单项数据流

    // 数据流向props是单向的 子组件改变props 父组件的不会变 但是由于数组和对象是引用数据类型 这也会变 建议使用扩展运算符...
    var arr = [...this.props.data]
    // 有个数据太复杂可以类似于angular的angular.copy() 
    // 就要自己封装函数
    export default function deepCopy(result, source) {
      for (var key in source) {
        var copy = source[key];
          if (source === copy) continue; // 如window.window === window，会陷入死循环，需要处理一下
          if (Object.prototype.toString.call(copy) === "[object Object]") {
          	result[key] = deepCopy(result[key] || {}, copy);
          } else if (Object.prototype.toString.call(copy) === "[object Array]") {
         	 result[key] = deepCopy(result[key] || [], copy);
          } else {
          	result[key] = copy;
          }
        }
      return result;
    }

8.兄弟组件通信

    // 使用一个空的 Vue 实例作为一个事件总线中心(central event bus)
    // 在入口的main.js中
    //创建一个bus用来传值
    const bus = new Vue();
    Vue.prototype.bus = bus;
    // A 组件的mothods中触发事件
    methods: {
      sendValue () {
      	// 使用bus触发事件
    	bus.$emit('data', this.msg);
      }
    }
    // B组件created中接受数据
    created () {
      // 给bus添加事件
      bus.$on('data', function (data) {
        console.log(data);
      });
    }

9.路由守卫

    export default {
      name: 'MyPage',
      beforeRouteEnter(to, from, next) {
        console.log(to);
        console.log(from);
        next(vm => {
          // 当前组件的实例
          console.log(vm);
        })
        // 不能获取组件实例 `this`
        // 因为当钩子执行前，组件实例还没被创建
        console.log('beforeRouteEnter');
      },
      beforeRouteUpdate(to, from, next) {
        next() // 可以访问组件实例 `this`
      },
      beforeRouteLeave(to, from, next) {
        next()
        // 导航离开该组件的对应路由时调用
        // 可以访问组件实例 `this`
      }
    }
