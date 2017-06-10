## Vue-Awesome-Swiper 轮播图

1.  安装

    ```
    $ cnpm install --save vue-awesome-swiper
    ```

2. 配置

    ```javascript
    import VueAwesomeSwiper from 'vue-awesome-swiper'
    Vue.use(VueAwesomeSwiper)
    ```

3. 注册局部组件

    ```javascript
    import { swiper, swiperSlide } from 'vue-awesome-swiper'

    export default {
        name: 'home',
        components: { swiper, swiperSlide }
    }
    ```

4. 编写HTML代码

    ```html
    <swiper :options="swiperOption" ref="mySwiper">
        <!-- slides -->
        <swiper-slide v-for="item in bannerData">
            <img :src="item.pic">
        </swiper-slide>
        <!-- Optional controls -->
        <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
    ```

5. 编写JS代码

    ```javascript
    data() {
        return {
            swiperOption: {
                autoplay: 1000,
                direction : 'horizontal',
                pagination : '.swiper-pagination',
                autoplayDisableOnInteraction:false,
            }
        }
    },
    // 如果你需要得到当前的swiper对象来做一些事情，你可以像下面这样定义一个方法属性来获取当前的swiper对象，同时notNextTick必须为true
    computed: {
        swiper() {
            return this.$refs.mySwiper.swiper
        }
    },
    ```