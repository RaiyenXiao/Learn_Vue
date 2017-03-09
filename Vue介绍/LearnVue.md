# Learn Vue

## 1. 基础介绍

Vue 是一种渐进式MVVM框架，其核心在于相应的数据绑定和组合视图组件

### 1.1 起步
在el 下的节点都会被检测，遇到`{{}}` 或者是指令的时候进行解析；

1） 绑定插入的文本内容
[DOME1](./dome1.html)
采用简洁模板语法来声明式的将数据渲染进去
```
<div id="app">
    {{message}}
</div>
<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue'
        }
    })
</script>
```

经过这样的声明，数据message 就和 DOM 绑定到一起了，通过在控制台修改 app.message ，浏览器就会重新渲染数据；

2） 绑定DOM 元素的元素属性
[DOME2](./dome2.html)
通过指令 `v-bind` 来实现，这种绑定也是动态的
`v-bind:attrName='key'`

```
<div id="app-2" v-bind:title='attr'>
    {{message}}
</div>
<script type="text/javascript">
    var app2 =  new Vue({
        el: '#app-2',
        data: {
            message: 'hello vue',
            attr: new Date()
        }
    })
</script>
```
将这个元素节点的 title 属性和 Vue 实例的 attr 属性绑定到一起

3） 条件与循环 绑定DOM结构到数据

1. 通过 `v-if` 来实现条件：
`v-if = 'key'`

[DOME3](./dome3.html)

```
<div id="app-3" v-bind:title='attr' v-if = 'onOff'>
    {{message}}
</div>
<script type="text/javascript">
    var app3 = new Vue({
        el: '#app-3',
        data: {
            message: 'hello',
            attr: 'title',
            onOff: true
        }
    })
</script>
```

在这里就将 如果 v-if 为真，才会渲染div，如果为假就不渲染了；

2. 通过 `v-for` 指令，实现绑定数据到数组来渲染 一个列表
`v-for = 'val in key'`
从数据的key中循环，循环的每一项赋值于val
[DOME4](./dome4.html)

```
<div id="app-4" v-bind:title='val' v-if='onOff'>
    <div  v-for='todo in todos'>
        {{todo.text}}
    </div>
</div>
<script type="text/javascript">
    var app4 = new Vue ({
        el: '#app-4',
        data: {
            val: new Date(),
            onOff: true,
            message: 'hello FF',
            todos: [
                {text: 'name1'},
                {text: 'name2'},
                {text: 'name3'},
                {text: 'name4'}
            ]
        }
    })
</script>
```

在这里，同样也是动态的，可以通过在控制台里输入 app4.todos.push({text: 'wo'});的方式动态添加到DOM中；

4) 处理用户输入

通过 `v-on` 指令绑定一个监听事件，调用我们vue中实例的方法(方法存放在 vue 对象的methods下)；
`v-on:eventName=key`

[DOME5](./dome5.html)

```
<div id="app-5">
    <p>{{message}}</p>
    <button v-on:click = 'handleClick'>{{message}}</button>
</div>
<script type="text/javascript">
    var app5 = new Vue ({
        el: '#app-5',
        data: {
            message: 'abcdefghijklmnopqrstuvwxyz'
        },
        methods: {
            handleClick: function (){
                this.message = this.message.split('').reverse().join('')
            }
        }
    })
</script>
```
在 handleClick 方法中，我们在没有接触 DOM 的情况下更新了应用的状态 - 所有的 DOM 操作都由 Vue 来处理，你写的代码只需要关注基本逻辑。

5) 表单的双向数据绑定：

[DOME6](.//dome6.html)
使用`v-model` 指令，可以将表单输入和数据进行双向数据绑定

```
<div id="app-6">
    <p>{{message}}</p>
    <input type="text" v-model='message'/>
</div>
<script type="text/javascript">
    var app6 = new Vue({
        el: '#app-6',
        data: {
            message: '双向绑定v-model'
        }
    })
</script>
```

6) 使用组件构建应用：

在vue 中，一个vue的实例就相当于一个组件：

```
<div id="app-7">
    <ol>
        <todo-item v-for="item in todoList" v-bind:todo="item"></todo-item>
    </ol>
</div>
<script type="text/javascript">
    Vue.component('todo-item', {
        props: ['todo'],//将数据从父作用域传到子组件
        template: '<li>{{todo.text}}</li>'
    })
    var app7 = new Vue ({
        el: '#app-7',
        data: {
            todoList: [
                {text: 'name1'},
                {text: 'name2'},
                {text: 'name3'},
                {text: 'name4'}
            ]
        }
    })
    //使用 v-bind 指令将 todo 传到每一个重复的组件中
</script>
```
注：
1. 使用 component（组成）创建一个组件，第一个参数是 组件的名称，第二个参数就是组件的内容
2. porps 储存了标签的属性, 必须要将需要使用的属性以 ['todo'] 的方式引用，以供模板使用
3. template 指的就是模板，也就是 todo-item 引入的东西
4. 将 data 引入的标签的属性中，然后建立起 component 与 数据之间的链接

原文如下：
这只是一个假设的例子，但是我们已经将应用分割成了两个更小的单元，子元素通过 props 接口实现了与父亲元素很好的解耦。我们现在可以在不影响到父应用的基础上，进一步为我们的 todo 组件改进更多复杂的模板和逻辑。
在一个大型应用中，为了使得开发过程可控，有必要将应用整体分割成一个个的组件。在后面的教程中我们将详述组件，不过这里有一个（假想）的例子，看看使用了组件的应用模板是什么样的：
```
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

7) props数据传递 ：

①组件实例的作用域：是孤立的，简单的来说，组件和组件之间，即使有同名属性，值也不共享。
```
    <div id="app">  
        <add></add>  
        <del></del>  
    </div>  
    <script>  
        var vm = new Vue({  
            el: '#app',  
            components: {  
                "add": {  
                    template: "<button>btn：{{btn}}</button>",  
                    data: function () {  
                        return {btn: "123"};  
                    }  
                },  
                del: {  
                    template: "<button>btn：{{btn}}</button>",  
                    data: function () {  
                        return {btn: "456"};  
                    }  
                }  
            }  
        });  
    </script>  
``` 
渲染结果是：2个按钮，第一个的值是123，第二个的值是456（虽然他们都是btn）   
 
②使用props绑定静态数据：
	【1】这种方法用于传递字符串，且值是写在父组件自定义元素上的。
	【2】下面示例中的写法，不能传递父组件data属性中的值
	【3】会覆盖模板的data属性中，同名的值。
``` 
<div id="app">  
    <add btn="h"></add>  
</div>  
<script>  
    var vm = new Vue({  
        el: '#app',  
        data: {  
            h: "hello"  
        },  
        components: {  
            "add": {  
                props: ['btn'],  
                template: "<button>btn：{{btn}}</button>",  
                data: function () {  
                    return {btn: "123"};  
                }  
            }  
        }  
    });  
</script>  
``` 
这种写法下，btn的值是h，而不是123，或者是hello
【4】驼峰写法
假如插值是驼峰式的，
而在html标签中，由于html的特性是不区分大小写（比如LI和li是一样的），因此，html标签中要传递的值要写成短横线式的（如btn-test），以区分大小写。
而在props的数组中，应该和插值保持一致，写成驼峰式的（如btnTest）
props: ['btnTest'],  
template: "<button>btn：{{btnTest}}</button>", 

<add btn-test="h"></add>

http://blog.csdn.net/sinat_17775997/article/details/52437962
http://cn.vuejs.org/v2/guide/components.html#prop-example-1