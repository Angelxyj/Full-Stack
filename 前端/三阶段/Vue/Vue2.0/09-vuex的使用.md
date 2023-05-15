# vuex的使用

vuex是什么？

vue项目中的数据库，其中的数据是不持久的，在多个组件要共用一些临时数据的时候使用vuex。

依赖第三方模块：

```shell
npm i vuex@3
```

配置：在src下新建了store文件夹，其中新建了index.js

```js
import Vue from 'vue'
import vuex from 'vuex'
Vue.use(vuex)
const store = new vuex.Store({
    // 定义数据
    state: {
        num1: 5
    }
})
export default store
```

在根组件中使用store，在main.js中：

```js
import store from '@/store'

new Vue({
    store
})
```

使用：

模板中使用：

```html
{{$store.state.num1}}
```

在逻辑代码中使用：

```js
console.log(this.$store.state.num1)
```

如果要将vuex中的数据放在自己组件中，当做自己这个组件的数据使用 的时候：

```html
<script>
    // 导入state数据
    import {mapState} from 'vuex'
    export default {
        computed:{
            ...mapState(['num1'])
        }
    }
</script>
```

此时当前组件就有计算属性 num1 了。