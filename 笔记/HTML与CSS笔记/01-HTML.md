#     初识HTML

> ##       认识：
>
> 1. HTML代表什么：超文本标记语言。
>
> 2. HTML是用来做什么的：HTML是用来做网页的标准标记语言。
>
> 3. 如果我要在桌面创建一个HTML文件应该怎么做：首先新建一个文本文档，然后将文本文档的后缀名改为.html或者.htm。
>
> 4. 常用浏览器：IE、火狐、谷歌、opera、Safari（苹果）五大浏览器。
>
> 5. 浏览器内核：负责读取网页内容，整理讯息，计算网页的显示方式并显示内容。
>
>    ## Web标准
>
>    为什么需要网页标准：不同开发人员的写出统一标准的网页。
>
>    ## 网页标准
>
>    构成：
>
>    1.结构：用于网页元素进行整理和分类，HTML;
>
>    2.表现：用于设置网页元素的版式、颜色、大小等外观样式，CSS;
>
>    3.行为：网页模型的定义及交互的编写，JavaScript
>
>    结构写到HTML文件中，表现写到CSS文件中，行为写到JavaScript文件中。

# html外部文件引入

CSS引入：

`<link rel="stylesheet" href="绝对/相对路径">`

# 网页的基本标签

w3c标准：所有标签不能有大写字母

## 标题标签`<h>`

> h1~h6

## 段落标签`<p>`

> `<p></p>`

特点： 1、文本在一个段落中会根据浏览器窗口的大小自动换行。

​			2、段落和段落之间保有空隙。

## `<em>`标签

用于给需要加特殊样式的文字的标签，以便区分

注：文字默认是斜体

## 文本格式化标签

![html_标签_text](..\image\html_标签_text.png)

 注：可以多个标签叠加；上下标可以用在 log、次方数 等

## 盒子标签`<div>	<span>`

> div和span

![](..\image\html_标签_div_span.png)

## 图片标签`<img>`

### 用法

语法：`<img src="">`

src是`<img>`标签的**必须属性**,它用于指定图像文件的路径和文件名。

### 相对路径

![html_tag_img](..\image\html_tag_img.png)

是相对于此.html文件来说的

同一级的用	./xxx.jpg	或者直接省略	./

### 属性

| 属性   | 属性值   | 说明                                     |
| ------ | -------- | ---------------------------------------- |
| src    | 图片路径 | 必须属性，用于指定图像文件的路径和文件名 |
| alt    | 文本     | 替换文本，当图片不能显示时，展示其文本   |
| title  | 文本     | 提示文本，光标移动到图片上时，展示其文本 |
| width  | 像素     | 设置图片宽度                             |
| heigth | 像素     | 设置图片高度                             |
| border | 像素     | 设置图片的边框粗细                       |

## 链接标签`<a>`

有href才是超链接标签，如果没有，就只是一个类似于p标签的普通标签

### 空链接

当没有链接跳转的地址时，用 # 来代替，`即<a href="#"></a>`

### 下载链接

当href地址是以 .exe 或 .zip 等压缩形式的文件时，点击将会下载

### 网页元素链接

在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接

![html_tag_herf](..\image\html_tag_herf.png)

### 锚点链接

点击链接可以快速定位到当前页面中的某个位置

使用方法：给需要定位的标签一个id，然后在href里面写上 #'id'（注：命名不能以数字开头）

如：`<div id="box">我是一个盒子</div>`		

​		`<a href="#box">回到盒子</a>`

## 特殊字符

空格、大小于符号最常用

使用方法：直接敲关键词，不需要加“<>”

![html_tag_specialtag](..\image\html_tag_specialtag.png)

## 音频视频标签

在HTML5中，新增了两个元素: audio元素和video元素，其中audio元素专门用来播放网络上的音频数据，而video元素专门用来播放网络上的视频或电影。使用这两个元素，就不需要使用其他的插件了，只要支持HTML5的浏览器即可。同时，在开发的时候，也不再需要object元素和embed元素编写复杂的代码了

### 音频标签——audio元素

HTML5规定了一种通过audio元素来包含音频的标准方法，Web前端开发者可以通过audio元素播放声音文件或者音频流。目前，audio元素支持三种音频格式，分别为Ogg、vorbis、MP3和MAV格式。由于现在浏览器能够支持的编译器不一致，为了确保一个音频能够同时被所有支持HTML5的浏览器支持，可以通过`<source>`元素来为同一个音频指定多个源，供不同的浏览器来选择合适自己的播放源。

必须属性：src 与 controls

`<source>`元素使用：指定多个源

![html_tag_audio_source](..\image\html_tag_audio_source.png)

### 视频标签——video元素

大致与audio一样，src与controls都是必须元素

### 常用属性

![html_tag_audio](..\image\html_tag_audio.png)

autoplay默认false

最后一个为video独特的属性

## 表格标签`<table>`

`<table></table>`用于定义表格的标签（表格的框架）

`<tr></tr>`用于定义表格中的行（必须嵌套在table标签中）

`<td></td>`用于定义表格中的单元格（必须嵌套在tr标签中）

### 表头单元格

一般表头单元格位于表格的第一行或第一列，表头单元格里面的文本内容加粗居中显示.表头单元格也是单元格,常用于表格第一行,突出重要性,表头单元格里面的文字会加粗居中显示。

表达语法：`<th>`标签表示HTML表格的表头部分(table head的缩写)

如：![html_tag_table](..\image\html_tag_table.png)

属性（实际开发不常用，一般通过CSS来设置）：

![html_tag_table_shuxing](..\image\html_tag_table_shuxing.png)

cellspacing会使表格变粗，因此常在css中写：

`border-collapse:collapse`（即最后一个	注：最后一个为样式而非属性）

### 表格结构化标签

使用场景:因为表格可能很长,为了更好的表示表格的语义，可以将表格分割成表格头部和表格主体两大部分.在表格标签中，分别用:`<thead>`标签表格的头部区域、`<tbody>`标签表格的主体区域.这样可以更好的分清表格结构。使语义更清晰。

之前的代码优化后：![html_tag_table_better](..\image\html_tag_table_better.png)

### 合并单元格

跨行合并：`rowspan = "目标单元格"`

如：`rowspan = "3"`，代码需要放在需要合并的第一列里面，且需要删掉多出来的两个单元格

跨列合并：`colspan = "合并单元格的个数"`

如：`colspan = "5"`，代码写在需要合并的列的最左列且需要删掉多出来的四列单元格

## 列表标签

表格是用来显示数据的，那么列表就是用来布局的。

列表最大的特点就是整齐、整洁、有序，它作为布局会更加自由和方便。

根据使用情景不同，列表可以分为三大类∶无序列表、有序列表、自定义列表。

### 无序列表`<ul>`

`<ul>`标签标识页面中项目的无序列表

语法：![html_tag_list_disorder](..\image\html_tag_list_disorder.png)

注意事项：1.无序列表的各个列表项之间没有顺序级别之分，是并列的。

2.`<ul><ul>`中只能嵌套`<li></li>`，直接在`<ul></ul>`标签中输入其他标签或者文字的做法是不被允许的。

3.`<li>`与`<li>`之间相当于一个容器，可以容纳所有元素。

4.无序列表会带有自己的样式属性，但在实际使用时，我们会使用CSS来设置。（属性是type，有disc-圆点点、circle-空心圆、square-实心正方形，但是type只能写在`<ul>`里面不能写在`<li>`里面）
CSS：`list-style: none`

### 有序列表`<ol>`

`<ol>`标签标识页面中项目的有序列表	（order list）

列表以数字来显示，且使用`<li>`标签来定义列表项

注意事项与无序列表类似，type属性也能更改前面默认的阿拉伯数字，如：![html_tag_ol_type](..\image\html_tag_ol_type.png)

### 自定义列表`<dl>`

它不仅是一个列表项，还包含了一系列术语及说明的组合

定义列表用`<dl>`表示，列表项用`<dt>`，列表项说明用`<dd>`，一个`<dt>`可以有多个`<dd>`声明

注意事项：1. `<dl></dl>`里面只能包含`<dt>`和`<dd>`

2. `<dt>`和`<dd>`个数没有限制， 经常是一个`<dt>`对应多个`<dd>`
3. dl里面只能放dt和dd不允许放其他标签了

## 表单标签`<form>`

作用：用户input输入，以此收集用户信息

组成：完整的表单由表单域、表单控件（又叫表单元素）和提示信息 3个部分组成

![html_tag_form](..\image\html_tag_form.png)

整个为表单域；红色方框为表单控件；蓝色方框为提示信息

### 表单域

`<form action="目标接口" method="提交方式（get/post）" name="表单域名称">
        各种表单元素控件
 </form>`

注：常用post，因为get只能传输2KB以内的，且会在网页上显示传输的信息

目的：`<form>`会把它范围内的表单元素信息提交给服务器

### 表单控件

#### `<input>`标签

##### type属性

属性type的值：

![html_tag_form_input](..\image\html_tag_form_input.png)

##### 一般属性

![html_tag_form_input_shuxing](..\image\html_tag_form_input_shuxing.png)

如：`用户名：<input type="text">   <br>`

单选按钮： 

> 性别：
>
> ```html
> <input type="radio" name="sex">男			
> 
> <input type="radio" name="sex">女 
> ```
>
> 如果多个按钮，只需要加个相同的name，就可以避免复选

属性加了value就可以将value的信息传给后端，而非是在前端显示

`<input type="button" value="提交">`（其他按钮同理，都需要value来写提示信息）

！点击input时，外边框可以用css来去掉	语法：outline : none

###### number属性

number类型的input输入框里只允许用用户输入数字类型的数据，在提交数据的时候，对数字内容进行数字有效性验证，可以保证数据的安全有效性，number类型的输入框对数字类型的限制是通过其他属性来实现的

属性：

![html_tag_form_number_shuxing](..\image\html_tag_form_number_shuxing.png)

step：如step="2"	只能输入-4  -2  0  2  4  6  ...

###### url属性

只有在输入http://或https://时才正确，其他属性同上

###### date属性

普通的文本输入框也可以用来输入日期和时间，但提交后的数据需要进行二次处理才能使用。HTML5， 提供date pickers(日期选择器)类型的选择框，很大程度的简化了这一过程，用户可以直接选择日期，时间等选项。

![html_tag_form_date](..\image\html_tag_form_date.png)

一般只使用date

###### `<label>`

1. `<label>`标签不是表单控件，只是lable标签经常与表单控件搭配使用，所以在这里讲解一下
2. `<label>`标签为input元素定义标注(标签)。
3. `<label>`标签用于绑定一个表单元素当点击`<label>`标签内的文本时，浏览器就会自动将焦点(光标)转到或者选择对应的表单元素上，用来增加用户体验.

**绑定方法（两种**）：![html_tag_form_label](..\image\html_tag_form_label.png)

#### `<select>`下拉标签

通过`<select>`和`<option>`标记，可以设计页面中的下拉列表框，从而为用户提供一些选项和信息的列表。列表框中，可以看到多个选项:下拉列表框，只能看到一个选项。

![html_tag_form_select](..\image\html_tag_form_select.png)

默认选项是第一个，如果要改变，只需在option里面加个 `selected = "selected"`，即可默认选中该选项

select的value也是传给后端看的

#### `<textarea>`表单标签

使用场景:当用户输入文本内容较多的情况下，比如网站的提供意见输入框时，就不能使用text文本框表单了，此时我们可以使用`<textarea>`标签。 在表单元素中，`<textarea>`标签是用于定义多行文本输入的控件。(特大的文本框)

![html_tag_form_textarea](..\image\html_tag_form_textarea.png)

# Emment语法

## VS Code快捷键

TAB	与	ENTER	作用相同

ALT + SHIFT + F							  		格式化

! + TAB									    	 		 快速生成网页基本代码

ALT + SHIFT + LeftMouse		  	 		 快速多选和操作

lorem														 快速生成一行文字

CTRL + / 	或	ALT + SHIFT + A			快捷注释

## HTML

生成多个相同标签加上 ‘ * ’ 就可以了比如	div * 3	就可以快速生成3个div

如果有父子级关系的标签，可以用 > 比如	ul > li	就可以了

如果有兄弟关系的标签，用  ‘＋’  就可以了比如div + p

如果生成带有类名或者id名字的标签，可以直接在标签后写 .demo 或者 #two + TAB键就可以了（ .XXX会生成XXX的class，#XXX会生成XXX的id）（如果不写标签，直接输写，则默认为div标签）

如果生成的div类名是有顺序的，可以用自增符号$（如：.demo $ * 6，就会生成6个class为demo1、demo2 ... 的div标签）

如果想要在生成的标签内部写内容可以用{}表示（如：div{我喜欢花}）

## CSS

w200按tab 可以生成width: 200px

lh26按tab 可以生成line-height: 26px

tac按tab 可以生产text-align:center

## 截图

qq：CTRL + ALT + A

可以获取颜色，不过是RGB的，十六进制需要按住CTRL，即可获取
