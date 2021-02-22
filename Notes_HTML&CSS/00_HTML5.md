# HTML5



## 伪元素

### 应用场景



## 介绍

* html+css构成了我们网页的基本页面结构和样式

* ```
  <!DOCTYPE html>:文档类型声明，表示该文件为 HTML5文件。<!DOCTYPE> 声明必须是 HTML 文档的第一行，位于 <html> 标签之前。
  ```

* ```
  <html></html>标签对：<html>标签位于HTML文档的最前面，用来标识HTML文档的开始；</html>标签位于HTML文档的最后面，用来标识HTML 文档的结束；这两个标签对成对存在，中间的部分是文档的头部和主题。
  ```

* ```
  <head></head>标签对：标签包含有关HTML文档的信息，可以包含一些辅助性标签。如<title></title>，<style></style>，<link/>，<meta/>等
  ```

* 

## 语义化标签

##### ``<span></span>``

* 这个标签是没有语义的，它的作用就是为了设置单独的样式用的。

#####  ``<div></div>``

* 用div标签为网页划分独立的版块.

##### ``<header></header>``

* 用来定义``<body>``头部区域

##### ``<footer></footer>``

* 用来定义``<body>`底部区域

##### ``<section></section>``

* 用来定义``<body>``中间的区域

##### ``<aside></aside>``

* 用来定义``<body>``侧边栏区域

## 效果标签

##### ``<br/>``

* 实现换行效果
* 是空标签(没有html内容的标签即为空标签，如``<hr/><img/>``)

##### `&nbsp; `

* 实现空格效果

##### `<hr/> `

* 水平分割线

##　列表标签

##### 无序标签``<ul><li>``

##### 有序标签``<ol><li>``

## 图片、链接及表格标签

##### 图片``<img>``

* 举例：

``<img src = "myimage.gif" alt = "My Image" title = "My Image" />``

* 讲解：

  * src：标识图像的位置；

  * alt：指定图像的描述性文本，当图像不可见时（下载不成功时），可看到该属性指定的文本；
  * title：提供在图像可见时对图像的描述(鼠标滑过图片时显示的文本)；
  * 图像可以是GIF，PNG，JPEG格式的图像文件。

##### 链接``<a></a>``

* 举例：

``<a  href="目标网址"  title="鼠标滑过显示的文本" target="_blank">链接显示的文本</a>``

* 讲解：
  * target属性，代表打开网页的方式。可选值为``_self``和``_blank``，默认值为``_self``，代表在当前页面打开链接，``_blank``代表在新窗口打开链接。

##### 表格``table、tr、th、td``

* 举例：

```
<table>
	<caption>这里定义表格标题</caption>
	<tr>
		<th>姓名</th>
		<th>电话</th>
		<th>邮件</th>
		<th>职务</th>
	</tr>
	<tr>
		<td>张三</td>
		<td>123456789</td>
		<td>zhangsan@163.com</td>
		<td>研发工程师</tdh>
	</tr>
</table>
```

* 讲解：

##### 表格``thead、tbody、tfoot``

* 举例：

```
<table>
	<thead>
		<tr>
			<th>科目</th>
			<th>分数</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>语文</td>
			<td>115</td>
		</tr>
		<tr>
			<td>数学</td>
			<td>126</td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<td>总分</td>
			<td>241</td>
		</tr>
	</tfoot>
</table>
```

* 讲解：
  * thead、tfoot 以及 tbody  元素使您有能力对表格中的行进行分组。当您创建某个表格时，您也许希望拥有一个标题行，一些带有数据的行，以及位于底部的一个总计行。
  * 这种划分使浏览器有能力支持独立于表格标题和页脚的表格正文滚动。当长的表格被打印时，表格的表头和页脚可被打印在包含表格数据的每张页面上。

## 表单标签``<form>``

* 表单是可以把浏览者输入的数据传送到服务器端，这样服务器端程序就可以处理表单传过来的数据
* 语法``<form method="传送方式" action="服务器文件">``
* 讲解：
  * ``<form>`` ：``<form>``标签是成对出现的。
  * action ：浏览者输入的数据被传送到的地方,比如一个PHP页面(save.php)。
  * method ： 数据传送的方式（get/post）。

##### 文本/密码输入框

* 示例：

  ```
  <form    method="post"   action="save.php">
          <label for="username">用户名:</label>
          <input type="text" name="username" placeholder="请输入用户名"/>
          <label for="pass">密码:</label>
          <input type="password" name="pass" placeholder="请输入密码"/>
  </form>
  ```

* 讲解：

  * type
    * 当type="text"时，输入框为文本输入框；
    * 当type="password"时, 输入框为密码输入框。
  * name：为文本框命名，以备后台程序ASP 、PHP使用。
  * value：为文本输入框设置默认值。(一般起到提示作用)
  * placeholder：设置提示信息。placeholder属性的值可以任意填写,当输入框输入内容时,占位符内容消失,输入框无内容时,占位符内容显示。占位符内容不是输入框真正的内容。

##### 数字输入框

* 示例：

  ```
  <input type="number" />
  ```

##### 网址输入框

* 示例：

  ```
  <input type="url" />
  ```

* 讲解：数字框的值需以http://或者https://开头,且后面必须有内容,否则表单提交的时候会报错误提示。

##### 邮箱输入框

* 示例：

  ```
  <input type="email" />
  ```

* 讲解：数字框的值必须包含@，@之后必须有内容,否则会报错误提示。

##### 单选框和复选框

* 示例：

  ```
  <form method="post" action="save.php">
  你是否喜欢旅游？<br/>
  <input type="radio" name="travelradio" value="喜欢" checked="checked"/> 喜欢
  <input type="radio" name="travelradio" value="不喜欢"/> 不喜欢
  <input type="radio" name="travelradio" value="无所谓"/> 无所谓
  <br/>
  你对哪些运动感兴趣？ <br/>
  <input type="checkbox" name="sportcheckbox" value="跑步" checked="checked"/> 跑步
  input type="checkbox" name="sportcheckbox" value="打球" checked="checked"/> 打球
  input type="checkbox" name="sportcheckbox" value="登山"/> 登山
  input type="checkbox" name="sportcheckbox" value="健美" checked="checked"/> 健美
  </form>
  ```

* 讲解：

  * type
    * 当 **type="radio"** 时，控件为**单选框**
    * 当 **type="checkbox"** 时，控件为**复选框**
  * name：为文本框命名，以备后台程序ASP 、PHP使用。
  * value：为文本输入框设置默认值。(一般起到提示作用)
  * **checked：**当设置 checked="checked" 时，该选项被默认选中
  * 注意：**同一组**的单选按钮，name 取值一定要一致，比如上面例子为同一个名称“radioLove”，这样同一组的单选按钮才可以起到单选的作用。

##### ``lable``为Input标签穿上衣服

* 示例：

  ```
  <form
    <label for="email">输入你的邮箱地址</label>
    <input type="email" id="email" placeholder="Enter email">
  </form>
  ```

* 讲解：

  * label标签不会向用户呈现任何特殊效果，它的作用是为鼠标用户改进了可用性。**如果你在 label  标签内点击文本，就会触发此控件**。就是说，当用户单击选中该label标签时，浏览器就会自动将焦点转到和标签相关的表单控件上（就自动选中和该label标签相关连的表单控件上）。
  * 注意：标签的 for 属性中的值应当与相关控件的 id 属性值一定要相同。

##### select、option标签创建下拉菜单

* 示例：

  ```
  <form method="post" action="save.php">
  你的爱好：
  	<select name="hobbies">
  		<option value="读书">读书</option>
  		<option value="运动" selected="selected">运动</option>
  		<option value="音乐">音乐</option>
  		<option value="旅游">旅游</option>
  		<option value="购物">购物</option>
  	</select>
  </form>
  ```

* 讲解：

  * select和option标签都是双标签，它总是成对出现的，需要首标签和尾标签。
  * select标签里面只能放option标签，表示下拉列表的选项。
  * option标签放选项内容，不放置其他标签。
  * value：表示向服务器提交的值。（**注意input里向服务器提交的值是name值**）
  * selected="selected"：设置selected="selected"属性，则该选项就被默认选中。

##### 文本域``<textarea>``

* 示例：

  ```
  <textarea  rows="行数" cols="列数">文本</textarea>
  ```

* 讲解：cols: 多行输入域的列数。rows: 多行输入域的行数。在``<textarea></textarea>``标签之间可以输入默认值。

##### 提交按钮

* 示例：

  ```
  <input   type="submit"   value="提交">
  ```

* 讲解：

  * type: 只有当type值设置为submit时，按钮才有提交作用
  * value: 按钮上显示的文字

##### 重置按钮

* 示例：

  ```
  <input type="reset" value="重置">
  ```

* 讲解：

  * type: **只有当type值设置为reset时，按钮才有重置作用**
  * value: 按钮上显示的文字

##### 







