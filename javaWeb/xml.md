# XML

## 树结构

实例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
     <book category="COOKING">
		<title lang="en">Everyday Italian</title>
		<author>Giada De Laurentiis</author>
		<year>2005</year>
		<price>30.00</price>
	</book>
</bookstore>
```
实例中的根元素是 `<bookstore>`。文档中的所有 `<book>` 元素都被包含在 `<bookstore>` 中。
`<book>` 元素有 4 个子元素：`<title>、<author>、<year>、<price>`。蓝色字体为属性。
第一行为声明。

## 语法规则

1. 必须要有根元素
2. 声明为可选，如存在需放在第一行。
3. 所有元素都必须要有一个关闭标签，如</book>
4. 必须正确嵌套。即关闭标签的顺序要一致。
5. 属性值必须加引号。
6. 实体引用，有5个预定义，用来替代这些符号，避免错误。

    | `&lt;`   | <    | less   than      |
    | ------ | ---- | ---------------- |
    | `&gt;`   | >    | greater   than   |
    | `&amp;`  | &    | ampersand        |
    | `&apos;` | '    | apostrophe       |
    | `&quot;` | "    | quotation   mark |

    当这些字符很多时可以用，`<![CDATA[这里写内容]]>`
7. 注释，格式为`<!-- This is a comment -->`
8. 空格会被保留，连续的空格不会被合并。
9. 以LF存储换行。

    - 在 Windows 应用程序中，换行通常以一对字符来存储：回车符（CR）和换行符（LF）。
    - 在 Unix 和 Mac OSX 中，使用 LF 来存储新行。
    - 在旧的 Mac 系统中，使用 CR 来存储新行。
    - XML 以 LF 存储换行。

10. 标签对大小写敏感。

## 元素

**概念**：元素是指从（且包括）开始标签直到（且包括）结束标签的部分。一个元素可以包含：其他元素，文本，属性，或混合以上所有。
**命名规则**：包含字母、数字以及其他的字符。不能以数字或标点符号开始。不能以任何xml三个字组成的开始。不能包含空格。
**命名习惯**：避免使用“-”“.”“:”
**空元素**：`<name/>`或者`<name> </name>`

## 属性

1. 属性必须加引号，单双引号均可。如果属性值本身包含双引号可以用单引号包含双引号，或者使用字符实体。
```xml
<gangster name=‘George "ShotGun" Ziegler’>
<gangster name=‘George &quot;ShotGun&quot; Ziegler’>
```
2. 一个元素可以有多个属性，用空格隔开。
3. 没有规定什么时候使用属性，什么时候使用元素，建议尽量避免使用属性。
4. 下面的 id 属性仅仅是一个标识符，用于标识不同的便签。它并不是便签数据的组成部分。
    在此我们极力向您传递的理念是：元数据（有关数据的数据）应当存储为属性，而数据本身应当存储为元素。
```xml
<messages>
<note id="501">
<to>Tove</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
<note id="502">
<to>Jani</to>
<from>Tove</from>
<heading>Re: Reminder</heading>
<body>I will not</body>
</note>
</messages>
```
## 命名空间

使用前缀来避免命名冲突。
**语法规则**：xmlns:前缀="URI",命名空间可以在他们被使用的元素中或者在XML根元素中声明。
默认命名空间如下。默认空间名不应用于属性。如下面的isbn属性没有命名空间。
取消绑定命名空间：`<purchase  xmlns="">`表示purchase范围内都位于无命名空间。`<purchase xmlns:lib="">`表示范围内lib不能使用，可以重新定义。

```xml
<?xml version="1.0"?>
<Book isbn="1234" xmlns="http://www.library.com">
    <Title>Sherlock Holmes - I</Title>
    <Author>Arthur Conan Doyle</Author>
    <purchase xmlns="http://www.otherlibrary.com">
        <Title>Sherlock Holmes - II</Title>
        <Author>Arthur Conan Doyle</Author>
    </purchase>
    <Title>Sherlock Holmes - III</Title>
    <Author>Arthur Conan Doyle</Author>
</Book>
```
在以上的示例中， 元素Book、 Title 和 Author 与命名空间`http://www.library.com` 关联，元素 purchase、 Title 和 Author 与命名空间`http://www.otherlibrary.com` 关联。
    