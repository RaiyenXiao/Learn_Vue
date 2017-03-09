# Learn Vue

## 1. vue 实例

### 1.1 Vue 构造器

`var vm = new Vue()`
所有的组件都是其实都是被扩展的vue实例

### 1.2 属性和方法
[DOME1](dome1.html)
每一个vue实例都会代理其 data 对象的所有属性

```
<script type="text/javascript">
    var vm = new Vue({
        data: {
            'a': '1'
        }
    });
    console.log(vm.a == vm._data.a);
</script>
```
这些被代理的属性是响应的。如果在实例创建之后添加新的属性到实例上，它不会触发视图更新;
只有被代理的属性在修改的时候才会触发视图的更新；

>注：vm.$data 就是存在vue实例中真实的data；

Vue 实例暴露了一些有用的实例属性与方法。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。例如：
```
<div id="dome1"></div>
<script type="text/javascript">
    var vm = new Vue({
        el: '#dome1',
        data: {
            'a': '1'
        }
    });
    console.log(vm.$el); //  <div id="dome1"></div>
    console.log(vm.el); // undefined
    console.log(vm.a == vm.$data.a);
    vm.$watch('a', function(nowValue,  oldValue){
        // 这个函数会在data的a进行修改的时候才会触发
    })
</script>
```
>注
1. 不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。


### 1.3 实例的声明周期

一个vue实例被创建的时候都要经历一系列的创建过程，在这个过程中实例会产生一些生命周期的钩子，例如`created`就会在实例被创建的时候触发

```
<div id="dome2">
    {{message}}
</div>
<script type="text/javascript">
    var vm = new Vue ({
        el: '#dome2',
        data: {
            message: 'hello'
        },
        created: function (){
            console.log('create');
        }
    })
</script>
```

实例的生命周期还包括：mounted、 updated 、destroyed

生命周期图示：
![生命周期](生命周期.png)
