# JQuery

## 语法

`$(selector).action();`

- 工厂函数​$()：将DOM对象转化为jQuery对象
- 选择器selector：获取需要操作的DOM元素
- 方法action()：jQuery中提供的方法，包括绑定事件处理的方法



1. `“$”`等同于`“jQuery”`
    - `$(document).ready()`等同于`jQuery(document).ready()`
2. `$(function() {});`是`$(document).ready(function(){ })`的简写
3. this，如果是取得元素的话，则对应当前元素的对象
    - `$(this)`则是当前元素被jQuery处理的对象。

## 事件

| **鼠标事件** | **键盘事件**                        | **表单事件**  | **文档/窗口事件** |
| ------------ | ----------------------------------- | ------------- | ----------------- |
| click        | keypress(按住不断触发,即产生字符时) | submit        | load              |
| dblclick     | keydown(按住只触发一次)             | change        | resize            |
| mouseenter   | keyup                               | focus获得焦点 | scroll            |
| mouseleave   | 指针离开被选元素时                  | blur失去焦点  | unload            |
| mousemove    | 鼠标指针离开任意子元素时也会被触发  |               |                   |

**事例**

```javascript
$("p").click(function(){
    // 动作触发后执行的代码!!
});
```

### 事件绑定

```javascript
$(selector).bind(event,data,function,map)
$(selector).on(event,childSelector,data,function,map)
```

| *event*    | 必需。规定添加到元素的一个或多个事件。   由空格分隔多个事件值。必须是有效的事件。 |
| ---------- | ------------------------------------------------------------ |
| *data*     | 可选。规定传递到函数的额外数据。                             |
| *function* | 必需。规定当事件发生时运行的函数。                           |
| *map*      | 规定事件映射   (*{event:function, event:function, ...})*，包含要添加到元素的一个或多个事件，以及当事件发生时运行的函数。 |

**例如:**

```javascript
1,多个事件
$("p").bind("mouseover mouseout",function(){
$("p").toggleClass("intro");
});
2,事件映射
$("button").bind({
click:function(){$("p").slideToggle();},
mouseover:function(){$("body").css("background-color","#E9E9E4");},  
mouseout:function(){$("body").css("background-color","#FFFFFF");}  
});
```

### 事件解绑

```javascript
$(selector).unbind(event,function,eventObj)
$(selector).off(event,selector,function(eventObj),map)
```

| **参数**   | **描述**                                                     |
| ---------- | ------------------------------------------------------------ |
| *event*    | unbind可选,off必需。规定一个或多个要从元素上移除的事件。   由空格分隔多个事件值。   如果只规定了该参数，则会删除绑定到指定事件的所有函数。 |
| *function* | 可选。规定从元素上指定事件取消绑定的函数名称。               |
| *eventObj* | 可选。规定要使用的移除的   event 对象。这个 *eventObj* 参数来自事件绑定函数。 |

## 动画效果

### 隐藏/显示

```javascript
$(selector).hide(speed,callback);隐藏
$(selector).show(speed,callback);显示
$(selector).toggle(speed,callback);切换
```

可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。

可选的 callback 参数是隐藏或显示完成后所执行的函数名称。

### 淡入/淡出

- jQuery fadeIn()/fadeOut()方法
- jQuery fadeIn() 用于淡入已隐藏的元素。

**语法**:

```javascript
$(selector).fadeIn(speed,callback);
```

可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

### 向下滑动/向上滑动

- slideDown()/slideUp()方法
- jQuery slideDown() 方法用于向下滑动元素。

语法:

```javascript
$(selector).slideDown(speed,callback);
```

可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。

jQuery slideToggle()

### 自定义动画

```javascript
(selector).animate({styles},speed,easing,callback)
```

例如:

```javascript
$("button").click(function(){
	$("#box").animate({height:"300px"});
});
```

### 停止动画

`$(selector).stop(stopAll,goToEnd)`  两个可选参数都默认false

## jQuery HTML/CSS

### 查找节点

三个简单实用的用于 DOM 操作的 jQuery 方法：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素标签的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值
- attr(attribute) - 设置或返回被选元素的属性/值

 

### 添加元素

#### 在被选元素的结尾插入内容

```javascript
$(selector).append(content,function(index,html))
```

#### 在被选元素的结尾插入 HTML 元素

```javascript
$(content).appendTo(selector)
```

content:必需。规定要插入的内容（必须包含 HTML 标签）。

如果 content 是已存在的元素，它将从它的当前位置被移除，并被插入在被选元素之后。

#### 在被选元素的开头插入内容

```javascript
$(selector).prepend(content,function(index,html))
```

#### 在被选元素的开头插入 HTML 元素

```javascript
$(content).prependTo(selector)  
```

content:必需。规定要插入的内容（必须包含 HTML 标签）。

如果 content 是已存在的元素，它将从它的当前位置被移除，并被插入在被选元素之后。

#### 在被选元素之后插入内容

```javascript
$(selector).after(content,function(index))
```

#### 在被选元素后插入 HTML 元素

```javascript
$(content).insertAfter(selector)
```

content:必需。规定要插入的内容（必须包含 HTML 标签）。

如果 content 是已存在的元素，它将从它的当前位置被移除，并被插入在被选元素之后。

#### 在被选元素之前插入内容

```javascript
$(selector).**before**(content,function(index))
```

#### 在被选元素前插入 HTML 元素

```javascript
$(content).**insertBefore**(selector)
```

content:必需。规定要插入的内容（必须包含 HTML 标签）。

如果 content 是已存在的元素，它将从它的当前位置被移除，并被插入在被选元素之后。

 

### 删除元素

移除被选元素（包含数据和事件）

```javascript
$(selector).remove()
```

从被选元素移除所有子节点和内容

```javascript
$(selector).empty()
```

 

### 替换,复制

把被选元素替换为新的 HTML 元素

```javascript
$(content).replaceAll(selector)  必须包含 HTML 标签
```

把被选元素替换为新的内容

```javascript
$(selector).replaceWith(content,function(index))
```

生成被选元素的副本,包含子节点、文本和属性。

```javascript
$(selector).clone(true|false)  是否需复制事件处理程序。
```

 

### CSS

1,向被选元素添加一个或多个类名

```javascript
$(selector).addClass(classname,function(index,oldclass))
```

2,从被选元素移除一个或多个类

```javascript
$(selector).removeClass(classname,function(index,currentclass))  
```

规定要移除的一个或多个类名称。如需移除若干个类，请使用空格分隔类名称。如果该参数为空，则将移除所有类名称。

## JSON

**JSON 语法规则**

JSON 语法是 JavaScript 对象表示法语法的子集。

- 数据在名称/值对中 ` "name": "菜鸟教程"`
- 数据由逗号分隔  `{ "name":"菜鸟教程" , "url":"www.runoob.com" }`
- 大括号保存对象  
- 中括号保存数组  

**JSON 值**

JSON 值可以是：

- 数字（整数或浮点数）
- 字符串（在双引号中）
- 逻辑值（true 或 false）  `{ "flag":true }`
- 数组（在中括号中）`[]`
- 对象（在大括号中）`{}`
- null  `{"runoob":null }`

## 表单

```javascript
$("form").serialize()
```

通过序列化表单值创建 URL 编码文本字符串:FirstName=Mickey&LastName=Mouse

```javascript
$("form").serializeArray()
```

通过序列化表单值来创建对象（name 和 value）的数组。对象数组[{name:,value:},…]

### 表单提交的三种方法

```javascript
//ajax提交  
$("#ajaxBtn").click(function() {  
    var params = $("#myform").serialize();  
    $.ajax( {  
        type : "POST",  
        url : "RegisterAction.action",  
        data : params,  
        success : function(msg) {  
        	alert("success: " + msg);  
        }  
    });  
})  

//jQuery提交  
$("#jqueryBtn").click(function(){
    $("#myFormId").attr("action", "userinfo.shtml");  
    $("#myform").submit();  
})  

//js提交   
$("#jsBtn").click(function(){  
    document.myform.action="RegisterAction.action";     
    document.myform.submit();     
})  
```

### JQuery遍历两种each方法

**each循环方法规定为每个匹配元素规定运行的函数。**

```javascript
$(selector).each(function(index,element))
```

- index - 选择器的 index 位置(匹配的节点数组的索引)
- element - 当前的元素（也可使用 "this" 选择器）

 

**$.each方法,遍历对象的子元素为其执行的方法**

遍历对象(有附加参数)：

```javascript
$.each(Object, function(p1, p2) {
      this;       //这里的this指向每次遍历中Object的当前属性值
      p1; p2;     //访问附加参数
 }, ['参数1', '参数2']);
```

 

遍历数组(有附件参数)：

```javascript
$.each(Array, function(p1, p2){
      this;       //这里的this指向每次遍历中Array的当前元素
      p1; p2;     //访问附加参数
 }, ['参数1', '参数2']);
```

 

遍历对象(没有附加参数)

```javascript
$.each(Object, function(name, value) {
      this;      //this指向当前属性的值
      name;      //name表示Object当前属性的名称
      value;     //value表示Object当前属性的值
 });
```

 

遍历数组(没有附加参数)

```javascript
$.each(Array, function(i, value) {
    this;      //this指向当前元素
    i;         //i表示Array当前下标
    value;     //value表示Array当前元素
});
```

