## CSS使用问题

在单文件组件中，编写的样式可以保证只对当前组件的模板具有效果，添加`scoped`属性到`style`标签中

```html
<style scoped>
    button {
        color: 'red';
    }
</style>
```



在一个`*.vue`文件中，可以包含两个`style`标签，一个是`scoped`和`non-scoped`

```html
<style>
/* global styles */
</style>

<style scoped>
/* local styles */
</style>
```



在单文件组件中引入CSS样式，使用`@import`关键字

```html
<style scoped>
  比如说我们的swiper 就可以在home组件中的style直接把swiper的css引进来
@import "assets/css/bootstrap.min.css"
</style>
```



在`main.js`中引入css文件

```javascript
// 引入重置样式表
import './assets/reset.css'
```









### 项目中使用less进行CSS样式编写

1.  安装

    ```
    $ npm install --save less-loader less
    ```

2. style标签添加样式

    ```html
    <style lang="less" scoped>
    </style>
    ```

3. 编写代码

    ```html
    <style lang="less" scoped>
    @color: red;
    #home {
        background-color: cyan;
        a {
            color: @color;
        }
        p {
            color: @color;
        }
    }
    </style>
    ```

4. 项目运行之后，会自动将less转为普通的css

5. less的使用规则，参照官网
    [http://www.bootcss.com/p/lesscss/](http://www.bootcss.com/p/lesscss/)
    **less特点**

    *   变量
    *   嵌套
    *   混合
    *   函数 & 运算
    *   …...