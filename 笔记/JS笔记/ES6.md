# ES6

## 变量声明

### let

特点

> 1、**不存在变量提升**
>
> 2、不能重复声明（不能再同一个作用域里面定义，见例4）（同名就会报错，不管是var声明还是let声明）
>
> 3、let声明的变量仅在块级作用域内有效（ {}内为一个块级作用域 ），外部无法访问内部变量
>
> 4、let进行循环i操作，都是新的变量，而非同一个i

例1：

```js
let a = 12;
function fn(){
    console.log(a);			//结果：erro	因为重复定义了a
    let a = 6;
}
```

例2：

```js
let a = 12;
function fn(){
    console.log(a);			//结果：12		因为let声明只后会立即执行赋值操作，且作用域查找会找到全局作用域
}
```

例3：

```js
function fn(){
    console.log(a);			//结果：erro	因为let没有变量声明的提升
    let a = 6;
}
```

例4：暂时性死区（本质：只要进入当前作用域，需要被使用的变量已经存在，但不可以获取，只要等到let声明代码出现后才能使用）

```js
var a = 12;
if(true){
    console.log(a);			//结果：erro	因为所声明的变量已存在
    let a = 6；
}	//这个if就是一个块级作用域

function (num){
    let num;				//erro	因为num已经存在
}

function (num){
    {
        let num;			//不报错，因为不是同一个作用域
    }
}
```

### const

声明的是常量

应用场景：确定不会再次赋值变量时使用const

特点：

> 1、不允许重复声明
>
> 2、不存在变量提升
>
> 3、块级作用域
>
> 4、值不允许被修改
>
> 5、声明必须赋初始值，否则就会报错

注：如果const是一个数组或对象，那么修改数组或对象内的属性、方法，不会报错
原因：const保存的是地址，而数据是放在堆中的，修改数据不会修改地址（**不要赋值操作，不然会改地址**）

例：

```js
const arr1 = [1, 2, 3];
arr1.push(4);				//结果：[1,2,3,4]

const arr2 = [0];
arr2 = [1];					//结果：erro	直接赋值会改变指针地址
```

#### Object.freeze()

冻结方法：将对象进行冻结，冻结后不能再修改、添加等操作，且其原型也不能被修改

语法：Object.freeze( obj )

参数：obj：需要给冻结的对象

该方法可以实现狭义的const定义（即不可修改性）

## 块级作用域

在es5只有全局作用域和函数（局部）作用域

因此会经常发生内层变量覆盖外层变量的情况

如：

```js
var a = '123';
function(){
	console.log(a);				//结果：undefined
	if (false)
		var a = '456';
}
```

es6中用let、const声明的变量多了个块级作用域

如：

```js
let a = 123;

function fn() {
    console.log(a);			//结果：123
    if (false) {
        let a = 465;
    }
}
fn();
```

### 闭包的变化

闭包见JS基础中的闭包板块

```js
var arr = [];
for (var i = 0; i < 5; i++) {
    arr[i] = function() {
        console.log(i);
    }
}

for (var j = 0; j < 5; j++) {
    arr[j]();					//结果：打印5个5
}
原因：变量i控制循环，循环结束后，i并未消失，泄露成了全局变量
之前解决办法：利用闭包：自执行函数（执行完后就销毁i）
```

将上述for循环内的var声明改为let声明，就可以触发闭包的作用

## 解构赋值

### 数组的解构赋值

定义：按照一定的模式，从数组中提取值，然后再对变量进行赋值

语法：`let [a,b,c] = ['草莓','苹果','尖尖']`
此时的 a 就是 ‘ 草莓 ’   ， b 就是 ' 苹果 '   ， c 就是 '尖尖'	a b c就是变量名

因此交换变量值，可以用：`[a,b] = [b,a]`，省去了中间变量的操作

注：数组的解构赋值对于var声明的变量也通用

#### 嵌套数组解构

不完全解构：等号左边只匹配一部分等号右边的数组

即等号左边小于等号右边

> 如：
>
> ```js
> let [,,[first,second]] = ['NO1','NO2',['NO3','NO4'],'NO5']
> //此时 first 是 NO3	，second 是 NO4
> ```

指定默认值（即：当等号左边变量大于等号右边时）

> 如：
>
> ```js
> let [red,pink = '粉色'] = ['红色']
> //此时 pink 是粉色 ， 如果不给pink赋值，那么会显示undefined
> ```
>
> 注：指定默认值必须是当前位置的项不存在或者是undefined，那么默认值才生效

默认值可以赋变量

> 如：
>
> ```js
> let [red,pink = red] = ['红色']
> ```
>
> 此时 red 是红色 ， pink 是红色
>
> ```js
> let [pink = red,red = '红色'] = []
> ```
>
> 此时会报错，因为red还未声明

### 对象的解构赋值

对象是无序的（相对于数组来说）

> 解构赋值时，变量名必须与属性名同名
>
> 如：
>
> ```js
> let Person = {
>     name: 'wz',
>     age: 18,
>     sex: 'male'
> }
> let { age, sex, name } = Person;	//结果：age 为 18 ， sex 为 'male' ， name 为 'wz'
> let { uage, usex, uname } = Person;	//结果：undefined
> ```

指定默认值（即：当等号左边变量大于等号右边时）

> 如：
>
> ```js
> let { a = 'aaa', b = 'bbb', c = a } = { a: 1, b: 2, c: 'max' }
> //结果：a 是 1 ，b 是 2 ，c 是 max
> let { A = 'aaa', B = 'bbb', C = a } = { A, B: 2, C: 'max' }
> //结果：erro，A错误	因为A再没有定义之前就使用了，因为没有给值（给''也是给值）
> let { x = 'aaa', y = 'bbb', z = a } = { x:null, y: 2, z: 'max' }
> //结果： x 是 null ，y 是 2 ， z 是 max
> let { i = 'aaa', j = 'bbb', k = a } = { i:undefined, j: 2, k: 'max' }
> //结果： i 是 aaa ，j 是 2 ， k 是 max
> ```
>
> 注：只有undefined的时候默认值才生效

解构变量名（取别名）

> 先找同名属性，再赋给对应得变量名
>
> 如：
>
> ```js
> let Person = {
>     name: 'wz',
>     age: 18,
>     sex: 'male'
> }
> let { age: uage, sex: usex, name: uname } = Person;
> //结果：uage 为 18 ， usex 为 'male' ， uname 为 'wz'
> //此时：name、age、name 为 not defined	(即报错)
> ```
>
> 即：age、sex、name、只是匹配模式，真正得变量是后面得uname等

#### 嵌套对象解构

用花括号包含

如：

```js
let Person = {
    name: 'wz',
    age: 18,
    sex: 'male',
    obj: {
        surname: 'w',
        EnglishName: 'max'
    }
}
let { obj } = Person;
//结果：{ surname: 'w', EnglishName: 'max' }
//给obj起别名就在此处取，不能在下面得代码处用引号取（即let { obj:abc } = Person;）

let { obj: { surname: my_surname } } = Person;
//结果：my_surname 为 w
```

### 函数得解构赋值

解构赋值用在函数得参数上，会增加一些麻烦

> 如：
>
> ```js
> let Person = {
>     name: 'wz',
>     age: 18,
>     sex: 'male',
>     obj: {
>         surname: 'w',
>         EnglishName: 'max'
>     }
> }
> 
> function fn({ name, age } = obj) {
>     console.log(name, age);
> }
> fn(Person);					//结果：wz 18
> //如果找不到name和age属性，则返回undefined
> 
> let arr = [1, [2, 3], [4, 5, 6]];
> 
> function Fn([name, age] = obj) {
>     console.log(name, age);	
> }
> Fn(arr);					//结果：1 [ 2, 3 ]
> ```
>
> 

默认值设置

> 如：
>
> ```js
> function fn({ name = 'wz', age = 18 } = obj) {
>     console.log(name, age);
> }
> ```

## 箭头函数

用处：一般用在定时器上

es6新增函数

写法一

> 语法：`() => {}`		
>
> =>这个中间不能有空格	
>
> 箭头指向一个代码块
>
> {}是函数体

写法二

> 语法：`const fn = () => {}`
>
> 把一个函数赋值给fn
>
> 相当于：`const fn = function() {}`

写法三：当箭头函数内只有return语句时

> 语法：
>
> ```js
> let sum = (a,b) => a + b;	
> sum(10,20);
> //结果是30
> ```
>
> 相当于：
>
> ```js
> function sum (a,b){
> 	return a + b;
> }
> sum(10,20);
> ```
>
> 如果形参只有一个，可以省略小括号 `let sum = c => c * 100;`
>
> 注：不能返回一个实际对象	let obj = id => { id : id , name : ' wz ' }	这样写会报错
> 因为将{}解析为代码块了，而非对象的括号
> 解决办法：let obj = id => ({ id : id , name : ' wz ' })	大括号外面加小括号

### 箭头函数this指向

箭头函数没有自己的this，但是它可以继承上一层的构造函数

#### this

es5中，顶层对象是window

es6中，顶层对象是global

普通函数内的this指向window

对象内的this指向的是该对象的实例（并非对象本身，而是其实例）

> 如：
>
> ```js
> let Person = function(name, age) {
>  this.name = name;
>  this.age = age;
>  this.sing = function() {
>      console.log(this.name + '会唱歌');
>      console.log(this);
>  }
> }
> let wz = new Person('wz', 18);
> console.log(wz);
> wz.sing();
> ```
>
> 此处的this指向 wz 这个实例对象，而非Person
>
> 例1：
>
> ```js
> var name = 'max';
> var age = 18;
> var obj = {
>     name: 'wz',
>     Myage: this.age,
>     sing: function() {
>         console.log(this.name + ' 的年龄是 ' + this.age);
>     }
> }
> console.log(obj.Myage);		//结果：18
> obj.sing();				   //结果：wz 的年龄是 undefined
> ```
>
> 第一个this指向的是window，二三两个this指向obj
>
> 例2：
>
> ```js
> var name = 'max';
> var age = 18;
> var obj = {
>     name: 'wz',
>     Myage: this.age,
>     sing: () => {
>         console.log(this.name + ' 的年龄是 ' + this.age);
>     }
> }
> console.log(obj.Myage);		//结果：18
> obj.sing();				   //结果：max 的年龄是 18
> ```
>
> 三个this都是指向window

DOM对象中，this指向该DOM对象（DOM对象为一个标签，又称事件源）

#### 箭头函数的this

在对象中，this是指向实例对象的，但是如果该对象有箭头函数，那么箭头函数内的this是指向window的

原理：箭头函数的this是指向被声明的作用域里面，但是对象没有作用域，所有指向全局window作用域

如：

```js
let wz = {
    name:'wz',
    sing:() => {
        console.log(this)
    }
}
//结果：this指向window，调用不了this.name
```

推广：定义（包含）箭头函数的作用域里面this指向谁，那么箭头函数内的this就指向谁

如：

```js
let Person = function(name, age) {
    this.name = name;
    this.age = age;
    this.sing = () => {
        console.log(this.name + '会唱歌');
        console.log('this:', this);
    }
}
let wz = new Person('wz', 18);
console.log(wz);
wz.sing();
//结果：this指向wz实例对象（因为wz是由构造对象来的，在该构造对象里this就是指向实例对象，因此在箭头函数中，也是this指向实例对象）
```

### 箭头函数的剩余参数

#### arguments

就是传参的一个伪数组，其原型是一个Object

> 使用：放在函数里面，里面存储了所有传递过来的实参
>
> arguments是一个伪数组
>
> > 1、具有数组length的属性
> >
> > 2、按照索引的方式存储  如arguments=[2]  ；因此也可以按照数组的方式遍历
> >
> > 3、没有真正数组的一些方法 如pop()  push()
>
> 只有函数才有argumens对象，且每个函数都内置好了这个argumens

如：

```js
function fn(a, b) {
    console.log(arguments);
}
fn('a', 'b', 'c', 'd')
//结果：['a', 'b', 'c', 'd']
```

#### 拓展运算符

在箭头函数中，没有arguments，但是可以用拓展运算符（es6新增）

拓展运算符：

> 语法：  `...变量名`		（放在形参中）
>
> 又名：rest参数
>
> 该返回的是一个真正的数组，而非arguments伪数组
>
> 作用：
>
> > 1、用于获取函数的多余的参数，避免了使用arguments对象
> >
> > 2、扩展作用

##### 函数剩余参数

> 如：
>
> ```js
> function fn(...args) {
>  console.log(args);
> }
> fn('a', 'b', 'c', 'd', 123);
> //结果：['a', 'b', 'c', 'd', 123]
> ```
>
> ```js
> function fn(a, b, ...args) {
>  console.log(a, b);				//结果：'a'  'b'
>  console.log(args);				//结果：['c', 'd', 123]
> }
> fn('a', 'b', 'c', 'd', 123);
> ```
>
> 注：rest参数必须放在形参最后
>
> ```js
> function fn(...args, a, b) {
>  console.log(a, b);				
> }
> fn('a', 'b', 'c', 'd', 123);		//结果：报错，rest参数必须放在形参最后
> ```
>
> > 普通函数和箭头函数都可以用rest参数
> >
> > ```js
> > fn = (...args) => {
> >     console.log(args);
> > }
> > fn('a', 'b', 'c', 'd', 123)
> > //结果：['a', 'b', 'c', 'd', 123]
> > ```
> >
> > 

##### 扩展作用

将数组转为逗号分隔开参数序列（相当于将数组的元素遍历出来）

如：

```js
let arr = ['苹果', '香蕉', '橘子'];
console.log(...arr);
//结果：苹果 香蕉 橘子
```

该扩展运算符也能搭配解构数组使用

如：

```js
let arr = ['苹果', '香蕉', '橘子'];
let [fruit, ...more] = arr;
console.log(fruit);			//结果：苹果
console.log(more);			//结果：[ '香蕉', '橘子' ]
```

##### 扩展运算符的应用

> 1、合并数组
>
> 2、将伪数组转换为数组
>
> 3、将字符串转为数组

###### 合并数组

es5中用concat方法

es6用扩展运算符

> 1、使用push加rest参数
>
> ```js
> let arr1 = [1, 2];
> let arr2 = [3, [4, 5]];
> arr1.push(...arr2);
> //结果：arr1：[1, 2, 3, [4, 5]]
> ```
>
> 2、多个rest参数
>
> ```js
> let arr1 = [1, 2];
> let arr2 = [3, [4, 5]];
> let arr3 = [...arr1, ...arr2];
> //结果：arr3：[1, 2, 3, [4, 5]]
> ```

###### 伪数组（类数组）转换为数组

比如在dom绑定中，querySelectorAll就是获取的一个伪数组

```js
let btn = document.querySelectorAll('button');
let newbtn = [...btn];
console.log(newbtn);
//结果：一个数组，数组元素是button标签
```

###### 字符串转为数组

相当于str.split('')

```js
let str = '王泽的英文名是max。'
let newstr = [...str];
console.log(newstr);
//结果：['王', '泽', '的', '英', '文', '名', '是', 'm', 'a', 'x', '。']
```



### 箭头函数和普通函数的区别

1、箭头函数是es6新增的

2、箭头函数没有this

3、箭头函数没有arguments，但是可以用rest参数代替

4、箭头函数**不能使用new定义**，否则会报错（即不能在构造函数中使用）

5、没有call apply bind 修改this值
