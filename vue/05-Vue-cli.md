## Vue-cli

vue-cli是官方提供的一个脚手架工具(帮组写好基础代码).可以快速生成一个vue的项目模板

**主要的功能**

-   规范的目录结构
-   本地调试
-   代码部署
-   热加载
-   单元测试



**vue-cli的优势**

-   成熟的vue项目的架构设计
-   本地测试服务器 node-server
-   集成打包上线



**环境要求**

-   nodejs>4.0
-   git
-   能够使用命令行的终端



使用Vue-cli脚手架搭建项目架构

1.  CommonJS 或 ES6 模块
2.  使用 webpack 或 browserif 打包
3.  单文件组件，把页面拆为多个小组件
4.  使用 vue-loader 进行转换



### ES6 + webpack + vue-loader 进行项目架构

[https://github.com/vuejs-templates/webpack](https://github.com/vuejs-templates/webpack)

npm版本要求为必须 3+



#### 命令安装

```
安装vue-cli
$ npm install -g vue-cli 

初始化项目
$ vue init webpack project-name 

进入项目文件夹
$ cd project-name 

安装所依赖的包
$ npm install

开发环境运行
$ npm run dev

进行编译打包
$ npm run build
```



#### 目录文件介绍

<img src="http://i4.buimg.com/567571/60cdfa675f15418a.jpg" width="200">

-   `build`和`config`是webpack的配置文件
-   node_modules中存放的是`npm install`安装的文件依赖代码库
-   `src`文件是存放的是项目源码
-   `static`存放的是第三方的静态资源，里面只有`.gitkeep`这个文件的意思是当这个目录为空也是可以提交git代码仓库里，如果没有这个文件git会忽略这个目录
-   `.babelrc`这个文件是`babel`的一些配置,主要就是用于将es6的代码转成es5的,详细介绍：[babel](http://www.ruanyifeng.com/blog/2016/01/babel.html)
-   `.editorconfig`是编辑器的一些配置
-   `.gitignore`用于向git声明需要忽略提交的文件
-   `.postcssrc.js`这个文件是postCSS的配置文件，postCSS是一款通过JS插件来转换CSS的工具，这些插件能帮你校验你的CSS代码、转换未来的CSS语法、支持变量和混写、以及内联图片等等，其中[自动前缀](https://github.com/postcss/autoprefixer)插件是PostCSS最受欢迎预处理器之一。默认就配置使用了`autoprefixer`也就是自动前缀的这个插件
-   `index.html`就是入口的html文件，在编译打包过程中会将资源文件插入到这个html文件中
-   `package.json`项目的配置文件，这个文件中是我们在初始化`vue-clic`的时候填入的信息：
    -   最重要的就是里面的`scripts`属性，表示的是我们可以执行的一些命令,比如`npm run dev`就是执行的`node build/dev-server.js`这个命令，然后`npm run build`就是执行的`node build/build.js`也就是打包的操作，我们也自己在`scripts`中去配置一些脚本
    -   `dependencies`里面放的项目生产环境的一些依赖，然后在安装一些模块的时候可以通过`--save`保存到这个属性下，比如要使用`vue-router`就可以使用`npm install vue-router --save`
    -   `devDependencies`里面放的编译过程中的一些依赖，在最后打包的时候不存在
-   `README.md`就是项目的描述文件





#### 项目运行

项目的入口文件是`main.js`,然后在`main.js`中依赖`app.vue`，`app.vue`中又依赖了`hello.vue`这个组件

一个标准组件的构成就是由`template, script, style`这三个标签构成，如果要在`app.vue`中使用`hello.vue`这个组件的话，需要把hello定义为app的子组件

```javascript
// 引入模块
import Hello from './components/Hello'

export default {
  name: 'app',
  components: { // 声明为子组件
    Hello
  }
}
```

