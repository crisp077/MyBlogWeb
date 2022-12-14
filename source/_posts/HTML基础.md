---
title: HTML基础
---
# HTML基础

## 1 基础认知

### 1.1 认知网页

- 网页有哪些部分组成？

  - 文字、图片、音频、视频、超链接

- 我们看到的网页背后本质是什么？

  - 代码

- 前端的代码是通过什么软件转换成用户眼中的页面的？

  - 通过浏览器转化（解析和渲染）成用户看到的网页

  

### 1.2 .1 五大浏览器

- 浏览器：是网页显示、运行的平台，是前端开发必备利器
- 常见的五大浏览器：
  - IE浏览器、火狐浏览器（FireFox）、谷歌浏览器（Chrome）、Safari浏览器、欧朋浏览器（Opera）



### 1.2.2 渲染引擎

- 渲染引擎（浏览器内核）：浏览器中专门对代码进行解析渲染的部分
- 浏览器出品的公司不同，内在的渲染引擎也是不同的：

|     浏览器     |  内核   |                  备注                   |
| :------------: | :-----: | :-------------------------------------: |
|       IE       | Trident | IE、猎豹安全、360极速浏览器、百度浏览器 |
|    FireFox     |  Gecko  |             火狐浏览器内核              |
|     Safari     | Webkit  |             苹果浏览器内核              |
| Chrome / Opera |  Blink  |       Blink 其实是 Webkit 的分支        |

- 注意：
  - 渲染引擎不同，导致解析相同的代码时的速度、性能、效果也不同的



### 1.3.1 为什么需要 Web 标准？

- 不同浏览器的渲染引擎不同，对于代码解析的效果会存在差异



### 1.3.2 Web标准的构成

- Web 标准中分为成三个构成：

| 构成 |    语言    |                        说明                        |
| :--: | :--------: | :------------------------------------------------: |
| 结构 |    HTML    |                   页面元素和内容                   |
| 表现 |    CSS     | 网页元素的外观和位置等页面样式（如：颜色、大小等） |
| 行为 | JavaScript |              网页模型的定义与页面交互              |



### 2.1.1 HTML 的概念

- HTML（Hyper Text Markup Language）中文译为：超文本标记语言
  - 专门用于网页开发的语言，主要通过HTML标签对网页中的文本、图片、音频、视频等内容进行描述
- 案例：文字变粗案例
  - 体验构建一个网页，需要在网页中显示一个加粗的文字

### 2.2.1 HTML页面固定结构

- 网页类似于一篇文章：
  - 每一页文章内容是由固定的结构的，如：开头、正文、落款等...
  - 网页中也是存在固定的结构的，如：整体、头部、标题、主题
- 网页中的固定结构是要通过特点的**HTML标签**进行描述的



## 2 网页开发工具

### 2.1 VSCode 工具生成骨架标签新增代码

1. `<!DOCTYPE>`标签
2. lang 语言
3. chatset 字符集

### 2.2  文档类型声明标签

​	`<!DOCTYPE>` 文档类型声明，作用就是告诉浏览器使用哪种HTML版本来显示网页

```html
<!DOCTYPE html>
```

​	这句代码的意思是：当前页面采取的是HTML5版本来显示网页

注意：

1. `<!DOCTYPE>`声明位于文档中的最前面的位置，处于`<html>`标签之前
2. `<!DOCTYPE>`不是一个HTML标签，它就是文档类型声明标签

### 2.3 lang 语言种类

​	用来定义当前文档显示的语言

1. en 定义语言为英语
2. zh-CN 定义语言为中文

​	其实对于文档来说，定义成en的文档也可以显示中文，定义成zh-CN的文档也可以显示英文

​	这个属性对浏览器和搜索引擎还是有作用的（例如翻译功能）

### 2.4 字符集

​	字符集（Character set）是多个字符的集合。一边计算机能够识别和存储各种文字

​	在`<head>`标签内，可以通过`<meta>`标签的charset属性来规定HTML文档应该使用哪种字符编码

```html
<meta charset="UTF-8"/>
```

​	charset 常用的值有：GB2312、BIG5、GBK 和 UTF-8，其中UTF-8也被称为万国码，基本包含了全世界所有国家需要用到的字符

​	注意：上面的语法是必须要写的代码，否则可能引起乱码的情况。一般情况下，统一使用UTF-8编码



## 3 HTML 常用标签

### 3.1 标签语义

​	学习标签是有技巧的，重点是记住每个标签的语义。简单理解就是指标签的含义

​	根据标签的语义，在合适的地方给一个最为合适的标签，可以让页面结构更清晰



### 3.2 标题标签

​	为了使网页更具有语义化，我们经常会在页面中用到标题标签。HTML提供了6个等级的网页标签

​	即`<h1>` - `<h6>`

```html
<h1> 我是一级标题 </h1>
```

​	单词 head 的缩写，意为头部、标题

​	标签语义：作为标题使用，并且依据重要性递减

特点：

1. 加了标题的文字会变粗，字号也会依次变大
2. 一个标题独占一行



### 3.3 段落和换行标签（重要）

​	在网页中，要把文字有条有理地显示出来，就需要将这些文字分段显示。

​	在HTML标签中，`<p>`标签用于定义段落，它可以将整个网页分为若干个段落

```html
<p> 这是一个段落标签 </p> 
```

​	单词 paragraph 的缩写，意为段落

标签语义：可以把HTML文档分割成若干段落

特点：

- 文本在一个段落中会根据浏览器窗口的大小自动换行
- 段落和段落之间保有空隙



​	在HTML中，一个段落中的文字会从左到右依次排列，直到浏览器窗口的右端，然后才自动换行。如果希望某段文本强制换行显示，就需要用到换行标签`<br/>`

```html
<br />
```

​	单词 break 的缩写，意为打断、换行

标签语义：强制换行

特点：

- `<br />`是个单标签
- `<br />`标签只是签单地开始新的一行，跟段落不一样，段落之间会插入一些垂直的间距



### 3.4 文本格式化标签

​	在网页中，有时需要为文字设置**粗体**、*斜体*或下划线等效果，这是就需要用到HTML中的文本格式化标签，使文字以特殊方式显示

​	标签语义：突出重要性，比普通文字更重要

| 语义   | 标签                           | 说明                                    |
| ------ | ------------------------------ | --------------------------------------- |
| 加粗   | `<strong></strong>`或`<b></b>` | 更推荐使用`<strong>`标签加粗 语义更强烈 |
| 倾斜   | `<em></em>`或`<i></i>`         | 更推荐使用`<en>`标签加粗 语义更强烈     |
| 删除线 | `<del></del>`或`<s></s>`       | 更推荐使用`<del>`标签加粗 语义更强烈    |
| 下划线 | `<ins></ins>`或`<u></u>`       | 更推荐使用`<ins>`标签加粗 语义更强烈    |



### 3.5 `<div>`和`<span>`标签

​	`<div>`和`<span>`是没有语义的，它们就是一个盒子，用来装内容

```html
<div> 这是头部 </div>
<span> 今日价格 </span>
```

​	div 是 division 的缩写，表示分割、分区，span 意为跨度、跨距。

特点：

- `<div>`标签用来布局，但是现在一行只能放一个`<div>`。大盒子
- `<span>`标签用来布局，一行上可以多个`<span>`。小盒子



### 3.6 图像标签和路径（重点）

**1 图像标签**

​	在HTML标签中，`<img>`标签用于定义HTML页面中的图像

```html
<img src="图像URL" />
```

​	单词image的缩写，意为图像

​	src 是`<img>`标签的必须属性，它用于指定图像文件的路径和文件名

​	所谓属性：简单理解就是属于这个图像标签的特性

**图像标签其他属性：**

| 属性   | 属性值   | 说明                                 |
| ------ | -------- | ------------------------------------ |
| src    | 图片路劲 | 必须属性                             |
| alt    | 文本     | 替换文本。图像不能显示的文字         |
| title  | 文本     | 提示文本。鼠标放到图像上，显示的文字 |
| width  | 像素     | 设置图像的宽度                       |
| height | 像素     | 设置图像的高度                       |
| border | 像素     | 设置图像的边框粗细                   |

**注意：**

- 图像标签可以拥有多个属性，必须写在标签名的后面
- 属性之间不分先后顺序，标签名与属性、属性与属性之间均以空格分开
- 属性采取键值对的格式，即key="value"的格式，属性="属性值"

**2 路径（前期铺垫知识）**

（1）目录文件夹和根目录：

​	目录文件夹：就是普通文件夹，里面只不过存放了我们所作页面所需要的相关素材

​	根目录：打开目录文件夹的第一层就是根目录

（2）路径

​	页面中的图片会非常多，通常我们会新建一个文件夹来存放这些图像文件（images），这时再查找图像，就需要采用“路径”的方式来指定图像文件的目录

路径可以分为：

1. 相对路径：以引用文件所在位置为参考基础，而建立出的目录路径

   | 相对路径分级 | 符号  | 说明                                                         |
   | ------------ | ----- | ------------------------------------------------------------ |
   | 同一级路径   |       | 通向文件位于HTML文件同一级 如`<img src="baidu.gif" />`       |
   | 下一级路径   | `/`   | 图像文件位于HTML文件下一级 如`<img src ="images/baidu.gif" />` |
   | 上一级路径   | `../` | 图像文件位于HTML文件上一级 如`<img src = "../baidu.gif" />`  |

2. 绝对路径：是指根目录下的绝对位置，直接到达努比奥位置，通常是从盘符开始的路径



### 3.7 超链接标签（重点）

​	再HTML中，`<a>`标签用于定义超链接，作用是从一个页面连接到另一个页面

**1 链接的语法格式**

```html
<a href="跳转目标" target="目标窗口的弹出方式"> 文本或图像 </a>
```

单词 anchor的缩写，意为：锚 

两个属性的作用如下：

| 属性   | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| href   | 用于指定链接目标的url地址，(必须属性)当标签应用href属性时，它就具有了超链接的功能 |
| target | 用于指定页面的打开方式，其中`_self`为默认值，`_blank`为在新窗口中打开方式 |

**2 链接分类**

1. 外部链接：例如

   `<a href="https://crisp077.xyz" target="_blank"> 三月江东的个人网站 </a>`

2. 内部链接：网页内部之间的相互链接。直接链接内部页面名称即可，例如：

   `<a href="index.html> 首页 </a>"`

3. 空链接：如果当前没有确定链接目标时

   `<a href="#" 首页 </a>`

4. 下载链接：如果href里面地址是一个文件或者压缩包，会下载这个文件

5. 网页元素链接：在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接，例如：

   `<a href="https://www.crisp077.xyz" target="_blank"><img src="image.jpg" width="400" /></a>`

6. 锚点链接：点击链接，可以快速定位到页面中的某个位置

   - 在链接文本的href属性中，设置属性值为`#名字`的形式，如`<a href="#top">回到开始位置</a>`
   - 找到目标位置标签，里面添加一个id属性=刚才的名字，如`<h1 id="top">开始位置</h1>`



## 4 HTML中的注释和特殊字符

### 4.1 注释

​	如果需要在 HTML 文档中添加一些便于阅读和理解但又不需要显示在页面中的注释文字，就需要使用注释标签

​	HTML 中的注释以`<!--`开头，以`-->`结束

```html
<!--注释语句-->    //快捷键：ctrl + /
```

​	注释标签里面的内容是给程序员看的，这个代码是不执行不显示到页面中的

​	添加注释是为了更好的解释代码的功能，便于相关人员理解和阅读代码，程序时不会执行注释内容的



### 4.2 特殊字符

​	在HTML页面中，一些特殊的符号很难或者不方便直接使用，此时我们就可以使用下面的字符来替代

| 特殊字符 | 描述           | 字符代码   |
| -------- | -------------- | ---------- |
|          | 空格符         | `&nbsp;`   |
| <        | 小于号         | `&lt;`     |
| >        | 大于号         | `&gt;`     |
| &        | 和号           | `&amp;`    |
| ￥       | 人民币         | `&yen;`    |
| ©        | 版权           | `&copy;`   |
| °        | 度             | `&deg;`    |
| ±        | 正负号         | `&plusmn;` |
| ×        | 乘号           | `&times;`  |
| ÷        | 除号           | `&divide;` |
| ²        | 平方2（上标2） | `&sup2;`   |
| ³        | 平方3（上标3） | `&sup3;`   |



# HTML标签（下）

## 1.表格标签

​	表格是实际开放中非常常用的标签：

1. 表格的主要作用
2. 表格的基本语法

### 1.1 表格的主要作用

​	表格主要用于显示、展示数据，因为它可以让数据显示的非常规整，可读性非常好。特别是后台展示数据的时候，能够熟练运用表格就显得很重要。一个清爽简约的表格能够把复杂的数据表现得有条理



### 1.2 表格的基本语法

```html
<table>
    <tr>
        <td>单元格内的文字</td>
        ...
    </tr>
    ...
</table>
```

1. `<table></table>`适用于定义表格的标签
2. `<tr></tr>`标签用于定义表格中的行，必须嵌套在`<table></table>`标签中
3. `<td></td>`用于定义表格中的单元格，必须嵌套在`<tr></tr>`标签中
4. 字母 td 指表格数据（tabledata），即数据单元格的内容



### 1.3 表头单元格标签

​	一般表头单元格位于表格的第一行或第一列，表头单元格里面的文本内容加粗居中显示

​	`<th>`标签表示HTML表格的表头部分（table head 的缩写）

```html
<table>
    <tr>
        <th>姓名</th>
        <td>单元格内的文字</td>
        ...
    </tr>
    ...
</table>
```



### 1.4 表格属性

​	表格标签这部分属性我们实际开发中不常用，后面通过 CSS 来设置

| 属性名      | 属性值              | 描述                                             |
| ----------- | ------------------- | ------------------------------------------------ |
| align       | left、center、right | 规定表格相对周围元素的对齐方式                   |
| border      | 1或""               | 规定表格单元是否拥有边框，默认为""，表示没有边框 |
| cellpadding | 像素值              | 规定单元边沿与其内容之间的空白，默认1像素        |
| cellspacing | 像素值              | 规定单元格之间的空白，默认2像素                  |
| width       | 像素值或百分比      | 规定表格的宽度                                   |
| height      | 像素值或百分比      | 规定表格的高度                                   |



### 1.5 表格结构标签

​	使用场景：因为表格可能很长，为了更好的表示表格的语义，可以将表格分割成表格头部和表格主体两大部分

​	在表格标签中，分别用：`<thead>`标签 表格的头部区域、`<tbody>`标签 表格的主体区域。这样可以更好的分清表格结构

- `<thead></thead>`：用于定义表格的头部。`<thead>`颞部必须拥有`<tr>`标签。一般位于第一行
- `<tbody></tbody>`：用于定义表格的主体，住哟啊用于放数据本体



### 1.6 合并单元格

​	特殊情况下，可以把多个单元格合并为一个单元格

1. 合并单元的方式
2. 目标单元格
3. 合并单元格的步骤

**合并单元格方式：**

- 跨行合并：rowspan="合并单元格的个数"
- 跨列合并：colspan="合并单元格的个数"

**目标单元格：**（写合并代码）

- 跨行：最上侧单元格为目标单元格，写合并代码
- 跨列：最左侧单元格为目标单元格，写合并代码

**合并单元格三部曲：**

1. 先确定是跨行还是跨列合并
2. 找到目标单元格，写上合并方式=合并的单元格数量。比如：`<td colspan="2"></td>`
3. 删除多余单元格



## 2 列表标签

​	表格是用来显示数据的，那么列表就是用来布局的

​	列表最大的特点就是整齐、灵活、整洁、有序，它作为布局会更加自由和方便

​	根据使用的情景不同，列表可以分为三大类：无序列表、有序列表和自定义列表

### 2.1 无序列表（重点）

​	`<ul>`标签表示 HTML 页面中项目的无序列表，一般会以项目符号呈现列表项，而列表项使用`<li>`标签定义

基本语法如下：

```html
<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    ...
</ul>
```

1. 无序列表的各个列表项之间没有顺序级别之分，是并列的
2. `<ul></ul>`中只能嵌套`<li></li>`，直接在`<ul></ul>`标签中出入其他标签或者文字的做法是不被允许的
3. `<li></li>`之间相当于一个容器，可以容纳所有元素
4. 无序列表会带有自己的样式属性，但在实际使用时，我们会使用CSS来设置



### 2.2 有序列表

​	有序列表即为有排列顺序的列表，其各个列表会按照一定的顺序排列定义

​	在 HTML 标签中，`<ol>`标签用于定义有序列表，列表排序以数字来显示，并且使用`<li>`标签来定义列表项

基本语法如下：

```html
<ol>
    <li>列表项1</li>
    <li>列表项2</li>
    ...
</ol>
```

1. `<ol></ol>`中只能嵌套`<li></li>`，直接在`<ol></ol>`标签中出入其他标签或者文字的做法是不被允许的
2. `<li></li>`之间相当于一个容器，可以容纳所有元素
3. 有序列表会带有自己的样式属性，但在实际使用时，我们会使用CSS来设置



### 2.3 自定义列表（重点）

自定义列表的使用场景：

​	自定义列表常用于对术语或名词进行解释和描述，定义列表的列表项前没有任何项目符号



​	在 HTML 标签中，`<dl>`标签用于定义描述列表（或定义列表），该标签会与`<dl>`（定义项目/名字）和`<dd>`（描述每一个项目/名字）一起使用

基本语法：

```html
<dl>
    <dt>名词1</dt>
    <dd>名词1解释1</dd>
    <dd>名词1解释2</dd>
    ...
</dl>
```

1. `<dl></dl>`里面只能包含`<dt>`和`<dd>`
2. `<dt>`和`<dd>`个数没有限制，经常是一个`<dt>`对应多个`<dd>`



### 2.4 列表总结

|   标签名    |    定义    | 说明                                                         |
| :---------: | :--------: | :----------------------------------------------------------- |
| `<ul></ul>` |  无序标签  | 里面只能包含`<li>`，没有顺序，使用较多。`<li>`里面可以包含任何标签 |
| `<ol></ol>` |  有序列表  | 里面只能包含`<li>`，有顺序，使用相对较少。`<li>`里面可以包含任何标签 |
| `<dl></dl>` | 自定义列表 | 里面只能包含`<dt>`和`<dd>`。`<dt>`和`<dd>`里面可以放任何标签 |



## 3 表单标签

​	现实中的表单，我们区银行办理信用卡填写的单子

### 3.1 为什么需要表单

​	使用表单目的是为了收集用户信息

​	在网页中，我们也需要跟用户进行交互，收集用户资料，此时就需要表单



### 3.2 表单的组成

​	在 HTML 中，一个完整的表单通常由表单域、表单控件（也称为表单元素）和提示信息3个部分组成



### 3.3 表单域

​	表单域是一个包含表单元素的区域

​	在 HTML 标签中，`<form>`标签用于定义表单域，以实现用户信息的收集和传递

​	`<form>`会把它范围内的表单元素信息提交给服务器

```html
<form action="url地址" method="提交方式" name="表单域名称">
    各种表单元素控件
</form>
```

**常用属性**:

| 属性   | 属性值   | 作用                                               |
| ------ | -------- | -------------------------------------------------- |
| action | url地址  | 用于指定接收并处理表单数据的服务器程序的url地址    |
| method | get/post | 用于设置表单数据的提交方式，其取值为get或post      |
| name   | 名称     | 用于指定表单的名称，以区分同一个页面中的多个表单域 |

这里只需要记住两点：

1. 在我们写表单元素之前，应该有个表单域把他们进行包含
2. 表单域是form标签



### 3.4 表单控件（表单元素）

​	在表单域中可以定义各种表单元素，这些表单元素就是允许用户在表单中输入或者选择的内容控件

1. input 输入表单元素
2. select 下拉表单元素
3. textarea 文本域元素

#### 3.4.1 `<input>`表单元素

​	`<input>`标签用于收集用户信息

​	在`<input>`标签中，包含一个 type 属性，根据不同的 type 属性值，输入字段拥有很多种形式（可以是文本字段、复选框、掩码后的文本控件、单选按钮、按钮等）

```html
<input type="属性值" />
```

- `<input/>`标签为单标签
- type 属性设置不同的属性值用来只当不同的控件类型

type 属性的属性值及其描述如下：

| 属性值   | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| buttom   | 定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）   |
| checkbox | 定义复选框                                                   |
| file     | 定义输入字段和“浏览”按钮，供文件上传                         |
| hidden   | 定义隐藏的输入字段                                           |
| image    | 定义图像形式的提交按钮                                       |
| password | 定义密码字段。该字段中的字符被掩码                           |
| radio    | 定义单选按钮                                                 |
| reset    | 定义重置按钮。重置按钮会清除表单中的所有数据                 |
| submit   | 定义提交按钮。提交按钮会把表单数据发送到服务器               |
| text     | 定义单行的输入字符，用户可在其中输入文本。默认宽度为20个字符 |

除了type 属性外，`<input>`标签还有很多其他属性：

| 属性      | 属性值       | 描述                                |
| --------- | ------------ | ----------------------------------- |
| name      | 由用户自定义 | 定义input元素的名称                 |
| value     | 由用户自定义 | 规定input元素                       |
| checked   | checked      | 规定此input元素首次加载时应当被选中 |
| maxlength | 正整数       | 规定输入字段中的字符的最大长度      |

1. name 和 value 是每个表单元素都有的属性值，主要给后台人员使用
1. name 表单元素的名字，要求按钮和复选框都要有相同的 name 值
1. checked 属性主要针对于单选按钮和复选框，主要作用一打开页面，就可以默认选中某个表单元素
1. maxlength 是用户可以在表单元素输入的最大字符数，一般较少使用



#### 3.4.2 `<label>`标签

​	`<label>`标签为 input 元素定义标注（标签）

​	`<label>`标签用于绑定一个表单元素，当点击`<label>`标签内的文本时，浏览器就会自动将焦点（光标）转到或者选择对应的表单元素上，用来增加用户体验

语法：

```html
<label for="sex">男</label>
<input type="radio" name="sex" id="sex" />
```

核心：`<label>`标签的 for 属性应当与相关元素的 id 属性相同



#### 3.4.3 `<select>`表单元素

​	使用场景：在页面中，如果有多个选项让用户选择，并且想要节约页面空间时，我们可以使用`<select>`标签控件定义下拉列表

语法：

```html
<select>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
    ...
</select>
```

1. `<select>`中至少包含一对`<option>`
2. 在`<option>`中定义`selected = "selected"`时，当前项即为默认选中项



#### 3.4.4 `<textarea>`表单元素

​	使用场景：当用户输入内容较多的情况下，我们就不能使用文本框表单了，此时我们可以使用`<textarea>`标签

​	在表单元素中，`textarea`标签适用于定义多行文本输入的控件

​	使用多行文本输入控件，可以输入更多的文字，该控件常见于留言板，评论

语法：

```html
<textarea row="3" col="20">
    文本内容
</textarea>
```

1. 通过`<textarea>`标签可以轻松地创建多行文本输入框
2. `cols="每行中的字符数"`,`row="显示的行数"`，我们在实际开发中不会使用，都是用 CSS 来改变大小



# 综合案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>注册界面案例</title>
</head>
<body>
    <h4>青春不常在，抓紧谈恋爱</h4>
    <table width="600">
        <tr>
            <td>性别</td>
            <td>
                <label for="man">男<input type="radio" name="sex" id="man">
                <img src="https://ts2.cn.mm.bing.net/th?id=OIP-C.XNaHSEoV-V10oVUe6u5v9AHaHc&w=249&h=250&c=8&rs=1&qlt=90&o=6&dpr=1.25&pid=3.1&rm=2" width="20"/>
                <label for="lady">&nbsp&nbsp&nbsp女<input type="radio" name="sex" id="lady">
                <img src="https://tse2-mm.cn.bing.net/th/id/OIP-C.Q0DFDdImaOLpxjX9bn18WgAAAA?w=181&h=181&c=7&r=0&o=5&dpr=1.25&pid=1.7" width="20"/>
            </td>
        </tr>
        <tr><td>生日</td>
            <td>
                <select name="---请选择出生年份---" id="birthyear">
                    <option>1998</option>
                    <option>1997</option>
                    <option>1999</option>
                    <option>2000</option>
                    <option>2001</option>
                    <option>2002</option>
                </select>
                <select name="---请选择出生月份---" id="birthmonth">
                    <option>1月</option>
                    <option>2月</option>
                    <option>3月</option>
                    <option>4月</option>
                    <option>5月</option>
                    <option>6月</option>
                    <option>7月</option>
                    <option>8月</option>
                    <option>9月</option>
                    <option>10月</option>
                    <option>11月</option>
                    <option>12月</option>
                </select>
            </td>
        </tr>
        <tr><td>所在地区</td>
            <td>
                <input type="text" name="请输入所在地区" value="北京东城区">
            </td>
        </tr>
        <tr><td>婚姻状况</td>
            <td>
                <label for="unmarried">未婚</label><input type="radio" name="Mstatus" id="unmarried">
                <label for="married">&nbsp&nbsp已婚</label><input type="radio" name="Mstatus" id="married">
                <label for="divorce">&nbsp&nbsp离婚</label><input type="radio" name="Mstatus" id="divorce">
            </td>
        </tr>
        <tr><td>学历</td>
            <td>
                <select name="education" id="education">
                    <option>幼儿园</option>
                    <option>小学</option>
                    <option>中学</option>
                    <option>大学本科</option>
                    <option>大学硕士</option>
                    <option>大学博士</option>
                </select>
            </td>
        </tr>
        <tr><td>喜欢的类型</td>
            <td>
                <label for="pure">清纯的</label><input type="checkbox" name="liketype" id="pure">
                <label for="sexy">&nbsp&nbsp性感的</label><input type="checkbox" name="liketype" id="sexy">
                <label for="honest">&nbsp&nbsp老实的</label><input type="checkbox" name="liketype" id="honest">
            </td>
        </tr>
        <tr><td>自我介绍</td>
            <td>
            <textarea rows="2" cols="30">介绍一下自己吧</textarea>
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <input type="button" value="免费注册">
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <input type="checkbox" name="agree_select" checked id="agree"><label for="agree">我同意所有条款</label>
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <a href="#" target="_blank">我是会员，立即登录</a>
            </td>
        </tr>
        <tr>
            <td></td>
            <td>
                <h4>我承诺</h4>
                <ul>
                    <li>年满18岁、单身</li>
                    <li>抱着严肃的态度</li>
                    <li>真诚寻找另一半</li>
                </ul>
            </td>
        </tr>
    </table>
</body>
</html>
```

