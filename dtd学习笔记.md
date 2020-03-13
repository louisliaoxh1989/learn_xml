# 一 概述
<p style="text-indent:2em">全称文档类型定义(Document Type Definition)定义合法的ＸＭＬ文档构建模块。也可以理解为用其来约束XML文档，使其在一定的规范下使用。</p>

<center>

![DTD原理图](DTD原理.jpg)

</center>

# 二 DTD文档的声明及引用方式

## 1 内部的DOCTYPE声明(内部DTD文档)

<p style="text-indent:2em">假如 DTD 被包含在您的 XML 源文件中，它应当通过下面的语法包装在一个 DOCTYPE 声明中。声明方式如下:
</p>

```xml
<!DOCTYPE 根元素名称 [定义内容(元素声明)] >
```
***实例***

```xml
<?xml version="1.0"?>
<!DOCTYPE myroot [
  <!ELEMENT myroot    (name,age,sex,address)>
  <!ELEMENT name      (#PCDATA)>
  <!ELEMENT age    (#PCDATA)>
  <!ELEMENT sex (#PCDATA)>
  <!ELEMENT address    (#PCDATA)>
]>
<!-- 根元素为myroot,并为根元素定义了name,age,sex,address四个子元素，在xml中这个四个元素必须出现且按照顺序出现，不能少也不能出现其他的元素 -->
<myroot>
<name>George</name>
<age>John</age>
<sex>Reminder</sex>
<address>Don't forget the meeting!</address>
</myroot> 
```

## 2 外部的DOCTYPE声明

<p style="text-indent:2em">假如 DTD 位于 XML 源文件的外部，那么它应通过下面的语法被封装在一个 DOCTYPE 定义中:
</p>

```xml
<!DOCTYPE 根元素名称 SYSTEM "文件名称" >
```
***实例***

```xml

<!ELEMENT myroot    (name,age,sex,address)>
<!ELEMENT name      (#PCDATA)>
<!ELEMENT age    (#PCDATA)>
<!ELEMENT sex (#PCDATA)>
<!ELEMENT address    (#PCDATA)>

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE myroot SYSTEM "example1.dtd">
<myroot>
    <name>George</name>
    <age>John</age>
    <sex>Reminder</sex>
    <address>Don't forget the meeting!</address>
</myroot>
```

## 3 内外部声明

<p style="text-indent:2em">DTD的内容也可以有些放在外部，有些放在内部</p>


```xml
<!DOCTYPE 根元素 SYSTEM "文件名称" [定义内容]>
```

***实例***

```xml
<!-- example2.xml文件-->
<?xml version="1.0"?>
<!DOCTYPE myroot system "example2.dtd" [
    <!ELEMENT myroot    (name,age,sex,address)>
]>
<myroot>
    <name>George</name>
    <age>John</age>
    <sex>Reminder</sex>
    <address>Don't forget the meeting!</address>
</myroot>

<!--example2.dtd-->
<!ELEMENT name      (#PCDATA)>
<!ELEMENT age    (#PCDATA)>
<!ELEMENT sex (#PCDATA)>
<!ELEMENT address    (#PCDATA)>
```


# 三 DTD文档的构成

**一个DTD文档包含：**

* 元素ELEMENT的定义规则
* 元素之间的关系规则
* 属性(ATTLIST)的定义规则
* 可使用的实体(ENTITY)或符号(NOTATION)规则

## 1 元素的定义

```xml
<!ELEMENT 元素名称 (元素类型) >
```

**元素类型有下列４种**

**(1). EMPTY 空元素 该元素不能包含子元素和文本，但可以有属性（空元素）**

```xml
<!ELEMENT 元素名称 EMPTY>
```

***实例***
```xml    
<!-- empty.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mybr [
<!ELEMENT mybr EMPTY>
]
>
<mybr/>
```
**(2). ANY 该元素可以包含任何在DTD中定义的元素内容**

```xml
<!ELEMENT 元素名称 ANY>
```
***实例***

```xml
<!-- myany.xml -->
<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE myany [
        <!ELEMENT myany    (other_element,many)>
        <!ELEMENT other_element (#PCDATA)>
        <!ELEMENT many ANY>
        <!ELEMENT other_element1 (#PCDATA)>
    ]
    >
<myany>
    <other_element>sdfdsfd</other_element>
    <many>
        <other_element1>dfdfdf</other_element1>
    </many>
</myany>
```

**(3). (#PCDATA) 该元素可以包含任何字符数据，但是不能在其中包含任何子元素**

```xml
<!ELEMENT 元素名称 ANY>
```
***实例***

```xml
<!ELEMENT name (#PCDATA)>
```
**(4). 其它类型(组合)，可以是子元素，子元素与修饰符组合，基本元素与子元素与修饰符组合。**

&nbsp;&nbsp;&nbsp;&nbsp;**a. 带有子元素（序列）的元素**

```xml
<!ELEMENT 元素名称 (子元素1,子元素2,...,子元素n)>
```
<p style="text-indent:2em">当子元素按照由逗号分隔开的序列进行声明时，这些子元素必须按照相同的顺序出现在文档中。在一个完整的声明中，子元素也必须被声明，同时子元素也可拥有子元素</p>

***实例***

```xml
<!ELEMENT note (to,from,heading,body)>
<!ELEMENT to      (#PCDATA)>
<!ELEMENT from    (#PCDATA)>
<!ELEMENT heading (#PCDATA)>
<!ELEMENT body    (#PCDATA)>
```
&nbsp;&nbsp;&nbsp;&nbsp;**b. 声明“非.../既...”类型的内容**

```xml
<!ELEMENT 元素名称 ((子元素1|子元素2),子元素3)>
```

<p style="text-indent:2em">当子元素按照由逗号分隔开的序列进行声明时，这些子元素必须按照相同的顺序出现在文档中。在一个完整的声明中，子元素也必须被声明，同时子元素也可拥有子元素</p>

***实例***

```xml
<!DOCTYPE example [
    <!ELEMENT example ((sub1|sub2),sub3)>
    <!ELEMENT sub2 (#PCDATA)>
    <!ELEMENT sub1 (#PCDATA)>
    <!ELEMENT sub3 (#PCDATA)>
]>
<example>
    <sub1>sub1</sub1>
    <!-- 或者不要sub1,要sub2-->
    <!-- <sub2>sub2</sub2>-->
    <sub3>sub3</sub3>
</example>
```
&nbsp;&nbsp;&nbsp;&nbsp;**c. 声明必须且只出现一次的子元素**

```xml
<!ELEMENT 元素名称 (子元素)>
```

***实例***

```xml
<!DOCTYPE example [
    <!ELEMENT example (sub)>
    <!ELEMENT sub1 (#PCDATA)>
]>
<example>
    <!--sub1必须出现杰只能出现一次 -->
    <sub1>sub1</sub1>
</example>
```

&nbsp;&nbsp;&nbsp;&nbsp;**d. 声明必须且只出现一次的子元素**

```xml
<!ELEMENT 元素名称 (子元素+)>
```

***实例***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
<!ELEMENT example (sub1+)>
<!ELEMENT sub1 (#PCDATA)>
]>
<example>
<sub1>sub1</sub1>
<sub1>sub1_1</sub1>
</example>
```

&nbsp;&nbsp;&nbsp;&nbsp;**e. 声明出现零次或一次的子元素**

```xml
<!ELEMENT 元素名称 (子元素?)>
```

***实例***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
<!ELEMENT example (sub1?,sub2)>
<!ELEMENT sub1 (#PCDATA)>
<!ELEMENT sub2 (#PCDATA)>
]>
<example>
<!-- 也可以不要sbu1元素,但最多只能出现一次 -->
<sub1>sub1</sub1>
<sub2></sub2>
</example>
```
&nbsp;&nbsp;&nbsp;&nbsp;**f. 声明出现零次或多次的子元素**

```xml
<!ELEMENT 元素名称 (子元素*)>
```

***实例***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
<!ELEMENT example (sub1*,sub2)>
<!ELEMENT sub1 (#PCDATA)>
<!ELEMENT sub2 (#PCDATA)>
]>
<example>
<!-- 也可以不要sbu1元素也可以出现多次，但必须是在sub2元素之前 -->
<sub1>sub1</sub1>
<sub1>sub1</sub1>
<sub2>sub2</sub2>
</example>
```
&nbsp;&nbsp;&nbsp;&nbsp;**g. 声明混合内容**

<p style="text-indent:2em">可以使用基本类型元素ANY,EMPTY或#PCDATA与子元素混合申明(这里还不明白是怎么样的)</p>


***实例***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE example [
<!ELEMENT example (#PCDATA|to)*>
<!ELEMENT to ANY>
]>
<example>
<to>dfdfdf</to>
</example>
```

## 2 ATTLIST 属性的定义

<p style="text-indent:2em">用于设置定义的元素可以有那些属性，属性是否必须以及属性的可取值</p>

```xml
<!ATTLIST 元素名称 
    属性名称1 属性类型 属性特点
    属性名称2 属性类型 属性特点
>
```
### **(1) 属性类型**

<table class="dataintable">
<tbody><tr>
<th style="width:35%;">类型</th>
<th>描述</th>
</tr>

<tr>
<td>CDATA</td>
<td>值为字符数据 (character data)，如

```xml
<!--dtd文件-->
<!-- 为元素学生定义一个属性名为‘学号’的属性，类型是字符数据（包括数字和中文）,且是必须有的属性-->
<!ATTLIST 学生 
    学号 CDATA #REQUIRED　
>
<!--xml文件-->
<学生 学号="01194">
```
</td>
</tr>

<tr>
<td>
(<i>en1</i>|<i>en2</i>|..)</td>
<td>此值是枚举列表中的一个值

***语法如下***
```xml
<!ATTLIST 元素名称 属性名称 (en1|en2|...) "默认值">
```
***如***
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE classroom [
    <!ELEMENT classroom (学生+)>
    <!ATTLIST classroom 
        编号 CDATA #REQUIRED
    >
    <!ELEMENT 学生 (#PCDATA)>
    <!ATTLIST 学生
        姓名 CDATA #REQUIRED
        性别 (男|女) "男">
]>
<classroom 编号="011">
    <学生 姓名="zhao" 性别='男' ></学生>
    <!-- 也可以不需要设置性别属性,默认为男-->
    <学生 姓名="liao" ></学生>
    <!-- 这个是不合法的xml
    <学生 姓名="xue"" 性别='不' ></学生>
    -->
</classroom>
```
</td>
</tr>

<tr>
<td>ID</td>
<td>值为唯一的 id(类型为ID的属性取值必须是唯一的)

</td>
</tr>

<tr>
<td>IDREF</td>
<td>值为另外一个元素的 id</td>
</tr>

<tr>
<td>IDREFS</td>
<td>值为其他 id 的列表</td>
</tr>

<tr>
<td>NMTOKEN</td>
<td>值为合法的 XML 名称</td>
</tr>

<tr>
<td>NMTOKENS</td>
<td>值为合法的 XML 名称的列表</td>
</tr>

<tr>
<td>ENTITY</td>
<td>值是一个实体</td>
</tr>

<tr>
<td>ENTITIES</td>
<td>值是一个实体列表</td>
</tr>

<tr>
<td>NOTATION</td>
<td>此值是符号的名称</td>
</tr>

<tr>
<td>xml:</td>
<td>值是一个预定义的 XML 值</td>
</tr>
</tbody></table>

### (2) **属性特性(属性默认值)**
<table>
<tbody><tr>
<th style="width:35%;">值</th>
<th>解释</th>
</tr>

<tr>
<td>值</td>
<td>属性的默认值</td>
</tr>

<tr>
<td>#REQUIRED</td>
<td>属性值是必需的(元素的所有实例都必须有该属性的值) 如

```xml
<!--dtd文件-->
<!ATTLIST sender company CDATA #REQUIRED>
<!--xml文件-->
<sender company="taobao">
```

</td>
</tr>

<tr>
<td>#IMPLIED</td>
<td>属性不是必需的(元素的实例中可以忽略该属性)。如

```xml
<!--dtd文件-->
<!ATTLIST contact fax CDATA #IMPLIED>
<!--xml文件-->
<contact fax="888-228833">
<!--也可以是没有该属性 -->
<contact />
```
</td>
</tr>

<tr>
<td>#FIXED value</td>
<td>属性值是固定的。如　

```xml
<!--dtd文件-->
<!ATTLIST sender company CDATA #FIXED "Microsoft">
<!--xml文件-->
<sender company="Microsoft">
```
</td>
</tr>
</tbody></table>

