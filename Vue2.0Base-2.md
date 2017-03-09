# 第二节： JS中Vue基本属性
## 实例挂载 el / template / render 
其实实例挂载的方式有三种，不仅是前面说到的el，还有template，render。
渲染的优先级是 **render渲染函数 > template渲染且编译 > el的挂载dom存在**
 > render 渲染函数，会通过Vue 构造函数将直接使用渲染函数渲染 DOM 树
```js
// App是一个组件，直接把他挂载到指定的dom上，2.0新增方法
import App from './app.vue';
const app = new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```
> template 模板，会先通过Vue 构造函数的编译生成渲染函数，然后再渲染 DOM 树
```html
<!-- 基本实现形式 -->
<div  id="vueInstance">
    <my-component></my-component>
</div>

<script>
    new Vue({
      el: '#vueInstance'
      components: {
        'my-component': '<div>A custom component!</div>'
      }
    })
</script>
```

> el挂载，如果挂载元素合法，会通过el属性获取挂载元素的 outerHTML 来作为模板，并编译生成渲染函数。


## data属性
毫无疑问，用来定义页面需要用来渲染的数据。比如之前的`name` \ `loginStatus` 都属于定义的属性。
普通实例的data是一个对象，而组件的data是一个函数.
这个在之后的组件中会讲到更详细的内容。
```js
// 普通实例对象的data
new Vue({
    //...
    data : {
        name: 'abc'
    }
})

// 组件的data
Vue.component('name-component', {
  template: '<p>{{ name }}</p>',
  data() {
    return {
        name :  'abc'
    }
  }
})
```

## 方法属性methods
由名字可知，他是用来定义一些方法的，这和以前定义时间没有任何区别。
methods对象，囊括了所有可能用到的方法。之后在需要的时候调用即可。
```js
    new Vue({
        //......
       methods : {
          getRandNum: function(min, max){
            return Math.floor(Math.random() * (max - min + 1)) + min;
          },
          sayName() {
              alert('welcome');
              // es6写法
          }
        }
    })
```

## 计算属性computed
计算属性的应用场景，一般是在有一个变量的值需要其他变量计算得到的时候，会用到。

比如，例如用户在输入框输入一个数 x，则自动返回给用户这个数的平方 x²。你需要对输入框进行数据绑定，然后当数据修改的时候，就马上计算它的平方。

 ```html
 <div id="vueInstance">
          输入一个数字: <input type="text" v-model="value">
          <p>计算结果：{{ square }}</p>
 </div>

 <script>
       var V = new Vue({
             el : '#vueInstance',
             data : {
                   value : 1
             },
             computed : {
                   square : function(){
                       // 注意格式，square 可以通过this.square 访问到，函数return回它的计算结果
                        return this.value * this.value;
                   }
             }
       });
 </script>
 ```

> 计算属性定义数值是通过定义一系列的function，就像我们先前定义methods对象的时候是一样的。比如square方法是用来计算变量“square”的，其方法的返回值是两个this.value的乘积。

接下来可以用computed做一个复杂一点例子。系统会随机取一个1~10以内的数字，用户可以在输入框随机输入一个1~10之内的数字，如果刚好用户的输入和系统的随机数刚好匹配，则游戏成功，反之失败。

 ```html
 <div id="vueInstance">
      输入一个数字: <input type="text" v-model="value1">
      <p>计算结果：{{ square }}</p>
      <br/><br/><br/>
      输入1~10内的数字: <input type="text" v-model="value2">
      <p>计算结果：{{ resultMsg }} - 其实是: {{ randNum }}</p>
</div>

<script>
  var V = new Vue({
    el : '#vueInstance',
    data : {
      value1 :1,
      value2 : null,
      randNum : 5//第一次随机数为5
    },
    methods : {
      getRandNum: function(min, max){
        return Math.floor(Math.random() * (max - min + 1)) + min;
      }
    },
    computed : {
      square : function(){
        return this.value1 * this.value1;
      },
      resultMsg : function(){
        if (this.value2 == this.randNum) {
          this.randNum = this.getRandNum(1, 10);
          return '你猜对了!';
        } else {
          this.randNum = this.getRandNum(1, 10);
          return '猜错了，再来!';
        }
      }
    }
  });
</script>
 ```

## 观察者属性watch
观察者属性就和字面意思一样，就是为了监视某个定义的data的变动，如果发生变动，则有对应的操作。
```js
new Vue({
    data: {
        // 想象question 绑定v-model在一个input上
        question: '',
    },
    watch: {
      // 比如监视question这个字段，当输入字符时   
      question: function (newValue,oldValue) {
        // 如果 question 发生改变，这个函数就会运行
        console.log(newValue, oldValue);
        // 函数的第一个参数是最新值，第二个参数是上一次的值
      }
}})
```

## computed对比methods
> 我们注意到，调用methods里面定义的方法也可以实现computed的效果。他们之间又有什么差别呢？
>
> 不同的是计算属性是基于它的**依赖缓存**。计算属性只有在它的相关依赖发生改变时才会重新取值。这就意味着只要依赖的data没有改变，computed的值立即返回之前的计算结果，而不会再次计算。
> 
> 但是每次调用methods的时候，即使依赖值没有发生改变，也会被执行。
> 
> 为什么需要缓存？因为当这个计算属性A需要依赖巨大的数组遍历和计算的时候，而别的属性又依赖A的情况下，没有缓存，则会频繁的getter，性能损耗大
> 

## computed对比watch

> computed 更简洁，高效，它return的计算结果本身就是可以根据依赖的值动态更新的。
>
> 这个能够根据依赖值动态更新的操作和watch的效果一致



