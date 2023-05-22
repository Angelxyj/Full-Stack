# vue3选项式API

vue3提供了两个语法，一个叫选项式API；另一个叫组合式API。

选项式API跟vue2的使用方式一样的：面向对象的开发模式

大部分内容都跟vue2相同的：

插值表达式

指令一样

methods

directives

mixins

computed

watch

生命周期钩子函数前6个一样。

不一样的地方：

创建vue实例不一样：

```js
const {createApp} = Vue
const app = createApp({
    // data不一样，必须是函数
    data() {
        return {
            
        }
    }
})
// 没有el，只有mount
app.mount('el')
```

v-if和v-for优先级不一样，vue2中v-for优先，vue3中v-if优先

vue3取消了filter过滤器

vue3自定义指令的钩子函数不一样，简写不一样

vue2简写代表：bind + update - 按钮权限，不能简写，只能用inserted来做

vue3的简写代表：mounted和updated - 按钮权限直接可以简写使用

生命周期钩子函数的最后两个销毁的函数：

​	beforeUnmount

​	unmounted

手动销毁实例，必须是：

````js
app.unmount()
````

