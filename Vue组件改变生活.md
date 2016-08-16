# Close2Vue

> This is a tutorial to vue‘s beginner.

> 项目取了Close to Vue的谐音(∩ᵒ̴̶̷̤⌔ᵒ̴̶̷̤∩)

> 内容如题，能够让你更靠近Vue ~


[第一章《从零开始学Vue》](https://github.com/AppianZ/Close2Vue/blob/master/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%AD%A6Vue.md)

[第二章《纪念即将逝去的Vue过滤器》](https://github.com/AppianZ/Close2Vue/blob/master/%E7%BA%AA%E5%BF%B5%E5%8D%B3%E5%B0%86%E9%80%9D%E5%8E%BBVue%E8%BF%87%E6%BB%A4%E5%99%A8.md)

[第三章《组件改变生活_揭开Vue组件的神秘面纱》](https://github.com/AppianZ/Close2Vue/blob/master/Vue%E7%BB%84%E4%BB%B6%E6%94%B9%E5%8F%98%E7%94%9F%E6%B4%BB.md)



## 第三章《组件改变生活_揭开Vue组件的神秘面纱》

在这一节里，我们将会了解到Vue的组件，理解组件是如何工作的，并利用一系列
的例子证明，用组件化的思想开发项目，会给你带来不一样感受。如果我们理解了Vue的组件化思想，我们就可以利用这个思想构造一个简化的评论投票系统，一个用户可以发布评论，其他用户可以在任意的评论上面投“赞成票”或者投“反对票”。

如果你是第一次接触Vue的话，你可以看看我之前的文章，[《从零开始学Vue》](https://github.com/AppianZ/Close2Vue/blob/master/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%AD%A6Vue.md)，了解Vue的基本语法。


### 理解组件
利用组件能够很好的把一个你正在构建的具有复杂接口的应用拆分开来，同时，组件也具有很高的复用性，即使是在你正在开发的是不同的项目也能封装复用。

先创建一个简单的html页面，并将Vue实例化后挂载在我们的DOM元素上。

 ```js
 <!DOCTYPE html>
       <html>
             <head>
                   <title>揭开Vue组件的神秘面纱</title>
             </head>
       <body>
             //这中间就是实例挂载点的实例边界
             <div id="vueInstance"></div>

             //Vue的CDN，之后会省略不写
             <script src="http://cdn.jsdelivr.net/vue/1.0.16/vue.js"></script>

             <script>
                   // 创建一个新的Vue实例，并设置挂载点
                   var V = new Vue({
                         el : '#vueInstance'
                   });
             </script>
       </body>
 </html>
 ```

现在我们已经把在[《从零开始学Vue》](https://github.com/AppianZ/Close2Vue/blob/master/%E4%BB%8E%E9%9B%B6%E5%BC%80%E5%A7%8B%E5%AD%A6Vue.md)的基础都准备好了，然后我们将创建我们的第一个简单的，可复用的组件。在Vue中你，可以使用Vue.component()来创建和注册你的组件，这个构造器有两个参数：
> * 组件的名字
> * 包含组件参数的对象

这个对象有点像Vue()构造器里的对象，它也有类似于Vue()里的el属性和data属性，但是又有点不一样。

Vue()构造器的el和data可以是对象。
Vue.component()构造器的el和data只能是函数。

现在来看看第一个组件是如何运作的。我想要注册一个组件，用p标签输出一行我的自我介绍。所以我创建了一个组件，并填入两个参数：
> * 组件的名字:'mine'
> * 包含组件参数的对象:这个对象包含一个属性'template'

```js
Vue.component('mine',{
    template : '<p>My name is Appian.</p>'
})
```

现在你已经有了自己的一个组件了，你可以在你的应用的任何地方使用它。只要你调用它的唯一标识(就是组件名字)，并用普通html标签的格式来书写，比如<mine></mine>，组件上注册的内容就会在你的自定义标签的地方插入。

```js
 <div id="vueInstance">
    <mine></mine>   //标识注册的内容会在这里插入
    <mine></mine>
    <mine></mine>   //重复插入注册内容
 </div>

 <script>
       Vue.component('mine',{   //这里就是注册的内容
           template : '<p>My name is Appian.</p>'
       });

       var V = new Vue({
             el : '#vueInstance'
       });
 </script>
```

Vue使用模板Template来代替组件，并使自定义的唯一标识用html标签插入到DOM结构中去，使得html更加简洁、整齐和直观。

### 利用template标签处理复杂组件
现在你可能会想，我写的组件怎么可能只有一行p标签？一行p标签还有必要组件这么麻烦吗？是的，组件是为了更复杂的封装复用而生的。所以，如果你只会Vue.component()构造器中的template属性定义html代码，利用字符串拼接拼出所有的代码，这样只会让你比用jq更加疲惫不堪。

为了避免上面的这种情况，所以我们可以用template标签（注意属性和标签是不一样的）来达到我们的目的。我们可以在页面的任何地方，定义template标签，并在template标签内，写好我们的模板。因为template标签在页面加载的时候不会渲染出来，只有在我们需要它的时候，这个标签内的内容才会被渲染出来。所以，你可以把template标签放在任何地方，并给它一个id，以便在组件注册的时候能够找到模板。

```js
 <div id="vueInstance">
    <mine></mine>   //标识注册的内容会在这里插入
 </div>

 <template id="mineTpl">
         <p>My name is Appian.</p>
         <button>点击没有任何事件</button>
 </template>

 <script>
       Vue.component('mine',{   //这里就是注册的内容
           template : '#mineTpl'
       });

       var V = new Vue({
             el : '#vueInstance'
       });
 </script>
```

我们现在已经可以利用这样的方法创建一个复杂的组件了。这样我们可能将复杂的代码进行功能分区之后组件化，用组件的思想避免代码一坨一坨的。组件化能够帮你更清晰的组织好你的模块，使你的组件更加vue化。

### 通过props向组件中传递数据
每次创建组件实例的时候，这个实例都划分了自己的组件范围，这个范围导致了在这个组件区域内无法获得其父组件的数据。所以，Vue是如何处理父组件向子组件中传递数据的呢？答案是，通过props。

先看一个最简单，从父组件向子组件中传递data的例子。注册的mine组件是子组件，他希望从父组件那里得到‘city’这个数据的信息，所以在mine的构造器里增加了一个参数props，用来接收父组件传递过来的city的值。

```js
Vue.component('mine',{
    template : '<p>Appian is from {{ city }}.</p>',
    props : ['city']
});
```

上面的例子中，我们定义了props作为一个数组，所以props可以用来接收多个字段，而这些字段就是子组件期望从父组件那里得到的。

props不一定要是数组，也可以是对象。可以在对象中详细的定义很多props的限制条件。

```js
Vue.component('mine',{
    template : '<p>Appian is from {{ city }}.</p>',
    props : {
        city : {
            type : String,//定义字符串类型
            required : true,//该字段是必须的
            default : 'China'//设置默认值
        }
    }
});
```
> 我们不需要每次都将props的限制条件都写出来，因为在这个例子中的数据是一个很简单的字段，上例中只是为了完整展示props的对象表示方法，所以才展开来写的。

那父组件那里又是怎么指派字段给子组件的呢？

```js
    <mine city="FuJian-YongAn"></mine>
```

这样直接在mine标签里面定义‘city’字段，就是父组件指派字段的方式之一。但是这样直接指派是很瞎的，我们需要的是动态变化的city，这个一会我们会说到的。

先简单介绍一下我们接下来要做的事。我们要假装我们正在搭一个博客，博客需要展示作者的基本信息，此时，我们可能会需要一些数据对象，可能是从数据库获得的，或者ajax请求到的，总之就是请求到了之后，将这个数据对象定义在父级的data中。并在html的template标签中准备渲染。

```js
<div id="vueInstance">
    <mine></mine> //标识注册的内容会在这里插入，之后也要展开来说！！！
</div>

<template id="mineTpl">
     <h1>{{ name }}</h1>
     <h2>{{ title }}</h2>
     <h3>{{ city }}</h3>
     <p>{{ content }}</p>
</template>

<script>
    //Vue.component()的构造在下文中展开来说！！！

    var V = new Vue({
         el : '#vueInstance',
         data : {
            name : 'Appian',
            title : 'This is a title',
            city : 'FuJian-XiaMen',
            content : 'There are some desc about Appians Blog'
         }
    });
</script>
```

准备好模板渲染之后，当然也要注册mine的构造器Vue.component()。除了基本的绑定模板id（#mineTpl）之外，还需要指定这个子组件想要的数据的字段名，并把期望得到的4个字段写在props中。

```js
Vue.component('mine',{
   template : '#mineTpl',
   props : ['name','title','city','content']
});
```

这样我们就能告诉父级，子组件需要的字段是哪些。接下来父级就可以指派字段的值给子组件了。前面的city字段是写死的，现在这里绑定的字段就是动态绑定的。

```js
<mine :name='name'
      :title='title'
      :city='city'
      :content='content'
  ></mine>
```

> 等号左右两边的字段名称可以不一样。
> 等号左边的字段名，是指在子组件的props中声明的名字。在html写成肉串式，但是在props中写成驼峰式。
> 等号右边的字段名，是指在父级里定义的字段的名字。

> ‘:’是‘v-bind’的缩写

这样就能把父组件的4个字段绑定到子组件上。现在，只要父组件指派的字段的值一发生改变，子组件的值也会发生相应的改变。

### 构建一个简易评论社区系统
现在就可以利用前面学到的内容，搭建一个简易的评论社区系统，样式什么的先不管，只讲究js的具体实现。

* 创建一个新的Vue实例
* 给实例挂载一个div（#vueInstance）
* 定义数据，然后渲染。

```js
<div id="vueInstance">
    <div class="container">
        <ul>
           //这里即将渲染出评论的投票列表
        </ul>
    </div>
</div>

<template id="postTpl">
     <li>
         <i class="up">我支持</i>
         <span>票数： {{ post.votes }}</span>
         <i class="down">我反对</i>
         <a>话题： {{ post.title }}</a>
       </li>
</template>


<script>
    //Vue.component()的构造在下文中展开来说！！！

    var V = new Vue({
         el : '#vueInstance',
         data : {
             posts: [{
                title: '请大大多多为我投票，我给大家发红包',
                votes: 15
             },{
                title: '投我准没错',
                votes: 53
             },{
                title: '不要投给我楼上的',
                votes: 10
             }]
         }
    });
</script>
```

现在先构造好数据，有投票的话题和投票人数。然后再构造好模板（template标签），这个模板值用来渲染单个话题。模板里有除了渲染话题和投票人数，还有两个按钮，用来投赞成票或者反对票。之后我们只需要在html循环模板，然后就能多次插入模板，渲染成列表。当然也可以在模板里渲染好列表再一次插入。我们先用前一种方法。

不管css的样式问题，现在就可以开始注册Vue.component()构造器了，以便我们渲染页面。我们在注册的时候要明确，我注册的子组件需要的props是父级的posts数组中的一个元素对象post。所以我们应该这样注册：

```js
Vue.component('post',{
    template : '#postTpl',
    props : ['post']
});
```

然后我们使用自定义的<post>标签插入在html中去。

```js
<div id="vueInstance">
    <div class="container">
        <ul>
           <post v-for="post in posts" :post="post"></post>
        </ul>
    </div>
</div>
```

这样我们就已经完成了循环输出posts数组了。因为我们把数组的元素指派给了子组件，所以子组件就可以渲染title和vote。接下来要做的就是增加“赞成”和“反对”按钮的逻辑代码，并我们需要对投票状态进行锁定，就是如果我们投了某个话题的赞成票就不能再投该话题的反对票，反之亦然。
所以让我们开始在模板里面定义点击事件吧，投赞成票的事件叫做“upvote”，投反对票的事件叫做“downvote”。

```js
<template id="postTpl">
     <li>
         <i class="up" @click="upvote">我支持</i>
         <span>票数： {{ post.votes }}</span>
         <i class="down" @click="downvote">我反对</i>
         <a>话题： {{ post.title }}</a>
       </li>
</template>
```

```js
Vue.component('post',{
    template : '#postTpl',
    props : ['post'],
    data : function (){
        return {  //data必须为function，定义投票状态
            upvoted: false,
            downvoted: false
        }
    },
    methods : {
        upvote : function() { //点击赞成的事件
            this.upvoted = !this.upvoted;
            this.downvoted = false;
        },
        downvote: function() { //点击反对的事件
            this.downvoted = !this.downvoted;
            this.upvoted = false;
        }
    }
});
```

> 注意，data里面的两个布尔值的定义是作为一个函数的返回值定义的。那是因为我们想要给每一个组件都设置一个是否投票的状态。
>（还记得之前我说过，这次的列表渲染是循环渲染多个模板，每个模板都只是一个话题的相关信息，而现在在组件中定义的data：upvoted和downvoted，也是专属于某个模板的。）
> 这样我们如果投了某个话题的赞成票，并不会影响剩下话题的投票情况。

接下来已经完成了点击事件的状态控制，那么点击“赞成”和“反对”会导致话题的票数发生变化。这个时候我们可以利用之前的教程中学过的computed属性进行计算。
> 组件也有computed属性哦~

```js
Vue.component('post',{
    template : '#postTpl',
    props : ['post'],
    data : function (){ //同上，略 },
    methods : {  //同上，略 },
    computed: {  //重点部分
        votes: function () {
            if (this.upvoted) {
                return this.post.votes + 1;
            } else if (this.downvoted) {
                return this.post.votes - 1;
            } else {
                return this.post.votes;
            }
        }
    }
});
```

到此为止，应该组件中的逻辑处理差不多完成了，现在要做的就是让我们在组件中修改的投票人数，能够在模板中渲染出来。因为我们现在的votes的值是在子组件中修改的，而一开始渲染的时候，我们的votes只是一味的接受父级传过来的值，所以，现在要把修改的值显示在模板上。
所以模板变成了这样:
```js
<template id="postTpl">
     <li>
         <i class="up" @click="upvote">我支持</i>
         <span>票数： {{ votes }}</span>
         <i class="down" @click="downvote">我反对</i>
         <a>话题： {{ post.title }}</a>
       </li>
</template>
```

现在我们的投票系统已经基本完成了。我们可能会希望我们的投票系统好看一点，直观一点。所以我们可以在按钮上绑定一些样式。比如当用户已经投了某个话题的赞成票或者反对票，就让这个按钮变成橙色。

```js
.disabled {
    color : orange;
}
```

Vue是利用v-bind:class来进行样式绑定的，可以简写成一个‘:’。其绑定的内容是一个对象，对象里面是class的名字和class对应的状态。
[了解更多样式绑定的信息点这里](http://cn.vuejs.org/guide/class-and-style.html)

```js
<template id="postTpl">
     <li>
         <i class="up" @click="upvote" :class="{disabled: upvoted}">我支持</i>
         <span>票数： {{ votes }}</span>
         <i class="down" @click="downvote" :class="{disabled: downvoted}">我反对</i>
         <a>话题： {{ post.title }}</a>
       </li>
</template>
```

如果我们点击了某个话题的赞成按钮，则它的upvoted就会变成true，则disabled的样式就绑定到赞成按钮上去。如果点了该话题的反对按钮，则它的upvoted就会变成false，则disabled就不会绑定到赞成按钮上。以上就是模板最终的样子。

### 组件的复用性的利用
我们已经利用了我们对Vue的基本语法还有一些组件的基本操作，建立起了一个基本的话题投票系统。在教程的最后一部分，我们将看看，我们应该如何将这个投票系统组件进行复用。

复用的关键在于组件的命名，让你的命名能够复用。至少你在构建你的组件的时候，你要问自己，“是否其他地方也能用到这个组件？”

比如，在上面的例子中，posts的意思是，这个数据是从ajax的post请求过来的数据，为了便于理解，才取名为posts，为了让自己的组件名字一看就知道关联，所以把模板的id定义为#postTpl，tpl是template的简写，自定义标签的命名也是post。总之，就是这样的细节，不仅方便自己阅读，也方便其他人阅读你的代码。

现在我们的评论系统需要增加一个功能，就是增加一个发布评论的功能。就是增加了一个发布评论的区域（#commentBox）。

```js
<div id="vueInstance">
    <div class="container">
        <ul>
           <post v-for="post in posts" :post="post"></post>
        </ul>

        <div id="commentBox">
            请输入评论内容并提交：
            <input type="text" v-model="comment" @keyup.enter="postComment">
            <button @click="postComment">提交评论</button>
        </div>
    </div>
</div>

//模板渲染不变

<script>
//Vue.component()的注册不变。

var V = new Vue({
     el : '#vueInstance',
     data : {
         posts: [{
            title: '请大大多多为我投票，我给大家发红包',
            votes: 15
         },{
            title: '投我准没错',
            votes: 53
         },{
            title: '不要投给我楼上的',
            votes: 10
         }],
          comment: ''
     },
     methods: {
         postComment: function() {
             this.posts.push({
               title: this.comment,
               votes: 0
             })
             this.comment = '';
         }
     }
});
</script>
```

输入框用v-model绑定了comment字段，为了无论用户输入什么，在提交的时候都能获得他输入的值。当用户按回车或者点击提交按钮的时候都会触发postComment方法。这里的事件绑定一个是用的回车事件 @keyup.enter，还有一个点击事件@click。postComment方法就是把话题和投票为0的对象push进posts数组中去，Vue会将新的模板自动渲染出来。

这样一个可复用的组件就构造完成了，组件化会节省开发者的很多时间。组件是否复用，也要结合开发的实际需求而定。


### 后记
到此为止，你就已经能够掌握了Vue的组件的基本使用，组件通过props向下传递数据，通过使用<template>标签来构造模板。我们在上文中利用构建了一个评论发布投票系统来说明了组件的用法，并且简单介绍了如何复用。

> * 利用 props，子组件可以定义需要父级传递的字段
> * 利用 'v-bind:' 或 ':' 在自定义标签的地方绑定父级指派的字段
> * 组件的 data 和 el 必须定义为function
> * 不要忘记，还是需要将Vue实例化，才能使用组件。

也可以在SegmentFault上关注文章的更新哦~

[第一章《从零开始学Vue》](https://segmentfault.com/a/1190000005041030)

[第二章《纪念即将逝去的Vue过滤器》](https://segmentfault.com/a/1190000005027001)

[第三章《组件改变生活_揭开Vue组件的神秘面纱》](https://segmentfault.com/a/1190000005045219)
