## 数据修改后，为何页面会发生改变

>   在Vue中，每个一个数据，都会生成一对set和get方法

每次给数据赋值的时候，都会执行set方法，内部触发页面的更新

每次取出数据的时候，都会执行get方法





有些特殊情况，数据中的set和get方法不会被添加上，此时数据的改变，不会影响到页面

```javascript
created() {
    setTimeout(() => {
        this.data = {
            err: 0,
            data: [
                { name: 'xiaoming' },
                { name: 'xiaohong' },
            ]
        }
        // 后面单独添加的msg不会生成set和get方法
        this.data.msg = 'OK'
    }, 1000);
},
```

此时msg和页面并不是绑定状态，需要使用`$set`解决

```javascript
// 局部用法
this.$set(this.data, 'msg', 'OK')

// 全局用法
Vue.set(this.data, 'msg', 'OK')
```

