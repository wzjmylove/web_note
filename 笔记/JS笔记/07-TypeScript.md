# TypeScript

渐进式，静态类型编程语言

优势：

> 1、规避大量低级错误，避免时间浪费
>
> 2、减少多人协作项目的成本，大型项目友好
>
> 3、良好的代码提示

注：ts代码无法使用node编译为js（只有js代码才能在浏览器和node中执行）

解决：

1、typescript编译器

> 安装：npm install typescript
>
> 编译：npx tsc xxx.ts	（如果typescript是全局包，则可以省去npx）		（编译的意思：将ts编译为js）
>
> 缺点：
>
> > 1、当变量类型改变时，虽报错，但仍能编译成功
> >
> > 2、只编译成js，不能直接执行ts文件（即，其中的打印等操作不会显示）

2、typescript-node

> 安装：npm install typescript-node
>
> 使用：npx ts-node xxx.ts
>
> 优点：一步完成编译+执行

## 类型

数字型：num

字符串型：string

布尔类型：boolean

null：null

未定义：undefined

symbol：symbol

bigint：bigint

任意类型：any

空值：void

未知：unknown

## 引用类型的使用

在ts中，引用类型更加细化，每个对象都有单独的语法

### 数组

> 语法：推荐使用第一行的写法
>
> | 数组中只能出现数字                   | 数组只能出现字符串                    | 数组中既有数字又有字符串                |
> | ------------------------------------ | ------------------------------------- | --------------------------------------- |
> | `let arr: number[] = [1, 2, 3]`      | `let arr: string[] = ['a', 'b']`      | `let arr: (number | string) = [1, 'a']` |
> | `let arr: Array<number> = [1, 2, 3]` | `let arr: Array<string> = ['a', 'b']` | 无                                      |
>
> 注：| 在TS中叫联合类型，只有单竖线，而非双
>
> 以此类推，可以加布尔类型：`arr: (number | boolean | null)[]`
> 如果少了括号：`arr: number | string[]`，则代表arr只能是 数字型（`arr=123`） 或 字符串型数组(`arr=['a']`)													

### 类型别名

当多个变量都是同一种数据类型时，如果每个都声明，太麻烦了，特别是联合类型

> 语法：使用关键字type
>
> ```typescript
> type XXX = (number | string)[];		//XXX是自定义的类型别名
> let arr1: XXX = [1, 'a'];
> let arr2: XXX = ['b', 2];
> ```
>
> 

### 函数

> 定义：指函数的参数和返回值的类型
>
> 语法：
>
> | 单独指定参数、返回值的类型                                   | 同时指定参数、返回值的类型                                   |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | `function add(num1: number, num2: number): number { return 1 }` | 无                                                           |
> | `let add = (num1: number, num2: number): number) => { return 1 }` | `let add: (num1: number, num2: number) => number = (num1, num2) => { return 1 }` |
>
> **注**：如果函数的返回值进行了类型声明，则必须有return；如果没return，则应声明为 void 或 any

#### 可选参数

> 原因：使用函数时，参数有时是可传值，也可不传，此时就需要要用到可选参数
>
> 语法：`function xx(变量1?: number, 变量2?: number) {}`
> 意思为：变量1是可选的，类型为number，变量2是可选的，类型也为number
>
> 如：
>
> ```ts
> function fn(uname: string, name?: string): void {
>     console.log(uname + name);
> }
> 
> fn('w');
> fn('w','z');		//此时两种都不会报错
> fn();				//此时会报错，因为至少传一个实参
> ```

### 对象

> 对象由属性和方法组成，需要对属性和方法的返回值分别进行类型声明
>
> 例二：
>
> ```ts
> let obj: {name: string; age: number; hobby(myHobby: string): void} = {		//一行写完
>     name: 'wz',
>     age: 18,
>     hobby(myHobby) {
>         console.log(this.name + 'love' + myHobby);
>     }
> }
> ```
>
> 例二：
>
> ```ts
> let obj: {				//多行写
>   name: string;
>   age: number;
>   hobby: (myHobby: string) => void;			//方法也可以用箭头函数写
> } = {
>   name: "wz",
>   age: 18,
>   hobby(myHobby) {
>     console.log(this.name + "love" + myHobby);
>   },
> };
> 
> console.log(obj);
> 
> ```
>
> **注**：
>
> > 1、类型声明必须用`{}`包裹
> >
> > 2、一行里面的属性和方法用`; 或 ,`隔开，见例一；多行则可省略，见例二
> >
> > 3、方法里面有形参，也需要注意形参的类型声明

