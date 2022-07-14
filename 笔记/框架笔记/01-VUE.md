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

## 响应式原理

> Vue2：es5中的  Object.defineProperty
>
> Vue3：es6中的  proxy

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

在Vue给HTML元素增加了自定义属性，它们都是以 “v-” 开头了。并且一些特殊的指令可以带参数和修饰符指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

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
> <p id="app" :style=[one,two]>数组样式</p>
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

### 修饰符

##### 通用修饰符

通用修饰符：这些修饰符是所有事件都通用的

| **修饰符** | **描述**                                      |
| ---------- | --------------------------------------------- |
| .stop      | 阻止事件冒泡（调用 event.stopPropagation()）  |
| .prevent   | 阻止默认事件（调用 event.preventDefault()。） |
| .capture   | 添加事件捕获                                  |
| .self      | 只能由自身触发事件                            |
| .once      | 该事件只会触发一次                            |

##### 按键修饰符

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

