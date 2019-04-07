# JavaScript

## 语法

HTML 中的脚本必须位于 `<script>` 与 `</script> `标签之间。

脚本可被放置在 HTML 页面的 `<body>` 和 `<head>` 部分中。

通常的做法是把函数放入 `<head> `部分中，或者放在页面底部(操作dom需要先加载dom,所以放底部)。

 

### 格式

```javascript
<script type="text/javascript"> 现在已经不用写type了,浏览器都默认了
<!-- JavaScript代码-->
</script>
<!-- JavaScript代码 -->是为当老版本浏览器不识别时,把代码注释掉.
```

行内使用:`<body onload="alert('你好,js!');"></body>`

外部引用:`<script src="myScript.js">JavaScript代码1</script>`

注意:JavaScript代码1会被myScript.js覆盖,无法执行.

### 变量

对大小写敏感,命名规则和java相同

用var声明变量,无需变量类型,文本值用""或'',否则当成数值.可一次声明多个变量

```javascript
var pi=3.14;
var person="John Doe";
var answer='Yes I am!';
var lastname="Doe", age=30, job="carpenter";
```

**细节**:

1. 未赋值的变量其值是undefined.

2. 如果重新声明 JavaScript 变量，该变量的值不会丢失.

3. 变量可以重复赋不同类型的值

4. 不写var也可以声明变量,等效于window.变量名

5. 句尾的;可以省略,但不推荐省略.

### 数据类型

字符串（String）、数字(Number)、布尔(Boolean)、数组(Array)、对象(Object)、空（Null）、未定义（Undefined）、函数(Function)、日期(Date)。null表示值为空,用于清空值,undefined表示未定义或定义未赋值

 

### typeof操作符

```javascript
typeof "John"                // 返回 string 
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
typeof function () {}         // 返回 function
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```

**注意**:JavaScript中的`==`比较值,`===`比较值和类型.

 

### for in循环

`for(var num in nums){}`类似于for each,用于遍历
num是nums的下标或属性名
循环对象属性：

```javascript
var person = {fname:"John", lname:"Doe", age:25}; 
var text = "";
var x;
for (x in person) {
	text += person[x];
}
```

 text 输出结果为:`John Doe 25 `

 

### 函数定义

函数声明格式

```javascript
function functionName(parameters) {
  执行的代码
}
```

函数可以通过函数表达式定义一个匿名函数存在变量里使用

**实例**:

```javascript
//var myFunction = new Function("a", "b", "return a * b"); 不推荐使用,JavaScript应避免使用new
var myFunction = function (a, b) {return a * b}
var x = myFunction(4, 3);
```

匿名函数的声明和使用：( **function(a,b){return ab}** )()加粗部分为声明

 

每个函数都包含两个非继承而来的方法,call()方法和apply()方法

```javascript
apply([thisObj [,argArray] ]);
call([thisObj[,arg1 [,arg2 [,...,argn]]]]);
```

如果没有提供thisObj参数，那么Global对象被用于thisObj

```javascript
function add(c,d){
	return this.a + this.b + c + d;
}
var s = {a:1, b:2};
console.log(add.call(s,3,4)); // 1+2+3+4 = 10
console.log(add.apply(s,[5,6])); // 1+2+5+6 = 14 
```

 

### 对象

**定义**:对象由花括号分隔。在括号内部，对象的属性以名称和值对的形式 (name : value) 来定义。属性由逗号分隔：

```javascript
var person = {firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"};
name : value的键值对称为对象的属性.JavaScript 对象是属性变量的容器。
```

**访问对象属性**:1)`person.lastName`;   2)`person["lastName"]`;

**对象方法**:

你可以使用以下语法创建对象方法：

`methodName : function() { code lines }`

你可以使用以下语法访问对象方法：

`objectName.methodName()`

你可以使用以下语法获取对象方法定义函数的字符串:

`objectName.methodName`



## 函数

alert("提示信息");

prompt("提示信息","输入框默认信息(可选项)");返回输入的字符串

confirm("确认信息?");  确定返回true,取消返回否false

isNaN(value)数值返回false,参数值为 NaN 或字符串、对象、undefined等非数字值返回true

eval(string)计算 JavaScript 字符串，并把它作为脚本代码来执行。

 

### 类型转换

**parseInt**(string,radix) radix表示string是什么进制的数(范围2~36)

- 忽略字符串前面的空格，直至找到第一个非空字符,还会将数字后面的非数字的字符串去掉。
- 如果第一个字符不是数字符号或者负号，返回NaN
- 会将小数取整。（向下取整）

 

**parseFloat**(string)

```javascript
document.write(parseFloat("10") + "<br>");          //10
document.write(parseFloat("10.33") + "<br>");       //10.33
document.write(parseFloat("34 45 66") + "<br>");    //34
document.write(parseFloat(" 60 ") + "<br>");        //60
document.write(parseFloat("40 years") + "<br>");    //40
document.write(parseFloat("He was 40") + "<br>");   //NaN
```

与parseInt一样，唯一区别是parseFloat可以保留小数。

 

**toString**()对象共有函数,返回字符串

**String**(object)

 

**Number**(object) object可选,不写返回0

```javascript
document.write(Number(new Boolean(true))+ "<br>");
document.write(Number(new Boolean(false))+ "<br>");
document.write(Number(new Date())+ "<br>");
document.write(Number(String("999"))+ "<br>");
document.write(Number(String("999 888"))+ "<br>");
```

1. 如果转换的内容本身就是一个数值类型的字符串，那么将来在转换的时候会返回自己。
2. 如果转换的内容本身不是一个数值类型的字符串，那么在转换的时候结果是NaN.
3. 如果要转换的内容是空的字符串，那以转换的结果是0.
4. 如果是其它的字符，那么将来在转换的时候结果是NaN.

 

**Boolean**(object) 转换成boolean,除了false,"",0,NaN,undefined都转换成true

!!(object)也可以转换成boolean

 

### 隐式转换:

```javascript
5 + null    // 返回 5         null 转换为 0
"5" + null  // 返回"5null"   null 转换为 "null"
"5" + 1     // 返回 "51"      1 转换为 "1"  
"5" - 1     // 返回 4         "5" 转换为 5
```

- 与string进行+运算都转成string(b例外)
- 与Number进行运算能转换的都转换,不行的NaN
- if的判断条件会自动转换成boolean

 

## 异常和调试

### try catch throw

```javascript
try {
  //在这里运行代码
} catch(err) {
  //在这里处理错误
}
```

- err.message 获取错误信息
- throw exception ,exception可以使JavaScript字符串,数字,逻辑值或对象

### 调试

- chrome浏览器可用,console.log()方法,用F12打开调试窗口,看console界面
- debugger;可以设置断点,只有在调试时才会产生作用.

 

## 事件

**语法**:`<some-HTML-element some-event="some JavaScript">`

| **事件**    | **描述**                                 |
| ----------- | ---------------------------------------- |
| onclick     | 用户点击   HTML 元素                     |
| ondblclick  | 当用户双击某个对象时调用的事件句柄。     |
| onmouseover | 用户在一个HTML元素上移动鼠标             |
| onmouseout  | 用户从一个HTML元素上移开鼠标             |
| onkeydown   | 用户按下键盘按键                         |
| onload      | 浏览器已完成页面的加载                   |
| onunload    | 用户退出页面。   ( <body> 和 <frameset>) |
| onfocus     | 元素获取焦点时触发(光标进入)             |
| onblur      | 元素失去焦点时触发(光标离开)             |
| onchange    | HTML   元素改变                          |
| onsubmit    | 表单提交时触发                           |
| onvalid     | input元素内的值无效时触发的事件          |

 

### event对象

| **属性**                                                     | **描述**                                       |
| ------------------------------------------------------------ | ---------------------------------------------- |
| [bubbles](http://www.runoob.com/jsref/event-bubbles.html)    | 返回布尔值，指示事件是否是起泡事件类型。       |
| [cancelable](http://www.runoob.com/jsref/event-cancelable.html) | 返回布尔值，指示事件是否可拥可取消的默认动作。 |
| [currentTarget](http://www.runoob.com/jsref/event-currenttarget.html) | 返回其事件监听器触发该事件的元素。             |
| eventPhase                                                   | 返回事件传播的当前阶段。                       |
| [target](http://www.runoob.com/jsref/event-target.html)      | 返回触发此事件的元素（事件的目标节点）。       |
| [timeStamp](http://www.runoob.com/jsref/event-timestamp.html) | 返回事件生成的日期和时间。                     |
| [type](http://www.runoob.com/jsref/event-type.html)          | 返回当前   Event 对象表示的事件的名称。        |

| **方法**          | **描述**                                 |
| ----------------- | ---------------------------------------- |
| initEvent()       | 初始化新创建的   Event 对象的属性。      |
| preventDefault()  | 通知浏览器不要执行与事件关联的默认动作。 |
| stopPropagation() | 不再派发事件。只能中断冒泡阶段           |



**相关方法**

`element.addEventListener(event, function, useCapture)`

event:    事件名去掉"on"

useCapture:    true在捕获阶段执行,false(默认)在冒泡阶段执行.

1. 事件检测先从外到内检测触发位置,这个过程称为捕获,之后逐层向外返回,称为冒泡阶段.

2. true的触发顺序总是在false之前；如果多个均为true，则外层的触发先于内层；如果多个均为false，则内层的触发先于外层.

 

 

## 对象

### Date对象:

```javascript
var d = new Date();当前时间
var d = new Date(milliseconds);
var d = new Date(dateString);"October 13, 1975 11:13:00"
var d = new Date(year, month, day, hours, minutes, seconds, milliseconds);
```

| **方法**                                                     | **描述**                                      |
| ------------------------------------------------------------ | --------------------------------------------- |
| [getDate()](http://www.runoob.com/jsref/jsref-getdate.html)  | 从 Date   对象返回一个月中的某一天 (1 ~ 31)。 |
| [getDay()](http://www.runoob.com/jsref/jsref-getday.html)    | 从 Date   对象返回一周中的某一天 (0 ~ 6)。    |
| [getFullYear()](http://www.runoob.com/jsref/jsref-getfullyear.html) | 从 Date   对象以四位数字返回年份。            |
| [getHours()](http://www.runoob.com/jsref/jsref-gethours.html) | 返回 Date   对象的小时 (0 ~ 23)。             |
| [getMilliseconds()](http://www.runoob.com/jsref/jsref-getmilliseconds.html) | 返回 Date   对象的毫秒(0 ~ 999)。             |
| [getMinutes()](http://www.runoob.com/jsref/jsref-getminutes.html) | 返回 Date   对象的分钟 (0 ~ 59)。             |
| [getMonth()](http://www.runoob.com/jsref/jsref-getmonth.html) | 从 Date   对象返回月份 (0 ~ 11)。             |
| [getSeconds()](http://www.runoob.com/jsref/jsref-getseconds.html) | 返回 Date   对象的秒数 (0 ~ 59)。             |
| [getTime()](http://www.runoob.com/jsref/jsref-gettime.html)  | 返回 1970   年 1 月 1 日至今的毫秒数。        |

 

### Array对象

```javascript
var cars=new Array("Saab","Volvo","BMW");
var cars=["Saab","Volvo","BMW"];
var cars=new Array(5); 返回一个元素值为undefined,长度为5的数组
var cars=new Array(); 返回length为0的数组
cars[0]="Saab"; 可以这样向空数组加对象
cars[1]="Volvo";
cars[2]="BMW";
```

**二维数组定义**

```javascript
Var aa=new Array(); //定义一维数组 
for(i=1;i<=10;i++) 
{ 
    aa[i]=new Array(); //将每一个子元素又定义为数组 
    for(n=0;n<=10;n++) 
    { 
        aa[i][n]=i+n; //此时aa[i][n]可以看作是一个二级数组 
    } 
} 
或者:Var aa =[[2,3],[5,6]];
```

| **方法**                                                     | **描述**                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| [join(separator)](http://www.runoob.com/jsref/jsref-join.html) | 把数组的所有元素放入一个字符串返回。             |
| [push(参数列表)](http://www.runoob.com/jsref/jsref-push.html) | 向数组的末尾添加一个或更多元素，并返回新的长度。 |
| [sort(sortfunction)](http://www.runoob.com/jsref/jsref-sort.html) | 对数组的元素进行排序。function为比较方法         |

## 小知识点

### hosting:在变量与函数的作用域内，不管变量与函数在何处声明，都会被提升到作用域的顶部，但是变量初始化的顺序不变**。**

```javascript
var myvar = '变量值'; 
(function() { 
    console.log(myvar); // undefined 
    var myvar = '内部变量值'; 
})();
```

等效于

```javascript
var myvar = '变量值'; 
(function() { 
    var myvar; 
    console.log(myvar); // undefined 
    myvar = '内部变量值'; 
})();
```

 

### ||和&&的用法

`var c=a||b`;其实等效于`(a||b)?a:b`;

`var c=a&&b`;其实等效于`(a&&b)?b:a`;

**理解**:由于JavaScript中所有值都可以判断其boolean,所以传True/False和传真值等效.

`a||b和a&&b`只需传最终决定boolean值的项.

 

### {}和[]的使用

{ } 大括号，表示定义一个对象，大部分情况下要有成对的属性和值，或是函数。 

如：`var LangShen = {"Name":"Langshen","AGE":"28"}; `

访问属性可以用`LangShen.Name`或`LangShen["Name"]`

 

### 事件绑定的function的this

JS中绑定:`$("sometag").click(somefunction);  `

html中绑定:`<div onclick='somefunction()'>  `

两种方式的区别是：前一种绑定方式，自动的把当前标签当作this元素传递给somefunction，在后一种方式不会把当前元素传递给somefunction函数， 如果需要，可以将当前元素当作somefunction的参数传入，如：

`<div onclick='somefunction(this)'>  `

在somefunction 中用一个参数（参数名不能是this，会js自己的this冲突）捕获this就可以了。

 

### setInterval(time(),1000)为什么只执行一次

该方法有两个重载setInterval(func,millisec)和setInterval(code,millisec)

time()是函数的调用,是函数time的返回结果,其实是执行了第二个方法

time才是function变量,所以应该写成setInterval(time,1000)

或者可以写成setInterval("time()",1000)调用第二个方法来实现

### JavaScript特性

解释性 用于客户端 基于对象

### function的自执行

```javascript
(function() { /* code */ })();  或者 (function () { /* code */ } ()); 
```

### eval()的巧用

可以把字符串拼接的json数据两边的""去掉,使其成为json数据

### JS的操作行内style的方法

document.getElementById("id").style.cssText    可以获取行内所有的css属性,对其赋值或者制空来设置或清空行内样式.

### js操作cookie的坑

cookie不同path下允许存在同名cookie,不同浏览器对于cookie的默认path处理不同

所以在用js设置cookie时的path和删除时的path容易因为不一致时会导致操作失效,推荐在操作cookie时带上path