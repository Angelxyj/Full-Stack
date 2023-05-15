# vuex

vuex中5个属性：

state

mutations

actions

getters



modules - 模块化管理数据

```js
modules: {
    命名空间名字: 数据,
    空间名字: 数据
}
```

数据是通过导入进来的，被导入的文件：

```js
export default {
    namespaced: true,
    state: {},
    getters: {},
}
```

state使用：

直接使用

模板中

```html
{{$store.state.空间名字.数据}}
```

逻辑代码中

```js
this.$store.state.空间名字.数据
```

将数据放在自己组件中：

```js
import {mapState} from 'vuex'

computed: {
    ...mapState('空间名字', ['数据名字'])
}
```

getters使用：

直接使用

模板中

```html
{{$store.getters['空间名字/数据']}}
```

逻辑代码中：

```js
this.$store.getters['空间名字/数据']
```

将getters当做自己组件的数据：

```js
import {mapGetters} from 'vuex'

computed: {
    ...mapGetters('空间名字', ['数据名字'])
}
```

mutations中定义方法，跟以前的定义方式一样

直接使用

```js
this.$store.commit('空间名称/方法名称')
```

当做自己方法使用

```js
// 导入
import {mapMutations} from 'vuex'
// 展开在methods中
methods: {
    ...mapMutations('空间名称', ['方法名称'])
}
```

actions中定义异步方法，跟以前的方法一样

直接使用：

```js
this.$store.dispatch('空间名称/方法名称')
```

当做自己的方法使用

```js
// 导入
import {mapActions} from 'vuex'

// 展开在自己的方法中
methods: {
    ...mapActions('空间名称', ['方法名称'])
}
```

