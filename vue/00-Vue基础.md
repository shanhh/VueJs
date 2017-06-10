# VueJS

![](https://ws1.sinaimg.cn/large/006tNbRwly1fg3tmjasyyj30b40b40sk.jpg)

**官网：https://cn.vuejs.org/**



## 简介

>   数据驱动组件，为现代化的Web页面而生



**特点**

*   简洁：HTML模板+JSON数据+Vue实例
*   数据驱动：自动追踪依赖的模板表达式和计算属性
*   组件化：用解耦、可复用的组件来构造世界
*   轻量：~24kb的包，无依赖
*   快速：精确有效的异步批量DOM更新
*   模块友好：通过npm或bower安装，无缝融入你的工作流



**Vue和其它框架的不同**

[对比其它框架 https://cn.vuejs.org/v2/guide/comparison.html](https://cn.vuejs.org/v2/guide/comparison.html)





## 认知vue

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="vue.js" charset="utf-8"></script>
    </head>
    <body>
        <div id="app">
            <h1>{{ message }}</h1>
        </div>
    </body>
    <script type="text/javascript">
        let vm = new Vue({
            el: '#app',
            data: {
                message: 'Hello Vue'
            }
        });
    </script>
</html>
```



