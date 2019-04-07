# HTML5

`<!DOCTYPE html>`声明是HTML5文件

`<!--...-->`定义一个注释

## 元数据和脚本

| 标签名   | 作用                                                       |
| -------- | ---------------------------------------------------------- |
| head     | 头部元素的容器，必须包含title，可以包含其他元数据标签      |
| title    | 标题（必需）                                               |
| meta     | 元数据，定义HTML本身的文档信息                             |
| base     | 为页面内的所有相对链接规定默认URL或默认目标                |
| link     | 定义文档与外部资源的关系，如链接css                        |
| style    | 定义CSS样式文件，属性type默认值为text/css。                |
| script   | 定义JavaScript脚本，                                       |
| noscript | 浏览器不支持脚本时显示内部的内容，内部可以包含任何HTML元素 |

###  meta标签

```html
<!-- 规定文档的字符编码。 -->
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
<meta charset="UTF-8">
<!-- 定义文档自动刷新的时间间隔，单位秒。 -->
<meta http-equiv="refresh" content="300">
<!-- 定义文档关键词，用于搜索引擎。 -->
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
<!-- 定义web页面描述。 -->
<meta name="description" content="Free Web tutorials on HTML and CSS">
<!-- 定义页面作者。 -->
<meta name="author" content="Hege Refsnes">
```

### base标签

#### 属性

- **href**：规定页面中所有相对链接的基准 URL。
- **target**：规定所有超链接和表单在何处打开。会被每个链接自身的target覆盖。值为：\_blank，\_parent，\_self，\_top，framename。

### link标签

#### 属性

- href：定义被链接文档的位置。值为URL。
- hreflang：定义被连接文档中文本的语言。
- ref：**必需**。定义当前文档与被连接文档之间的关系。值为stylesheet，icon等

- media：规定链接文档将显示在什么设备上。

### script标签

#### 属性

- src：规定外部脚本的URL。
- type：默认为text/javascript。
- charset：规定脚本中使用的字符编码（仅适用于外部脚本）。
- async：规定异步执行脚本（仅适用于外部脚本）。
- defer：规定当页面已完成解析后，执行脚本（仅适用于外部脚本）。

![284aec5bb7f16b3ef4e7482110c5ddbb_articlex](images/html/284aec5bb7f16b3ef4e7482110c5ddbb_articlex.jpg)

1. 没有 `defer` 或 `async`，**文档的解析将停止，并立即下载并执行脚本**，脚本执行完毕后将继续解析文档。
2. 有 `async`，文档的**解析不会停止**，**其他线程将下载脚本，脚本下载完成后开始执行脚本**，脚本执行的过程中文档将停止解析，直到脚本执行完毕。
3. 有 `defer`，文档的解析不会停止，其他线程将下载脚本，**待到文档解析完成，脚本才会执行**。
4. `async`和`defer`都不能保证脚本按照加载顺序执行。

## 文档章节

| 标签名  | 作用                                 |
| ------- | ------------------------------------ |
| body    | 定义文档主体的部分                   |
| h1~h6   | 定义HTML标题，不同级别标题样式不同。 |
| hgroup  | 被用来对标题元素进行分组。           |
| article | 定义一个文章内容。                   |
| aside   | 定义 `<article> `标签外的内容。      |
| address | 定义文档作者或拥有者的联系信息。     |
| section | 定义了文档的某个区域。               |
| header  | 定义一个文档头部部分。               |
| footer  | 定义一个文档底部。                   |
| nav     | 定义导航链接的部分。                 |

## 文本级别语义

| 标签名 | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| span   | 定义文档中的行内元素。前后不会换行。没有结构上的意义。       |
| a      | 定义超链接。                                                 |
| ruby   | 定义 ruby 注释（中文注音或字符）。                           |
| rt     | 在 ruby 中使用，定义字符（中文注音或字符）的解释或发音。     |
| rp     | 在 ruby 中使用，以定义不支持 ruby 元素的浏览器所显示的内容。 |
| em     | 呈现为被强调的文本。效果是类似斜体。                         |
| strong | 定义重要的文本。效果是类似加粗。                             |
| dfn    | 定义一个定义项目。                                           |
| code   | 定义计算机代码文本。                                         |
| samp   | 定义样本文本。                                               |
| kbd    | 定义键盘文本。它表示文本是从键盘上键入的。它经常用在与计算机相关的文档或手册中。 |
| var    | 定义变量。您可以将此标签与 `<pre> `及 `<code> `标签配合使用。 |
| abbr   | 定义一个缩写。                                               |
| q      | 定义短的引用。属性cite：规定引用的源URL。                    |
| cite   | 定义作品（比如书籍、歌曲、电影、电视节目、绘画、雕塑等等）的标题。 |
| time   | 定义一个日期/时间。也可以用属性datetime规定日期/时间。       |
| i      | 定义斜体文本。                                               |
| b      | 定义粗体文本。                                               |
| sub    | 定义下标文本。                                               |
| sup    | 定义上标文本。                                               |
| small  | 定义小号文本。                                               |
| mark   | 定义带有记号的文本。背景高亮的文本。                         |
| ins    | 定义被插入文本。即更新修正后的文本，体现为下划线。属性：cite和datetime。 |
| del    | 定义已删除的文本。即有删除线的文本。属性：cite和datetime。   |
| s      | 对那些不正确、不准确或者没有用的文本进行标识。也有删除线，但替换和删除的文本用del标签。 |
| bdi    | 允许您设置一段文本，使其脱离其父元素的文本方向设置。         |
| bdo    | 定义文本的方向。属性dir设置文本方向：ltr从左向右和rtl从右向左。 |
| wbr    | 规定在文本中的何处适合添加换行符。如单词换行时机。           |

###  a标签

#### 属性

- href：规定连接的目标URL。
- hreflang：规定被连接文档的语言。
- ref：规定当前文档和目标URL的关系。
- target：规定在何处打开目标 URL。值有：_blank，\_parent，\_self，\_top，framename。
- type：规定目标文档的 MIME 类型。
- download：定义下载链接的地址或者是下载文件的名称（不用写后缀会自动加上）。
- media：规定目标 URL 的媒介类型。默认值：all。仅在 href 属性存在时使用。

## 组合内容

| 标签名     | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| br         | 插入一个简单的换行符。                                       |
| hr         | 定义水平线。                                                 |
| div        | 定义一个块级元素。前后会自动换行。                           |
| p          | 定义一个段落                                                 |
| ol         | 定义一个有序列表                                             |
| ul         | 定义一个无序列表                                             |
| li         | 定义一个列表项。是ol和ul的子标签。                           |
| dl         | 定义一个描述列表。                                           |
| dt         | 定义一个描述列表的项目/名字。是dl的子标签。                  |
| dd         | 用来描述一个描述列表的项目/名字。是dl的子标签，紧跟dt。      |
| figure     | 规定独立的流内容（图像、图表、照片、代码等等）。             |
| figcaption | 为figure标签定义一个标签。一般位于figure标签的第一个或最后一个元素。 |
| pre        | 定义预格式文本。通常会保留空格和换行符。而文本也会呈现为等宽字体。 |
| blockquote | 定义摘自另一个源的块引用。                                   |

## 表单

| 标签名   | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| form     | 定义一个 HTML 表单，用于用户输入。                           |
| input    | 定义一个输入控件                                             |
| datalist | 规定了 input 元素可能的选项列表。配合input的list属性指定datalist的id。配合子标签option。 |
| label    | 定义 input 元素的标注。点击该元素会自动触发for属性里id指向的input元素的焦点。 |
| textarea | 定义多行的文本输入控件。属性：cols，rows，wrap提交时文本是否换行（soft不换行，hard换行） |
| select   | 定义选择列表（下拉列表）。属性：multiple多选，size显示的项目数 |
| optgroup | 定义选择列表中相关选项的组合。属性：label组名称，disabled禁用。 |
| option   | 定义选择列表中的选项。属性：selected选中，label              |
| button   | 定义按钮。属性：type为button，reset，submit。                |
| fieldset | 定义围绕表单中元素的边框。属性：disabled，form，name。       |
| legend   | 定义 fieldset 元素的标题。                                   |
| output   | 定义一个计算的结果                                           |
| progress | 定义运行中的任务进度（进程）。                               |
| meter    | 定义度量衡。仅用于已知最大和最小值的度量。                   |
| keygen   | 规定用于表单的密钥对生成器字段。                             |

###  form标签

#### 属性：

- **action**：规定提交表单时请求的URL
- **target**：规定在何处打开action的URL，值为：_blank，\_parent，\_self，\_top，framename。
- **method**：规定用于发送表单数据的 HTTP 方法，值为get和post。
- **enctype**：属性规定在将表单数据发送到服务器之前如何对其进行编码。只有method=“post”时才使用。
  - **application/x-www-form-urlencoded**：默认。在发送前对所有字符进行编码（将空格转换为 "+" 符号，特殊字符转换为 ASCII HEX 值）。
  - **multipart/form-data**：不对字符编码。当使用有文件上传控件的表单时，该值是必需的。
  - **text/plain**：将空格转换为 "+" 符号，但不编码特殊字符。
- **name**：规定表单的名称。
- **novalidate**：如果使用该属性，表单提交时不进行验证。
- **accept-charset**：规定表单提交时使用的字符编码列表，多个字符编码使用空格分隔。

### input标签

#### 属性

- **type**：规定元素的类型。值为：text（默认，文本字段），button（按钮），image（图片按钮），submit（提交），reset （重置按钮，重置为默认值），radio（单选框），checkbox（复选框），file（文件），hidden（隐藏），number（数字），password（密码），color（拾色器），date（年月日），datetime（日期时间时区），datetime-local（日期时间），time（时间），month（月份和年），week（周几和年份），range（范围取值），search（搜索字段），tel（电话号码），email，url
- **accept**：规定type为file时上传文件的类型限制。值为`audio/*`，`video/*`,`image/*`等，多个值用逗号分隔。
- **alt**：规定图像无法显示时，代替显示的文本。只适用type为image。。

- **src**：规定图像的URL。只适用type为image。

- **height**：规定图像的高度。只配合type为image。。

- **width**：规定图像的宽度。只配合type为image。。

- **checked**：布尔属性。会在页面加载时被预先选定。只适用type为checkbox和radio。

- **list**：规定需要预定义选项的datalist的id。

- **multiple**：布尔属性。允许用户输入多个值。适用type为email和file。

- **pattern**：规定验证用的正则表达式。适用type为text、search、url、tel、email和password。

- **max**：规定最大值。适用type为number、range、date、datetime、datetime-local、month、time 和 week。

- **min**：规定最小值。适用type为number、range、date、datetime、datetime-local、month、time 和 week。

- **step**：规定合法数字间隔。以0位起点，正负都可以。适用type为number、range、date、datetime、datetime-local、month、time 和 week。

- **required**：布尔属性。规定提交前必须填写该元素。适用type为text、search、url、tel、email、password、date pickers、number、checkbox、radio和file。

- **name**：规定元素的名称。

- **value**：规定元素的值。

- **size**：规定可见宽度，以字符数计，默认20。

- **maxlength**：规定允许的最大字符数。

- **placeholder**：规定用户输入值之前显示在输入字段中的提示信息。

- **autocomplete **：否应该启用自动完成功能。值为on（启动）和off（关闭）。

- **readonly**：布尔属性。规定元素为只读，不能修改，可以选中。

- **autofocus**：布尔属性。当页面加载时会自动获得焦点。

- **disabled**：布尔属性。禁用元素，无法使用和无法点击。

- **formaction**：用于覆盖form的action属性。适用于type为submit和image。

- **formenctype **：用于覆盖form的enctype属性。适用于type为submit和image。

- **formmethod **：用于覆盖form的method属性。适用于type为submit和image。

- **formtarget  **：用于覆盖form的target属性。适用于type为submit和image。

- **formnovalidate  **：用于覆盖form的novalidate 属性。适用于type为submit和image。

- **form**：规定所属的一个或多个表单。值为form的id。



## 表格数据

| 标签名   | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| table    | 定义 HTML 表格。属性：border值为1有边框。                    |
| colgroup | 对表格中的列进行组合，以便对其进行格式化。属性：span定义colgroup横跨的列数。 |
| col      | 规定colgroup内部每一列的列属性。属性：span定义colgroup横跨的列数。 |
| caption  | 定义表格的标题。放置到 table 标签之后。                      |
| th       | 定义表格中的表头单元格。属性：colspan，rowspan，headers，scope |
| tr       | 定义表格中的行。                                             |
| td       | 定义表格中的单元。属性：colspan，rowspan，headers            |
| thead    | 定义表格中的表头内容。                                       |
| tbody    | 定义表格中的主体内容。                                       |
| tfoot    | 定义表格中的表注内容（脚注）。                               |

 

## 嵌入式内容

| 标签名 | 作用                                                         |
| ------ | ------------------------------------------------------------ |
| img    | 定义图像。                                                   |
| map    | 定义图像映射。                                               |
| area   | 定义图像地图内部的区域。map的内部标签。                      |
| embed  | 定义了一个容器，用来嵌入外部应用或者互动程序（插件）。属性：height，width，src，type。 |
| canvas | 定义画布元素，供脚本画图。属性：height和width。              |
| object | 定义嵌入的对象。属性：data(资源的URL)，form，height，width，name，type，usemap。 |
| param  | 定义对象的参数。object的内部标签。属性：name和value。        |
| iframe | 定义内联框架。                                               |
| audio  | 定义声音，比如音乐或其他音频流。                             |
| video  | 定义一个音频或者视频                                         |
| source | 定义媒体资源（比如video>和 audio）。属性：media，src，type。 |
| track  | 定义媒体文件的字幕文件或其他文本文。                         |

###  图像映射

```html
<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">
<map name="planetmap">
  <area shape="rect" coords="0,0,82,126" href="sun.htm" alt="Sun">
  <area shape="circle" coords="90,58,3" href="mercur.htm" alt="Mercury">
  <area shape="circle" coords="124,58,8" href="venus.htm" alt="Venus">
</map>
```

img的属性usemap指定map的id或name，map标签用来规定img内可点击区域的映射。

#### area标签

##### 属性

- **href**：规定区域要跳转的目标URL。
- **hreflang**：规定区域要跳转的目标URL的语言。
- **target**：规定在何处打开目标URL。
- **type**：规定目标URL的MIME类型。
- **media**：规定区域要跳转的目标URL是为何种媒介/设备优化的。默认all。
- **rel**：规定区域要跳转的目标URL与当前html文档之间的关系。
- **shape**：规定区域的形状。值为：default（全部区域），rect（矩形），circle（圆形），poly（多边形）。
- **coords**：规定区域的坐标。
  - shape为rect：x1,y1,x2,y2 规定矩形左上角和右下角的坐标。
  - shape为circ：x,y,radius 规定圆心的坐标和半径。
  - shape为poly：x1,y1,x2,y2,...,xn,yn  规定多边形各顶点的值。如果第一个坐标和最后一个坐标不一致，那么为了关闭多边形，浏览器必须添加最后一对坐标。
- **alt**：规定区域的替代文本。悬浮时显示。

### iframe标签

#### 属性

- **src**：规定显示文档的URL。
- **sandbox**：内容定义一系列额外的限制。
  - **""**：启用所有限制条件。
  - **allow-same-origin**：允许将内容作为普通来源对待。如果未使用该关键字，嵌入的内容将被视为一个独立的源。
  - **allow-top-navigation**：嵌入的页面的上下文可以导航（加载）内容到顶级的浏览上下文环境（browsing context）。如果未使用该关键字，这个操作将不可用。
  - **allow-forms**：允许表单提交。
  - **allow-scripts**：允许脚本执行。
- **seamless**：布尔属性。没有边框和滚动条。
- **srcdoc**：规定要显示在内联框架中的页面的 HTML 内容。配合sandbox和seamless属性。
- **name**：定义名称。
- **height**：定义高度。
- **width**：定义宽度。

### audio标签

#### 属性

- **autoplay**：布尔属性。音频在就绪后马上播放。
- **controls**：布尔属性。向用户显示音频控件（比如播放/暂停按钮）。
- **loop**：布尔属性。每当音频结束时重新开始播放。
- **muted**：布尔属性。音频输出为静音。
- **preload**：规定当网页加载时，音频是否默认被加载以及如何被加载。值为：auto（载入音频），meta（载入元数据），none（不载入）。
- **src**：规定音频文件的 URL。

### video标签

#### 属性

- **autoplay**：布尔属性。视频在就绪后马上播放。
- **controls**：布尔属性。向用户显示视频控件（比如播放/暂停按钮）。
- **loop**：布尔属性。每当视频结束时重新开始播放。
- **muted**：布尔属性。视频的音频输出为静音。
- **preload**：规定是否在页面加载后载入视频。如果设置了 autoplay 属性，则忽略该属性。值为：auto（载入视频/音频），meta（载入元数据），none（不载入）。
- **src**：规定视频文件的 URL。
- **poster**：规定视频正在下载时显示的图像URL，直到用户点击播放按钮。
- **height**：设置视频播放器的高度。
- **width**：设置视频播放器的宽度。

## 交互元素

| 标签名  | 作用                                                         |
| ------- | ------------------------------------------------------------ |
| menu    | 定义了一个命令列表或菜单。属性：label和type。                |
| command | 定义用户可能调用的命令。属性：checked，disabled，icon，label，radiogroup，type。 |
| details | 规定了用户可见的或者隐藏的需求的补充细节。属性：open规定是否可见。 |
| summary | 定义一个可见的标题。为details的子标签。                      |