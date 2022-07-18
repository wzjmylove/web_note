# VUE理论基础

定义：VUE是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。采用的是。采用MVVM模式。

核心库和插件：视图层渲染、组件、路由（vue-router）、状态管理（vuex）

## MVVM模式

Model-View-ViewModel

![Vue_MVVM模型](..\image\Vue_MVVM模型.png)

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
> 语法：`el: 'TagName/id/class/document.querySelect('xx')'`
>
> 注：
>
> > 可以绑定id和class、类名等
> >
> > 绑定元素的其他方法：
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
>     <input type="radio" v-model="sex" value="男">男
>     <input type="radio" v-model="sex" value="女">女
>     <p>{{sex}}</p>
> </div>
> <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
> <script>
>     let vm = new Vue({
>         data: {
>             msg: 'hello Vue',
>             sex: '男'			//单选按钮会默认选择value和sex匹配的按钮（如果input不给value，则差值表达式会显示null）
>         }
>     }).$mount('.app')
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
> 对于对象来说：`<li v-for="(item, key, index) in arr">{{item}}</li>`			key是指对象的属性（/方法）名
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

> 语法：`v-bind:属性名="常量||变量名"`		（v-bind可以省略）
>
> 作用：动态地绑定一个或多个属性，或者一个组件prop到表达式
>
> 注：v-bind必须跟JS表达式

错误写法：`v-bind:src="{{xxx}}"`

#### 对类名的控制 :class

> 语法：`<p :class=['one', 'two']>`			||			`<p :class=['one', {'two': flag}]>`	（表示flag控制two这个类是否生效）
> 			`<p :class=[myclass: true]>`		（myclass是一个变量， :true 表示生效该class）	
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
> > 				  2、定义函数，对应钩子函数bind和update
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
> 3、过滤器函数可以传参，在标签中，变量默认为第一个参数，括号内跟的参数为第二个（详见例子的 unit 形参）
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

> 