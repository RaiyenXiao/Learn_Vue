# Learn Vue

# 1 事件处理器

## 1.1 事件监听 `v-on`

`v-on:evType='  '`
`v-on:`可以简写成`@`
可以用 v-on 指令监听 DOM 事件来触发一些 JavaScript 代码。
[DOME](dome.html)

```
<div id="dome">
    <button @click='counter++'>点击</button>
    <p>这个按钮被点击了{{counter}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome',
        data: {
            counter: 0
        }
    })
</script>
```

## 1.2 方法事件处理器 `methods`

直接在`v-on`处理复杂逻辑的行为就不合适了，因此 v-on 可以接收一个定义的方法来调用。
[DOME1](dome1.html)

```
<div id="dome1">
    <button @click='handleClick'>点击</button>
    <p>这个按钮被点击了{{counter}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome1',
        data: {
            counter: 0
        },
        methods: {
            handleClick(){
                this.counter ++
            }
        }
    })
</script>
```
## 内联处理器方法
这种方式是通过事件绑定的形式触发的，相当于 `el.onclick = handleClick;`
还可以实现 `el.onclick = function () {fn()}`
除了直接绑定到一个方法，也可以用内联 JavaScript 语句：
[DOME2](dome2.html)

```
<div id="dome1">
    <button @click='handleClick(1)'>点击</button>
    <p>这个按钮被点击了{{counter}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome1',
        data: {
            counter: 0
        },
        methods: {
            handleClick(num){
                this.counter += num
            }
        }
    })
</script>
```

## 1.3 事件对象 `$event`

[DOME3](dome3.html)
有时也需要在内联语句处理器中访问原生 DOM 事件。可以用特殊变量 $event 把它传入方法：

```
<div id="dome3">
    <button @click='handleClick(1,$event)'>点击</button>
    <p>这个按钮被点击了{{counter}}</p>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome3',
        data: {
            counter: 0
        },
        methods: {
            handleClick(num, ev){
                console.log(ev);
                this.counter += num
            }
        }
    })
</script>
```

## 1.3 修饰符

### 1.3.1 事件修饰符

事件处理器 `methods` 最好仅处理数据逻辑，而与DOM相关的事件细节如'stopPropagation'/'preventDefault' 就需要使用事件修饰符

>.stop -- 阻止事件冒泡
>.prevent -- 提交事件不再重载页面 (?)
>.capture -- 使用事件捕获模式
>.self -- 只当事件在该元素本身（而不是子元素）触发时触发回调
>.once -- 2.1.4 新增，在 component 处详将

使用示例：
`<div v-on:click.stop></div>`

注： 修饰符可以串联

## 1.3.2 按键修饰符

在监听键盘事件时，我们经常需要监测常见的键值。 Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：

按键修饰符：
>.[keyCode]
>.enter
>.tab
>.delete (捕获 “删除” 和 “退格” 键)
>.esc
>.space
>.up
>.down
>.left
>.right

2.1.0 新增：

>.ctrl
>.alt
>.shift
>.meta

```
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

使用实例：
```
<div id="dome3">
    <input type="text" name="" id="" v-model='val' @keydown.enter = 'add($event)'>
    <ul>
        <li v-for="item in items">{{item}}</li>
    </ul>
</div>
<script type="text/javascript">
    var vm = new Vue({
        el: "#dome3",
        data: {
            items: [],
            val: ''
        },
        methods: {
            add(ev){
                console.log(ev.keyCode);
                this.items.push(this.val);
                this.val = ''
            }
        }
    })
</script>
```
使用：`@keydown.13 = 'add($event)` 也能表示按下enter键

可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
`Vue.config.keyCodes.f1 = 112`


补充：为什么在 HTML 中监听事件?（原文）

你可能注意到这种事件监听的方式违背了关注点分离（separation of concern）传统理念。不必担心，因为所有的 Vue.js 事件处理方法和表达式都严格绑定在当前视图的 ViewModel 上，它不会导致任何维护上的困难。实际上，使用 v-on 有几个好处：
1. 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
2. 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
3. 当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何自己清理它们。
