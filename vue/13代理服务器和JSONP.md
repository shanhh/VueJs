## 代理服务器

1.  找到`config`文件夹下的`index.js`文件

2.  添加代理相关代码

    ```javascript
    proxyTable: {
        '/api': { // 名字
          target: 'https://m.juanpi.com/index/getMenu?select=1_1', // 目标地址
            changeOrigin: true,
              pathRewrite: {
                '^/api': '/' // 名字
              }
        },
    }
    ```

3.  重启`npm run dev`

4.  正常请求

    ```javascript
    this.axios.get('api').then(res => {
        console.log(res.data);
    });
    ```





## JSONP请求

1.  安装jsonp模块

    ```
    $ npm install --save jsonp
    ```

2.  在`main.js`引入

    ```javascript
    import jsonp from 'jsonp'
    Vue.prototype.jsonp = jsonp
    ```

3.  编写代码发起网络请求

    ```javascript
    this.jsonp('https://shop.juanpi.com/gsort?key=zuixinzhekou&type=1&zhouyi_ids=p8_c4_l1_18_51_5&machining=hotcoupon&page=1&rows=10&dtype=JSONP&cm=1&cm_channel=1&callback=gsort_callback', (err, data) => {
        console.log(data);
    })
    ```

    ​