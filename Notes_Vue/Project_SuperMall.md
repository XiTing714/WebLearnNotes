## 准备工作

#### 本地仓库与远程仓库

* github上创建仓库。
* 脚手架3初始化一个项目``vue create 项目名(一般为项目文件名)``，并将项目文件夹初始化为git仓库。
* 然后将本地项目与远程仓库关联。再把本地项目push到远程。

#### 目录划分

(每个公司都有自己的划分风格)

* public: 自动生成的，相当于以前学的static文件名，它里面的文件在npm run build的时候会原封不动地复制到dist文件夹中去

* assets: 资源
  * css
  * img
* components: 公共的组件
  * common: 和业务不相关的公共组件，可以在其他项目使用的
  * content: 和当前业务相关的公共组件。

* views: 大的视图，例如首页、分类、购物车、我的.
  * home
  * category
  * ......
* router:  路由配置文件
* store: vuex相关的文件
* network: 网络相关的封装
* common: 公共的js文件,
  * const.js: 公共的常量
  * utils.js: 公共的方法



#### 样式

* css文件夹中引入normalize.css，被base.css引入
* 创建base.css文件：
  * 引入normalize.css
  * ``:root`是个伪类，获取根元素html
  * ``--color-text``等是变量，方便后面的样式代码中引用



#### 配置

##### 别名alias

* 内部默认已经配置好别名@，表示src
* 别名是为了引入其他文件的时候方便简写，例如``src/components``就可以写成``@/components``

##### 编辑风格

* 创建.editorconfig文件，在里面配置编辑风格，比如缩进等。



# 首页

## 底部导航栏MainTabBar

#### 实现思路

总思想：组件可复用

* 底部导航栏分成1个外层大组件和里面4个子组件。
* 大组件里定义1个插槽，4个子组件替换掉这个插槽。
* 每个子组件里面又定义图片和文字上下两个插槽。
* 子组件按flex布局排列。



#### 新增知识点

###### 样式

* ``#tab-bar``  box-shadow设置盒子阴影
* ``.tab-bar-item ``  49px是比较常见的高度
* vertical-align 属性设置元素(比如图片)的垂直对齐方式

###### 其他注意点

* 只要引用了第三方东西，例如axios，就不能在每个组件都去引用这个第三方的东西，不能让组件对第三方的东西产生依赖，否则一旦第三方的东西不再被维护，那么后期就得一个一个组件文件去改。



#### 大致步骤

* 用脚手架2初始化项目

* components下创建tabbar文件夹（放公共的组件，而views文件夹下放的是非公共组件），并在其中创建TabBar和TabBarItem组件，并**实现组件样式**(其中包括在assets/img文件夹中创建tabbar文件夹，在其中放置组件中引用的图片)。// **App.vue中使用组件，包括使用插槽**。

* src文件夹下创建views文件夹，在该文件夹下分别创建home、categories、cart、profile文件夹，并分别在四个文件夹下创建文件名分别为Home、Categories、Cart、Profile的.vue文件。// 配置路由（4个路径分别转到上面4个组件），// **然后通过代码(给元素绑定事件)实现路由跳转到上一步的这四个页面 (涉及Vue父传子通信这个知识点)**

* 实现 TabBarItem被点击时呈现样式的效果。

  * 图片方面结合v-if知识点。新增知识点：
    * 字符串的indexOf方法
    * 注意像TarBarItem这种被复用2次及2次以上的组件在父传子通信中的特点(computed属性中isActive中的this.path)
  * 文字方面：绑定style属性，属性值为computed属性中某方法的返回值。新增知识点：
    * 为了可以让使用TarBarItem子组件的人更改文字颜色，运用父传子通信的知识点，在父组件中使用子组件时给子组件绑定设定的属性activeColor并传入值，然后在子组件中使用属性这个数据(即activeColor)。

* 目前App.vue里看起来代码比较多，为保持App.vue文件的整洁，再再抽离出一个大组件MainTabBar

* 文件路径问题：

  * 现在发现每次改动文件夹都要手动去更改导入文件的路径，这很麻烦

  * 所以可以去配置相应的东西。在webpack.base配置文件中的resolve对象专门解决路径问题，其中，在alias下配置：

    * ``'@': resolve('src')`` 表示给src起了个别名，意味着在导入文件的时候，例如引入``import *** from '../components/tabbar/TabBar'``, 就可以写成````import *** from '@/components/tabbar/TabBar'``，其中的@会根据配置的别名去找src文件。

    * 配置其他的别名：

      'assets': resolve('src/assets'),

       'components': resolve('src/components'),

       'views': resolve('src/views')

    * 当你不是通过import来引入文件，而是在元素里，如img中src属性引入文件的话，需要在加路径前面加符号``～``

## 浏览器图标Icon

* public文件夹中的index.html里：``<link rel="icon" href="<%= BASE_URL %>favicon.ico">``其中，``<%= BASE_URL %>``这一串玩意是获取当前文件所在路径，这是JSP的语法。npm run build之后dist文件夹下的index.html中就没有这一串玩意了。


## 头部导航栏NavBar

* 一般导航栏的高度是44px
* 创建NavBar.vue，因为这个组件在多个视图中出现、且可以放在其他项目中，所以该组件放在components文件夹下的common文件夹中

##　轮播图HomeSwiper

* 老师的习惯是先把数据(在这里即图片)请求过来。因此先安装axios进行网络封装。
* 然后在Home.vue里引入网络封装模块，请求到数据，并把数据保存到data里。
* 轮播图组件封装：在components文件夹-common文件夹下的swiper。
* 利用**父传子props**把Home.vue中请求到的数据传到轮播图子组件。

##　标签切换TabControl

* 为什么不像底部导航栏那样安插槽而是用v-for？
  * 因为其他地方用到这个组件的时候只有文字不一样
* **position: sticky**实现：当向下滚动到某地方的时候TabControl固定住不滚动。
* **BUG：**vue使用v-for时vscode报错 Elements in iteration expect to have 'v-bind:key' directives
  * Vue 2.2.0+的版本里，当在组件中使用v-for时，key是必须的。**（原因很重要！可能会面试！！！）**
* 选中哪一个tab，哪一个tab的文字颜色就变色、且出现border-bottom。

#### 请求首页商品数据

* 首先，设计数据结构，保存网络请求的数据
  * 为什么要有page? (因为网络请求参数需要啊)
* 然后发送网络请求

#### 吸顶效果

##### 步骤一：获取``TabControl的offsetTop``

* 先获取``TabControl的offsetTop``，即顶部的偏移距离

  * 在Home组件的data设置一个变量``tabOffsetTop``，用来保存这个偏移距离。

  * 首先我们考虑在Home组件的mounted中获取``TabControl的offsetTop``并保存到变量``tabOffsetTop``中。由于组件没有``offsetTop``，因此要通过``$el``获取TabControl这个组件中的div元素。

    ```
    consolo.log(this.$refs.tabControl.$el.offsetTop)
    this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
    ```

  * 打印``TabControl的offsetTop``发现这个距离偏小、是不符实际的。这是由于轮播图的图片比较大、加载比较慢 (轮播图下面的那些图片比较小、加载比较快)，这时拿到的offsetTop是不包含轮播图片加载完时的。

  * 因此，要监听轮播图图片加载，等图片都加载完成了再拿到offsetTop。

    * 监听``HomeSwipper``中的img加载
    * 设置加载后发出事件，Home组件接收事件、获取正确的值。
    * 补充：为了不让``HomeSwipper``多次发出事件，在``HomeSwipper``中设置了``isLoad``变量进行状态的记录

##### 步骤二：判断滚动距离

* 当滚动距离＞``TabControl的offsetTop``时，显示标签切换组件
  * 在``nav-bar``组件下面再放一个``tab-control``组件1，设置``v-show="isTabFixed"``
  * 当滚动距离＞``TabControl的offsetTop``时，``isTabFixed``为true，即让``tab-control``组件1显示。
* 一个小问题：假设用户一开始先点击“精选”标签，即``tab-control``组件2中“精选”为红色、显示被选中，但用户滚动到下面``tab-control``组件1显示时，``tab-control``组件1中仍然是“流行”为红色。因此，如何让两个组件状态保持一致呢？
  * 标签样式是和``TabControl``中的currentIndex变量有关的
  * 因此，在点击其中一个组件时，同时改变另一个组件的currentIndex变量，让这两个组件的currentIndex变量保持一致。

## Better-Scroll框架

* 这是一个第三方工具，因此要对它进行封装。
* Scroll.vue组件：

1. 监听滚动
   * probeType: 0/1/2(鼠标移到上面时的滚动)/3(只要是滚动)
   * this.scroll.on('scroll', pos => {}) 
2. 上拉加载
   * pullUpLoad: true
   * this.scroll.on('pullingUp', () => {})
3. click
   * click: false时，Scroll组件中的button可以监听点击，但div不可以
   * click: true时，div才可以监听点击

* **BUG1:** 

  * ~~不知道为什么决定backTop是否显示那里没有作用。（刷新一下就好了）~~
  * ~~设置打印position，结果position一下子打印出来而非随着滚动打印 (要按着鼠标滚动)~~

* **BUG2:** 首页向下滚动到某个位置的时候明明还有内容但滚动不了了，这是因为BetterScroll可滚动区域的高度一开始是根据内容**(BetterScroll的content中的子组件)**决定的，那时还加载图片、没有计上图片的高度，图片加载完之后内容高度就变高了，但BetterScroll可滚动区域高度没有更新。
  
  * 如何解决？监听每一章图片是否加载完成，只要有一张图片加载完成就refresh一次。那如何监听图片是否加载完成了？
  * Vue中监听: 在GoodsListItem组件的img元素中添加方法``@load = 'methodName'``，这个方法想要调用Scroll的refresh方法，可以通过向Home组件emit事件、再在Home组件中取到Scroll再调用Scroll的refresh方法。（原生的js监听图片：``img.onload = function() {}``）
  
  * GoodsListItem组件和Home组件之间涉及到**非父子组件的通信**，所以这里选择了**事件总线bus**。
  
      * 在main.js中给Vue原型加上一个$bus
    * 分别在GoodsListItem组件和Home组件利用$bus：``this.$bus.emit('itemImgLoad')``、``this.$bus.on('itemImgLoad', ()=> {})``
  
  * **BUG3**：监听到后在Home组件中调用Scroll的refresh方法，发现浏览器console中报错。原因是调用refresh方法是在created中的，也就是Home组件一创建好的时候就调用(refresh方法还是在一个回调函数当中的)，这时候是拿不到scroll元素的。另外，Scroll组件中变量scroll的创建是在mounted中的，因此可能Scroll还没有挂载好而获取不到变量scroll。
  
    * 解决方法：
  
        * Home组件中监听GoodsListItem组件中的图片加载事件放在mounted中。
        * scroll中的refresh方法先加个判断条件，先判断``this.scroll``和``this.scroll.refresh``有值，有值再执行``this.scroll.refresh()``
        
        ```
        refresh() {
                this.scroll.refresh()
                this.scroll && this.scroll.refresh && this.scroll.refresh()
              }
        ```
    
  * 对于refresh非常频繁的问题进行**防抖操作**。
  
    * **面试重点**：防抖debounce/节流throttle
    * 防抖函数起作用的过程：
      * 如果我们直接执行refresh，那么refresh函数会被执行30次。
      * 可以将refresh函数传入到debounce函数中，生成一个新的函数(即debounce返回的函数，refresh函数就在这个返回的函数中)。
      * 之后在调用非常频繁的时候就使用新生成的函数。
      * 防抖函数这种功能性函数，一般不要直接写在组件里，一般专门写在一个js文件中。

## 离开Home时记录状态和位置

* 在deactiveted函数中获得离开时的scroll.y并保存于新建变量saveY。
* 在activeted函数中通过``scroll.scrollTo``回到原来位置



# 3. 购物车



## 3.1 点击加入购物车

#### 监听点击

* 监听
* 获取商品信息: id/price/image/title

#### 将商品添加到Vuex中

* 安装与配置Vuex
* 定义mutations，将商品添加到state.cartList



## 3.2 购物车导航栏

* vuex中mapGetters的用法
  * mapGetters的作用是把vuex的getters转为组件中的computed属性。
  * 示例：在Cart组件中使用了



## 3.3 购物车商品展示

#### 3.3.1 选中与不选中的样式

* 要有个东西来记录这个商品是否选中，这一定是在对象模型中的某个属性记录的。模型发生改变，界面才发生改变。
  * 什么是对象模型？cartList为商品数组，里面有很多商品：cartList[商品1,, 商品2, ...]。其中的商品1、商品2等就是对象模型。
* 需要在这个对象模型中自定义一个属性，假设为checked属性（为布尔值，true或false）。
  * mutations.js文件中，在添加商品的时候就给商品添加checked属性。
* 一旦用户点击按钮，就改模型对象中的checked属性 (而不是去改界面，即CheckButton组件中的isChecked变量) ，从而改变选中和不选中的样式。



## 3.4 底部计算栏

#### 3.4.1 全选按钮

* 思路：
  * 全选按钮的显示状态：
    * 如果全部商品选中，全选按钮显示为选中
    * 如果有一个未选中，那全选按钮显示为不选中
  * 点击全选按钮：
    * 如果有一个未选中，那点击全选按钮则全部选中
    * 如果全部都未选中，那点击全选按钮则全不选中

* **ES6: 数组的forEach方法** 对遍历的每一项item进行处理

* **ES6: 数组的find方法** 查找第一个符合条件的item并返回这个item

* **ES6: 数组的filter方法** 过滤出符合条件的item并返回这个item数组

  

#### 3.4.2 计算总价

* 面试重点!!!：**数组的reduce方法**
* Number的``num.toFixed(x)``方法，指定保留x位数。
* vuex中的mapGetters的应用



#### 3.4.3 计算已选中的个数

* **ES6: 数组的filter方法**



#### 3.4.3 Toast实现页面中的弹窗(有BUG)

* 比如将某商品加入购物车，出现弹窗“已加入购物车”的消息，然后渐变消失。
* 比如用户点击“去结账”时、但未选中一个商品，出现弹窗“请选择商品”的消息，然后渐变消失。
* 步骤：
  * 在src文件夹下的main.js中导入toast组件，并安装: `Vue.use(toast)`。（这本质上就是执行components-common-toast文件夹index.js里的install函数
  * components-common中新建文件夹toast，并在其中新建文件index.js和toast.vue
  * 
* BUG：toast组件里的show函数
  * 为什么setTimeout里的函数必须为箭头函数，否则无效



# 其他细节

#### fastclick移动端点击延迟

* 用来减少移动端的点击延迟
* 用法：
  * 首先，终端安装：``npm install fastclick``
  * 然后，在main.js文件中导入：``import FastClick from 'fastclick'``
  * 最后在main.js中调用attach方法：``FastClick.attach(document.body)``



#### 图片懒加载

* 什么是图片懒加载：即需要图片加载时在进行加载，提高性能。（但并不是所有公司都要求图片懒加载）
* 基本用法：
  * 首先，终端安装：``npm install vue-lazyload --save``
  * 然后，在main.js文件中导入：``import VueLazyLoad from 'vue-lazyload'``
  * 接着、通过Vue.use安装：``Vue.use(VueLazyLoad)``
  * 最后，修改img中的``:src`` 为`` v-lazy``
* 可以设置占位图，就是图片还没来得及加载的时候显示的图片



####　px2vw-css单位转换插件

* 作用：可以统一将px单位转化为vw，这样在不同移动端下页面各部分比例是一样的? （移动端适配）

* 使用：

  * 终端安装：``npm install postcss-px-to-viewport --save-dev ``

  * 在根目录下的postcss.config.js文件中进行配置：

    ```
    module.exports = {
      plugins: {
        autoprefixer: {},
        "postcss-px-to-viewport": {
          viewportWidth: 375, // 视窗的宽度，对应的是设计稿的宽度（这里对标的是iPhone6的设计稿
          viewportHeight: 667, // 视窗高度
          unitPrecision: 5, // 指定px转换为视窗单位值的小数位数（很多时候无法整除
          viewportUnit: 'vw', //指定需要转换成的视窗单位，建议使用vw
          selectorBlackList: ['ignore', 'tab-bar', 'tab-bar-item'],//指定不需要转换的类
          minPixelValue: 1, // 小于或等于1px不转换为视窗单位
          mediaQuery: false, //允许在媒体查询中转换
          exclude: [/TabBar/]
        }
      }
    }
    
    // 1.在JS中使用正则：/正则表达式/
    // 2.exclude中存放的元素必须是正则表达式
    // 3.按照排除的文件写对应的正则
    // 4.正则的规则：
          // 1)^abc :表示匹配的内容必须以abc开头
          // 2)abc$: 表示匹配的内容必须以abc结尾
          // 3)
    ```



# 项目部署

* 在公司，你需要把项目打包给服务器人员，但也有可能上传到公司github后自动部署。不管是手动部署还是自动化部署，都需要先对项目进行打包 (当项目写完后第一件事就是对项目进行打包)。
* 首先打包，即在终端执行``npm run build``，打包完成后，就有个名为dist的文件夹。
* 然后进行部署(**即把打包的dist文件夹放在服务器上**)，部署的方法有很多种。这里选择nginx，这个一个很强大的服务器。
  * 服务器就是一个没有显示器的电脑，是主机、24小时开着，为用户提供服务。
    * 大部分公司没有自己的服务主机，因为自己维护主机成本很高。
    * 他们一般是租借 阿里云/华为云/腾讯云的服务器。
    * 主机上要有操作系统，一般装Linux系统。
  * 在服务器上安装软件，如nginx、tomcat。这里选择安装nginx进行部署项目。
  * 老师的服务器：
    * 老师用一个Python文件进行爬虫、把爬到的数据存到服务器的MangoDB数据库。
    * 再通过另一个名为app.run.py的Python文件取出MangoDB里的数据、返回到前端（老师用是Python的flask框架开发接口。）

