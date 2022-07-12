# VUE

定义：VUE是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。采用的是。采用MVVM模式。

核心库和插件：视图层渲染、组件、路由（vue-router）、状态管理（vuex）

## MVVM模式

Model-View-ViewModel

![Vue_MVVM模型](F:\学习\前端培训\笔记\image\Vue_MVVM模型.png)

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
> <h2>{{msg + num}}</h2>
> <h3>{{arr[1]}}</h3>
> <h4>{{arr.length}}</h4>
> <h5>{{obj.name}}</h5>
> <h6>{{三元表达式}}</h6>
> ```
>
> 
