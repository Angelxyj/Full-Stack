# 组合式API

vue3的文件中，Vue不再是一个构造函数，而是一个对象，对象中封装了很多方法，每个方法都有其独立的作用。

我们在使用vue3的时候，需要从vue中解构各个用到的方法。

## setup

setup是vue3提供的对象中的一个方法，也是vue3主要使用的方法。我们在模板中需要的数据，直接定义在这个函数中，然后返回即可。函数也是一样。

```html
<body>
<div id="app">
    {{name}}
    <input type="text" v-model="name">
    <button @click="btnOnclick()">
        按钮
    </button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp} = Vue

createApp({
    setup() {
        let name = '张三'
        let btnOnclick = () => {
            console.log(666)
        }
        return {
            name,
            btnOnclick
        }
    }
}).mount('#app')
</script>
```

模板中可以使用插值表达式将setup返回的数据直接显示出来。

## ref

setup中返回的数据是一个普通的字符串，并不是一个响应式数据，在模板中显示没有问题，但是不再双向绑定了。调试工具无法修改数据，模板中数据改变了，我们定义的数据也不会改变。

要得到一个响应式的数据，需要使用vue3提供的ref方法。我们将数据作为ref方法的参数，就可以得到一个响应式数据。

```html
<div id="app">
    {{name}}
    <input type="text" v-model="name">
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, ref} = Vue

createApp({
    setup() {
        let name = ref('张三')

        return {
            name
        }
    }
}).mount('#app')
</script>
```

经过ref处理的数据，不再是一个基础类型字符串了，变成了一个对象，在模板中使用直接使用数据的变量名，在js中操作数据使用要使用`数据.value`

```html
<body>
<div id="app">
    {{name}}
    <input type="text" v-model="name">
    <button @click="editName()">改名</button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, ref} = Vue

createApp({
    setup() {
        let name = ref('张三')

        let editName = () => {
            name = '王五' // 修改无变化
            // name.value = '李四' // 修改了响应式数据
            console.log(name);
        }

        return {
            name,
            editName
        }
    }
}).mount('#app')
</script>
```

## reactive

如果我们希望一个引用类型数据是响应式的话，vue3提供了一个组合式API叫reactive。

```html
<body>
<div id="app">
    <input type="text" v-model="obj.name">
    <button @click="editName()">改名</button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, reactive} = Vue

createApp({
    setup() {
        let obj = reactive({
            name: '张三',
            age: 12
        })
        let editName = () => {
            obj.name = '李四'
        }
        return {
            obj,
            editName
        }
    }
}).mount('#app')
</script>
```

其实ref也可以修饰引用类型数据，只是内部也会调用reactive。只是使用ref的时候需要多加`.value`

```html
<body>
<div id="app">
    <input type="text" v-model="obj.name">
    <button @click="editName()">改名</button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, ref} = Vue

createApp({
    setup() {
        let obj = ref({
            name: '张三',
            age: 12
        })
        let editName = () => {
            obj.value.name = '李四'
        }
        return {
            obj,
            editName
        }
    }
}).mount('#app')
</script>
```

## computed

在vue3中，计算属性也是组合式API。

```html
<body>
<div id="app">
    <p>{{arr.join('-')}}</p>
    <p>长度：{{length}}</p>
    <button @click="add()">添加</button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, reactive, computed} = Vue

createApp({
    setup() {
        let arr = reactive(['a', 'b', 'c'])
        let add = () => {
            arr.push('d')
        }
        let length = computed(() => {
            return arr.length
        })
        return {
            arr,
            add,
            length
        }
    }
}).mount('#app')
</script>
```

## watch

```html
<body>
<div id="app">
    <p>{{arr.join('-')}}</p>
    <p>长度：{{length}}</p>
    <button @click="add()">添加</button>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
const {createApp, reactive, computed, watch} = Vue

createApp({
    setup() {
        let arr = reactive(['a', 'b', 'c'])
        let add = () => {
            arr.push('d')
        }
        let length = computed(() => {
            return arr.length
        })

        watch(arr, (newArr, oldArr) => {
            console.log(newArr, oldArr);
        })

        return {
            arr,
            add,
            length
        }
    }
}).mount('#app')
</script>
```

其他用法：

```js
// 如果要监视多个数据，就需要多次调用watch
// 简写
watch([数据1, 数据2], (newvalue, oldvalue) => {
    // 此时newvalue和oldvalue都是数组了，分别对应数据1和数据2
}, {deep: true})

// 当watch监听reactive定义的数据，不能准确获取到oldvalue了，且无法关闭默认的深度监听 - 只要被监视的数据的上一个引用类型数据，就无法准确获取到oldvalue
// 如果要准确获取到oldvalue，需要改变背监听的数据
watch(() => 对象.键, (newvalue, oldvalue) => {
    
})
// 如果监听的是对象中的多个属性
watch([() => 对象.键, () => 对象.键2], (newvalue, oldvalue) => {
    
})
```

vue3新增了watchEffect函数，默认就开启了第一次没有发生变化的时候也能执行，且他的监视比较智能，当函数用到哪个数据了，当这个数据发生变化的时候就能监视到。

```js
watchEffect(() => {
    // 这里用到哪个数据了，就能监视到哪个数据变化
})
```

## 其他组合API

vue3提供了toRef，让setup中返回的一个数据，指向一个对象的属性，并形成响应式数据：

```js
setup({
    let obj = {
    	name: '张三',
    	age: 13,
    	wife: {
    		name: '翠花'
		}
	}
      
    return {
    	name: toRef(person, 'name'),
    	wifeName: toRef(person.wife, 'name')
    }
})
```

这样可以让模板中使用的时候，不用对象.键，而直接使用键。

注意：toRef更新数据视图不会更新，需要将对象作为响应式数据才能实现视图更新。

但如果对象中的键需要在模板中使用的数量较多的话，就特别不方便，vue3还提供了更加强大的toRefs：

```js
setup({
    let obj = {
    	name: '张三',
    	age: 13,
    	wife: {
    		name: '翠花'
		}
	}
    let person = toRefs(obj)
    return {
		...person
    }
})
```

这样person中的每个数据，都通过toRef处理了，模板中可以直接使用person中的键了。

toRef和toRefs这两个组合API都是将响应式对象中的某个属性单独返回给某个变量时使用。

如果我们在操作一个层级比较深的对象数据的时候，只有外层属性会发生变化，为了提高效率，就可以只给最外层的属性做响应式，深层次的数据不做响应式，此时就可以将reactive换成shallowReactive。同理，如果ref定义的数据值处理基本数据类型，不处理对象类型，就可以换成shallowRef。

使用场景：

如果有一个对象数据，结构比较深，但变化的只是最外层的属性，就使用shallowReactive

shallowRef，仅处理基本类型为响应式，而不处理引用类型数据

如果某个数据，只希望展示，而不希望被修改，就可以使用readonly修饰：

```js
setup({
    let obj = {
    	name: '张三'，
    	age: 12,
    	wife: {
    		name: '翠花'
		}
	}
      
    obj = readonly(obj)
	return {obj}
})
```

以后再修改obj数据的时候，无法修改了，数据不会发生变化。

shallowReadonly让一个对象中的外层数据只读，但是深层数据可以修改

```js
setup({
    let obj = {
    	name: '张三'，
    	age: 12,
    	wife: {
    		name: '翠花'
		}
	}
      
    obj = shallowReadonly(obj)
	return {obj}
})
```

此时，obj的name和age以及wife不允许修改，但是wife中的name和age是可以修改的。

使用场景：当数据是别的组件定义好的，只是希望我们拿来使用，而不希望我们修改， 否则会影响数据所在组件效果。

vue3提供了反响应式数据操作，使用toRaw组合API

roRaw函数可以将一个由reactive定义好的响应式数据变成源数据

vue3提供了让一个对象中某个深层次的数据变成非响应式数据，使用markRaw组合API

```js
<div id="app">
    <button @click="fn">按钮</button>
    <p>{{obj.name}}</p>
    <p>{{obj.age}}</p>
    <p>{{obj.wife.uname}}</p>
    <p>{{obj.sex ? obj.sex : '无'}}</p>
    <hr>
    <button @click="ff">按钮</button>
    <li v-for="item in obj.children">{{item}}</li>
</div>

setup() {
        let obj = reactive({
            name: '张三',
            age: 12,
            wife: {
                uname: '翠花'
            }
        })

        let fn = () => {
            // console.log(obj);
            // console.log(toRaw(obj.wife));
            // markRaw(obj.wife)
            // obj.wife.uname = '如花'

            obj.sex = '女'
            // obj.children = ['阿大', '阿二']
            obj.children = markRaw(['阿大', '阿二'])

        }

        let ff = () => {
            obj.children.push('小明')
        }
        return {
            obj,
            fn,
            ff
        }
    }
```



自定义ref

```html
<body>
<div id="app">
    <input type="text" v-model="msg">
    <p>{{msg}}</p>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
Vue.createApp({
    setup() {
        function myRef(val, delay) {
            let timer = null
            return Vue.customRef((tract, trigger) => {
                return {
                    get() {
                        tract() // 通知get追踪val的变化
                        return val
                    },
                    set(value) {
                        clearTimeout(timer)
                        timer = setTimeout(() => {
                            val = value
                            trigger() // 通知vue重新渲染模板
                        }, delay)
                    }
                }
            })
        }
        let msg = myRef('hello', 500)
        return {
            msg
        }
    }
}).mount('#app')
</script>
```

祖宗组件和后代组件之间的通信：

```html
<body>
<div id="app">
    <p>姓名：{{name}}</p>
    <p>年龄：{{age}}</p>
    <father></father>
</div>
</body>
<script src="./node_modules/vue/dist/vue.global.js"></script>
<script>
let child = Vue.defineComponent({
    name: 'child',
    setup() {
        let grandPA = Vue.inject('person') // 接收祖宗组件的数据
        console.log(grandPA);
        return {grandPA}
    },
    template: `
        <div>
            <p>爷爷姓名：{{grandPA.name}}</p>
            <p>爷爷年龄：{{grandPA.age}}</p>
        </div>
    `
})
let father = Vue.defineComponent({
    name: 'father',
    setup() {

    },
    components: {
        child
    },
    template: `
        <div>
            <child></child>
        </div>
    `
})
Vue.createApp({
    setup() {
        let person = Vue.reactive({
            name: '张三',
            age: 12
        })
        Vue.provide('person', person) // 给后代传递数据，参数1数据名称，参数2具体数据
        return {...person}
    },
    components: {
        father
    }
}).mount('#app')
</script>
```

响应式数据的判断：

isRef：判断一个数据是否是一个ref对象

isReactive：判断一个对象是否有reactive创建的响应式对象

isReadonly：判断一个对象是否有readonly创建的只读数据

isProxy：检查一个对象是否是由reactive或readonly方法创建的响应式数据

全局属性的改变：

Vue.component -> app.component

Vue.directive -> app.directive

Vue.mixin -> app.mixin

Vue.use -> app.use

Vue.prototype -> app.config.globalProperties

如果挂载一次，需要后面的每次都能简单的直接使用，以前是将内容挂载到原型上的，现在vue3需要将内容挂载到app.configGlobalProperties上，然后，每个app就能用了



事件移除了自定义按键修饰符，移除了阿斯克码按键修饰符，移出了native修饰符

vue组件中，事件可以被指定为自定义事件或原生js事件，例：

```vue
<!-- 父组件绑定 -->
<mycomponent @close="close()" @click="click()"></mycomponent>
```

子组件指定事件类型：

```html
<script>
    {
        emits: ['close', 'click'] // 被指定的事件都是自定义事件
    }
</script>
```

vue3中移除了过滤器filter，建议我们使用方法或计算属性去实现

