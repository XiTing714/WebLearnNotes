## 准备工作

* 新建项目文件夹

* 在项目中新有以下文件和文件夹

  ```
  * dist
  * src
    * js
    * vue
    	* App.vue
    * css
    * main.js
  * index.html
  ```

* 初始化项目`npm init`

* 安装webpack`npm install webpack --save-dev`

* 安装vue`npm install vue --save` 

* 建立vue挂载点`index.html`文件: 创建id为app的div元素

* 新建webpack配置文件`webpack.config.js`



## webpack配置

### 处理Vue

* 准备工作: 

  * 在main.js中导入vue和App.vue, 然后new一个Vue实例, 在根实例中注册组件App, 并在template中使用

    ```
    import Vue from 'vue'
    import App from './vue/App.vue'
    
    const app = new Vue({
      data: {
        el: '#app',
        components: {
          'App': App
        },
        template: '<App/>'
      }
    })
    ```

* webpack配置vue(详见VUE笔记)

  * 指定vue编译版本runtime-compiler
  * 安装相应的loader并配置(注意版本问题)



### 配置打包html的插件

### 处理ES6

### 搭建本地服务器



## 想了下还是决定不用webpack了

* 整个过程就是先弄好做好布局，再一步步去实现每一个需求。
* 待办需求：
  * CSS
    * 整个配色
    * 边框阴影
    * 字体颜色
    * 字形
  * 自定义命令？
  * 一个菜单栏，运用到vuex和vue-router知识
  * 菜单栏包括今日任务、待办项目、已完成项目、专注模式(轮播图)四块
  * 运用Scroll
  * 运用Toast
* 梳理其中涉及的所有知识点，小心被问到！
* 这个项目的不足和改进



## TodayTask组件(Bug)

* 配置路由与组件

  * Bug: 浏览器Console报错`NavigationDuplicated: Avoided redundant navigation to current location`

  * 原因 : 出现这个问题，控制台会报`[NavigationDuplicated {_name: "NavigationDuplicated", name: "NavigationDuplicated"}]`。其原因在于Vue-router在3.1之后把`$router.push()`方法改为了Promise。**所以假如没有回调函数，错误信息就会交给全局的路由错误处理，因此就会报上述的错误。**

    如果你仔细观察并复现了多次错误你会发现，vue-router是先报了一个`Uncaught (in promise)`的错误(因为push没加回调)，然后再点击路由的时候才会触发`NavigationDuplicated`的错误(路由出现的错误，全局错误处理打印了出来)。

  * 解决: 

    *  方案1

      固定vue-router版本到3.0.7以下。这个方案没什么说的，就是简单粗暴，没有任何理由。但是你能确保以后不升级vue-router吗？

    * 方案2

      禁止全局路由错误处理打印，这个也是**vue-router开发者给出的解决方案**：

      ```
      import Router from 'vue-router'
      
      const originalPush = Router.prototype.push
      Router.prototype.push = function push(location, onResolve, onReject) {
        if (onResolve || onReject) return originalPush.call(this, location, onResolve, onReject)
        return originalPush.call(this, location).catch(err => err)
      ```

    ​	把这段代码放在引入vue-router之后就行，一般在main.js里，如果你的路由单独抽取出来了，那可能在其他的路由文件中。

    * 方案3(高成本高收益)

      vue-router的开发者也给出了解决方法，你需要为每个router.push增加回调函数。

      ```
      router.push('/location').catch(err => {err})
      ```

    ​	对于我们来说这个解决方案的成本可能很高，但是是值得的。在vue-router 3.1版本之前的push调用时不会返回任何信息，假如push之后路由出现了问题也不会有任何的错误信息。如果你使用方案1固定了vue-router的版本，那么以后的项目需要路由的回调时你根本无从下手。

    * 方案4

      如果你使用了Element-UI，并且方案2无法解决你的问题。那么你只能用方案1来固定你的vue-router版本了。这是因为Element-UI的`el-menu`在重复点击路由的时候报的错误，而且这个错误是Element-UI内部的路由问题，你无法通过方案2和3去解决。只能选择暂时不升级Vue-router。

      好消息是Element-UI已经有了解决方案，预计在2.13.0版本会解决这个问题。参考Github上issue#17269。

* 每一次更改state中todos的操作，都向store提交相应事件

* 在mutations的每个更改todos的事件中都添加`window.localStorage.setItem('todos', JSON.stringify(state.todos))`来及时变更本地存储的todos数据

* 剩余操作：
  * 清除所有完成任务的按钮
  * 待完成组件和未完成组件
    * 还有没有更好的方法来操作state数据？？



## 需求分析

* 输入框: 设置input输入框默认内容

* 添加任务: 输入框按下回车键，即添加任务

  * 先获取输入框内容 `e.target.value`
  * 去除所获取内容两端空格: `string.trim()`
  * 如果输入内容不为空，则向todos添加对象，
  * <span style="color: red">如果输入内容为空格，则弹出组件"请输入内容"</span>

* 任务管理: 

  * 单个任务完成——点击按钮后：
    * 划线且颜色变暗
    * 左下角显示剩余未完成任务的数目
    * 右下角出现“clear completed”按钮
  * 单个任务删除——删除按钮：
    * 鼠标移到时呈现×
    * 点击×删除任务
  * **单个任务编辑——双击编辑：**
    * 双击进入编辑状态
      * 输入框呈现项目名
      * <span style="color: red">输入框自动聚焦</span>
    * 按下回车或者失焦后呈现新编辑的内容
      * 如果内容为空，则删除该任务
      * 如果内容不为空，则改为输入框内容
    * 取消编辑，即修改了内容后又想取消
  * **全选按钮**：
    * 状态：全部任务被选中时颜色变深
    * 点击：点击全选按钮后全部任务被选中
  * 删除所有已完成任务：
    * 点击“clear completed”后 所有被划线的已完成任务 被删除

* **界面管理（即路由管理，路由就是路径和页面的映射关系）**: 

  * 没有任务时只留输入框其他部分隐藏
  * 一旦有任务被勾选时呈现“clear completed”按钮
  * 计算所有、已完成、未完成任务这三项的数目
  * 点击“all”, 显示所有任务，**且颜色改变**
    * 点击all, 连接添加hash值“#/all”
    * hash值一旦改变，触发某事件，该事件中自定义变量filterText改为all
    * filterText改变，那么`v-for="(item, index) in filterTodos"`中的`filterTodos`改变，则最终页面发生改变
  * 点击“active”, 显示未完成任务，**且颜色改变**
  * 点击completed, 显示已完成任务，**且颜色改变**

* todos数据保存在localStorage

  * 通过watch监视数据的改变，一旦数据变化，就通过localStorage存储:
  
  * `window.localStorage.setItem('key', 'value')`存储 key/value 对的数据
    * `window.localStorage.getItem('key')`获取数据
  
    ```
    window.localStorage.setItem('a', {foo: 'bar'})
  window.localStorage.getItem('a')  //"[object Object]"
    ```
  
  * 因为`window.localStorage.setItem('key', 'value')`里的参数必须是字符串，所以把数据转为JSON字符串。JSON.stringify() 方法用于将 JavaScript 值转换为 JSON 字符串: 
  
    ```
    window.localStorage.setItem('a', JSON.stringify({foo: 'bar'}))
    window.localStorage.getItem('a')  //"{"foo":"bar"}
    ```
  
  * data中的todos数据从localStorage中获取，但获取到的数据是JSON格式的字符串，需要把它转为JS对象。JSON.parse() 方法用于将JSON字符串转为JS值
  
    ```
    JSON.parse(window.localStorage.getItem('a'))
    ```

## 常见数组方法

数组方法大集合: 

* every: 如果有一个元素不满足条件则返回false
* some: 如果有一个元素满足条件就返回true
  * array.some(() => {condition})
* filter: 返回符合条件的数组
* find: 返回第一个符合条件的元素
* forEach: 对数组的每一个元素进行相同的操作
* splice(index, n): 表示从第几个位置开始删除n个元素



## 知识总结

### CSS

* `:last-child`指定同类元素的最后一个元素



### JS

* HTML5种的web storage包含两种存储方式：localStorage和sessionStorage，这两种方式存储的数据不会自动发给服务器，仅仅是本地保存，有大小限制。





