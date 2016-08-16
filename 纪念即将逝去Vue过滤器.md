# Close2Vue

> This is a tutorial to vue‘s beginner.

> 项目取了Close to Vue的谐音(∩ᵒ̴̶̷̤⌔ᵒ̴̶̷̤∩)

> 内容如题，能够让你更靠近Vue ~


[第一章《从零开始学Vue》](https://github.com/AppianZ/Close2Vue/blob/master/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%AD%A6Vue.md)

[第二章《纪念即将逝去的Vue过滤器》](https://github.com/AppianZ/Close2Vue/blob/master/%E7%BA%AA%E5%BF%B5%E5%8D%B3%E5%B0%86%E9%80%9D%E5%8E%BBVue%E8%BF%87%E6%BB%A4%E5%99%A8.md)

[第三章《组件改变生活_揭开Vue组件的神秘面纱》](https://github.com/AppianZ/Close2Vue/blob/master/Vue%E7%BB%84%E4%BB%B6%E6%94%B9%E5%8F%98%E7%94%9F%E6%B4%BB.md)



## 第二章《纪念即将逝去的Vue过滤器》

在这个教程中，我们将会通过几个例子，了解和学习VueJs的过滤器。我们参考了一些比较完善的过滤器，比如orderBy 和 filterBy。而且我们可以链式调用过滤器，一个接一个过滤。因此，我们可以定义我们自己的过滤器在我们的Vue实例中。

阅读这个教程的前提是你对Vue已经有了基本的语法基础。


### VueJs中的过滤器基础
过滤器是一个通过输入数据，能够及时对数据进行处理并返回一个数据结果的简单函数。Vue有很多很便利的过滤器，可以参考官方文档，<http://cn.vuejs.org/api/#过滤器>，过滤器通常会使用管道标志 “ | ”， 比如：

```js
 {{ msg | uppercase  }}

  // 'abc' => 'ABC'
```

在上述的例子中，在插值的时候，使用了Vue的一个uppercase过滤器，msg可以是直接写死，写成{{ ‘abc’ | uppercase  }}，也可以利用用户输入来改变msg的值。

>  uppercase过滤器 : 将输入的字符串转换成大写字母的过滤器。


### 链式过滤
VueJs允许你链式调用过滤器，简单的来说，就是一个过滤器的输出成为下一个过滤器的输入，然后再次过滤。接下来，我们可以想象一个比较简答的例子，使用了Vue的 filterBy + orderBy 过滤器来过滤所有商品products。过滤出来的产品是属于电器类商品，并且按电器字母排序。

> filterBy过滤器 : 过滤器的值必须是一个数组，filterBy + 过滤条件。
> 过滤条件是：‘string || function’ + in ‘optionKeyName’

> orderBy过滤器 : 过滤器的值必须是一个数组，orderBy + 过滤条件。
> 过滤条件是：‘string || array ||function’ +  ‘order ≥ 0 为升序 ||  order < 0 为降序’

接下来，我们可以看下第二个例子：
我们有一个商品数组products，我们希望遍历这个数组，并把他们打印成一张清单，这个用v-for很容易实现。

```js
<li v-for="product in products">
      {{ product.name | capitalize }} - {{ product.price | currency }}
</li>
```

>  capitalize过滤器 : 将输入的字符串首字母转换成大写字母的过滤器。
>  currency过滤器 : 将输入的数字转换成现金的过滤器。可以跟上货币符号（默认$）和保留的小数位（默认2）。

利用上面的两个过滤器，能够很好的把一个json数组的商品清单格式化成通熟易懂的商品价格清单。

如果只想要电器类商品，可以在v-for上加过滤条件：

```js
<li v-for="product in products | filterBy 'electronics' in 'category' ">
      {{ product.name | capitalize }} - {{ product.price | currency }}
</li>
```

上面的例子，就是用filterBy 过滤在 'category'中含有'electronics' 关键字的列表，返回的列表就是只含有 'electronics' 关键字的列表。

如果想要多个搜索条件：

```js
<li v-for="product in products | filterBy 'electronics' in 'category'  'name' ">
      {{ product.name | capitalize }} - {{ product.price | currency }}
</li>
```

上面的例子，就是用filterBy 过滤在 'category' 和 'name' 中含有'electronics' 关键字的列表。

最后我们还需要将这个列表用字母进行排序。我们可以在 filterBy 过滤器的基础上，链式调用orderBy 过滤器。

```js
<ul>
       <li v-for="product in products
                   | filterBy 'electronics' in 'category'
                   | orderBy  'name' "
       >
            {{ product.name | capitalize }} - {{ product.price | currency }}
      </li>
</ul>
```

orderBy 的排序方式默认是升序，如果想要降序，只需要加一个小于0的参数：
```js
<li v-for="product in products
           | filterBy 'electronics' in 'category'
           | orderBy  'name'  -1 "
>
```


### 自定义过滤器
虽然VueJs给我们提供了很多强有力的过滤器，但有时候还是不够。值得庆幸的，Vue给我们提供了一个干净简洁的方式来定义我们自己的过滤器，之后我们就可以利用管道 “ | ” 来完成过滤。

定义一个全局的自定义过滤器，需要使用Vue.filter()构造器。这个构造器需要两个参数。

> Vue.filter() Constructor Parameters:
> 1.filterId: 过滤器ID，用来做为你的过滤器的唯一标识；
> 2.filter function: 过滤器函数，用一个function来接收一个参数，之后再将接收到的参数格式化为想要的数据结果。

回到之前的例子：
现在设想我们有一个网上商城，并给我们的所有商品打五折。

为了完成这个例子，我们需要完成的是
* 使用Vue.filter()构造器创建一个过滤器叫做discount
* 输入商品的原价，能够返回其打五折之后的折扣价


```js
  Vue.filter( 'discount' , function(value) {
        return value  * .5 ;
  });

  new Vue({
        el : 'body',
        data : {
              products : [
                    {name: 'microphone', price: 25, category: 'electronics'},
                    {name: 'laptop case', price: 15, category: 'accessories'},
                    {name: 'screen cleaner', price: 17, category: 'accessories'},
                    {name: 'laptop charger', price: 70, category: 'electronics'},
                    {name: 'mouse', price: 40, category: 'electronics'},
                    {name: 'earphones', price: 20, category: 'electronics'},
                    {name: 'monitor', price: 120, category: 'electronics'}
              ]
        }
  });
```

现在就可以像使用Vue自带的过滤器一样使用自定义过滤器了


```js
<ul>
      <li v-for="product in products">
           {{ product.name | capitalize }} - {{ product.price | discount | currency }}
      </li>
</ul>
```


上面的html代码将会输出打了五折之后的所有商品的清单列表。

那如果我们想要的是任意折扣呢？我们应该在discount过滤器里增加一个折扣数值参数，改造一下我们的过滤器。

```js
  Vue.filter( 'discount' , function(value,discount) {
        return value  * ( discount / 100 ) ;
  });
```

然后重新调用我们的过滤器

```js
<ul>
      <li v-for="product in products">
           {{ product.name | capitalize }} - {{ product.price | discount 25 | currency }}
      </li>
</ul>
```

我们也可以在我们Vue实例里构造我们的过滤器，这样构造的好处是，这样就不会影响到其他不需要用到这个过滤器的Vue实例。


```js
  new Vue({
        el : 'body',
        data : {
              products : [
                    {name: 'microphone', price: 25, category: 'electronics'},
                    {name: 'laptop case', price: 15, category: 'accessories'},
                    {name: 'screen cleaner', price: 17, category: 'accessories'},
                    {name: 'laptop charger', price: 70, category: 'electronics'},
                    {name: 'mouse', price: 40, category: 'electronics'},
                    {name: 'earphones', price: 20, category: 'electronics'},
                    {name: 'monitor', price: 120, category: 'electronics'}
              ]
        },
        filters: {
              discount : function(value, discount){
                    return value * ( discount / 100 );
              }
        }
  });
```

定义在全局就能在所有的实例中调用过滤器，如果定义在了实例里就在实例里调用过滤器。

###  结束语
多亏了简洁的管道过滤器构造器，我们不仅可以调用它原生的过滤器，也可以自定义属于我们自己的过滤器。


也可以在SegmentFault上关注文章的更新哦~

[第一章《从零开始学Vue》](https://segmentfault.com/a/1190000005041030)

[第二章《纪念即将逝去的Vue过滤器》](https://segmentfault.com/a/1190000005027001)

[第三章《组件改变生活_揭开Vue组件的神秘面纱》](https://segmentfault.com/a/1190000005045219)
