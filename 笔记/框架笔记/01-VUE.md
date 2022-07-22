# VUE理论基础

定义：VUE是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。采用的是。采用MVVM模式。

核心库和插件：视图层渲染、组件、路由（vue-router）、状态管理（vuex）

## MVVM模式

Model-View-ViewModel

<img src="..\image\Vue_MVVM模型.png" alt="Vue_MVVM模型" style="zoom: 50%;" />

代码中 层 的体现，见实例创建

> View 视图层：html中的标签（视觉上的东西）
>
> Model 数据层：Vue对象中的管理数据的属性（即data属性）
>
> viewModel 管理者：指Vue对象

------

## 引入方式

### vue2引入

> 1、直接CDN引入
>
> 方法：
>
> ```html
> <!--开发环境版本，包含了有帮助命令行和警告-->
> <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
> 
> <!--生产环境版本，删除了警告，优化了大小和速度-->
> <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
> ```

> 2、下载引入
>
>  方法：
>
> 开发环境版本，包含了有帮助命令行和警告： https://cdn.jsdelivr.net/npm/vue/dist/vue.js
>
> 生产环境版本，删除了警告，优化了大小和速度： https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.esm.browser.js

> 3、NPM安装
>
> 方法：使用vue-cli安装

### vue3引入

CDN

```html
<script src="https://unpkg.com/vue@next"></script>
```

------

## 实例创建

### Vue2创建

```html
<body>
    <!-- View 视图层 -->
    <div id="app">{{msg}}</div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        //viewModel 管理者
        let vm = new Vue({
            el: '#app', 		//elem元素，用于绑定dom(TagName、id和class都可以)
            //也可以是   el: document.querySelect('#app')
            name: 'one',		//给这个vm实例命名（可以是中文）
            //Model 数据层
            data: { 			//提供数据
                msg: 'hello Vue2'
            }
        });
        console.log(vm);
    </script>
</body>
```

或

```js
let vm = new Vue({});
vm.$mount('#app');
```



#### el

> 作用：指向View层，用于绑定页面元素的
>
> 语法：`el: 'TagName/id/class/document.querySelector('xx')'`
>
> 注：
>
> > 1、可以绑定id和class、类名等
> >
> > 2、只能绑定一个元素，若多个元素都是一个class，则只绑定第一个（解决：用一个大盒子将其包裹起来）
> >
> > 3、el只能在实例中使用（在创建构造器的时候，使用el会报错；因此el只能在new出来的实例中使用）
> >
> > 4、绑定元素的其他方法：
> >
> > ```js
> > let vm = new Vue({}).$mount('#app');
> > ```

#### data

> 作用：指向Model 数据层
>
> 注：在Vue2中，data是一个对象；但是在Vue3中，是一个函数，return 一个对象
>

### Vue3创建

```html
<body>
    <div id="app">{{msg}}</div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        let vm = Vue.createApp({
            data(){
                return {
                    msg: "hello Vue3"
                }
            }
        });
        let app = vm.mount('#app');	//挂载到根组件（元素）上
        console.log(vm);
    </script>
</body>
```

注：在Vue3中，data是一个组件，起必须是函数，必须带return（原因：组件是可复用的）（返回的对象都是独立的，避免了数据污染）

------

## 响应式原理

> Vue2：es5中的  Object.defineProperty
>
> Vue3：es6中的  proxy

------

## 插值表达式

> 定义：直接在html标签内放入变量名
>
> 语法：`{{name}}`
>
> 参数：name：变量名
>
> 注：每一个大括号里面只能包含一个表达式或变量（即不能在两个变量中加空格，但是可以用+拼接）
>
> 如：
>
> ```html
> <h2>{{msg + num}}</h2>		拼接输出
> <h3>{{arr[1]}}</h3>			输出数组
> <h4>{{arr.length}}</h4>
> <h5>{{obj.name}}</h5>
> <h6>{{三元表达式}}</h6>
> ```
>
> 注：
>
> > 1、null和undefined都不会输出
> >
> > 2、只支持表达式，不支持其他语句，如 `{{if(1) return msg}}`
> >
> > 3、优缺点：优点是常用，缺点是存在闪烁问题（可以用v-clock解决）

------

# Vue基本指令

## 通常指令

指带有v-属性的特殊属性。

在Vue给HTML元素增加了自定义属性，它们都是以 “v-” 开头了（原因：表明这是一个vue指令）。并且一些特殊的指令可以带参数和修饰符指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

三种将数据渲染到页面的方法：插值运算符、v-text、v-html

### v-text

> 语法：`<div v-text = 'msg'></div>`
>
> 参数：msg：一个变量，一般是vue实例中的属性
>
> 注：
>
> > 1、多个变量不能空格隔开，而是用+拼接
> >
> > 2、拼接字符串应该注意引号
> >
> > 3、在v-text标签中不能添加内容，会被覆盖（v-text的内容会覆盖标签原有内容）
>
> 优缺点：
>
> > 优点：无闪烁
> >
> > 缺点：覆盖标签原内容；无法渲染HTML格式

### v-html

类似innerHTML，即将字符串当成HTML渲染

> 语法：`<div v-text = 'msg'></div>		msg ='<p>我是HTML标签</p>'`	
>
> 参数：msg：一个变量，一般是vue实例中的属性
>
> 注：
>
> > 1、可以拼接字符串
> >
> > 2、可以使用三元运算符
>
> 优缺点：
>
> > 优点：无闪烁；可渲染
> >
> > 缺点：覆盖标签原内容

### v-clock

> 语法：
>
> ```html
> <style>
> 	[v-clock]{
> 		display
> 	}
> </style>
> <body>
> 	<div v-clock>{{msg}}</div>		//（不需要表达式，即无需 = ）
> </body>
> ```
>
> 作用：解决插值表达式闪烁问题

### v-once

> 语法：`<p v-once>{{msg}}</p>`
>
> 作用：只在初始化的时候插入一次值，当数据发生变化时，**不再发生更新**

### v-pre

> 作用：不编译指令，即让插值表达式等不执行

### v-model

> 语法：`<p v-modele = 'msg'></p>`
>
> 作用：针对表单元素和组件，实现双向绑定数据
>
> 针对表单元素的用法：
>
> ```html
> <div class="app">
>     <input v-model="msg"></input>
>     <p>{{msg}}</p>
> </div>
> <script>
>     let vm = new Vue({
>         data: {
>             msg: 'hello Vue'
>         }
>     }).$mount('.app')
> </script>
> ```
>
> v-model可以解决input标签中单选按钮必须加name的问题
>
> ```html
> <div class="app">
>    	<input type="radio" v-model="sex" value="男">男
>    	<input type="radio" v-model="sex" value="女">女
>    	<p>{{sex}}</p>
> </div>
> <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
> <script>
>    	let vm = new Vue({
>            data: {
>            msg: 'hello Vue',
>            sex: '男'			//单选按钮会默认选择value和sex匹配的按钮（如果input不给value，则差值表达式会显示null）
>    		}
>    	}).$mount('.app')
> </script>
> ```

#### 修饰符

修饰符可以连续调用，如`<p v-modele.lazy.trim = 'msg'></p>`

##### lazy

> 语法：`<p v-modele.lazy = 'msg'></p>`
>
> 作用：不需要实例和标签实时发生改变时，需要加入lazy修饰符
> 即：当输入框内容变化时，vue实例的属性不发生变化；只有 敲回车 或 失去焦点 时，才更新实例

##### number

> 语法：`<p v-modele.number = 'msg'></p>`
>
> 作用：将输入的值，转换为数值类型（含浮点数）
>
> 注：
>
> > 1、一开始如果输入的是数字，就算后面是字母，也只输出数字（如：“123abc45”，在实例中是123）。类似于Number()
> >
> > 2、一开始不输入数字，则输出字符串（如：“ab123”，实例中是“abc123”）

##### trim

> 语法：`<p v-modele.trim = 'msg'></p>`
>
> 作用：去除首尾空格

### v-show

> 语法：`<p v-show="判断表达式" = 'msg'>`
>
> 作用：通过判断，是否显示该内容。如果值为true，则显示。否则就隐藏
>
> 如：
>
> ```html
> <p v-show="flag" class="app">{{msg}}</p>	//显示与否与flag真假有关
> <script>
> 	let vm = new Vue({
> 	    data: {
> 	        flag: '1'
> 	    }
> 	}).$mount('.app')
> </script>
> ```
>
> 注：不占页面位置，但是占用dom节点位置
>
> 切换频繁就用v-show

### v-if 

> 语法：`<p v-if="判断表达式" = 'msg'>`
>
> 作用：判断是否加载固定的内容，如果为真，就加载；否则不加载（既看不到元素节点，也看不到内容）
>
> 注：
>
> > v-else：与v-if搭配使用，原理与if-else一样（v-else需要紧跟v-if 或 v-else-if）
> >
> > v-else-if：`v-if='x'	v-else-if='xx'	v-else-if='xxx'	v-else='xxxx'`

### v-for

> 语法：`<li v-for="item in arr">{{item}}</li>`
>  		  `<li v-for="(item, index) in arr">{{item}}</li>`
>
> 参数：
>
> > item：数组的每一项
> >
> > arr：原数组
> >
> > index：下标
> >
> > in：in 和 of 都可以，没有区别
>
> 对于对象来说：`<li v-for="(item, key, index) in obj">{{item}}</li>`			item指属性值，key是指对象的属性（/方法）名
>
> 作用：将一个数组遍历或枚举一个对象循环显示，需要结合着 in 或 of 来使用

#### :key

> 语法：`:key="xx"`
>
> 参数：xx：一般为number、string
>
> 目的：使标签与key的值绑定，实现高效的更新虚拟DOM
>
> 使用：官方推荐在使用v-for时，给对应的元素或组件添加上一个:key属性
>
> 注：key的值不易与下标index相同，因为index会随着数组变化而变化

> 如果不加key，那对于节点的增加或删除，只要触发节点操作，就执行一次；操作多次，执行多次
> 加了key，因为key与id一样，具有唯一性，因此对于节点的操作只有一次
>
> 即：无key，状态默认绑定的是位置（index 下标）
> 有key，状态根据key值绑定的内容（建议用id绑定，以达到唯一性）

### v-bind

v-bind和v-on都是事件指令

> 语法：`v-bind:属性名= "常量||变量名"`		（v-bind可以省略）
>
> 作用：动态地绑定一个或多个属性，或者一个组件prop到表达式
>
> 注：v-bind必须跟JS表达式

错误写法：`v-bind:src="{{xxx}}"`

#### 对类名的控制 :class

> 语法：`<p :class=['one', 'two']>`			||			`<p :class=['one', {'two': flag}]>`	（表示flag控制two这个类是否生效）
> 			`<p :class=[myclass: true]>`		（myclass是一个变量， : true 表示生效该class）	
>
> 作用：实现JS对类名的添加和删除

#### 对css的增删 :style

与JS对CSS的控制类似

> 语法：`<p :style="{color: 'red', fontSize: '50px'}">`
>
> 如：
>
> ```html
> <p id="app" :style="[one,two]">数组样式</p>
> <script>
>     let vm = new Vue({
>         data: {
>             one: {
>                 color: 'deeppink'
>             },
>             two: {
>                 fontSize: '50px'
>             }
>         }
>     }).$mount('#app')
> </script>
> ```
>
> 注：**驼峰命名**	跟对象时，对象属性值必须是字符串

### v-on

> 语法：`v-on：事件名=“方法名”`		||		`@ 事件名=“方法名”`
>
> 作用：绑定一个事件或多个事件，事件函数的参数名代表了在方法对象methods中所定义的函数

如：

```html
<div id="app">
    <p>{{num}}</p>
    <button v-on:click="num++">自增</button>
    <button @click="Print()">打印</button>
    //没有传参的情况下，可以不写冒号那种
    <button @click="Print">log</button>
</div>
<script>
    let vm = new Vue({
        data: {
            num: 0
        },
        methods: {
            Print: function() {
                console.log(this);
                console.log(this.num);
            },
        	doSomething(){
                
            }
        }
    }).$mount('#app')
</script>
```

#### v-on 传参

> 1、不加括号绑定事件，第一个形参默认为事件对象（如点击事件等）
>
> 2、加了括号，如需要得到事件对象，可以使用 **$event**
>
> ```html
> <div id="app">
>     <button @click="Do">不加括号，默认获得事件对象</button>
>     <button @click="Print('wz',$event)">手动获得事件对象</button>
> </div>
> <script>
>     let vm = new Vue({
>         data: {
>             num: 0
>         },
>         methods: {
>             Do(e) {
>                 console.log(e);
>             },
>             Print(str, event) {
>                 console.log('str:', str);
>                 console.log(event);
>             },
>         }
>     }).$mount('#app')
> </script>
> ```

#### 绑定多个事件

```html
<div id="app">
    <button v-on="{click:one, dblclick:two}">绑定多个事件</button>			//注意双击是 dblclick
</div>
<script>
    let vm = new Vue({
        data: {
            num: 0
        },
        methods: {
            one() {
                console.log('单击');
            },
            two() {
                console.log('双击');
            },
        }
    }).$mount('#app')
</script>
```

------

## 自定义指令

定义指令尽量不用大写字母

### 全局指令

> 语法：`Vue.directive(id, definition)`
>
> 参数：
>
> > id：指令名称（一般自定义）		id在节点元素中，需要绑定才能生效（即：`<div v-id> xxx </div>`，其中id为直接取的名字，如v-focus等）
> >
> > definition：1、定义对象，其属性一般是提供的钩子函数（如bind、inserted等）
> > 				  2、定义函数，**对应钩子函数bind和update**
>
> 注：全局指令需要写在vue实例之前

如：加载完页面光标自动在第二个输入框

```html
<div id="app">
    姓名：<input type="text"> <br> 特长：<input type="text" v-focus>
</div>
<script>
    Vue.directive('focus', {
        inserted: (el) => {
            el.focus();
        }
    })
    let vm = new Vue({
        el: '#app',
        data: {}
    })
</script>
```

### 局部指令

> 语法：在Vue实例中加入directives属性	`directives:{ id: definition }`		（当definition为函数时， “ : ” 和 function 可以一起省略）
>
> 作用域：只作用在绑定的标签中

如：转化大写

```html
<div id="app">
    <h3 id="app" v-upper-text="msg"></h3>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            msg: 'i am a message'
        },
        directives: {
            'upper-text': (el, binding) => {
                el.innerHTML = binding.value.toUpperCase();
            }
        }
    })
</script>
```

### 过滤器

> 作用：自定义一个过滤器，最终实现文本转化、数据类型转变等
>
> 作用地方：双花括号插值	和	v-bind表达式
>
> 过滤器包含：
>
> > 全局过滤器：Vue.filter
> >
> > 局部过滤器：filters{}
>
> 注：利用methods方法也可以代替过滤器，但其语义化不明确（所有事情需要方法来处理），耦合性高

全局过滤器

> 语法：`Vue.filter(id, function)`
>
> 使用：`<div> {{ xxx | id }} </div>`

局部过滤器

> 语法：在Vue实例中使用，同局部指令

如：

```html
<div id="app">
    <h3 id="app">猪肉价格：{{price | formatPrice("￥/kg")}}</h3>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            price: 158
        },
        filters: {
            formatPrice(price, unit) {
                return (price / 10).toFixed(2) + unit;
            }
        }
    })
</script>
```

注：

> 1、过滤器的函数中，必须return，否则输出undefined
>
> 2、过滤器是使用在插值表达式中，用 | 和变量隔开	（ | 叫做管道符）
>
> 3、过滤器函数可以传参，在标签中，变量默认为第一个参数，括号内跟的参数为形参第二个（详见例子的 unit 形参）
> 即：如果只需要处理一个参数，则函数不需要加括号
>
> 4、多个过滤器函数，可以用多个 | 隔开（如：`<div> {{ msg | fn1 | fn2 }} </div>`）
> 注意后面的fn2可能会覆盖fn1

------

## 修饰符

### 通用修饰符

通用修饰符：这些修饰符是所有事件都通用的

| **修饰符** | **描述**                                      |
| ---------- | --------------------------------------------- |
| .stop      | 阻止事件冒泡（调用 event.stopPropagation()）  |
| .prevent   | 阻止默认事件（调用 event.preventDefault()。） |
| .capture   | 添加事件捕获                                  |
| .self      | 只能由自身触发事件                            |
| .once      | 该事件只会触发一次                            |

### 按键修饰符

按键修饰符：这些修饰符是便捷一些按键操作的，比如要判断按键的键值的时候

| **修饰符** | **描述**     |
| ---------- | ------------ |
| .enter     | 回车         |
| .tab       | tab键        |
| .delete    | 删除”和“退格 |
| .esc       |              |
| .space     |              |
| .up        | ↑            |
| .down      | ↓            |
| .left      | ←            |
| .right     | →            |

如：

```html
<input @keyup.enter="Print($event)" id='app'>			//只有当键盘的enter抬起时才出发Print函数
<script>
    let vm = new Vue({
        data: {
        },
        methods: {
            Print(e) {
                console.log(e);					
                console.log(e.keyCode);
            },
        }
    }).$mount('#app')
</scripts>
```

------

# Vue的实例方法

## 数据

### Vue.set()

通过下标索引到的数组，并更改时，Vue实例得到更改，但是视图层并不发生改变

解决：使用replace方法更改数组	或	使用Vue.set()

> 语法：`Vue.set(target,index,value)`	（全局起作用）	||		`vm.$set(target,index,value)`		（只有vm实例起作用）
>
> 参数：
>
> > target：目标（需要更改的对象，一般为数组或对象）
> >
> > index：下标
> >
> > value：更改后的内容

如：

```js
let vm = new Vue({
    data: {
        arr = [1, 'a', 2]
    },
    methods: {
        Do() {
            this.$set(this.arr, 1, 'b');			//结果：arr = [1, 'b', 2]
        },
    }
}).$mount('#app')
```

------

# 钩子函数

## 生命周期

生命周期的定义：每个 Vue 实例在被创建时都要经过一系列的初始化过程，例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

![Vue_生命周期](E:\笔记\image\Vue_生命周期.png)

```js
let vm = new Vue({
	el: '',
    data: {},
    methdos: {},
    beforeCreate() {},
    created() {},
    beforeMount() {},
    mounted() {},
    beforeUpdate() {},
    updated() {},
    beforeDestroy() {},
    destroyed() {}
})
```

### 生命周期的钩子函数

生命周期里也有8个钩子函数：`beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、beforeDestroy、destroyed`

#### beforeCreate

> 表示 实例在完全被创建之前会执行的函数，data、methods都没有初始化
>
> 因此在beforeMount里调用data和methods都会报错
>
> 语法：`beforeMount() {}`

#### created

> 表示 data、methods已经初始化好了
>
> 但是实例还没有挂载，因此不能获取dom元素（如$refs）
>
> 语法：`created() {}`

#### beforeMount

> 表示 挂载前，template已经在内存中编译好了，但还没有挂载到页面上
>
> 语法：`beforeMount() {}`

#### mounted

> 表示 实例已经脱离了创建阶段，进入到了运行阶段
>
> 一般操作dom，最早需要在mounted中进行
>
> 语法：`mounted() {}`

#### beforeUpdate

> 表示 当数据发生更新时，页面的数据还未改变，但是data中的数据改变了
>
> 语法：`beforeUpdate() {}`

#### updated

> 表示 虚拟dom已经是最新的缓存，即页面已发生更新
>
> 语法：`updated() {}`

#### beforeDestroy

> 表示 实例已经进入销毁阶段，data和methods还保留着（用于页面销毁时保存一些数据），但是dom只存在于缓存中，没法调用
> **！！！亲测中，所有数据和方法都没了！！！**
>
> 语法：`beforeDestroy() {}`

#### destroyed

> 表示 组件已经完全被销毁，所有数据和方法都不可用
>
> 语法：`destroyed() {}`

## 指令对象的钩子函数

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

| **函数**         | **描述**                                                     |
| ---------------- | ------------------------------------------------------------ |
| bind             | 只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。 |
| inserted         | 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。 |
| update           | 所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 。 |
| componentUpdated | 指令所在组件的 VNode 及其子 VNode 全部更新后调用。           |
| unbind           | 只调用一次，指令与元素解绑时调用。                           |

指令钩子函数会被传入以下参数（有顺序）：

| **函数** | **描述**                                                     |
| -------- | ------------------------------------------------------------ |
| el       | 指令所绑定的元素，可以用来直接操作 DOM。                     |
| binding  | 包含指令相关的对象，包含以下这些属性：name（id名）、rawName（v-id名）、modifiers（一个对象，其属性可以查看id的修饰符）、expression（该id绑定的内容，即 = 后面的内容）、value（ = 后面如果是变量，则value是变量的值）、oldValue、arg |
| vnode    | vue 编译生成的虚拟节点。                                     |
| oldVnode | 上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。 |

如：

```html
<body>
    <input type="text" id="ipt" v-focus.trim="msg">
</body>
<script>
    Vue.directive('focus', {
            inserted: (el, binding) => {
                console.log(binding);		
            }
        })
        let vm = new Vue({
            el: '#ipt',
            data: {
                msg: 'i am a message'
            }
        })
</script>
```

打印结果：					![Vue_钩子函数_binding](E:\笔记\image\Vue_钩子函数_binding.png)

------

# 计算属性和侦听器

## 计算属性

语法：`computed:{}`

和方法的区别：方法每次使用都会重新调用一遍；但是计算属性利用缓存机制，可以实现多个重复方法只执行一次，只有当结果发生变化时才重新执行

如：

```html
<div id="app">
    <p>{{total}}</p>
    <p>{{total}}</p>
    <p>{{total}}</p>
    <p>{{total}}</p>
</div>
<script>
    let vm = new Vue({
        el: '#app',
        data: {
            num: 27,
            price: 5
        },
        computed: {
            total() {
                console.log('我只执行一次');
                return this.num * this.price + '￥';
            }
        }
    })
</script>
```

结果：只打印一次 ‘我只执行一次’

注：total是一个属性，因此**在标签中不加括号**

> 计算属性默认有两个属性：getter和setter（getter默认都有，用于获取值，setter用于设置/修改值）
>
> 语法：
>
> ```js
> computed:{
> 	fullName:{
>         	get(){},
>         	set(val){}
>    	}
> }
> ```
>
> get：回调函数，必须跟return
>
> set：val这个形参是get返回的内容

## 侦听器

> 语法：`watch:{ 需要被侦听的数据 }`
>
> 作用：侦听指定数据变化，当发生变化时，就调用侦听器内的函数
>
> 如：
>
> ```js
> let vm = new Vue({
> 	el: '#app',
> 	data: {
> 	    num: 0
> 	},
> 	methods: {
> 	    add() { //数量+1
> 	        this.num++;
> 	    },
> 	    reduce() { //数量-1
> 	        this.num--;
> 	    }
> 	},
> 	watch: {	//侦听mun的变化
> 	    num(newVal, oldVal) {		//newVal：num变化后的值		odlVal：num变化前的值
> 	        doSomethings();
> 	    }
> 	}
> })
> 
> 或者
> vm.$watch('num', function(){ doSomethings(); }, {immediate:true} )	//immediate：立即侦听
> //最后一个{}表示选项对象，可以选择deep、immdiate等
> ```
>
> 注：侦听器和methods同级

### 监听引用类型

> 原因：因为引用类型的改变，指挥改变堆数据，指针不会变，因此普通侦听没有用
>
> 解决：deep

#### deep

> 语法：`deep: true`		默认为false
>
> 使用：
>
> ```js
> watch: {
>    	num: {
>    	    //handler：类似于基本类型的侦听的函数
>    	    handler(newVal, oldVal) {
>    	        doSomethings();
>    	    }
>    	},
> 	deep: true		//开启深度侦听
> }
> ```
>
> 作用：侦听对象的全部属性和方法
>
> **注**：如果不想只有当数据变化时才侦听，即想一开始就侦听，那么可以在deep属性前加 `immediate: true`

#### 打点侦听

> 语法：对象加引号并在后面加 `.属性名`
>
> 作用：侦听对象某一个属性
>
> 使用：
>
> ```js
> watch: {
>    	'num.totle': {
>    	    handler(newVal, oldVal) {
>    	        doSomethings();
>    	    }
>    	},
> }
> ```
>
> 

### 取消侦听器

Vue本身的$watch方法就是取消侦听器的方法，因此可以直接在$watch的回调函数中调用$watch

即：`let unwatch = vm.$watch('data', function() { unwatch() } )`

因此，监听的回调函数中，如果代码写在unwatch()之前，则表示监听一次后取消监听；写在unwatch()之后，则没有效果

------

# 组件

作用：组件可以扩展 HTML 元素，封装可重用的代码

## 组件命名

可以使用 	`		-		小驼峰		大驼峰`		但是在标签中，大写的字母必须使用 - 隔开，并化为小写（在Vue中可以不考虑，因为会隐式转换，但是Vue有 - 时，必须用引号包裹）

## 创建 extend

创建构造器，又称为拓展

> 定义：Vue构造器的拓展，调用Vue.extend创建的是一个组件构造器
>
> 语法：`Vue.extend({obj})`
>
> 参数：obj：对象，一般有template、data（注：**data此时必须为函数**，**且有return**，一般return一个对象）

创建构造器后，可以选择将其挂载到已有实例上，或者选择注册一个新的组件实例（注册见component）

如：

```js
let myextend = Vue.extend({
	template: `<div>xx
				  <p>xxx</p>
			  </div>`
});
new myextend().$mount('#app');		//将myextend构造器挂载到id为app的标签上
```

### template

> 使用原因：组件没有挂载点，因此需要先有个template模板
>
> 语法：`template: '<div></div>'`
>
> 使用地方：
>
> > 1、组件：
> >
> > <img src="..\image\Vue_组件_template.png" alt="image-20220719182242008" style="zoom:67%;" />
> >
> > 2、template标签里（模板为保持唯一性，必须使用id）：
> >
> > <img src="..\image\Vue_组件_template_2.png" alt="image-20220719182522762" style="zoom: 67%;" />
> >
> > 3、script标签里（一定要加`type="text/x-template"`，让浏览器知道此标签是html标签，而非js）
> > 不常用
> >
> > <img src="..\image\Vue_组件_template_3.png" alt="image-20220719183103658" style="zoom:67%;" />
>
> 注：
>
> > 1、template**只有一个根节点**，即不能存在多个父级元素；因此若有多个标签元素，需要彼此嵌套
> >
> > 2、标签元素中是可以绑定Vue指令的

## 注册 component

> 作用：将构造器的拓展 注册为组件实例（即将extend注册为一个Vue实例）
>
> 语法：`Vue.component(自定义标签, myextend)`
>
> 参数：
>
> > 自定义标签：自己可以定义的标签（注意不能有大写）
> >
> > myextend：组件构造器，可以直接用一个obj来代替extend创建这一过程（实际开发直接使用另外的vue文件）

如：

```html
<mycpn></mycpn>
<mycpn></mycpn>
<mycpn></mycpn>
//可以有多个自定义标签，且都会被注册为实例并绑定
<script>
	let myextend = Vue.extend({
		template: `<div>xx
					  <p>xxx</p>
				  </div>`
	});
	Vue.component('mycpn', myextend)
</script>
```

### 全局组件

> 定义：调用Vue.component()注册组件时，组件的注册是全局的，这意味着该组件可以在任意Vue示例下使用
>
> 作用域：只要是挂载了的标签，组件绑定后都能正常使用（即：满足Vue实例、实例中存在绑定的自定义标签）

如：

```html
<div id="app">
    <my-cpn></my-cpn>
</div>
<script>
	 Vue.component('my-cpn', { //选项对象
	    template: `
				<div>
					<h3 v-for="val in name">汤圆：{{val}}</h3>
				</div>
			`,
	    data: function() {
	        return {
	            name: ["黑芝麻", "水果味"]
	        }
	    }
	})
	new Vue({
	    el: '#app' //挂载对象
	})
</script>
```

### 局部组件

放在Vue实例中

语法：`components: {'自定义标签/组件名称': {选项对象} }`

```js
new Vue({
	el: '#app', //挂载对象
    components: {
        'test': {
            template: `
					<div>
						<h3>汤圆</h3>
					</div>
					`
        }
    }
})
```

## 组件嵌套

直接创建两个构造器/选项对象，然后在父的template中添加子，会报错（显示没有注册child子组件）

即：

```html
<div id="app">
    <my-father></my-father>
    <my-child></my-child>
</div>
<script>
    let child = {
        template: `
        <div>
            <h3>I am child</h3>
        </div>
        `
    }
    let father = {
        template: `<div style="border 2px solid red">
            <h3>I am father</h3>
            <my-child></my-child>					//报错，显示没有注册my-child子组件
        </div>
        `
    }
    new Vue({
        el: '#app',
        components: {
            "my-child": child,
            "my-father": father
        }
    })
</script>
```

解决：

在父构造器中局部注册一个子组件

```html
<div id="app">
    <my-father></my-father>
</div>
<script>
    let child = {
        template: `
        <div>
            <h3>I am child</h3>
        </div>
        `
    }

    let father = {
        template: `<div style="border: 2px solid red">
            <h3>I am father</h3>
            <my-child></my-child>
        </div>
        `,
        components: {
            'my-child': child
        }
    }
    new Vue({
        el: '#app',
        components: {
            "my-father": father
        }
    })
</script>
```

## 组件的data

组件中的data选项必须是一个函数，返回一个数据对象

原因：同vue2和vue3的data一样，都是避免数据污染

## 组件通讯

组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。如果在子组件中强行使用父组件的数据，就会报错

组件之间的通讯又叫做组件的传值

传值的原因：在开发过程中，数据从服务器请求可能是由大组件来完成，因此小组件的数据可能需要从大组件中获取

组件之间的关系：

<img src="..\image\Vue_组件_通讯_关系.png" alt="Vue_组件_通讯_关系" style="zoom:67%;" />

### 父子组件通讯



<img src="..\image\Vue_组件_通讯_父子传值.png" alt="image-20220719185517483" style="zoom:50%;" />

#### 父传子

父传子：需要将父组件的数据绑定到子组件的标签中；子组件在创建的时候也需添加一个props来接受数据（props只可读，不可写）

实际开发中，props使用是为了测试，开发中常用vuex来传值

props可以是一个数组，也可以是一个对象（对象的属性值，可以做属性限制，也能传递多个值，如下所示）

```html
<div id="app">
    <my-son :movies="msg"></my-son>
</div>

<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <ul v-for="i in movies">
            <li>{{i}}</li>
        </ul>
        <h5>{{myMsg}}</h5>
    </div>
</template>

<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: {
            movies: Array,
            myMsg: {						//父组件没有传值给myMsg，因此会调用default的值；若父组件传值了，则使用传值
                type: String,				 //定义数据类型
                default: '我是默认值'
            }
        }
    }
    let vm = new Vue({
        el: '#app',
        data: {
            msg: '钢铁侠'
        },
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

结果：![Vue_组件_父传子_props对象](..\image\Vue_组件_父传子_props对象.png)

报错原因：props属性值限制了数据类型（数据类型不要用引号包裹）

因此，可以一个属性限制多个数据类型：`movies: [String, Array]`

给子组件传值时，props中的属性也是一个对象，如movies、myMsg都是对象，其属性有：

> type：定义数据类型（可以定义多个，如上所示）
>
> default：默认值，父组件不传值时显示默认值
>
> required：数据是否传值，true为必传（不穿就报错），false（默认值）为可传可不传
>
> 注：default 和 required 不能共用

##### 静态使用子组件属性

```html
<div id="app">
    <!-- 静态传值 给 movies -->
    <my-son movies="唐人街探案三,复仇者联盟"></my-son>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <h4>电影名称：{{movies}}</h4>
    </div>
</template>
<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: ['movies']
    }
    let vm = new Vue({
        el: '#app',
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

因为在标签中给movies写定了值，因此是静态的

##### 动态使用子组件属性

相较于静态，只多个v-bind，将父组件的data的属性，传递给子组件（父组件的data的属性改变，子组件也跟着改变，从而实现了动态）

```html
<div id="app">
    <!-- 动态传值 给 movies -->
    <my-son :movies="msg"></my-son>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h4>这是子组件</h4>
        <h4>电影名称：{{movies}}</h4>
    </div>
</template>
<script>
    //创建子组件
    let MySon = {
        template: '#son',
        //数组是放自定义属性名
        props: ['movies']
    }
    let vm = new Vue({
        el: '#app',
        data: {
            msg: '钢铁侠'
        },
        //注册子组件
        components: {
            MySon
        }
    })
</script>
```

##### propsData

通过new创建子组件的时候给子组件传值

如：

```html
<div id="app">
</div>

<template id="temp">
    <div style="border: 1px solid red;">
        <h4>propsData:{{msg}}</h4>
    </div>
</template>

<script>
    //创建子组件
    let myExt = Vue.extend({
        template: '#temp',
        props: ['msg']
    });
    //实例化对象
    new myExt({
        propsData: {
            'msg': '我是propsData传的数据'
        }
    }).$mount('#app')
</script>
```

##### ref 和 $refs

> ref：用html标签上，指向的是该标签的dom元素，在Vue实例中调用$refs即可使用
>
> 如：
>
> ```html
> <p ref='myp' id='app'></p>
> <script>
>     let vm = new Vue({
>         el: '#app',
>     });
>     vm.$refs.myp			//结果：打印显示 id为app 的 p 标签
> </script>
> ```

##### props 和 ref的区别

> props：强调数据，不能调用子组件的属性和方法
>
> ref：强调引用，可以调用子组件的属性和方法，但不能传递数据

#### 子传父

当子组件需要向父组件传递数据时，就要用到自定义事件了

自定义事件的流程：

> 在子组件中，通过$emit()来触发事件
>
> 在父组件中，通过v-on来监听子组件事件

##### $emit()

> 语法：`vm_son.$emit(method, msg)`			这个方法是写在子组件上的
>
> 参数：
>
> > method：发送这个事件的名称
> >
> > msg：需要传递的参数或内容

##### v-on

自定义一个methods方法，方法这个函数的形参就是子组件传递过来的参数或内容

例：

```html
<div id="app">
    <!-- 注意自定义事件 update不需要加括号，加了括号的一个个形参必须是$event -->
    <my-son @get-msg="update($event)"></my-son>				//接受子组件通过$emit传值事件，并触发父组件update方法，以调用传值内容
    <h4>父组件：{{father_msg}}</h4>
</div>
<template id="son">
    <div style="border: 1px solid red;">
        <h3>{{msg}}</h3>
        <button @click="deliver()">传值</button>
    </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    let Son = {
        template: '#son',
        data() {
            return {
                msg: '我是子组件的内容'
            }
        },
        methods: {
            deliver() {
                this.$emit('get-msg', this.msg);		//子组件通过$emit发起传值
                console.log('传值成功');
            }
        }
    }
    let vm = new Vue({
        el: '#app',
        data: {
            father_msg: ''
        },
        methods: {
            update(val) {								//用来处理传值内容的父组件方法，
                console.log('接受传值成功');
                console.log(val);
                this.father_msg = val;
            }
        },
        components: {
            mySon: Son
        }
    })
</script>
```

##### 升级版

通过`.sync`修饰符，省去了父组件v-on绑定方法这一步骤

变化之处：子组件的`$emit`，需要增加一个update；父组件的v-on绑定方法改为`v-on:xx.sync`并绑定data的属性

```html
<div id="app">
    <!-- 通过v-on绑定msgs事件（msgs是自定义事件），并赋值为父组件的data：fMsg -->
    <my-son :msgs.sync="fMsg"></my-son>
    <h4>子组件传过来的值：{{fMsg}}</h4>
</div>

<template id="MySon">
    <div>
        这是子组件
    <button @click="deliver()">发射</button>
    </div>
</template>

<script>
    let MySon = {
        template: '#MySon',
        data() {
            return {
                msg: '我是子组件的msg'
            }
        },
        methods: {
            deliver() {
                this.$emit('update:msgs', this.msg);		//emit的第一个参数，必须跟update；msgs是自定义事件，配合父组件的v-on使用
            }
        }
    }

    let vm = new Vue({
        el: '#app',
        data: {
            fMsg: ''
        },
        components: {
            MySon
        }
    })
</script>
```



------

# 插槽

## 使用原因

组件的插槽：让封装的组件更加具有扩展性

> 对于一个封装的组件，若要复用，肯定会有相同的和不同之处（比如底部导航栏，可能是首页+我的，也可能是购物车+设置）
>
> 解决：抽取共性，保留不同：
> 将共性抽取到组件中，将不同暴露为插槽。让使用者根据自己的需求，决定插槽中插入什么内容

## 基本使用

### 默认插槽 slot

在父组件中的子组件标签内写内容，会被覆盖（子组件的template模板会覆盖）

```html
<div id="app">
    <my-son>我是父组件</my-son>				//结果：“我是父组件”不会显示，而是显示子组件的template模板 的内容
</div>
```

但是如果子组件预留了插槽slot，则能够实现 内容 的显示

slot给了默认值，当父组件的子组件标签无内容，则显示默认值；有内容，则显示内容（多个内容也全部显示）

```html
<div id="app">
    <my-son></my-son>												//结果：先显示 “我是子组件” ， 再显示 “我是插槽”
    <my-son>我是父组件</my-son>										 //结果：先显示 “我是子组件” ， 再显示 “我是父组件”
    <my-son> <div>我是div</div> <span>我是span</span> </my-son>			 //结果：先显示 “我是子组件” ， 再显示 “我是div” “我是span”
</div>

<template id="my-son">
    <div>
    	<h1>我是子组件</h1>
    	<slot>我是插槽</slot>
    </div>
</template>
```

### 具名插槽

使用具名插槽之前，如果子组件有多个slot，那么父组件的内容将会吧子组件的slot全部替换，如：

```html
<div id="app">
    <my-son>我是父组件</my-son>										 //结果：显示三个 “我是父组件” 
</div>

<template id="my-son">
    <div>
    	<p><slot>我是插槽1</slot></p>
    	<p><slot>我是插槽2</slot></p>
    	<p><slot>我是插槽3</slot></p>
    </div>
</template>
```

给slot一个name标签，就可以解决全部替换的问题，如：

```html
<div id="app">
    <my-son slot="no1">我是父组件</my-son>										 //结果：显示 “我是父组件”  “我是插槽2”  “我是插槽3”
</div>

<template id="my-son">
    <div>
    	<p><slot name="no1">我是插槽1</slot></p>
    	<p><slot name="no2">我是插槽2</slot></p>
    	<p><slot name="no3">我是插槽3</slot></p>
    </div>
</template>
```

在slot标签中，name也是一个属性，因此可以通过v-bind绑定，实现对name值的动态绑定

### 作用域插槽 v-slot

#### 编译作用域

父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

如：

```html
<div id="app">
    <my-son></my-son>
    <my-son v-show="isShow"></my-son>								//子组件的isShow是false，但是页面仍然显示。因为父组件的isShow为true
</div>

<template id="mySon">
    <div style="border: 2px solid deeppink; margin: 20px 40px;">
        <h4>我是子组件</h4>
    </div>
</template>

<script>
    let mySon = {
        template: '#mySon',
        data() {
            return {
                isShow: false
            }
        }
    }
    let vm = new Vue({
        data: {
            isShow: true
        },
        components: {
            mySon
        }
    }).$mount('#app')
</script>
```

#### v-slot

> 作用：当组件需要在多个父组件多个界面展示的时候，将内容放在子组件插槽中，父组件只需要告诉子组件使用什么方式展示界面。
>
> 注：v-slot只能用在 template模板 或 components 里面

```html
<div id="app">
    <my-son>
        <!-- 使用v-slot接受具名插槽abc的数据，并把它命名为cba（cba是随便自定义的） -->
        <template v-slot="cba">
            {{cba}}
        </template>
    </my-son>
</div>

<template id="mySon">
    <div style="border: 2px solid deeppink; margin: 20px 40px;">
        <h4>我是子组件</h4>
        <!-- 定义具名插槽abc，并且传入数据arr -->
        <slot :abc="arr">
            <ul v-for="i of arr">
                <li>{{i}}</li>
            </ul>
        </slot>
    </div>    
</template>

<script>
    let mySon = {
        template: '#mySon',
        data() {
            return {
                arr: ['no1', 'no2', 'no3']
            }
        }
    }
    let vm = new Vue({
        data: {},
        components: {
            mySon
        }
    }).$mount('#app')
</script>
```

结果：			{ "abc": [ "no1", "no2", "no3" ] }

返回的是一个对象，因此可以在插值表达式里面调用属性：cba.abc

------

# 脚手架

## vue-cli3

1、创建目录以及引用：在cli3项目中，main.js里面创建的vue实例一般只引用App，自己的vue文件等都是在components文件下下创建，然后再去App.vue里面引用（而非直接在main.js里面引用）

2、向外暴露：在自己的vue文件下的<script>标签里，利用`export default{}`，完成向外暴露。同时也可以实现子组件的引入，只需在<script>标签里import，然后`export default { components:{xxx} }`，即可引入xxx子组件。

`export default {}`：里面可以包含vue实例的所有属性，如props、name等

------













