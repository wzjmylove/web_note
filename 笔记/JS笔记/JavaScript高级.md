# JavaScript高级

## 面向对象

面向过程：

> 定义：就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了。
>
> 核心：强调实现功能的过程
>
> 优点：性能比面向对象高，适合跟硬件联系很紧密的东西
>
> 缺点：没有面向对象易维护、易复用、易扩展

面向对象：

> 定义：是把事务分解成为一个个对象，然后由对象之间分工与合作
>
> 核心：强调的是对象，然后调用对象实现功能
>
> 优点：灵活、代码可复用、容易维护和开发，适合多人合作的大型软件项目；由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统 更加灵活、更加易于维护
>
> 缺点：性能比面向过程低
>
> 特点：
>
> > 封装：隐藏对象的属性和实现细节，仅对外提供公共访问方式。封装可以将变化隔离，便于使用，提高复用性和安全性
> >
> > 继承：通过父子关系引用，子类无需书写父类成员，也能使用父类的成员。继承主要提高代码复用性
> >
> > 多态：父类或接口定义的引用变量可以指向子类或具体实现类的实例对象。多态用来提升程序的拓展性

### 对象

拥有属性和方法的一个集合

特点：

> 1、对象是引用数据类型，操作对象实则是操作其内存地址
>
> 2、存储方法是 键值对（key，value）	key是字符串类型，value是任意类型
>
> 3、对象的属性名可以有特殊符号（如空格、关键字、保留字等），但是**需要给其加引号**，且引用需要用**对象名 [ ' 属性名 ' ]**	如：`iPhone { "12 ++" : '特殊字符' }`  

#### 创建对象

##### 字面量创建

```js
var obj = { 
			uname : ‘张三’ ,
			age : 18 ,
			sex = ‘男’ ,
			//以上是属性
			//以下是方法（方法常用新定义的函数来执行）
			sayHi : function ( ) {
				console.log(‘Hi~’) ;
		  	}
		  }
```

注：方法冒号后面跟的是一个匿名函数

##### 利用New Object创建

```js
var obj = new Object ( ) ;
obj.uname = ‘张三’ ;
obj.age = 18 ;	
obj.sex = ‘男’ ;
obj.sayHi = function ( ) {
	console.log( ‘Hi~’ );
} ;

```

##### 工厂模式

如：

```js
function student (name,age) {
    var obj = {}
    obj.nmae = name;
    obj,age = age;
    obj.hoby = function(){
        dosomething();
    }
    return obj;
}
//使用
var stu1 = student('wz',18);
console.log(stu1,stu1.hoby());
```

##### 利用构造函数创建

```js
function 构造函数名 ( ) {
	this.属性 = 值 ;
	this.方法 = function ( ) { }
}
//使用
new 构造函数名 ( ) ;
```

例：

```js
function Star ( uname , age , sex ) {
					this.name = uname ; 
					this.age = age ;
					this.sex = sex ;
					this.sing = function ( sang ) {
						console.log (uname + '的歌曲' + sang ) ;
					}
				}	//这里面的name、age、sex都是属性，而unmae、age、sex都是形参
				var ldh = new Star( ‘刘德华’ , 18 , ‘男’ ) ; 
				var zxy = new Star( ‘张学友’ , 20 , ‘男’ ) ; 
				console.log( ldh.name ) ;			//结果是 刘德华
				ldh.sing ( ‘冰雨’ ) ;				//结果是 刘德华的歌曲冰雨
```

注：

> 1、this正常是指向全局windows的，在函数里面时才指向该函数
>
> 2、该函数无需return，默认返回这个对象
>
> 3、构造函数名首字母要大写（惯用方法，便于区分普通函数）
>
> 4、var ldh = new Star ( ) 这个调用函数返回的是一个对象，即console.log( typeof ldh )     结果是object
>
> 5、调用构造函数时，必须使用new
> new有一个隐式三步骤：`var this = {} ; this.xxx ; return this ;`
>
> 6、只要调用了new Star ( ) ,就创建了一个新对象
>
> 7、属性和方法之前必须添加this

###### 缺点

可能会存在浪费内存的问题，因为如果构造函数里面由共享的属性或方法，每次创建都会开辟地址来储存

###### 解决办法

1、将共享的属性或方法放在构造函数的外面，但要注意同名覆盖问题

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
    this.job = '前端开发';
    this.skill = sing;
}

function sing() {
    console.log(this.name + '会唱歌');
}

var wz = new Student('王泽', 18);
var wzj = new Student('王紫晶',16);

wz.skill();	//结果：王泽会唱歌

wzj.age === wz.age 		//结果：false,因此每创建一个实力对象，都会开辟新的job的地址，造成空间浪费
wzj.skill === wz.skill	//结果：true（是同一个内存地址）
```

1.1、解决命名冲突问题

将方法放到一个新的对象中去

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
    this.job = '前端开发';
    this.skill = pfns.sing;
}
var pfns = {
    function sing() {
    	console.log(this.name + '会唱歌');
	}
}

```

2、通过原型的方式

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype.job = '前端开发';

var wz = new Student('王泽', 18);
var wzj = new Student('王紫晶',16);
console.log(wz);		//结果：Student { name: '王泽', age: 18 }	这里面没有job属性，但该实例对象下的prototype对象																 里面有job属性
console.log(wz.job);	//结果：前端开发

wzj.age === wz.age 		//结果：false
wz.job === wzj.job		//结果：true（是同一个内存地址）
```

![js_原型](F:\学习\前端培训\笔记\image\js_原型.png)

##### ES6 class类方式创建

类是建立在原型上的（类是使用构造函数 + 原型 创建的对象），且class本质上依旧是函数，而非引用类型（与引用类型相似）

constructor函数：

> 1、类里面有个constructor函数，可以接受传递过来的参数，同时返回实例对象。
>
> 2、constructor函数，只要new了一下，就会自动调用这个函数，如果我们不写这个函数，class类也会自动调用。
>
> 3、类里面的所有函数不用加function
>
> 4、类的共有属性是放在constructor函数里面的
>
> 5、类里面的方法不需要加逗号，以及同第三点，无需加function

如：

```js
class Student {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    skill() {
        console.log('干饭');
    }
    sing() {
        console.log(this.name + '开始唱歌');
    }
};

var wz = new Student('王泽', 18);
console.log(wz);
wz.skill();	//结果：干饭
wz.sing();	//结果：王泽开始唱歌
```

注：new 一个实例对象不能省略new

#### 增、改属性/方法

直接在对象后面加 Object.XXX = XX；
若对象无XXX属性，则增加改属性 ； 若对象有XXX属性，则更改其值

#### 删除属性

delete Obiject.XXX;
删除对象的XXX属性

#### 遍历

##### for in 遍历对象

for ( 变量 in 对象 ) { }

例：

```js
for ( var k in obj ) {
	console.log ( k ) ;		// k是变量，输出得到的是属性名（如name、age、sex）
	console.log ( obj[k] ) ;	//输出得到的是属性值（如张三、18、男）
}
```

##### Object自带方法

Object . keys ( )		 访问对象的属性名，如 name、age、sex

Object . value ( )		访问对象的属性值，如 刘德华、18、男

这两个方法返回的都是数组

例：

```js
var key = Object keys(Star);
for(var i = 0; i < key.length; i++){
    console.log(key[i]);		//返回属性名
    console.log(Star[key[i]]);	//返沪属性值
}
```

### 成员

定义：通常称呼一个类的字段、函数为成员

成员组成：

> 属性：这个类包含的一些键值
>
> 方法：这个类包含的一些函数，用类或者对象来调用该函数
> 方法可以挂到构造函数的 “ 原型 ” 上，也可以直接挂到构造函数上，此时方法又分为：
>
> > 实例方法：由类的实例对象使用
> >
> > 静态方法：也称类方法，由这个类本身调用，挂在构造函数上

如：

```js
var Person = function(name, age) {
    this.name = name;
    this.age = age;
    this.skill = function() {
        console.log('干饭');
    }
};

//创建一个实例对象 wz
var wz = new Person('王泽', 18);

//访问实例成员，只能通过实例化的对象（wz）来访问
console.log(wz);
wz.skill() //结果：干饭

//不可以通过构造函数访问实例成员
console.log(Person.age); //结果：undefined

//静态成员，在构造函数本身添加的成员就是静态成员
Person.sex = '男';
console.log(Person.sex); //结果：男

//静态成员只能通过构造函数来访问，不能通过实例对象来访问
console.log(wz.sex); //结果：undefined
```

## 原型

定义：原型是function对象的一个属性,它定义了构造函数制造出来的公共祖先.通过该构造函数构造函数产生的对象,可以继承该原型的属性和方法（引用类型数据：数组、对象）

> 1、所有引用类型都有一个`__proto__`（隐式类型）属性，属性值是一个普通对象（Object）
>
> 2、所有函数都有一个prototype（原型）属性，属性值是一个普通的对象
> 	  即：创建函数都会有一个prototype属性，该属性指向了原型对象
>
> 3、引用类型的`__proto__`属性都指向它 构造函数的 prototype

### 构造函数需要原型对象的原因

让多个实例之间通用（/共享）方法使用一个引用，减少空间的占用

### 原型对象的两个属性

> 1、constructor：指向与原型对象关联的构造函数
>
> 2、`__proto__`：指向其构造函数的原型，默认为Object

#### constructor

constructor 主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数

如果修改了原来的原型对象，即给原型对象prototype赋值了一个新的对象，则必须手动利用constructor指回原来的构造函数

##### 给原型对象追加属性 

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype.job = '前端开发';
Student.prototype.sayName = function() {
    console.log('你好' + this.name);
};

var wz = new Student('王泽', 18);

wz.constructor === Student //结果：true（是同一个内存地址）
```

##### 给原型对象赋值一个对象

例：

```js
var Student = function(name, age) {
    this.name = name;
    this.age = age;
}

Student.prototype = {
    job: '前端开发',
    sayName: function() {
        console.log('你好' + this.name);
    }
}

var wz = new Student('王泽', 18);

wz.constructor === Student 	//结果：false（因为实例对象wz的prototype对象没有constructor了）
```

解决：

```js
Student.prototype = {
    constructor:Student,
    job: '前端开发',
    sayName: function() {
        console.log('你好' + this.name);
    }
}

wz.constructor === Student	//结果：true
```

#### `__proto__`

每个对象都会有一个属性`__proto__` 指向构造函数的prototype 原型对象，之所以我们对象可以使用构造函数prototype 原型对象的属性和方法，就是因为对象有`__proto__` 原型的存在

![js_原型_proto](F:\学习\前端培训\笔记\image\js_原型_proto.png)

例：

```js
//原型对象
function Person() {}
console.log(Person.prototype);
/* 结果： constructor: ƒ Person()
		 [[Prototype]]: Object		*/

var Student = function() {}
console.log(Student.prototype);
/* 结果： constructor: ƒ ()
		 [[Prototype]]: Object		*/

//实例化对象
var stu1 = new Student();		//注：实例与构造函数没有直接关系，但和原型对象有
console.log(stu1.prototype);	//结果：undefined
//实例化对象获取原型对象的方法：
console.log(stu1.__proto__);	// stu1.__proto__ === Student.prototype
```

综上，若需要获得对象的原型，引用类型需要用 `XXX.__proto__`；函数需要用`XXX.prototype`

### 构造函数、实例、原型 三者的关系

![js_原型_关系](F:\学习\前端培训\笔记\image\js_原型_关系.png)

### 原型链

![js_原型链](F:\学习\前端培训\笔记\image\js_原型链.png)

原型链查找规则：

> ① 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性
>
> ② 如果没有就查找它的原型（也就是 `__proto__`指向的 prototype 原型对象）
>
> ③ 如果还没有就查找原型对象的原型（Object的原型对象）
>
> ④ 依此类推一直找到 Object 为止（null）
>
> ⑤ `__proto__`对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线

Object的原型对象：例如daibai这个实例对象，其原型（daibai的原型是`__proto__`）指的是Person这个构造函数，而Person构造函数中的constructor指向了Object（即构造函数Person的原型指的是Object）

### instanceof

作用：用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

语法：stu1 instanceof Student
即，检测stu1是否在Student上，是则返回true

