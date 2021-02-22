# ToDoApp

##　不懂

* currentEditing为什么不能是布尔值，而是item？

* 布局问题：绝对定位元素里的子元素是怎么流动的？

  ```
  <body>
    <section class="todoapp" id="app">
      
      <header class="header">
        <h1>todos</h1>
        <input class="new-todo" placeholder="What needs to be done?" v-focus
        @keydown.enter="handleAdd">
      </header>
     
      <section class="main">
        <input id="toggle-all" 
        class="toggle-all" 
        type="checkbox">
        <label for="toggle-all">Mark all as complete</label>
      </section>
  
    </section>
  </body>
  ```

  * `body`样式

    ```
    body {
    	min-width: 230px;
    	max-width: 550px;
    	margin: 0 auto;
    
    	font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
    	
    	
    	background: #f5f5f5;
    	color: #111111;
    	-webkit-font-smoothing: antialiased;
    	-moz-osx-font-smoothing: grayscale;
    	font-weight: 300;
    }
    ```

  * `.todoapp`这个section为相对定位

    ```
    .todoapp {
    	position: relative;
    	margin: 130px 0 0px 0;
    	
    	background: #fff;
    	box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
                  0 25px 50px 0 rgba(0, 0, 0, 0.1);
      border: solid 0.5px red;
    }
    ```

  * `.todoapp`子元素header的子元素h1为绝对定位

    ```
    .todoapp h1 {
    	position: absolute;
    	top: 0px;
    	
    	border: solid 1px blue;
    	width: 100%;
    	font-size: 80px;
    	font-weight: 200;
    	text-align: center;
    	color: #b83f45;
    	-webkit-text-rendering: optimizeLegibility;
    	-moz-text-rendering: optimizeLegibility;
    	text-rendering: optimizeLegibility;
    }
    ```

    



## 准备工作

* 在TODOMVC官网上最下面右下角有个``template``，点击、进入github，通过``git clone``下载该应用模板。
* vscode打开项目，通过``npm install``安装依赖包。



## 需求分析

* 输入框: 设置input输入框默认内容

* 添加任务: 

  * 输入框输入内容后，按下回车键，即添加任务
  * 如果输入内容为空，按下回车键后弹出窗口"请输入内容"

* 任务管理: 

  * 单个任务完成——点击按钮后：
    * 划线且颜色变暗
    * 左下角显示剩余未完成任务的数目
    * 右下角出现“clear completed”按钮
  * 单个任务删除——删除按钮：
    * 鼠标移到右边时呈现×
    * 点击×删除任务
  * **单个任务编辑——双击编辑：**
    * 双击进入编辑状态
    * 按下回车或者失焦后呈现新编辑的内容
      * 如果内容为空，则删除该任务
      * 如果内容不为空，则呈现新内容
  * **全选按钮**：
    * 状态：全部任务被选中时颜色变深
    * 点击：点击全选按钮后全部任务被选中
  * 删除所有已完成任务：
    * 点击“clear completed”后 所有被划线的已完成任务 被删除

* **界面管理（即路由管理，路由就是路径和页面的映射关系）**: 

  * 点击“all”, 显示所有任务
    * 点击all, 连接添加hash值“#/all”
    * hash值一旦改变，触发某事件，该事件中自定义变量filterText改为all
    * filterText改变，那么`v-for="(item, index) in filterTodos"`中的`filterTodos`改变，则最终页面发生改变
  * 点击“active”, 显示未完成任务
  * 点击completed, 显示已完成任务

* todos数据保存在localStorage

  * window.localStorage.setItem('key', value)存储 key/value 对的数据
  * window.localStorage.getItem('key')获取数据

  ```
  window.localStorage.setItem('a', {foo: 'bar'})
  window.localStorage.getItem('a')  //"[object Object]"
  ```

  * JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串

  ```
  window.localStorage.setItem('a', JSON.stringify({foo: 'bar'}))
  window.localStorage.getItem('a')  //"{"foo":"bar"}"
  ```

  ​	这样获得的就是JSON格式的字符串

  * JSON.parse() 方法用于将JSON字符串转为JS值

  ```
  JSON.parse(window.localStorage.getItem('a'))
  ```

  ​    这样获得的就是JS值(该例中JS值为对象)了。





## 输入框

* 输入框默认文字: placeholder
* 按下enter键即触发添加项目事件



## 添加项目

### 获取输入框内容

* input元素上的事件可以默认传递e参数，通过`e.target.value`获取input内容
* 字符串方法trim() 作用是去掉字符串两端的多余的空格



数组方法大集合: 

every: 如果有一个元素不满足条件则返回false

some: 如果有一个元素满足条件就返回true

filter: 返回符合条件的数组

find: 返回第一个符合条件的元素

forEach: 对数组的每一个元素进行相同的操作







