# Learn Vue

# 1. 计算属性

在模板中使用表达式是十分方便的，但是如果这个表达式中包含了很多逻辑的话，就会变得十分难以理解，会让模板过重且难以维护；

```
<div id="dome1">
    {{message.split('').sort(function(a,b){return a-b}).join('')}}
</div>
```
所以如果涉及到复杂的逻辑时，应该使用计算属性；

## 1.1 基础示例

```
<div id="dome2">
    <p>original message : {{message}}</p>
    <p>Computed reverse message : {{reverseMessage}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue ({
        el: '#dome2',
        data: {
            message: 'this is a old message'
        },
        computed: {
            reverseMessage (){
                return this.message.split('').reverse().join('');
            }
        }
    })
</script>
```

这里我们声明了一个计算属性 reversedMessage 。我们提供的函数将用作属性 vm.reversedMessage 的 getter 。即是当获取 reversedMessage 的时候，触发函数，接受返回值；

这种属性同样也是动态的，当在控制台去修改message 的时候，同时也会修改reversedMessage；
你可以打开浏览器的控制台，修改 vm 的vm.reversedMessage 的值始终取决于 vm.message 的值；

## 1.2 计算缓存

### 1.2.1 计算缓存与methods
[DOME3](dome3.html)
使用 methods 我们也可以达到上述效果：

```
<div id="dome3">
    <p>{{message}}</p>
    <p>{{reverseMessage()}}</p>
</div>
<script>
    var vm = new Vue ({
        el: '#dome3',
        data: {
            message: 'find you'
        },
        methods: {
            reverseMessage (){
                return this.message.split('').reverse().join('');
            }
        }
    })
</script>
```
在这里的 methods 和 使用 computed 还是有区别的，对于 methods 在每一次重新渲染的时候，都会重新计算，但是 computed 中，计算是基于计算缓存的到的，计算属性只有当它的依赖发生变化的时候，才会重新进行计算，如果其依赖没有变化就会直接从其计算缓存中取得结果；

### 1.2.2 计算属性与Watched Property
[DOME4](dome4.html)
在 vue 中还提供了 watch 方法，用于检测 vue 实例上的数据变化；

```
<div id="dome4">
    <p>{{message}}</p>
    <p>{{reverseMessage}}</p>
</div>
<script>
    var vm = new Vue ({
        el: '#dome4',
        data: {
            message: 'find you',
            reverseMessage: ''
        },
        watch: {
            message (val){
                this.reverseMessage = val.split('').reverse().join('')
            }
        }
    })
</script>
```
这里 就是去监控 data 中的message 并且传入函数；当message去修改的时候，就会触发message对应的函数；

当然是用 computed 会更加方便；

### 1.2.3 计算属性 setter
[DOME5](dome5.html)
在computed中，计算属性默认只有getter，但是也能够添加setter；
```
<div id="dome4">
    <p>{{firstName}}</p>
    <p>{{lastName}}</p>
    <p>{{fullName}}</p>
</div>
var vm = new Vue ({
    el: '#dome4',
    data: {
        firstName: 'woHaHa',
        lastName: 'FF'
    },
    computed: {
        fullName: {
            get(){
                return this.firstName + ' ' + this.lastName
            },
            set: function(val) {
                this.firstName = val.split(' ')[0];
                this.lastName = val.split(' ')[1] ?
                    val.split(' ')[1]:' ';
            }
        }
    }
})
```
现在在运行 vm.fullName = 'John Doe' 时， setter 会被调用， vm.firstName 和 vm.lastName 也会被对应更新。
