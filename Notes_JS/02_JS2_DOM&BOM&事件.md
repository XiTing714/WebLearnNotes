# JS(慕课网笔记)

## 1 事件

### 什么是事件

* JavaScript 创建动态页面。事件是可以被 JavaScript 侦测到的行为。 网页中的每个元素都可以产生某些可以触发 JavaScript 函数或程序的事件。

* 比如说，当用户单击按钮或者提交表单数据时，就发生一个鼠标单击（onclick）事件，需要浏览器做出处理，返回给用户一个结果。

![http://img.mukewang.com/53e198540001b66404860353.jpg](http://img.mukewang.com/53e198540001b66404860353.jpg)



## 2 BOM 浏览器对象

BOM(浏览器对象模型)，提供了很多对象，用于访问浏览器的功能，这些功能与任何网页内容无关。

#### window对象

![img](http://img.mukewang.com/535483720001a54506670563.jpg)



#### location对象

location用于获取或设置窗体的URL，并且可以用于解析URL。

**语法:**

```
location.[属性|方法]
```

**location对象属性图示:**

[![img](http://img.mukewang.com/53605c5a0001b26909900216.jpg)](http://img.mukewang.com/53605c5a0001b26909900216.jpg)

**location 对象属性：**

**[![img](http://img.mukewang.com/5354b1d00001c4ec06220271.jpg)](http://img.mukewang.com/5354b1d00001c4ec06220271.jpg)**

**location 对象方法:**

**[![img](http://img.mukewang.com/5354b1eb00016a2405170126.jpg)](http://img.mukewang.com/5354b1eb00016a2405170126.jpg)**

#### history对象

* history对象记录了用户曾经浏览过的页面(URL)，并可以实现浏览器前进与后退相似导航的功能。

* 注意:从窗口被打开的那一刻开始记录，每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。

**语法：**

```
window.history.[属性|方法]
```

**注意：**window可以省略。

**History 对象属性**

[![img](http://img.mukewang.com/53548c030001759e05840068.jpg)](http://img.mukewang.com/53548c030001759e05840068.jpg)

**History 对象方法**

**[![img](http://img.mukewang.com/53548c200001228206210123.jpg)](http://img.mukewang.com/53548c200001228206210123.jpg)**

#### screen对象

screen对象用于获取用户的屏幕信息。

**语法：**

```
window.screen.属性
```

**对象属性:**

**[![img](http://img.mukewang.com/5354d2810001a47706210213.jpg)](http://img.mukewang.com/5354d2810001a47706210213.jpg)**



#### navigator对象



## 3 DOM 控制HTML元素                    

* 文档对象模型DOM（Document Object Model）定义访问和处理HTML文档的标准方法。DOM 将HTML文档呈现为带有元素、属性和文本的树结构（节点树）。

**DOM节点层次图:**

[![img](http://img.mukewang.com/5375ca7e0001dd8d04830279.jpg)](http://img.mukewang.com/5375ca7e0001dd8d04830279.jpg)

* HTML文档可以说由节点构成的集合，DOM节点有:

  **1.** **元素节点: **上图中`<html>`、`<body>`、`<p>`等都是元素节点，即标签。

  **2.** **文本节点: **向用户展示的内容，如`<li>...</li>`中的JavaScript、DOM、CSS等文本。

  **3.** **属性节点:** 元素属性，如`<a>`标签的链接属性`href="http://www.imooc.com"`

### 3.1 获取元素节点

####  querySelector() 方法

仅仅返回匹配指定选择器的第一个元素，例子: `document.querySelector('.contain')`

#### Node.cloneNode

**`Node.cloneNode() `**方法返回调用该方法的节点的一个副本.

```
var dupNode = node.cloneNode(deep);
```

- `node`

  将要被克隆的节点

- `dupNode`

  克隆生成的副本节点

- `deep` 可选

  是否采用深度克隆`,如果为true,`则该节点的所有后代节点也都会被克隆,如果为`false,则只克隆该节点本身.`

#### 区别getElementByID, getElementsByName, getElementsByTagName                          

**以人来举例说明，人有能标识身份的身份证，有姓名，有类别(大人、小孩、老人)等。**

1. ID 是一个人的身份证号码，是唯一的。所以通过getElementById获取的是指定的一个人。

2. Name 是他的名字，可以重复。所以通过getElementsByName获取名字相同的人集合。

3. TagName可看似某类，getElementsByTagName获取相同类的人集合。如获取小孩这类人，getElementsByTagName("小孩")。

**把上面的例子转换到HTML中，如下:**

```
<input type="checkbox" name="hobby" id="hobby1">  音乐
```

* input标签就像人的类别。
* name属性就像人的姓名。
* id属性就像人的身份证。



### 3.2 更改元素节点的属性

#### getAttribute()方法                          

通过元素节点的属性名称获取属性的值。

**语法：**

```
elementNode.getAttribute(name)
```

**说明:**

1. elementNode：使用getElementById()、getElementsByTagName()等方法，获取到的元素节点。

2. name：要想查询的元素节点的属性名字

#### setAttribute()方法

setAttribute() 方法增加一个指定名称和值的新属性，或者把一个现有的属性设定为指定的值。

**语法：**

```
elementNode.setAttribute(name,value)
```

**说明：**

1.name: 要设置的属性名。

2.value: 要设置的属性值。

**注意：**

1.把指定的属性设置为指定的值。如果不存在具有指定名称的属性，该方法将创建一个新属性。

2.类似于getAttribute()方法，setAttribute()方法只能通过元素节点对象调用的函数。



### 3.3 节点属性

在文档对象模型 (DOM) 中，每个节点都是一个对象。DOM 节点有三个重要的属性 ：

1. nodeName : 节点的名称

2. nodeValue ：节点的值

3. nodeType ：节点的类型

**一、nodeName 属性:** 节点的名称，是只读的。

1. 元素节点的 nodeName 与标签名相同
2. 属性节点的 nodeName 是属性的名称
3. 文本节点的 nodeName 永远是 #text
4. 文档节点的 nodeName 永远是 #document

**二、nodeValue 属性：**节点的值

1. 元素节点的 nodeValue 是 undefined 或 null
2. 文本节点的 nodeValue 是文本自身
3. 属性节点的 nodeValue 是属性的值

**三、nodeType 属性:** 节点的类型，是只读的。以下常用的几种结点类型:

**元素类型   节点类型**
  元素      1
  属性      2
  文本      3
  注释      8
  文档      9

### 3.4 查节点

#### 子节点childNodes

#### firstChild

#### lastChild

#### parentChild

#### firstElementChild第一个子元素

### 3.5 增删改节点

##### 1) 创建节点

* 创建元素节点: `document.createElement('tagName')`
* 创建文本节点: `document.createTextNode('data')`

##### 2) 插入节点

* `node.appendChild(newnode)`

  在指定节点的最后一个子节点之后添加一个新的子节点。

  **参数**: 

  ​	newnode: 指定追加的节点。
  
* `parentNode.insertBefore(newnode,node)`

  在已有的子节点前插入一个新的子节点。

  **参数:**

  ​	newnode: 要插入的新节点。

  ​	node: 指定此节点前插入节点。

##### 3) 替换节点

​	`	node.replaceChild (newnode,oldnew ) `

​	实现子节点(对象)的替换。返回被替换对象的引用。 

​	**参数：**

​	 	newnode : 必需，用于替换 oldnew 的对象。 
 		oldnew : 必需，被 newnode 替换的对象。

##### 4) 删除节点

​	`nodeObject.removeChild(node)`

​	从某节点的子节点列表中删除某个节点。如删除成功，此方法可返回被删除的节点，如失败，则返回 NULL。

​	**参数:**

​		node ：必需，指定需要删除的节点。



### 3.6 浏览器窗口可视区域大小&网页尺寸

获得浏览器窗口的尺寸（浏览器的视口，不包括工具栏和滚动条）的方法:

**一、对于IE9+、Chrome、Firefox、Opera 以及 Safari：**

• window.innerHeight - 浏览器窗口的内部高度

• window.innerWidth - 浏览器窗口的内部宽度

**二、对于 Internet Explorer 8、7、6、5：**

• document.documentElement.clientHeight表示HTML文档所在窗口的当前高度。

• document.documentElement.clientWidth表示HTML文档所在窗口的当前宽度。

或者Document对象的body属性对应HTML文档的`<body>`标签

• document.body.clientHeight

• document.body.clientWidth

**在不同浏览器都实用的 JavaScript 方案：**

```
var w= document.documentElement.clientWidth
      || document.body.clientWidth;
var h= document.documentElement.clientHeight
      || document.body.clientHeight;
```

#### 区分 (可以到MDN查询)

#### window.innerHeight

![innerHeight vs outerHeight illustration](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerHeight/firefoxinnervsouterheight2.png)

#### documentElement.clientHeight

#### body.clientHeight

* window.**innerHeight**属于BOM（浏览器对象模型）,获取的高度包含横向滚动条

* document.**documentElement**.clientHeight属于DOM，不包含横向滚动条
* document.**body**.clientHeight属于DOM，body高度，如果设置body height=100%，document.documentElement.clientHeight=document.body.clientHeight

![Image:Dimensions-client.png](https://developer.mozilla.org/@api/deki/files/185/=Dimensions-client.png)

#### scrollHeight&offsetHeight

#####　scrollHeight

https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollHeight

scrollHeight和scrollWidth，获取**网页<span style="color:red">内容</span>**高度和宽度。

![img](https://developer.mozilla.org/@api/deki/files/840/=ScrollHeight.png)

判定元素是否滚动到底（如果元素滚动到底，下面等式返回true，没有则返回false）
 `element.scrollHeight - element.scrollTop === element.clientHeight`	

**一、针对IE、Opera:**

​	scrollHeight 是网页内容实际高度，可以小于 clientHeight。

​	**二、针对NS、FF:**

​	scrollHeight 是网页内容高度，不过最小值是 clientHeight。也就是说网页内容实际高度小于 clientHeight 时，scrollHeight 返回 clientHeight 。

​	**三、浏览器兼容性**

```
var w=document.documentElement.scrollWidth
   || document.body.scrollWidth;
var h=document.documentElement.scrollHeight
   || document.body.scrollHeight;
```

​	**注意:区分大小写**

​	scrollHeight和scrollWidth还可获取Dom元素中内容实际占用的高度和宽度。

##### offsetHeight

​	https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetHeight

​	offsetHeight和offsetWidth，获取网页内容高度和宽度(包括滚动条等边线，会随窗口的显示大小改变)。

​	**一、值**

​	offsetHeight = clientHeight + 滚动条 + 边框。

​	**二、浏览器兼容性**

```
var w= document.documentElement.offsetWidth
    || document.body.offsetWidth;
var h= document.documentElement.offsetHeight
    || document.body.offsetHeight;
```



#### scrollTop&offsetTop

**我们先来看看下面的图：**

[![img](http://img.mukewang.com/5347b2b10001e1a307520686.jpg)](http://img.mukewang.com/5347b2b10001e1a307520686.jpg)

**scrollLeft:**设置或获取位于给定对象左边界与窗口中目前可见内容的最左端之间的距离 ，即左边灰色的内容。

**scrollTop:**设置或获取位于对象最顶端与窗口中可见内容的最顶端之间的距离 ，即上边灰色的内容。

**offsetLeft:**获取指定对象相对于版面或由 offsetParent 属性指定的父坐标的计算左侧位置 。

**offsetTop:**获取指定对象相对于版面或由 offsetParent 属性指定的父坐标的计算顶端位置 。



## Ajax

#### 简介

*  Ajax,是对 Asynchronous JavaScript + XML 的简写。
* Ajax 技术的核心是 XMLHttpRequest 对象(简称 XHR)，XHR 为向服务器**发送请求**和**解析服务器响应**提供了流畅的接口。可以使用 XHR 对象取得新数据,然后再通过 DOM 将新数据插入到页面中。
* 　

#### XHR用法

* 先创建XHR对象：``var xhr = new XMLHttpRequest()``
* 





* 跨域请求









