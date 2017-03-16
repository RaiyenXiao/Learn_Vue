# Learn Vue

# 1. 渲染

## 1.1 条件渲染
## 1.1.1 v-if & v-else
[DOME1](dome1.html)
vue 中通过 v-if 实现条件渲染，如果条件为真才会渲染；
```
<div id="dome1" v-if='onOff'>
    {{message}}
</div>
<script type="text/javascript">
    var vue = new Vue({
        el: '#dome1',
        data: {
            message: 'Hello Everybody',
            onOff: true
        }
    })
</script>
```
vue 中还提供了 `v-else` 可以同时存在 `v-if` 以及 `v-else`

### 1.1.2 templata 中的 v-if 条件组

```
Vue.component('example', {
    props: ['m'],
    template: `<div v-if="Math.random()>0.5">
                    true
                </div>
                <div v-else>
                    false
                </div>
                `
})
var vm = new Vue({
    el: '#dome2',
    data: {
        message: 'hello'
    }
})
```
>注：
v-if 是真实的条件渲染，因为它会确保条件块在切换当中适当地销毁与重建条件块内的事件监听器和子组件。
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——在条件第一次变为真时才开始局部编译（编译会被缓存起来）。

### 1.1.3 v-else-if

- 2.1.0 新增

效果类似于 switch
[DOME2](dome2.html)
```
Vue.component('example', {
    props: ['m'],
    template: `<div v-if="Math.random()>0.8">
                    大于0.8
                </div>
                <div v-else-if="Math.random()>0.6">
                    大于0.6
                </div>
                <div v-else>
                    一个随机数
                </div>
                `
})
var vm = new Vue({
    el: '#dome2',
    data: {
        message: 'hello'
    }
})
```
### 1.1.4 通过key来控制元素的可重用性

[DOME3](dome3.html)
Vue 尝试尽可能高效的渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 更快之外还可以得到一些好处。如下例，当允许用户在不同的登录方式之间切换:

```
<div id="dome2">
    <template v-if="loginType === 'username'">
        <label>Username</label>
            <input placeholder="Enter your username" key="username-input">
    </template>
    <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address" key="email-input">
    </template>
    <input type="button" value="changeType" @click='handleClick'>
</div>
<script type="text/javascript">
    Vue.component('templata',{
        templata: '<div></div>'
    })
    var vm = new Vue ({
        el: '#dome2',
        data: {
            loginType: 'username'
        },
        methods: {
            handleClick(){
                this.loginType = this.loginType=='username'? 'eamil':'username'
            }
        }
    })
</script>
```
在这里重复的input 和lable 两个标签会重复使用，仅仅改变了两个标签的内容；但是因此有个问题：input的内容不会重置，仅仅是替换了他们的 placeholder；
如果需要修复这个问题，可以使用key；

key属性是vue 提供一种方式来决定是否需要重复使用元素，key 具有唯一性

[DOME4](dome4.html)
```
<input placeholder="Enter your username" key='1'>
<input placeholder="Enter your email address" key='2'>
```
在key中添加描述信息

### 1.1.5 v-show

v-show 指令，是通过修改 display 的方式来决定是否显示元素，但是不支持 template 语法；

```
<div id="dome5" :style="styleObj" v-show='onOff' @mouseover='handleMouse'></div>
<script type="text/javascript">
    var vm = new Vue ({
        el: '#dome5',
        data: {
            onOff: true,
            styleObj: {
                width: '400px',
                height: '400px',
                background: 'red'
            }
        },
        methods: {
            handleMouse(){
                this.onOff = false
            }
        }
    })
</script>
```

## 1.2 列表渲染
我们用 v-for 指令根据一组数组的选项列表进行渲染。 v-for 指令需要以 item in arr 形式的特殊语法， arr 是源数据数组并且 item 是数组元素迭代的别名。
[DOME6](dome6.html)
```
<div id="dome6">
    <p  v-for="item in arr">{{item.infor}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: "#dome6",
        data: {
            arr:[
                {infor: 'name:ff'},
                {infor: 'age:24'},
                {infor: 'job:student'}
            ]
        }
    })
</script>
```

在`v-for`块中，具有对父级作用域的完全访问权限，并且在`v-for`中还存在第二个参数代表索引

```
<div id="dome6">
    <p  v-for="(item, index) in arr">{{item.infor}}-{{index}}</p>
</div>
```

注： 使用 `of` 代替 `in` 结果是相同的，使用`of`就是利用了迭代器

## 1.2.1 `v-for` 与 template
[DOME7](dome7.html)
```
<div id="dome7">
    <example v-for="item in message" :data='item.d'>
    </example>
</div>
<script type="text/javascript">
    Vue.component('example', {
        props:['data'],
        template: '<div>{{data}}</div>'
    })
    var vm = new Vue({
        el: '#dome7',
        data: {
            message: [
                {d:'这里是for与template'},
                // '这里是第二句'
            ]
        }
    })
</script>
```
------------ 补充 template
```
<ul id="dome4">
    <template v-for="(item, key, index) in items">
        <li>{{name}}-{{index}}-{{key}}-{{item}}</li>
    </template>
</ul>
<script type="text/javascript">
    var vm = new Vue ({
        el: '#dome4',
        data: {
            name: 'infor',
            items: {
                name: 'ff',
                age: 24,
                job: 'student'
            }
        }
    })
</script>
```

## 1.2.2 对象迭代
[DOME8](dome8.html)
`v-for` 不仅仅可以迭代一个数组还可以迭代一个对象

```
<div id="dome8">
    <div  v-for="item in obj">{{item}}</div>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome8',
        data: {
            obj: {
                name: 'ff',
                age: '24'
            }
        }
    })
</script>
```
显示：ff 24
你也可以提供第二个的参数为键名：
`<div  v-for="(item, key) in obj">{{item}}-{{key}}</div>`
显示：ff-name 24-age
第三个参数为索引：
`<div  v-for="(item, key, index) in obj">{{item}}-{{key}}-{{index}}</div>`
显示：ff-name-0 24-age-1

-----------补充 template

```
<ul id="dome4">
    <template v-for="(item, key, index) in items">
        <li>{{name}}-{{index}}-{{key}}-{{item}}</li>
    </template>
</ul>
<script type="text/javascript">
    var vm = new Vue ({
        el: '#dome4',
        data: {
            name: 'infor',
            items: {
                name: 'ff',
                age: 24,
                job: 'student'
            }
        }
    })
</script>
```

![template](template.png)

显示：

```
infor-0-name-ff
infor-1-age-24
infor-2-job-student
```

注：
1. 这里的template 并没有被渲染成为一个真实的标签；
2. 在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

### 1.2.3 整数迭代 - for
[DOME9](dome9.html)
```
<div id="dome8">
    <div  v-for="n in 10">{{n}}</div>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome8'
    })
</script>
```
结果: 1 2 3 4 5 6 7 8 9 10


### 1.2.4 for 与 组件

[DOME10](dome10.html)

1） 实例： to-Do-List

```
<div id="to-do-list">
    <input type="text" v-model="newList" @keyup.enter='add' placeholder="add a item">
    <ul>
        <li
            is='todoItem'
            :title="item"
            v-for="(item, index) in list"
        ></li>
    </ul>
</div>
<script type="text/javascript">
    Vue.component('todoItem', {
        props: ['title'],
        template: '\
            <li>\
                {{title}}\
                <button>remove</button>\
            </li>\
        '
    })
    var vm = new Vue ({
        el: '#to-do-list',
        data: {
            newList:'',
            list: [1,2,3,4]
        },
        methods: {
            add(){
                this.list.push(this.newList);
                this.newList=''
            }
        }
    })
</script>
```

注：组件有自己独立的作用域。为了传递迭代数据到组件里，我们要用 props ：
这里使用 $emit(eventName) 触发事件，remove是绑定在DOM中的函数；
！ 发现一个细节，就是组件模板template分行要加一个\,另外双引号里面有分号也要加一个\，我们也开始使用es6中的`（这个符号在tab键上面的~键英文状态输入）；

### 1.2.4 key

当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “就地复用” 策略。如果数据项的顺序被改变，Vue将不是移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素，适用于不依赖子组件状态或临时 DOM 状态（例如：表单输入值）的列表渲染输出。
为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有唯一 id。这个特殊的属性相当于 Vue 1.x 的 track-by ，但它的工作方式类似于一个属性，所以你需要用 v-bind 来绑定动态值（在这里使用简写）：
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
建议尽可能使用 v-for 来提供 key ，除非迭代 DOM 内容足够简单，或者你是故意要依赖于默认行为来获得性能提升。
因为它是 Vue 识别节点的一个通用机制， key 并不特别与 v-for 关联，key 还具有其他用途，我们将在后面的指南中看到其他用途。

### 1.2.5 数组的更新检测

```
push()
pop()
shift()
unshift()
splice()
sort()
reverse()
```
1) 重塑数组

变异方法(mutation method)，顾名思义，会改变被这些方法调用的原始数组。相比之下，也有非变异(non-mutating method)方法，例如： filter(), concat(), slice() 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组：
```
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
在Vue中，新的数组将会重新使用旧数组占用的DOM元素

2) 注意事项
Vue 不能检测以下变动的数组：
1. 当你利用索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
可以使用
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice`
example1.items.splice(indexOfItem, 1, newValue)
2. 当你修改数组的长度时，例如： vm.items.length = newLength
可以使用example1.items.splice(newLength)

### 1.2.6 显示过滤、排列结果

我们可以通过 methods 或者 component 配合，来对数组进行处理

[DOME11](dome11.html)
```
<ul id="numList">
    <li v-for="re in comLists">{{re}}</li>
</ul>
<script type="text/javascript">
    var vm = new Vue({
        el: "#numList",
        data: {
            num: [1,2,3,4,5,6]
        },
        computed: {
            comLists(){
                return this.num.filter(function(item){
                    return item > 3
                })
            }
        }
    })
</script>
```
或者，你也可以在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 使用 method 方法：

```
<ul id="numList">
    <li v-for="re in even(num)">{{re}}</li>
</ul>
<script type="text/javascript">
    var vm = new Vue({
        el: "#numList",
        data: {
            num: [1,2,3,4,5,6]
        },
        methods: {
            even (num){
                return num.filter (function(item){
                    return item & 1
                })
            }
        }
    })
</script>
```
