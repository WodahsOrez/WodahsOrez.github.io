# CSS

## **语法**

### 行内样式

```css
<h1 style="color:red; font-size:14px;">行内样式</h1>
```

### 内嵌样式

```css
<style type = "text/css">
    选择器{
       属性1:值;
       属性2:值;
    }
</style>
```

### 外部样式表

链接式：`<link type="text/css" rel="stylesheet" href="css文件路径">`

特点：属于xhtml，先加载CSS。

导入式：`<style type="text/css">@import url("css文件路径")</style>`

特点：属于CSS，先加载HTML后加载样式。

注释：`/*注释内容*/`

### 多重样式优先级

行内样式 > 内嵌样式 > 外部样式 > 浏览器默认样式

## 选择器

| **选择器**                 | **示例**              | **示例说明**                                                 |
| -------------------------- | --------------------- | ------------------------------------------------------------ |
| .*class*                   | .intro                | 选择所有class="intro"的元素                                  |
| #*id*                      | #firstname            | 选择所有id="firstname"的元素                                 |
| *                          | *                     | 选择所有元素                                                 |
| *element*                  | p                     | 并集选择器，选择所有`<p>`元素                                |
| *element,element*          | div,p                 | 选择所有`<div>`元素和`<p>`元素                               |
| *element* *element*        | div p                 | 后代选择器，选择`<div>`元素内的所有`<p>`元素   （不限层级）  |
| *element*>*element*        | div>p                 | 子元素选择器，选择所有父级是`<div>` 元素的`<p> `元素   （限第一层后代） |
| *element*+*element*        | div+p                 | 相邻兄弟选择器，选择所有紧接着`<div>`元素之后的`<p>`元素（限相邻的第一个同级别元素） |
| *element1*~*element2*      | p~ul                  | 后续兄弟选择器，选择p元素之后的每一个ul元素（不需要相邻，且所有ul元素） |
| [*attribute*]              | [target]              | 选择所有带有target属性元素                                   |
| [*attribute*=*value*]      | [target=-blank]       | 选择所有使用target="-blank"的元素                            |
| [*attribute*~=*value*]     | [title~=flower]       | 选择标题属性包含单词"flower"的所有元素                       |
| [*attribute*\|=*language*] | [lang\|=en]           | 选择 lang属性以en为开头的所有元素                            |
| :link                      | a:link                | 选择所有未访问链接                                           |
| :visited                   | a:visited             | 选择所有访问过的链接                                         |
| :active                    | a:active              | 选择活动链接                                                 |
| :hover                     | a:hover               | 选择鼠标在链接上面时                                         |
| :focus                     | input:focus           | 选择具有焦点的输入元素                                       |
| :first-letter              | p:first-letter        | 选择每一个`<P>`元素的第一个字母                              |
| :first-                    | p:first-line          | 选择每一个`<P>`元素的第一行                                  |
| :first-child               | p:first-child         | 指定只有当`<p>`元素是其父级的第一个子级的样式。              |
| :before                    | p:before              | 在每个`<p>`元素之前插入内容                                  |
| :after                     | p:after               | 在每个`<p>`元素之后插入内容                                  |
| :lang(*language*)          | p:lang(it)            | 选择一个lang属性的起始值="it"的所有<p>元素                   |
| [*attribute*^=*value*]     | a[src^="https"]       | 选择每一个src属性的值以"https"开头的元素                     |
| [*attribute*$=*value*]     | a[src$=".pdf"]        | 选择每一个src属性的值以".pdf"结尾的元素                      |
| [*attribute**=*value*]     | a[src*="runoob"]      | 选择每一个src属性的值包含子字符串"runoob"的元素              |
| :first-of-type             | p:first-of-type       | 选择每个p元素是其父级的第一个p元素                           |
| :last-of-type              | p:last-of-type        | 选择每个p元素是其父级的最后一个p元素                         |
| :only-of-type              | p:only-of-type        | 选择每个p元素是其父级的唯一p元素                             |
| :only-child                | p:only-child          | 选择每个p元素是其父级的唯一子元素                            |
| :nth-child(*n*)            | p:nth-child(2)        | 选择每个p元素是其父级的第二个子元素                          |
| :nth-last-child(*n*)       | p:nth-last-child(2)   | 选择每个p元素的是其父级的第二个子元素，从最后一个子项计数    |
| :nth-of-type(*n*)          | p:nth-of-type(2)      | 选择每个p元素是其父级的第二个p元素                           |
| :nth-last-of-type(*n*)     | p:nth-last-of-type(2) | 选择每个p元素的是其父级的第二个p元素，从最后一个子项计数     |
| :last-child                | p:last-child          | 择每个p元素是其父级的最后一个子级。                          |
| :root                      | :root                 | 选择文档的根元素                                             |
| :empty                     | p:empty               | 选择每个没有任何子级的p元素（包括文本节点）                  |
| :target                    | #news:target          | 选择当前活动的#news元素（包含该锚名称的点击的URL）           |
| :enabled                   | input:enable          | 选择每一个已启用的输入元素                                   |
| :disabled                  | input:disabled        | 选择每一个禁用的输入元素                                     |
| :checked                   | input:checked         | 选择每个选中的输入元素                                       |
| :not(*selector*)           | :not(p)               | 选择每个并非p元素的元素                                      |
| ::selection                | ::selection           | 匹配元素中被用户选中或处于高亮状态的部分                     |
| :out-of-range              | :out-of-range         | 匹配值在指定区间之外的input元素                              |
| :in-range                  | :in-range             | 匹配值在指定区间之内的input元素                              |
| :read-write                | :read-write           | 用于匹配可读及可写的元素                                     |
| :read-only                 | :read-only            | 用于匹配设置   "readonly"（只读） 属性的元素                 |
| :optional                  | :optional             | 用于匹配可选的输入元素                                       |
| :required                  | :required             | 用于匹配设置了   "required" 属性的元素                       |
| :valid                     | :valid                | 用于匹配输入值为合法的元素                                   |
| :invalid                   | :invalid              | 用于匹配输入值为非法的元素                                   |

## 属性

### 字体属性

| **Property**                                                 | **描述**                             | **值**                        |
| ------------------------------------------------------------ | ------------------------------------ | ----------------------------- |
| [font-style](http://www.runoob.com/cssref/pr-font-font-style.html) | 指定文本的字体样式                   | normal/italic/oblique/inherit |
| [font-variant](http://www.runoob.com/cssref/pr-font-font-variant.html) | 以小型大写字体或者正常字体显示文本。 | normal/small-caps/inherit     |
| [font-weight](http://www.runoob.com/cssref/pr-font-weight.html) | 指定字体的粗细。                     | normal/bold/bolder/lighter    |
| [font-size](http://www.runoob.com/cssref/pr-font-font-size.html) | 指定文本的字体大小                   | 必须                          |
| [font-family](http://www.runoob.com/cssref/pr-font-font-family.html) | 指定文本的字体系列                   | 必须                          |
| [font](http://www.runoob.com/cssref/pr-font-font.html)       | 在一个声明中设置所有的字体属性       |                               |

### 文本属性

| **属性**                                                     | **描述**                 | **值**                               |
| ------------------------------------------------------------ | ------------------------ | ------------------------------------ |
| [color](http://www.runoob.com/cssref/pr-text-color.html)     | 设置文本颜色             | rgba(225,225,225,0.5)红,绿,蓝,透明度 |
| [line-height](http://www.runoob.com/cssref/pr-dim-line-height.html) | 设置行高,行间距          |                                      |
| [text-align](http://www.runoob.com/cssref/pr-text-text-align.html) | 对齐元素中的文本         | left,center,right                    |
| [text-decoration](http://www.runoob.com/cssref/pr-text-text-decoration.html) | 向文本添加修饰           | underline,line-through,none          |
| [direction](http://www.runoob.com/cssref/pr-text-direction.html) | 设置文本方向。           |                                      |
| [letter-spacing](http://www.runoob.com/cssref/pr-text-letter-spacing.html) | 设置字符间距             |                                      |
| [text-indent](http://www.runoob.com/cssref/pr-text-text-indent.html) | 缩进元素中文本的首行     |                                      |
| [text-shadow](http://www.runoob.com/cssref/css3-pr-text-shadow.html) | 设置文本阴影             |                                      |
| [text-transform](http://www.runoob.com/cssref/pr-text-text-transform.html) | 控制元素中的字母         | capitalize首字母大写                 |
| [unicode-bidi](http://www.runoob.com/cssref/pr-text-unicode-bidi.html) | 设置或返回文本是否被重写 |                                      |
| [vertical-align](http://www.runoob.com/cssref/pr-pos-vertical-align.html) | 设置元素的垂直对齐       |                                      |
| [white-space](http://www.runoob.com/cssref/pr-text-white-space.html) | 设置元素中空白的处理方式 | 除非遇到<br>或块级,内部不换行        |
| [word-spacing](http://www.runoob.com/cssref/pr-text-word-spacing.html) | 设置字间距               |                                      |
| display                                                      | 设置元素应该生成的框类型 | block块级                            |

### 背景属性

| **Property**                                                 | **描述**                                     | **值**                                              |
| ------------------------------------------------------------ | -------------------------------------------- | --------------------------------------------------- |
| [background](http://www.runoob.com/cssref/css3-pr-background.html) | 简写属性，作用是将背景属性设置在一个声明中。 | 顺序如表排列顺序                                    |
| [background-color](http://www.runoob.com/cssref/pr-background-color.html) | 设置元素的背景颜色。                         |                                                     |
| [background-image](http://www.runoob.com/cssref/pr-background-image.html) | 把图像设置为背景。                           | url("URL")                                          |
| [background-position](http://www.runoob.com/cssref/pr-background-position.html) | 设置背景图像的起始位置。                     | x:left,center,right,值,%   y:top,middle,bottom,值,% |
| [background-repeat](http://www.runoob.com/cssref/pr-background-repeat.html) | 设置背景图像是否及如何重复。                 | (no-)repeat(-x/y)                                   |
| [background-attachment](http://www.runoob.com/cssref/pr-background-attachment.html) | 背景图像是否固定或者随着页面的其余部分滚动。 | scroll,fixed                                        |

### 列表属性

| **属性**                                                     | **描述**                                           | **值**                     |
| ------------------------------------------------------------ | -------------------------------------------------- | -------------------------- |
| [list-style](http://www.runoob.com/cssref/pr-list-style.html) | 简写属性。用于把所有用于列表的属性设置于一个声明中 | none                       |
| [list-style-type](http://www.runoob.com/cssref/pr-list-style-type.html) | 设置列表项标志的类型。                             | disc,circle,square,decimal |
| [list-style-image](http://www.runoob.com/cssref/pr-list-style-image.html) | 将图象设置为列表项标志。                           |                            |
| [list-style-position](http://www.runoob.com/cssref/pr-list-style-position.html) | 设置列表中列表项标志的位置。                       |                            |

### 超链接伪类

| [:link](http://www.runoob.com/cssref/sel-link.html)       | a:link    | 选择所有未访问链接     |
| --------------------------------------------------------- | --------- | ---------------------- |
| [:visited](http://www.runoob.com/cssref/sel-visited.html) | a:visited | 选择所有访问过的链接   |
| [:hover](http://www.runoob.com/cssref/sel-hover.html)     | a:hover   | 把鼠标放在链接上的状态 |
| [:active](http://www.runoob.com/cssref/sel-active.html)   | a:active  | 选择正在活动链接       |

CSS规定:hover必须在link和visited后,active必须在hover后

### input伪类

| [:valid](http://www.runoob.com/cssref/sel-valid.html)     | input:valid   | 选择所有有效值的属性   |
| --------------------------------------------------------- | ------------- | ---------------------- |
| [:invalid](http://www.runoob.com/cssref/sel-invalid.html) | input:invalid | 选择所有无效的元素     |
| [:focus](http://www.runoob.com/cssref/sel-focus.html)     | input:focus   | 选择具有焦点的输入元素 |

### 鼠标cursor属性

| **值**    | **描述**                                                     |
| --------- | ------------------------------------------------------------ |
| *url*     | 需使用的自定义光标的 URL。   注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。 |
| default   | 默认光标（通常是一个箭头）                                   |
| text      | 此光标指示文本。                                             |
| move      | 此光标指示某对象可被移动。                                   |
| crosshair | 光标呈现为十字线。                                           |
| pointer   | 光标呈现为指示链接的指针（一只手）                           |
| wait      | 此光标指示程序正忙（通常是一只表或沙漏）。                   |
| help      | 此光标指示可用的帮助（通常是一个问号或一个气球）。           |

### 盒子模型

![box-model](images/CSS/box-model.gif)

Margin(外边距) - 清除边框外的区域，外边距是透明的。

auto浏览器计算边距

Padding(内边距) - 清除内容周围的区域，内边距是透明的。

顺序:上 右 下 左(省略的左等于右,下等于上)

Border(边框) - 围绕在内边距和内容外的边框。

border:5px solid red;

Content(内容) - 盒子的内容，显示文本和图像。width和height指的是content的大小.

### 浮动属性

float:left/right/none

clear:left/right/both/none 指定周围不允许有浮动元素

overflow:hidden/visible/scroll/auto 指定内容溢出时的处理方式,父元素设为hidden可以清除浮动

#### float对宽度的影响

默认情况下，div的宽度是自适应auto的，在没有设置float的情况下，宽度是自动调整最大化，设置float之后，宽度自动调整最小化。

### 定位属性position

static:默认

fixed:相对于浏览器窗口的位置

relative:相对于默认的偏移

absolute:相对于最近的已定位(非static)的父元素,否则相对于<html>

**元素重叠**:z-index:一个带正负号的int值决定堆叠顺序,没有指定时,最后定位的在上面

### 尺寸单位

| **单位** | **描述**                                                     |
| -------- | ------------------------------------------------------------ |
| %        | 百分比                                                       |
| in       | 英寸                                                         |
| cm       | 厘米                                                         |
| mm       | 毫米                                                         |
| em       | 1em 等于当前的字体尺寸。   2em 等于当前字体尺寸的两倍。   例如，如果某元素以 12pt 显示，那么 2em 是24pt。   在 CSS 中，em 是非常有用的单位，因为它可以自动适应用户所使用的字体。 |
| ex       | 一个 ex 是一个字体的   x-height。 (x-height 通常是字体尺寸的一半。) |
| pt       | 磅 (1 pt 等于 1/72 英寸)                                     |
| pc       | 12 点活字 (1 pc 等于 12 点)                                  |
| px       | 像素 (计算机屏幕上的一个点)                                  |

