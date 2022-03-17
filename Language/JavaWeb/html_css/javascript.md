JavaScript 能够改变 HTML 内容
JavaScript 能够改变 HTML 属性
JavaScript 能够改变 HTML 样式 (CSS)
JavaScript 能够隐藏 HTML 元素
JavaScript 能够显示 HTML 元素

<script> 标签
在 HTML 中，JavaScript 代码必须位于 <script> 与 </script> 标签之间。

JavaScript 函数和事件
JavaScript 函数是一种 JavaScript 代码块，它可以在调用时被执行。
例如，当发生事件时调用函数，比如当用户点击按钮时。


<head> 或 <body> 中的 JavaScript
您能够在 HTML 文档中放置任意数量的脚本。
脚本可被放置与 HTML 页面的 <body> 或 <head> 部分中，或兼而有之。

外部脚本
脚本可放置与外部文件中：


外部 JavaScript 的优势
在外部文件中放置脚本有如下优势：
分离了 HTML 和代码
使 HTML 和 JavaScript 更易于阅读和维护
已缓存的 JavaScript 文件可加速页面加载
如需向一张页面添加多个脚本文件 - 请使用多个 script 标签：


外部引用
可通过完整的 URL 或相对于当前网页的路径引用外部脚本：

JavaScript 显示方案
JavaScript 能够以不同方式“显示”数据：
使用 window.alert() 写入警告框
使用 document.write() 写入 HTML 输出
使用 innerHTML 写入 HTML 元素
使用 console.log() 写入浏览器控制台

使用 innerHTML
如需访问 HTML 元素，JavaScript 可使用 document.getElementById(id) 方法。
id 属性定义 HTML 元素。innerHTML 属性定义 HTML 内容：



使用 document.write()
出于测试目的，使用 document.write() 比较方便：
注意：在 HTML 文档完全加载后使用 document.write() 将删除所有已有的 HTML ：


JavaScript 程序
计算机程序是由计算机“执行”的一系列“指令”。
在编程语言中，这些编程指令被称为语句。
JavaScript 程序就是一系列的编程语句。
注释：在 HTML 中，JavaScript 程序由 web 浏览器执行。


JavaScript 行长度和折行
为了达到最佳的可读性，程序员们常常喜欢把代码行控制在 80 个字符以内。
如果 JavaScript 语句太长，对其进行折行的最佳位置是某个运算符：


JavaScript 代码块
JavaScript 语句可以用花括号（{...}）组合在代码块中。
代码块的作用是定义一同执行的语句。
您会在 JavaScript 中看到成块组合在一起的语句：


JavaScript 关键词
JavaScript 语句常常通过某个关键词来标识需要执行的 JavaScript 动作。

下面的表格列出了一部分将在教程中学到的关键词：

关键词				描述
break			终止 switch 或循环。
continue		跳出循环并在顶端开始。
debugger		停止执行 JavaScript，并调用调试函数（如果可用）。
do ... while	执行语句块，并在条件为真时重复代码块。
for				标记需被执行的语句块，只要条件为真。
function		声明函数。
if ... else		标记需被执行的语句块，根据某个条件。
return			退出函数。
switch			标记需被执行的语句块，根据不同的情况。
try ... catch	对语句块实现错误处理。
var				声明变量。


JavaScript 值
JavaScript 语句定义两种类型的值：混合值和变量值。

混合值被称为字面量（literal）。变量值被称为变量。


JavaScript 字面量
书写混合值最重要的规则是：
写数值有无小数点均可：
字符串是文本，由双引号或单引号包围：


JavaScript 变量
在编程语言中，变量用于存储数据值。
JavaScript 使用 var 关键词来声明变量。
= 号用于为变量赋值。

JavaScript 变量
JavaScript 变量是存储数据值的容器。

JavaScript 标识符
所有 JavaScript 变量必须以唯一的名称的标识。
这些唯一的名称称为标识符。
标识符可以是短名称（比如 x 和 y），或者更具描述性的名称（age、sum、totalVolume）。
构造变量名称（唯一标识符）的通用规则是：
名称可包含字母、数字、下划线和美元符号
名称必须以字母开头
名称也可以 $ 和 _ 开头（但是在本教程中我们不会这么做）
名称对大小写敏感（y 和 Y 是不同的变量）
保留字（比如 JavaScript 的关键词）无法用作变量名称
提示：JavaScript 标识符对大小写敏感。

赋值运算符
在 JavaScript 中，等号（=）是赋值运算符，而不是“等于”运算符。


JavaScript 数据类型
JavaScript 变量可存放数值，比如 100，以及文本值，比如 "Bill Gates"。
在编程中，文本值被称为字符串。
JavaScript 可处理多种数据类型，但是现在，我们只关注数值和字符串值。
字符串被包围在双引号或单引号中。数值不用引号。
如果把数值放在引号中，会被视作文本字符串。

一条语句，多个变量
您可以在一条语句中声明许多变量。
以 var 作为语句的开头，并以逗号分隔变量
声明可横跨多行：
```js
var person = "Bill Gates",
carName = "porsche",
price = 15000;
```

Value = undefined
在计算机程序中，被声明的变量经常是不带值的。值可以是需被计算的内容，或是之后被提供的数据，比如数据输入。
不带有值的变量，它的值将是 undefined。
变量 carName 在这条语句执行后的值是 undefined

重复声明 JavaScript 变量
如果再次声明某个 JavaScript 变量，将不会丢它的值。
在这两条语句执行后，变量 carName 的值仍然是 "porsche"：


ECMAScript 2015
ES2015 引入了两个重要的 JavaScript 新关键词：let 和 const。
这两个关键字在 JavaScript 中提供了块作用域（Block Scope）变量（和常量）。
在 ES2015 之前，JavaScript 只有两种类型的作用域：全局作用域和函数作用域。


JavaScript 块作用域
通过 var 关键词声明的变量没有块作用域。
在块 {} 内声明的变量可以从块之外进行访问。


重新声明变量
使用 var 关键字重新声明变量会带来问题。
在块中重新声明变量也将重新声明块外的变量

使用 let 关键字重新声明变量可以解决这个问题。
在块中重新声明变量不会重新声明块外的变量：：

HTML 中的全局变量
使用 JavaScript 的情况下，全局作用域是 JavaScript 环境。
在 HTML 中，全局作用域是 window 对象。
通过 var 关键词定义的全局变量属于 window 对象：

提升
通过 var 声明的变量会提升到顶端。如果您不了解什么是提升（Hoisting），请学习我们的提升这一章。


















