# Vue

## 面试题

* 如何理解Vue的生命周期
* 如何进行非父子组件通信

#### Vue的响应式原理(中/高级)

* 



## 介绍

#### 核心特点

* 数据驱动视图，减少DOM操作（JQuery是封装了DOM操作）
* 组件化。方便模板重用，增加代码可维护性。

#### 俺对Vue基础的总结

* 改变内容
* v-bind 改变属性，包括：样式属性、src属性等等
* v-on 绑定事件
* v- model 双向数据绑定
* v-if/v-show 条件渲染
* v-for 列表渲染



## 基础

* 挂载

```html
// 声明式编程（js实现的编程是命令式编程）
        const app = new Vue({
            el: '#app', //el说明了Vue管理模板的入口节点。把vue实例挂载到id为app的节点
            data: {
                message: 'hello',
            }, 
            // data中的数据被称为响应式数据，即其中的数据发生改变，
            // 所有绑定该数据的DOM都会跟着改变（MVVM）
        })
```

* methods:
  * methods方法中不要使用箭头函数，否则绑定的是Window

#### 插值操作

* Mustache八字胡：{{}}

* v-html：会解析data中的html标签

  ```
  <body>
      <div id="app">
          <h1> {{url}} </h1>
          <h1 v-html="url"> </h1>  
      </div>
      <script src="node_modules/vue/dist/vue.js"></script>
      <script>
          const app = new Vue({
              el: "#app",
              data:{
                  url:'<a href="http://www.baidu.com">百度以下</a>'
              }
          })
      </script>
  </body>
  ```

* v-text：会渲染data中的数据

  ```
  <body>
      <div id="app">
          <h1> {{msg}} </h1>
          <h1 v-text="msg"> </h1>  
      </div>
      <script src="node_modules/vue/dist/vue.js"></script>
      <script>
          const app = new Vue({
              el: "#app",
              data:{
                  msg:'你好啊'
              }
          })
      </script>
  </body>
  ```

* v-pre：该指令告诉Vue不要解析这个节点内部的内容，浪费时间

  ```
  <h1 v-pre> {{msg1}} </h1>
  ```

* v-once：只渲染一次，不会随着数据变化而变化

* v-cloak：浏览器在解析的过程中，发现具有v-cloak的属性隐藏不显示，当Vue解析替换完之后，Vue会自动把v-cloak属性移除

#### 绑定属性 v-bind:属性名

v-bind只能用用于属性，**它的值是一个JS表达式，和{{}}里的语法一模一样**。

唯一的区别就是，{{}}用于标签文本绑定，v-bind用于标签属性绑定。

v-bind 绑定属性, **冒号后面的叫指令参数**

##### class属性

* 对象语法

  * :class="{类名:布尔值}"

  * :class="{active: isActive， line: isLine}"（实例data中的isActive为true或false）

  * 如果过多，可以放在methods或者computed中，例如：

    ```
    <body>
        <div id="app">
            <h1 :class="getClasses()"> {{msg}} </h1>
        </div>
        <script src="node_modules/vue/dist/vue.js"></script>
        <script>
            const app = new Vue({
                el: "#app",
                data:{
                    msg:'你好啊',
                    isActive: true,
                    isLine: true
                },
                methods:{
                	getClasses: function() {
                		return {active: this.isActive， line: this.isLine}
                	}
                }
            })
        </script>
    </body>
    ```

    

* 数组语法

  ```
  <body>
      <div id="app">
          <h1 :class="[activeName, lineName]"> {{msg}} </h1>
      </div>
      <script src="node_modules/vue/dist/vue.js"></script>
      <script>
          const app = new Vue({
              el: "#app",
              data:{
                  msg:'你好啊',
                  activeName: 'active'
                  lineName: 'line'
              }
          })
      </script>
  </body>
  ```

  

##### style属性

* 对象语法

  *  :style="{backgroundColor:'red', fontSize:'50px'}" 

  * （为什么这里冒号后面又要写成字符串呢？前面说了，属性值是和{{}}里的语法一模一样。如果属性值是data中没有的，那么就写成字符串；如果不写成字符串，就当成变量来解析）

    ```
    <body>
        <div id="app">
            <h1 :style="{backgroundColor:'bgC', fontSize:'fZ'}"> {{msg}} </h1>
        </div>
        <script src="node_modules/vue/dist/vue.js"></script>
        <script>
            const app = new Vue({
                el: "#app",
                data:{
                    msg:'你好啊',
                    bgC: 'red'
                    fZ: '50px'
                }
            })
        </script>
    </body>
    ```

    

* 数组语法

#### 计算属性 computed

* 该成员就是一个方法，但是在使用的时候必须当做属性来用，不能当方法来调用

* 计算属性会进行缓存，如果多次使用、也只调用一次

* 简写方式，**一个函数，作为get方法**:

  * computed: {

    ​	getFullname: function() {

    ​		returm this.firstName + " " + this.lastName

    ​	}

    }

* 应用：

  ```
  <body>
      <div id="app">
          <h1> 总价格：{{totalPrice}} </h1>
      </div>
      <script src="node_modules/vue/dist/vue.js"></script>
      <script>
          const app = new Vue({
              el: "#app",
              data:{
                  books:[
                  	{id:001, name:'Unix艺术编程', price:119},
                  	{id:002, name:'Unix艺术编程', price:120},
                  	{id:003, name:'现代操作系统', price:159},
                  ]
              },
              computed: {
              	totalPrice: function() {
              		let sum = 0
              		for(let i=0;i<this.books.length;i++){
              			sum = sum + this.books[i].price
              		}
              		return sum
              	}
              }
          })
      </script>
  </body>
  ```

  

#### 事件监听 v-on

* v-on: 事件="myFunction()"
* @事件="myFunction()"

##### 修饰符

* .stop调用event.stopPropagation

  ```
  @click.stop="myFunction()"
  ```

* .prevent 调用event.preventDefault()  

  ```
  @click.prevent="myFunction()"
  @submit.prevent //没有表达式
  @click.stop.prevent="myFunction()" //串联使用
  ```

  ``回忆：表单的submit默认行为是刷新页面，a链接默认行为是跳转。``

* .(keyCode | keyAlias) - 当触发特定键时触发事件

  ```
  @keyup.enter="myFunction()"
  @keydown.13="myFunction()"
  ```

* .once-只触发一次

  ```
  @click.once="myFunction()"
  ```

* .native-监听组件根元素的原生事件

  * 当我们在组件元素中添加事件(如@click)时，必须加上.native

#### 条件判断 v-if

* 和v-show的区别
  * v-if为false时，包含该指令的元素根本就不会存在于DOM中（即元素不被渲染）
  * v-show为false时，包含该指令的元素的display属性被设置为none。行内样式为``style=“display: none”``

#### 循环遍历 v-for

* 遍历数组：``<li v-for="(item, index) in array"> </li>``

* 遍历对象：``<li v-for="(key, value) in obj"> </li>``

* 官方推荐在使用v-for的时候给对应的元素或组件添加一个:key属性（key不能乱绑定，要保证key值和内容是一一对应的）

  * ```
    <body>
        <div id="app">
            <ul>
            	<li v-for="item in letters" :key="item">{{ item }}</li>
            </ul>
        </div>
        <script src="node_modules/vue/dist/vue.js"></script>
        <script>
            const app = new Vue({
                el: "#app",
                data:{
                    letters:['A','B','C','D','E']
                }
            })
        </script>
    </body>
    ```

  * 然后你给数组中间添加一个"F": ``app.letters.splice(2, 0, "F")``，企图让数组变为``['A','B','F','C','D','E']``

  * 

#### 双向绑定 v-model

* v-model双向数据绑定可以简单理解为：
  * 后端定义的数据改变，前端页面展示的时候会自动改变
  * 数据通过前端页面修改的时候，后端定义的数据内容也会随之改变。
* v-model只能应用在表单元素中

##### 原理

 * v-model其实是一个语法糖，背后本质上包含两个操作：

   * v-bind绑定一个value属性
   * v-on指令给当前元素绑定input事件（input元素值发生改变时触发某事件）

   ```
   <body>
       <div id="app">
           <!-- <input type="text" v-bind:value="msg" v-on:input="valueChange"> -->
           <input type="text" v-bind:value="msg" @input="msg = $event.target.value">
           <h2> {{msg}} </h2>
       </div>
   
       <script src="./node_modules/vue/dist/vue.js"></script>
       <script>
           const app = new Vue({
               el: '#app',
               data: {
                   msg: '你好啊'
               },
               methods: {
                   valueChange(event) {
                       console.log(event.target)
                       this.msg = event.target.value
                   }
               }
           })
       </script>
   </body>
   
   ```


##### 结合radio类型使用

```
<body>
    <div id="app">
        <input type="radio" name="sex" value="male" v-model="sex">男
        <input type="radio" name="sex" value="female" v-model="sex">女
        <h2> 性别：{{sex}} </h2>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                sex: ''
            }
        })
    </script>
</body>
```

##### 结合checkbox使用

```
<body>
    <div id="app">
        你的爱好：<br/>
        <input type="checkbox" name="hobby" value="阅读" v-model="hobbies">阅读
        <input type="checkbox" name="hobby" value="跑步" v-model="hobbies">跑步
        <input type="checkbox" name="hobby" value="听音乐" v-model="hobbies">听音乐
        <input type="checkbox" name="hobby" value="看电影" v-model="hobbies">看电影
        <input type="checkbox" name="hobby" value="烘焙" v-model="hobbies">烘焙
        <h2> 爱好：{{hobbies}} </h2>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                hobbies: []
            }
        })
    </script>
</body>
```

##### 结合select使用

```
<body>
    <div id="app">
        你的爱好：
	    <select name="hobbies" v-model="hobbies">
            <option value="读书">读书</option>
            <option value="运动">运动</option>
            <option value="音乐">音乐</option>
            <option value="旅游">旅游</option>
            <option value="购物">购物</option>
	    </select>
        <h2>爱好：{{hobbies}} </h2>

        你喜欢的水果：
	    <select name="fruits" v-model="fruits" multiple>
            <option value="苹果">苹果</option>
            <option value="香蕉">香蕉</option>
            <option value="雪梨">雪梨</option>
            <option value="西瓜">西瓜</option>
            <option value="猕猴桃">猕猴桃</option>
	    </select>
        <h2>水果：{{fruits}} </h2>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                hobbies: "",
                fruits: []
            }
        })
    </script>
</body>
```

##### 修饰符

* lazy: 让数据在失去焦点或者按enter键的时候才更新，如: v-model.lazy=" " 
* number: 让输入框中输入的内容自动转成数字类型
* trim: 可以过滤内容左右两边的空格



#### 哪些数组方法是响应式的

* 响应式：当数据发生改变时，vue会自动检测数据变化，视图会发生对应的更新
* 并不是所有的数组方法都是响应式的，响应式的数组方法：
  * push\ pop\ shift\ unshift\ spike\ sort\ reverse
* 通过索引值修改数组的元素：
  * this.array[0] = 'bbbbbb' -但这种方式不是响应式的
  * Vue.set(this.array, 索引值, "修改后的值")



## 组件化开发

#### 组件之间的联系

(自己的总结哈) 组件之间的联系包括：

* 父传子(传数据)：``props``
* 子传父 (传事件)：``$emit()``
* 父访问子(获取组件对象)：``$refs`` (所有组件都有一个属性``$el``，用于获取组件中的元素)
* 非父子组件通信：``this.$bus.emit('')``、``this.$bus.on()``

#### 什么是组件化

* 把一个页面拆分成一个个小的功能块(组件)，每个功能块完成属于自己这部分独立的功能，这样容易页面的管理和维护。
* Vue组件化思想：
  * 任何应用都会被抽象成一颗组件树 (树结构)
  * 应用：在开发页面的时候，尽可能把页面拆分成一个个小的、可复用的组件

#### 注册组件的三个基本步骤

* 创建组件构造器：``Vue.extend()``

  * 传入的template代表我们自定义组件的模板
  * 该模板就是在使用组件的地方、要显示的HTML代码
  * 事实上，这种写法在Vue2.x的文档中几乎已经看不到了，它会直接使用下面会讲到的语法糖。但很多资料还是会提到这种方式，这种方式是学习后面方式的基础 (vue源码里还是调用Vue.extend)

* 注册组件(全局/局部)：``Vue.component()``

  * 传递两个参数：1. 注册组件的标签名 2. 组件构造器

* 使用组件：在Vue实例的作用范围内使用组件

  > 组件必须挂载在某个Vue实例所挂载的节点中，否则它不会生效。
  >
  > ```
  > <div id="app">
  > 	<my-cpn></my-cpn>
  > </div>
  > ```

#### 全局和局部组件

* 全局组件

  * 全局组件：通过调用Vue.component()注册的组件

  * 例如

    ```
    // 1.创建组件构造器对象
    const compnConstruc = Vue.extend({
                template: `
                <div>
                    <h2>我是标题</h2>
                    <p>我是内容，hhhhh</p>
                </div>`
            })
      
    // 2.注册全局组件
    Vue.component("my-cpn",compnConstruc)
    
    const app1 = new Vue({
    	el:'#app1',
    	data:{
    		msg:'你好啊'
    	}
    })
    ```

  * 可以在多个Vue实例下使用

    ```
    <div id="app1">
    	<my-cpn></my-cpn>
    </div>
    
    <div id="app2">
    	<my-cpn></my-cpn>
    </div>
    ```

    

* 局部组件

  * 局部组件：注册的组件是在某个实例中

  * 例如：
  
    ```
    // 1.创建组件构造器对象
    const compnConstruc = Vue.extend({
                template: `
                <div>
                    <h2>我是标题</h2>
                    <p>我是内容，hhhhh</p>
                </div>`
            })
      
    // 2.注册局部组件
    const app = new Vue({
    	el:'#app',
    	data:{
    		msg:'你好啊'
    	},
    	components:{
    		//标签名(可以加引号也可以不加！！！): 组件构造器
    		my-cpn: compnConstruc
    	}
  })
    ```

  * 只能在所在Vue实例中使用

#### 父组件和子组件

* 如下代码：

  * cpn1在cpn2组件构造器中注册，cpn1是cpn2的子组件，cpn2是父组件。
  * 子组件cpn1只能在父组件cpn2中使用，不能在Vue实例中使用 (除非cpn1在Vue实例中再注册一次)

  ```
  // 1.创建组件构造器对象
  const compnConstruc1 = Vue.extend({
              template: `
              <div>
                  <h2>我是标题</h2>
                  <p>我是内容，hhhhh</p>
              </div>`
          })
          
  const compnConstruc2 = Vue.extend({
              template: `
              <div>
                  <h2>我是标题</h2>
                  <p>我是内容，hhhhh</p>
                  <cpn1></cpn1>
              </div>`,
              components:{
              	cpn1: compnConstruc1
              }
          })
    
  // 2.注册组件
  // Vue实例其实就是根组件，是cpn2的父组件
  const app = new Vue({
  	el:'#app',
  	data:{
  		msg:'你好啊'
  	},
  	components: {
  		cpn2: compnConstruc2
  	}
  })
  ```

* 如果子组件cpn1没有在父组件cpn2注册，那么父组件在编译时就找不到组件cpn1，那么就会到全局组件中去找(不会到Vue实例去找)。运行如下代码：

  ```
  <body>
      <!--3. 使用组件-->
      <div id="app">
          <cpn2></cpn2>
      </div>
  
      <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          // 1.创建组件构造器对象
          const compnConstruc1 = Vue.extend({
              template: `
              <div>
                  <h2>我是标题</h2>
                  <p>我是内容，hhhhh</p>
              </div>`
          })
          
          const compnConstruc2 = Vue.extend({
              template: `
              <div>
                  <h2>我是标题</h2>
                  <p>我是内容，hhhhh</p>
                  <cpn1></cpn1>
              </div>`,
              /* components:{
              	cpn1: compnConstruc1
              } */
          })
    
          Vue.component('cpn1', compnConstruc1)
  
          // 2.注册组件
          const app = new Vue({
              el:'#app',
              data:{
                  msg:'你好啊'
              },
              components: {
                  cpn2: compnConstruc2
              }
          })
  
      </script>
  ```

#### 语法糖注册组件

* 省去了调用Vue.extend()步骤

  ```
  <body>
      <!--3. 使用组件-->
      <div id="app">
          <my-cpn></my-cpn>
          <cpn></cpn>
      </div>
  
      <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          // 全局组件
          Vue.component('my-cpn', {
              template: `
              <div>
                  <h2>我是标题</h2>
                  <p>我是内容，hhhhh</p>
              </div>`，
              data() {
                  return {
                      title: '我是标题hahahahahahah'
                  }
              }
          })
  
          // 局部组件
          const app = new Vue({
              el: '#app',
              data: {
                  msg: '你好啊'
              },
              components: {
                  'cpn': {
                      template: `
                      <div>
                          <h2>我是标题</h2>
                          <p>我是内容，hhhhh</p>
                      </div>`,
                      data() {
                          return {
                              title: '我是标题hahahahahahah'
                          }
                      }
                  }
              }
          })
      </script>
  </body>
  ```

#### 模板分离写法

* 使用script标签，注意type

  ```
  <!--用script标签，注意type-->
      <script type="text/x-template" id="cpn">
          <div>
              <h2>我是标题</h2>
              <p>我是内容，hhhhh</p>
          </div>
      </script>
  ```

* 使用template标签

  ```
  <!--用template标签-->
      <template id="cpn">
          <div>
              <h2>我是标题</h2>
              <p>我是内容，hhhhh</p>
          </div>
      </template>
      
    <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          // 全局组件
          Vue.component('my-cpn', {
              template: '#cpn'
          })
  
          // 
          const app = new Vue({
              el: '#app',
              data: {
                  msg: '你好啊'
              }
          })
      </script>
  ```
  
  

#### 组件数据的存放

* 子组件是不能引用父组件或者Vue实例的数据的

* 存放在组件对象的data属性中，这个data属性必须是一个函数

  * 为什么必须是个函数呢：如果组件data像Vue实例中data那样存储的是一个对象，那，当你重复用这个组件的时候(即有好几个``组件实例``)，如果改变其中一个``组件实例``中的数据(通过组件中methods里的函数来改变)，由于其他组件实例中的数据指向的都是组件data中的对象，那么其他``组件实例``中的相应数据就会跟着改变。
  * 所以Vue把data设置成函数就是为了防止多个组件实例之间相互干扰

* 这个函数返回一个对象，对象内部保存着数据

  ```
  <body>
      <!--3. 使用组件-->
      <div id="app">
          <my-cpn></my-cpn>
          <my-cpn></my-cpn> <!--组件实例-->
          <my-cpn></my-cpn>
      </div>
  
      <!--用template标签-->
      <template id="cpn">
          <div>
              <h2> {{title}} </h2>
              <p>我是内容，hhhhh</p>
          </div>
      </template>
  
      <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          // 全局组件
          Vue.component('my-cpn', {
              template: '#cpn',
              data() {
                  return {
                      title: '我是标题hahahahahahah'
                  }
              }
          })
  
          // 
          const app = new Vue({
              el: '#app'
          })
      </script>
  </body>
  ```

  

#### 父子组件的通信

前面提到，子组件是不能引用父组件或者Vue实例的数据的

#####　父传子

* 通过props向子组件传递数据。有两种方式：

  * 方式一：字符串数组

  * 方式二：对象，对象可以设置传递时的类型、默认值等

    ```
    <body>
        <div id="app">
            <cpn v-bind:cfoots="foots" :cmsg="msg"></cpn>
        </div>
    
        <template id="cpn"> 
            <div>
                <h1> {{cmsg}} </h1>
                <ul>
                    <li v-for="item in cfoots"> {{item}} </li>
                </ul>
            </div>
        </template>
    
        <script src="../node_modules/vue/dist/vue.js"></script>
        <script>
    
            const cpn = {
                template: '#cpn',
                // props: ['cmsg', 'cfoots']
                /* props: {
                    cfoots: Array,
                    cmsg: String
                } */
                props: {
                    cmsg: {
                        type: String,
                        default: 'aaa', // 默认值，即当实例<cpn>没有v-bind:cmsg的时候
                        required: true // 表示实例<cpn>必须绑定v-bind:cmsg，否则报错
                    },
                    cfoots: {
                        type: Array,
                        default: [] // 2.5.17以下为[]
                        // 更高版本以上中，当类型是数组或对象时，默认值必须是一个函数
                        /* default() {
                            return []
                        }*/
                        
                        /* type: Object,
                        default() {
                            return {msg: 'hello'}
                        }
                        */
                    }
                }
            }
    
            const app = new Vue({
                el: '#app',
                data: {
                    msg: '菜单',
                    foots: ['卤鸡爪', '红烧猪蹄', '爆炒花甲', '土豆红萝卜']
                },
                components: {
                    'cpn': cpn
                }
            })
        </script>
    </body>
    ```
    
  * **注意传参中加冒号和不加冒号的区别**：

    ``<cpn v-bind:cfoots="foots" :cmsg="msg"></cpn>``

    加冒号时，foots代表一个变量；不加冒号时，foots只是表示一个字符串

* props还能自定义类型，如下自定义Person：

  ```
  function Person(fn, ln) {
  	this.fn = fn
  	this.ln = ln
  }
  
  Vue.component('blog-post', {
  	props: {
  		author: Person
  	}
  })
  ```

* 注意，v-bind的参数是不支持驼峰式命名的，例如：

  ```
  v-bind:cMsg="msg"
  ```

  如果props里的名称是驼峰，在v-bind里应该这样写：

  ```
  v-bind:c-msg="msg"
  ```

  

##### 子传父

* 通过事件向父组件发送消息。

* 这个时候需要使用自定义事件来完成子传父(v-on不仅可以监听DOM事件，还可以监听自定义事件).

* 自定义事件流程：

  * 在子组件中，通过$emit()来触发事件
  * 在父组件中，通过v-on来监听子组件事件

  ```
  <body>
      <!--父组件模板-->
      <div id="app">
          <cpn @itemclick="cpnClick"></cpn>
      </div>
  
      <!--子组件模板-->
      <template id="cpn"> 
          <div>
              <button v-for="item in categories" @click="btnClick(item)"> {{item.name}} </button>
          </div>
      </template>
  
      <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          // 子组件
          const cpn = {
              template: '#cpn',
              data() {
                  return {
                      categories: [
                          {id: '01', name: '热门推荐'},
                          {id: '02', name: '手机数码'},
                          {id: '03', name: '家用家电'},
                          {id: '04', name: '电脑办公'},
                      ]
  
                  }
              },
              methods: {
                  btnClick(item) {
                      /* console.log(item) */
                      this.$emit('itemclick', item)
                  }
              }
              
          }
  
          // 父组件
          const app = new Vue({
              el: '#app',
              data: {
                  msg: '菜单'     
              },
              components: {
                  'cpn': cpn
              },
              methods: {
                  cpnClick(item) {
                      console.log('hhhh', item)
                  }
              }
          })
      </script>
  </body>
  ```

  

#### 父子组件访问

##### 父访子: $children $refs

* ``this.$refs.cpn3``这种获取到元素的方法，还能访问该组件的属性和方法，如``this.$refs.cpn3.name``

```
<body>
    <div id="app">
        <cpn ref="cpn1"></cpn>
        <cpn ref="cpn2"></cpn>
        <cpn ref="cpn3"></cpn>
        <button @click="btnClick()">按钮</button>
    </div>

    <template id="cpn">
        <div>我是子组件</div>
    </template>
    
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        const cpn = {
            template: '#cpn',
            data() {
                return {
                    name: '子组件名字为cpn'
                }
            },
            methods: {
                showMsg() {
                    console.log('smg')
                }
            }
        }

        const app = new Vue({
            el: "#app",
            data: {
                msg: "你好啊"
            },
            methods: {
                btnClick() {
                    // 1. $children 数组类型
                    /* console.log(this.$children) //获得一个数组
                    for(let c of this.$children) {
                        console.log(c.name)
                        c.showMsg()
                    } */

                    // 2. $refs 对象类型
                    console.log(this.$refs)
                    console.log(this.$refs.cpn3)
                }
            },
            components: {
                'cpn': cpn
            }
        })
    </script>
</body>
```



##### 子访父: $parent $root

```
<body>
    <div id="app">
        <cpn></cpn>
    </div>

    <template id="cpn">
        <div>
            <h2>我是cpn组件</h2>
            <ccpn></ccpn>
        </div>
    </template>

    <template id="ccpn">
        <div>
            <h2>我是ccpn组件</h2>
            <button @click="btnClick">按钮</button>
        </div>
    </template>
    
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        const cpn = {
            template: '#cpn',
            data() {
                return {
                    name: '我是cpn组件的name'
                }
            },
            components: {
                ccpn: {
                    template: '#ccpn',
                    methods: {
                        btnClick() {
                            // 1. 访问父组件 $parents
                            console.log(this.$parent) 
                            // 在开发里不建议使用子访问父，否则耦合度太高，组件复用性就不强了
                            console.log(this.$parent.name)

                            // 2. 访问根组件 $root
                            console.log(this.$root)
                            // 也很少访问根组件
                        }
            },
                }
            }
        }

        const app = new Vue({
            el: "#app",
            data: {
                msg: "你好啊"
            },
            components: {
                'cpn': cpn
            }
        })
    </script>
</body>
```



#### 插槽 slot

* ``<slot></slot>``，组件模板中可以被替换的标签。子组件中预留插槽，父组件使用子组件的时候可以决定插槽内容 。
* 组件的插槽是为了让我们封装的组件更加具有扩展性。(用人话说就是，你想在组件内部搞点内容的时候，你就给这个组件预留插槽。)
* 让使用者可以决定组件内部的一些内容到底展示什么
* 栗子：移动网站中的导航栏
  * 移动开发中，几乎每个页面都有导航栏
  * 导航栏我们必然会封装成一个插件，比如nav-bar组件
  * 一旦有了这个组件，我们就可以早多个页面中复用了
* 如何封装合适呢？——抽取共性，保留不同
  * 最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽
  * 一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容
  * 如搜索框、文字、菜单等等

##### 具名插槽的使用

```
<body>
    <div id="app">
        <my-cpn><span slot="center">标题</span></my-cpn>
        <my-cpn><button slot="left">返回</button></my-cpn>
    </div>

    <template id="cpn">
        <div>
            <slot name="left"><span>左边</span></slot>
            <slot name="center"><span>中间</span></slot>
            <slot name="right"><span>右边</span></slot>
        </div>
    </template>

    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>
        // 全局组件
        Vue.component('my-cpn', {
            template: '#cpn'
        })

        // 
        const app = new Vue({
            el: '#app',
            data: {
                msg: '你好啊'
            }
        })
    </script>
</body>
```



##### 作用域插槽

* 作用域

  * 父组件模板的所有东西都会在父级作用域内编译
  * 子组件模板的所有东西都会在子级作用域内编译

* 什么是作用域插槽：父组件替换插槽的标签，但是内容由子组件来提供

* 案例需求：获取子组件中的数据、在父组件中进行展示

  * 子组件cpn中包括一组数据

    ```
    const cpn = {
                template: '#cpn',
                data() {
                    return {
                        pLanguages: ['JS', 'C++', 'Java', 'C#', 'Python', 'Go', 'Swift']
                    }
                }
            }
    
            const app = new Vue({
                el: '#app',
                data: {
                    msg: '你好啊'
                },
                components: {
                    'my-cpn': cpn
                }
            })
    ```

  * 然后我要多次使用这个子组件cpn：

    ```
    <div id="app">
        <my-cpn></my-cpn>
        <my-cpn>
        </my-cpn>  
    </div>
    ```

  * 然后呢，我需要在第二个my-cpn中改变这个数据的展示形式，把原本为列表展示的改为横向展示。那么，这第二个my-cpn就需要获取到子组件cpn中的数据。

  ```
  <body>
      <div id="app">
          <my-cpn></my-cpn>
          <hr/>
          <my-cpn>
              <!--目的是获取子组件中的pLanguages-->
              <template slot-scope="slot">
                  <span v-for="item in slot.abc">{{item}}__</span>
              </template>
          </my-cpn>
          
      </div>
  
      <template id="cpn">
          <div>
              <slot :abc="pLanguages">
                  <ul>
                      <li v-for="item in pLanguages"> {{item}} </li>
                  </ul>
              </slot>
          </div>
      </template>
  
      <script src="../node_modules/vue/dist/vue.js"></script>
      <script>
          const cpn = {
              template: '#cpn',
              data() {
                  return {
                      pLanguages: ['JS', 'C++', 'Java', 'C#', 'Python', 'Go', 'Swift']
                  }
              }
          }
  
          const app = new Vue({
              el: '#app',
              data: {
                  msg: '你好啊'
              },
              components: {
                  'my-cpn': cpn
              }
          })
      </script>
  </body>
  ```

  

## 模块化开发

#### 为什么使用模块？

* 既避免变量重名（虽然可以用闭包）

* 也保证代码复用（但闭包引起代码不可复用）

  ```
  var ModuleA = (function() {
  	// 1.定义一个对象
  	var obj = {}
  	// 2.给对象添加属性和方法
  	obj.flag = true
  	obj.myfunc = function(info) {
  		console.log(info)
  	}
  	// 3.将对象返回
  	return obj
  })()
  ```

  ```
  if(ModuleA.flag) {
  	console.log('你是个天才')
  }
  
  ModuleA.myfunc('你长得真帅')
  ```

  

#### 常见的模块化规范

模块化有导出和导入两个核心。

常见的模块化规范有：CommonJS、ES6的modules、AMD、CMD

##### CommonJS 

​	Node里模块化就是按照这个规范实现的

  * 导出

    ```
    module.exports = {
    	flag: flag
    	sum: sum
    }
    ```

  * 导入

    ```
    let {flag, sum}= require('./aaa.js') //同时声明并初始化flag和sum连个变量
    
    //等同于：
    let _ma = require('./aaa.js')
    let flag = _ma.flag
    let sum = _ma.sum
    ```

    


##### ES6的Modules

  * 导出

    ```
    export {flag, sum}
    
    // 或者：
    export var flag = true
    export function sum(num1, num2) {
    	...
    }
    export class Person() {
    	constructor(name, age) {
    		this.name = name
    		this.age = age
    	}
    	
    	run() {
    		
    	}
    }
    ```

    

    * export default
      * 可以让导入者来命名模块

    ```
    // export default
    const address = "北京市"
    export default address
    
    // 导入的时候就可以自由命名模块了：
    // import aaa from "./aaa.js"
    ```

    ```
    export default function() {
    	console.log("北京市")
    }
    
    // import addr from "./aaa.js"
    ```

    

  * 导入

      * 注意要在HTML``<script>``中引入js文件，并且类型要设置为”module“

      * 在另一个js文件中导入模块：

        ```
        import {flag, sum} from "./aaa.js" //如果这里不是路径的话就会到node_modules文件夹里去找对应的东西
        ```

    * 统一全部导入

      ```
      import * as moduleA from "./aaa.js"
      consoloe.log(moduleA.flag)
      ```

      

## Webpack

#### 简介

* <b>是什么</b>
  * 本质上来说，是一个现代的JS应用的静态<b>模块</b><b>打包</b>工具，必须依赖于<b>Node</b>环境。
  * 打包：就是将webpack的各种资源模块合并成一个或多个包(Bundle)。打包过程中，还可以对资源进行处理，比如压缩图片，将scss转成css，将ES6语法转成ES5语法，将TypeScript转成JS等等操作。
* <b>作用</b>
  * 老师原话：本来用AMD、CMD、CommonJS、ES6这些模块是很难用的，即使你能用，但浏览器不识别，这个时候Webpack可以给你做一个底层的支撑。你在Webpack里用模块，它会帮你处理模块之间的关系网，并处理模块这些代码、把它们处理成浏览器能识别的代码，最终帮你打包，你再把打包的这些东西部署到服务器就可以了 (或者在index.html文件的script标签中引用这个打包的文件)。
  * 而且不仅仅是JS文件，CSS、图片、json文件等在webpack中都可以被当做模块来使用。这就是webpack中模块化的概念。
* <b>总之</b>：既保留了单个模块的可维护性，**又减少了页面的http请求**，减少了页面加载时间
* <b>必须要打包吗？</b>
  * 如果页面请求一次的话页面需要将许多次请求压缩成一次，就会导致页面第一次加载很慢，这也是打包过程中的一个弊端。

#### 安装(bug)

* 全局：在终端直接执行webpack命令，使用的是全局安装的webpack

* 局部：在package.json中定义了scripts时，其中包含了webpack命令，那么使用的是局部webpack

* Linux18.04全局安装 (不写@版本号的话安装的就是最新版本)：

  ```
  sudo npm install webpack@版本号 -g --unsafe-perm
  ```

* 局部安装本地webpack：在项目文件的终端中执行命令``npm install webpack@版本号 --save-dev``，其中``--save-dev``表示开发式依赖（即只有在开发时需要，在真正运行时不需要的。在运行时需要的话叫运行时依赖）

* 安装时解决bug：在教学视频中注意老师使用的vue版本、webpack版本以及下面所学的loader版本，版本太新或者太旧都可能会报错。

#### 基本使用

<b>一、准备工作</b>

* 创建文件夹：
  * dist文件夹：用于存放之后打包好的文件
  * src文件夹：用于存放源文件，如img\css\js文件夹、main.js

<b>二、打包</b>

* 有三种方式：

  * <b>方式一</b>：在当前文件夹中，终端输入命令：

    ```
    webpack ./src/main.js ./dist/bundle.js
    ```

  * <b>方式二：</b>.给webpack进行配置。新建一个webpack.config.js，配置文件中写：

    ```
    const path = require('path')
    // path是node的一个包，也就是说这要依赖node环境了
    // 所以，先进行初始化：npm init
    // 初始化完成之后就会自动生成package.json文件
    
    // 配置webpack的入口和出口
    module.exports = {
        entry: './src/main.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            // path里必须写绝对路径，我们要动态地获取路径
            // _dirname获取的就是现在这个文件所在的文件夹的绝对路径
            filename:'bundle.js'
        }
    }
    
    ```

    然后在终端执行命令：``webpack``

  * <b>方式三：</b>

    在package.json的script中添加：``"build":"webpack"``, 然后在终端执行命令：``npm run build``  （其实相当于方式二、在终端执行命令：``webpack``。那区别在哪呢？方式三这会优先在本地package.json找命令webpack。举个例子，如果你全局安装的是4.4版本的webpack，那么直接执行webpack的时候用的也是4.4版本，但这个项目是用3.5版本的webpack打包的呀，和全局的不一样，那这样如果直接执行webpack来打包就可能会有错误。————所以在项目文件夹中要局部安装和项目所依赖的webpack版本一样的本地webpack：``npm install webpack@版本号 --save-dev``，其中``--save-dev``表示开发式依赖）

* 打包好之后，dist文件夹里就会有一个名为hundle的js文件，这个就是打包好的文件

<b>三、引入</b>

* **最后在index.html的script标签中引入该bundle.js文件**



#### 处理CSS文件

* 在main.js引入css文件，加上如下代码即可：

  ```
   // 3.依赖CSS文件
   require('./css/normal.css')
  ```

需要css-loader和style-loader。根据文档提示使用loader

 (webpack中文文档→LOADERS→样式→css-loader)：

* 步骤一：安装。npm安装需要使用的loader。

* 步骤二：配置。在webpack.config.js中的modules关键字下进行配置。

  ```
  module: {
          rules: [
          {
              test: /\.css$/, //$符号表示全部的意思
              // css-loader只负责将css文件进行加载
              // style-loader只负责将样式添加到DOM中
              // 使用多个loader时，是从右向左读的
              use: ['style-loader', 'css-loader']
          }
          ]
      }
  ```

* 最后运行``npm run build``重新打包

#### 处理img文件

* 在src文件夹中再创建一个名为img的文件夹、并放一张名为test.jpg的图片。然后在normal.css文件中将背景设置为图片：

  ```
  body {
      /* background-color: rebeccapurple; */
      background: url('../img/test.jpg');
  }
  ```

  

##### 一、当图片小于limit时

* 需要url-loader。根据文档提示使用loader

 (webpack中文文档→LOADERS→样式→url-loader)：

* 步骤一：安装。

* 步骤二：配置。在webpack.config.js中的modules关键字下进行配置。

  ```
  module: {
          rules: [
          {
              test: /\.(png|jpg|gif|jpeg)$/,
              use: [
                {
                  loader: 'url-loader',
                  options: {
                    limit: 80000 //这个就是Bytes限制,可以自己手动更改！
                    // 当图片小于limit时，会将图片编译成base6字符串形式
                    // 当图片大于limit时，需要使用file-loader模块进行加载。
                  }
                }
              ]
           }
          ]
      }
  ```

* 最后运行``npm run build``重新打包

##### 二、当图片大于limit时(不懂)

* 除了url-loader还需要file-loader。

* 步骤一：再安装file-loader (安装后不需要进行配置)。

* 步骤二：publicPath配置

  * 当图片小于limit时，加载一张图是编译成为base64。而当图片大于limit时，图片就被打包、打包到dist文件夹里 (被打包形成的图片文件的文件名是通过哈希自动生成的，以防止文件名重复) 。

  * 假如我们现在直接就``npm run build``, 然后打开index.html，会发现图片不显示。鼠标在网页按右键点“Inspect” 查看"Elements"，如下图：

    ![image-20200923174818343](/home/cxt/.config/Typora/typora-user-images/image-20200923174818343.png)

    发现最底下"Styles"、"body"中background的url后面括号里是“2039d4e....jpg”（这是默认情况下webpack打包后生成的url, **这说明图片和index.html是在同一个文件夹下，但实际上并不在同一个文件夹下啊，所以要想办法在图片路径前加个dist/**）。这个jpg文件就是图片打包文件。

    所以我们知道，在网页中图片不显示是因为路径不对，因为我们整个程序是打包在dist文件夹下的，所以这里我们需要在路径下再添加一个dist/，要想办法让这个url变为“dist/2039d4e....jpg” (实际上是想办法让这个图片打包文件像hundle.js那样被找到、被解析？是这样吧？我自己猜的。。。。)。

  * 因此，这里需要在webpack.config.js这里进行配置，在output加一个publicPath：

    ```
    module.exports = {
        entry: './src/main.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            // path里必须写绝对路径，我们要动态地获取路径
            // _dirname获取的就是现在这个文件所在的文件夹的绝对路径
            filename:'bundle.js',
            publicPath: 'dist/' //只要涉及url的东西，都会自动在前面加上"dist/"
        },
    ```

* 最后运行``npm run build``重新打包

* 补充：图片打包文件名是哈希生成的，想要管理图片打包文件及文件名，可以在webpack.config.js中配置name：

  ```
  {
              test: /\.(png|jpg|gif|jpeg)$/,
              use: [
                {
                  loader: 'url-loader',
                  options: {
                      // 当图片小于limit时，会将图片编译成base6字符串形式
                      // 当图片大于limit时，需要使用file-loader模块进行加载。
                    limit: 80000,
                    name: 'img/[name].[hash:8].[ext]' //ext表示extention，即扩展名
                  }
                }
              ]
            }
  ```

#### 处理ES6

有些浏览器并不支持ES6，需要把ES6转成ES5。需要使用babel-loader

* 步骤一：安装

  ````
  npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
  ````

* 步骤二：配置。

  ```
  module: {
          rules: [
          {
              test: /\.js$/,
              exclude: /(node_modules|bower_components)/,
              // exclude表示排除，意思是不需要转化这些文件夹里的js文件
              use: {
                loader: 'babel-loader',
                options: {
                  presets: ['es2015']
                }
              }
          }
          ]
      }
  ```

* 最后运行``npm run build``重新打包

#### 使用Vue的配置过程

* 步骤一：安装vue模块。``npm install --save vue``

* 步骤二：在main.js导入：

  ```
  // 5.使用VUE开发
  import Vue from 'vue'
  const app = new Vue({
      el:'#app',
      data: {
          msg: 'helllo webpack'
      }
  })
  ```
  
* 然后在index.html挂载Vue实例：

  ```
  <div id="app">
     <h2>{{msg}}</h2>
  </div>
  ```

* 步骤四：指定runtime-compiler版本。vue在构建最终发布版本的时候构建了两类版本：

  * runtime-only 代码中不可以有任何的template
  * runtime-compiler 可以有template，因为有compiler可以编译template

  因此，要使用vue，就要指定runtime-compiler版本。

  在webpack.config.js中进行配置，在module.exports添加resolve对象：

  ```
  module.exports = {
  	resolve: {
      	// alias 别名
          alias: {
              // $表示结尾的意思
              'vue$': "vue/dist/vue.esm.js" 
              //当import vue的时候按照这个路径去node_modules文件夹中找vue模块。
              //vue.esm.js包括run-complier，可以编译template
              //而默认情况下指定的是node_modules/vue/dist文件夹中的vue.runtime.js        
          }
      }
  }
  ```

  

* 最后运行``npm run build``重新打包



#### Vue中el和template的关系

* 上面说到使用Vue的配置过程，其中Vue实例是挂载到index.html模板中。但实际上在项目中我们并不希望频繁修改index.html。

* 对此，可以这样处理：

  * 在index.html中只保留：

    ```
    <div id="app"></div>
    ```

  * 在main.js的Vue实例中增添template属性：

    ```
    const app = new Vue({
        el:'#app',
        template: `
            <div>
                <h2>{{msg}}</h2>
                <h2>我是template</h2>
            </div>
        `,
        data: {
            msg: 'helllo webpack'
        }
    })
    ```

  * **当el和template同时存在的时候**，Vue会将index.html中``<div id="app"></div>``这部分内容替换为template中包含div在内的内容

#### 终极处理.Vue文件

* 上面教程中Vue组件是写在main.js中的，但一个组件以这样的方式来使用其实是非常不方便的，比如编写template这块内容比较麻烦。

* 对此，我们以全新的方式来组织一个vue组件：

  * (main.js文件中)在Vue根组件中注册一个组件 'App'，并在template属性中使用。另外还引入App.vue文件：

    ```
    // 5.使用VUE开发
    import Vue from 'vue'
    
    import App from './vue/App.vue'
    const app = new Vue({
        el:'#app',
        components: {
            'App': App
        },
        template: '<App/>'
    })
    ```

  * 编写组件App。

    * 在src文件夹下创建名为vue的新文件夹，专门保存vue文件。
    * 在其中创建文件App.vue，输入vue然后按回车键就自动打出模板，其中template和js脚本是分开的。在其中又注册一个局部组件 'Cpn'，另外也引入Cpn.vue文件。

    ```
    <template>
      <div>
            <h2 class="msg">{{msg}}</h2>
            <h2>我是template</h2>
            <Cpn></Cpn>
        </div>
    </template>
    
    <script>
    import Cpn from './Cpn.vue'
    export default {
        name: 'App',
        data() {
            return {
                msg: 'helllo webpack'
            }
        },
        methods: {
    
        },
        components: {
            'Cpn': Cpn
        }
    }
    </script>
    
    <style>
        .msg {
            color: blueviolet;
        }
    </style>
    ```

  * 编写局部组件Cpn。在vue文件夹中再创建新文件Cpn.vue。

    ```
    <template>
      <div>
          <h2>{{msg}}</h2>
      </div>
    </template>
    
    <script>
    export default {
        name: 'Cpn',
        data() {
            return {
                msg: 'Hello Cpn!!!'
            }
        },
        methods: {
            
        }
    }
    
    </script>
    <style>
    </style>
    ```

* 由于现在有.vue文件，需要安装相应的loader：

  ```
  npm install vue-loader vue-template-compiler --save-dev
  ```

* 然后在webpack.config.js中进行配置：

  ```
   {
  	test: /\.vue$/,
  	use: ['vue-loader']
   }
  ```

* 配置之后执行npm run build会报错，大致意思是说需要plugin。这是因为使用14.版本以上的vue-loader（"^15.4.2"）还需要另外配置一个插件，但如果我们不想要安装这个插件，那就使用低版本的vue-loader：在package.json中把vue-loader版本改为：

  ```
  "vue-loader": "^13.0.0" //^符号表示会安装 ≥13.0.0且＜14.0.0的版本
  ```

  改完版本后删除node_moudules文件夹，然后执行npm install重新安装。

* 最后再执行``npm run build``

#### 认识plugin

* 是什么：webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化、文件压缩等等。
* loader和plugin的区别：
  * loader主要用于转换某些类型的模块，它是一个转换器。
  * plugin是插件，它是对webpack本身的扩展，是一个扩展器。
* plugin的使用过程：
  * 步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件就不需要安装)
  * 步骤二：在webpack.config.js中的plugins中配置插件。

##### 添加版权的plugin

* 先来使用一个最简单的插件，该插件名字叫BannerPlugin，属于webpack自带的插件，用于为打包的文件添加版权声明。

* 在webpack.config.js文件进行配置：

  ```
  // 由于这个plugin是webpack自带的，
  // 因此不需要通过npm安装，只需要通过require导入webpack
  const webpack = require('webpack')
  
  module.exports = {
  	...
  	plugins: [
  		new webpack.BannerPlugin('最终版权归codewhy所有')
  	]
  }
  ```

* 重新执行npm run build，会看见bundle.js文件的头部有如下信息：

  ```
  /*! 最终版权归codewhy所有 */
  ```

##### 打包html的plugin

* 目前我们的index.html文件是存放在项目的根目录下的。

* 在真实发布项目时，发布的是dist文件夹中的内容，但dist文件夹中如果没有index.html，那么打包的js等文件也就没有意义了。

* 所以，需要将index.html文件打包的dist文件夹中，这个时候就可以使用HTMLWebpackPlugin插件。

* **这个插件的作用**：

  * **自动生成一个index.html文件(可以指定模板来生成)**
  * **将打包的js文件，自动通过script标签插入到body中**

* 首先，先安装插件：

  ```
  npm install html-webpack-plugin --save-dev
  ```

* 然后在webpack.config.js中进行配置：

  ```
  // 1. 先引入插件
  const HtmlWebpackPlugin = require('html-webpack-plugin').
  
  // 2. 再使用
  module.exports = {
  	...
  	plugins: [
  		......
  		new HtmlWebpackPlugin({
  		  template: 'index.html'
  		})
  	]
  }
  ```

* 执行npm run build，这时候dist文件夹中多了一个文件index.html。这时有两个问题：

  * 第一个问题：路径问题。因为现在index.html在dist文件夹下了，那之后引用除了bundle.js的、其他打包在dist文件夹下的文件 (因为和index.html同一个文件夹) 也不需要再添加公共路径publicPath了。因此，把webpack.config.js中output下的publicPath注释掉。

    ```
    module.exports = {
        entry: './src/main.js',
        output: {
            path: path.resolve(__dirname, 'dist'),
            // path里必须写绝对路径，我们要动态地获取路径
            // _dirname获取的就是现在这个文件所在的文件夹的绝对路径
            filename:'bundle.js',
            /* publicPath: 'dist/' //只要涉及url的东西，都会自动在前面加上"dist/" */
        },
        module: {
            ...
        },
        resolve: {
           ...
        },
        plugins: [
           ...
        ]
    }
    
    ```

    另外，原来的index.html中的这条语句也可删掉了，因为之后插件HTMLWebpackPlugin会自动在新生成的index.htm中插入这行代码的。``<script src="./dist/bundle.js"></script>``

  * 第二个问题：要在dist文件夹下这个新生成的index.html中自动添加``<div id="app"></div>``。这可以通过指定原来的index.html作为模板(因为有``<div id="app"></div>``)来生成新的index.html。这只需要在webpack.config.js配置插件时传入一个参数：

    ```
    module.exports = {
    	...
    	plugins: [
    		new webpack.BannerPlugin('最终版权归codewhy所有'),
    		new HtmlWebpackPlugin({
    			template: 'index.html'
    		})
    	]
    }
    ```

* 搞定完上述两个问题，最后重新执行npm run build

##### 压缩js的plugin

* 在项目发布之前，需要对js文件进行压缩处理，否则很占空间。（**注意：在开发阶段并不需要丑化，因为需要进行调试，应该在要发布的时候再进行丑化。**）

* 首先安装：``npm install uglifyjs-webpack-plugin@1.1.1 --save-dev``

* 然后在webpack.config.js中进行配置：

  ```
  // 1. 先引入插件
  const UglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
  
  // 2. 再使用
  module.exports = {
  	...
  	plugins: [
  		new webpack.BannerPlugin('最终版权归codewhy所有'),
  		new HtmlWebpackPlugin({
  			template: 'index.html'
  		}),
  		new UglifyjsWebpackPlugin()
  	]
  }
  ```

* 最后重新执行npm run build进行打包。会发现dist文件夹下的bundle.js里的代码已被丑化。

* 注意：在开发阶段并不需要丑化，因为需要进行调试，应该在要发布的时候再进行丑化。

#### 搭建本地服务器

* webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。（这个express可以服务于某个文件夹，比如让它服务于dist文件夹，那之后它就会实时监听这里面发生的代码的改变，一旦发生改变，那就会对代码重新进行编译。重新编译的话它并不会生成新的文件，但它会生成新的需要编译的东西、但没有放到磁盘里面、而是暂时缓存到内存里面。执行npm run build，它再将这些东西输入到磁盘里面）

* 首先安装模块：``npm install webpack-dev-server@2.9.3 --save-dev``

* 然后在webpack.config.js中进行配置 (**只有在开发阶段的时候需要这个配置，因为开发的时候需要搭建一个本地服务，而在真正要打包、需要发布的时候其实是不需要的**)：

  ```
  module.exports = {
  	...
  	devServer: {
  		contentBase: './dist', //为哪一个文件夹提供本地服务
  		inline: true //是否实时监听页面
  		//port 端口号
  		// historyApiFallback: 在SPA单页面中，依赖HTML5DEhistory模式
  	}
  }
  ```

* 接着在package.json里配置一个脚本（--open 表示，当你执行这个脚本之后会自动打开网页）：

  ```
  "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1",
      "build": "webpack",
      "dev": "webpack-dev-server --open"
    },
  ```

* 最后跑起来，终端执行命令: ``npm run dev``

* 这样之后，当你修改代码并保存的时候，页面就会自动刷新。

* 总之，就是搭建一个本地服务，之后就在这个本地服务中进行测试。测试完了之后再执行npm run build进行打包，最后再把dist文件夹放在服务器进行部署

#### 分离webpack配置文件

* 由于开发时依赖的配置和发布时依赖的配置的不同，我们需要对webpack配置进行分离（知道这个分离配置文件的意义，对于理解脚手架的配置相对来说比较容易一点）。
* ①安装：``npm install webpack-merge --save-dev``
* ②创建文件夹和三个文件：
* ③package.json中分别为脚本build和dev指定配置文件：
* ④base.config.js中修改打包出口路径：

## CLI脚手架

#### 简介

* 为什么用：
  * 如果你只是简单写几个vue的Demo程序，那么不需要CLI。
  * 如果在开发大型项目，那么必然需要CLI。因为使用Vue.js开发大型应用时，我们需要考虑代码目录结构、项目结构和部署、热加载、代码单元测试等事情。如果每个项目都要手动完成这些配置，那么效率就很低，所以通常我们会使用一些脚手架工具来帮助完成。
* CLI是什么意思：
  * Command-Line Interface，翻译为命令行界面，但是俗称脚手架。
  * Vue CLI是一个官方发布vue.js项目脚手架
  * **使用vue-cli可以快速搭建Vue开发环境以及对应的webpack配置**。
* 依赖于Node环境，也依赖于webpack

#### 安装

* 全局安装：``npm install -g @vue/cli``

  可能遇到安装失败的问题，解决：

  * 以管理员身份打开终端执行命令
  * 清除文件：``npm clean cache -force``

* 目前安装的是脚手架3以上的版本，但我们现在也需要用脚手架2，就需要拉一个模板：``npm install @vue/cli-init -g``。安装好之后，我们既可以用脚手架2也可以用脚手架3了。

* 脚手架2初始化项目命令：``vue init webpack my-project``。执行命令之后就会在项目里生成webpack相关的配置文件

* 脚手架3创建项目命令：``vue create 项目名字``

#### CLI2初始化过程

##### CLI2详解

* 终端执行初始化命令：``vue init webpack vuecli2test``
* 根据提示输入东西：
  * ? Project name：直接敲回车
  * ? Project description：项目描述信息 ``test vue cli2``
  * ? Author：``XiTing`` 会默认读取全局gitconfig文件
  * **? Vue build**：选择Runtime-only而非Runtime +Compiler
  * ? Install vue-router：暂时不需要
  * **? Use ESLint to lint your code?**：ES-Lint表示对js代码进行限制，这就要求JS代码严格规范。如果你选择之后发现不喜欢或不习惯里面的限制，你也可以取消，在config文件夹下的index.js中找到useEslint，设置为false。目前暂时需要：选择Standard。
  * ? Set up unit tests：单元测试，暂时不要。
  * ? Setup e2e tests with Nightwatch?：端到端测试。暂时不需要。
  * ? Should we run `npm install` for you after the project has been created? (recommended) ：选择NPM

##### runtime和runonly的区别(p95集)

* Vue程序运行过程：①**template**传给Vue的时候Vue会做一个保存，保存到Vue实例下的options中。②然后对template进行parse，解析为**ast(抽象语法树)**，③再之后把ast编译为**render函数**，④然后通过render函数把template翻译为**虚拟DOM**，⑤最终渲染成**真实DOM**。

* 小结：template→ast→render函数→虚拟DOM→真实DOM。

* 选择runtime+compiler，那么就是按上面所说的步骤来运行Vue。

* 如果选择runtime-only，那么生成的main.js里会有render函数，而没有template

  ```
  import Vue from 'vue'
  import App from './App'
  
  Vue.config.productionTip = false
  
  /* eslint-disable no-new */
  new Vue({
    el: '#app',
    render: h => h(App)
  })
  ```

  相当于：

  ```
  new Vue({
    el: '#app',
    render: function (h) {
    	return h(App)
    }
  })
  // 其中h是函数createElement的缩写。
  ```

  那么Vue就直接从render函数开始通过render函数把APP这个组件变成虚拟DOM *（引入的App里是没有template的，其中template已经被vue-template-compiler这个loader解析为render函数了）*，这样的话Vue就不需要render函数之前的步骤，因此源代码量更少，性能也更高。因此选择runtime-only更好。

* 简单总结：
  * 如果之后的开发中你使用template，那么选择Runtime-compiler
  * 如果之后的开发中使用的是.vue文件夹开发，那么选择runtime-only。

##### 目录结构详解

* 先看package.json中的脚本：

  * "build": "node build/build.js"

    * node为JS代码提供一个运行环境，核心是v8引擎。v8引擎可以直接把js代码转为二进制代码来运行，为浏览器执行JS代码做了底层支撑。node就是我们在服务端执行JS代码的一个底层支撑。

    * 例如，创建文件test.js，在里面写一行代码``console.log('aaa')``。打开终端进入test文件所在的文件夹，输入node命令：``node test.js``，就可以执行这个test文件、并看到输出``aaa``
    * 因此，``npm run build``表示用node执行后面的js文件
* build和config文件夹里的文件都是webpack配置文件
* static文件夹放的是静态资源，里面的资源到时候会原封不动地复制到dist文件夹里面，如果资源放到src文件夹下的某个文件夹、那么到时候就会根据配置中的limit来处理。另外，里面的gitkeep文件作用是是保证文件上传到git？。
* babelrc文件：

  * 将ES6转为ES5的配置。``"browsers": ["> 1%", "last 2 versions", "not ie <= 8"]``告诉我们哪些浏览器需要适配，其中：``"> 1%"``表示浏览器市场份额占1％以上，``"last 2 versions"``表示浏览器最后两个版本。
  * stage 2表示ES其中一个阶段，只针对这个阶段的代码进行转换。
* editorconfig文件：对代码风格进行统一，比如缩进风格、最后一行换行、清除多余空格。正规公司一般都有这个文件。第一行root，为true时才开始解析下面的东西。
* eslintignore文件：对哪些文件夹或文件中不符合js规范的地方进行忽略。
* eslintrc文件：
* .gitignore文件：配置不需要上传到github的文件
* .postcssrc文件：CSS文件转化的配置
* index.html：模板。根据这个模板打包生成另一个放在dist文件夹中的index.html文件

##### 运行项目(bug)

* 终端输入：``npm run dev``把项目跑起来。

* 结果报错：vue Error from chokidar...... SPC: System limit for number of file watchers reached......

* 百度解决方案（有效，已解决）：

  It’s hitting your system's file watchers limit 

  Try ` echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p ` 

  Read more about what’s happening at https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers#the-technical-details https://github.com/gatsbyjs/gatsby/issues/11406

* 先执行命令` echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p ` ，再执行``npm run dev``，项目即可跑起来。

#### CLI3初始化过程

和CLI2没什么太大差异。

##### CLI3配置文件的查看和修改

* 方案一：终端执行全局命令``vue ui``启动配置服务器。

* 方案二：node_modules, @vue, cli-serve, webpack.config.js(可以看到导入文件'./lib/Service')，因此知道配置文件在lib文件夹中。

* 方案三：创建文件vue.config.js（必须这样命名），这个配置文件会自动和隐藏的配置进行合并

  ```
  module.exports = {
  	
  }
  ```

  

## vue router

#### 前提知识

##### 路由和渲染

* 什么是路由

  * 路由是一个网络工程里的术语，就是通过互联的网络把信息从源地址传输到目的地址的活动。

  * 路由中有一个非常重要的概念叫做路由表。路由表本质上就是一个映射表，决定了数据包的指向。

  * 映射表：

    [内网ip1: 电脑mac地址1

    内网ip2: 电脑mac地址2

    ......

    ]

    ？当信息来到路由器，会先查询电脑mac地址，然后根据映射表找到对应的内网ip，再决定信息传到哪个设备。
    
  * **路由是根据不同的 url 地址展示不同的内容或页面**

* 什么是前端路由、后端路由

* 什么是前端渲染、后端渲染

  * 前端渲染：浏览器中显示的页面中的大部分内容都是由前端写的js代码在浏览器中执行、最终渲染出来的页面。

##### 网页开发的几个阶段

* **后端路由阶段**（后端路由，后端渲染）
  * 早期的网站开发整个html页面是由服务器来渲染的。
  * 服务器直接生产渲染好对应的html页面，返回给客户端进行展示。
  * 但是一个网站这么多页面，服务器怎么处理呢？
    * 一个页面有自己对应的网址，也就是url，url会发送到服务器，服务器会通过正则对该URL进行匹配，并且最后交给一个Controller进行处理。
    * Controller进行各种处理，最终生成html或者数据，返回给前端，这就完成了一个IO操作。
  * 上面的这种操作，就是后端路由（由后端处理url和网页之间的映射关系的路由）。当我们页面中需要请求不同的路径内容时，交给服务器来进行处理，服务器渲染好整个页面，并且将页面返回给客户端。这种情况下渲染好的页面，不需要单独加载任何的Js和CSS，可以直接交给开浏览器展示，这样也有利于SEO优化。
  * 后端路由的缺点：
    * 整个页面的模块是由后端人员来编写和维护的。
    * 前端开发人员如果要开发页面，需要通过PHP和Java等语言来编写页面代码。
    * 而且通常情况下HTML代码和数据以及对应的逻辑会混在一起，编写和维护都是非常糟糕的事情。
* **前后端分离阶段**（后端路由，前端渲染）
  * 随着Ajax的出现，有了前后端分离的开发模式。后端只提供API来返回数据，前端通过Ajax获取数据，并且可以通过JS将数据渲染到页面中。
  * 这样做最大的优点就是前后端责任清晰，后端专注于数据，前端专注于交互和可视化上。并且当移动端出现后，后端不需要进行任何处理，依然使用之前的一套API即可。
  * 目前很多网站依然采用这种模式开发。
* **单页面富应用阶段**（前端路由，前端渲染）
  * SPA最主要的特点就是在前后端分离的基础上加了一层前端路由，也就是前端来维护一套路由规则（url和组件的映射关系）。

##### 改变url但不刷新的方法

前端路由的核心就是改变ulr但页面不进行整体的刷新。实现方法有：

* 方法一：改变URL的hash

  ```
  location.hash = "home"
  ```

* 方法二：HTML5的history模式：pushState （类似栈结构那样保存push的值）

  ```
  history.pushState({}, '', 'home')
  history.pushState({}, '', 'about')
  history.pushState({}, '', 'me')
  history.back()  //删除hash值me
  ```

* 方法三：HTML5的history模式：replaceState (替换值，而非像栈结构那样保存值)

  ```
  history.replaceState({}, '', 'home')
  history.replaceState({}, '', 'about')
  ```

* 方法四：HTML5的history模式：go (返回到某个“栈”)

  * history.back()等价于history.go(-1), history.forward()等价于history.go(1)

#### 简介

* vue-router是Vue的路由插件，适用于构建单页面应用(SPA)。
* 它是基于路由和组件的。路由用于设定访问路径，将路径和组件映射起来。在vue-router的单页面应用中，页面的路径改变就是组件的切换。

#### 基本使用

##### 搭建路由

* 步骤一，安装：``npm install vue-router --save`` （或者在初始化项目的时候安装：初始化一个项目(这里先用vue2)``vue init webpack vuerouter``,  在提示中选择安装vue-router）

* 步骤二，在模块化工程中使用它(因为它是一个插件，所以可以通过Vue.use()来安装路由功能)

  * 第一步：导入路由对象，并且调用Vue.use(VueRouter)

  * 第二步：创建路由实例，并配置路由映射关系

    ```
    // src文件夹下的router文件夹中的index.js文件中
    import Vue from 'vue'
    import Router from 'vue-router'
    
    // 1.通过Vue.use()安装插件
    Vue.use(Router)
    // 相当于底层执行Router.install操作
    
    // 2.创建VueRouter对象
    export default new Router({
      routes
    })
    ```
  ```
  
  ```
  
* 第三步：在Vue实例中挂载创建的路由实例
  
    ```
    //src文件夹下的main.js文件
    mport Vue from 'vue'
    import App from './App'
    import router from './router'
    
    Vue.config.productionTip = false
    
    /* eslint-disable no-new */
    new Vue({
      el: '#app',
      router,
      render: h => h(App)
    })
    ```

##### 配置路由(bug)

* 步骤一：创建路由组件

  * 在src-components文件夹中创建home和about两个组件

    ```
    <template>
      <div>
          <h2>我是home</h2>
      </div>
    </template>
    
    <script>
    export default {
        name: "home"
    }
    
    </script>
    <style>
    </style>
    ```

    

* 步骤二：配置路由映射：组件和路径映射关系

  ```
  // src文件夹下的router文件夹中的index.js文件中
  import Vue from 'vue'
  import Router from 'vue-router'
  import home from "../components/home"
  import about from "../components/about"
  
  // 1.通过Vue.use()安装插件
  Vue.use(Router)
  
  // 2.创建VueRouter对象
  // 这个routes变量的const声明一定要放在new Router之前，否则最终显示不出组件。
  const routes = [
  { //表示路径中只要出现了/home，那么就显示下面对应的组件
    path: '/home',
    component: home
  },
  {
    path: '/about',
    component: about
  }
  ]
  
  export default new Router({
    routes
  })
  
  ```

  

* 步骤三：使用路由。在App.vue中通过``<router-link>``和``<router-view>``标签。

  * ``<router-link>``该标签是一个vue-router中已经内置的组件，它会被渲染成一个``<a>``标签。
  * ``<router-view>``该标签会根据当前的路径动态渲染出不同的组件。
  * 网页的其他内容，比如顶部的标题/导航，或者底部的一些版权信息等会和``<router-view>``处于同一个等级。
  * 在路由切换时，切换的是``<router-view>``挂载的组件，其他内容不会发生改变。

##### 路由的默认路径：重定向

* 我们希望在默认情况下进入网站的首页，那如何让路径默认跳到首页、``<router-view>``渲染首页组件呢？

* 只需要多配置一个映射(重定向)就可以了：

  ```
  const routes = [
    {
      path: '',
      redirect: '/home'
    },
    ......
    ]
  ```

##### 更改路径改变的方式

上述路径的改变是通过改变url的hash来完成的，如果希望使用HTML5的history模式，只需要在index.js、new Router中进行配置：

```
export default new Router({
  routes,
  mode: 'history'
})
```

##### router-link补充

在前面的router-link中，我们只是使用了一个**属性to，用于指定跳转的路径**。它还有一些其他属性：

* tag：可以指定router-link之后渲染成什么组件，比如被渲染成li元素而非a元素

  ```
  <router-link to="/home" tag="button">主页</router-link>
  ```

* replace：指定replace的情况下，浏览器中后退键不能返回到上一个页面中。

  ```
  <router-link to="/home" tag="button" replace>主页</router-link>
  ```

* active-class：

  * 在网页中按f12进入后台，在Elements中你会发现，当你点击网页中某个router-link的时候Element中被渲染的对应的a元素会有个默认值为router-link-active的class属性。

  * 设置active-class可以修改默认的class值。例如把class默认值改为“active”

    ```
    <router-link 
    to="/home" 
    tag="button" 
    replace
    active-class=“active”>主页</router-link>
    ```

  * 当有多个router-link的class属性值需要统一修改为“active”的时候，可以在index.js的new Router中进行配置：

    ```
    export default new Router({
      routes,
      mode: 'history',
      linkActiveClass: "active"
    })
    ```

##### 通过代码跳转路由

不用``<router-link>``来实现路由跳转，例如：

```
<template>
  <div id="app">
    <a @click="homeClick">主页</a>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
  	homeClick() {
  	// 这个router就是index.js中new出来的router
  		this.$router.push('/home')
  		// this.$router.replace('/home')
  	}
  }
}
</script>

<style>

</style>
```

##### 动态路由

* 在某些情况下，一个页面的path路径可能是不确定的，比如我们进入用户界面时，希望是这样的路径：``/user/zhangsan``或``/user/lisi``，除了有前面的/user之外还跟上了目前登录用户的ID。

* 这种path和Component的匹配关系，我们称之为动态路由。

* 案例：

  * 新建一个组件，在component文件夹中新建文件user.vue

    ```
    <template>
      <div>
          <h2>我是用户界面</h2>
      </div>
    </template>
    
    <script>
    export default {
        name: "user"
    }
    
    </script>
    <style>
    </style>
    ```

  * 在index.js中配置路由映射关系（注意path有所不同）：

    ```
    import Vue from 'vue'
    ......
    import user from "../components/user.vue"
    
    // 1.通过Vue.use()安装插件
    Vue.use(Router)
    
    // 2.创建VueRouter对象
    const routes = [
      {
        path: '',
        redirect: '/home'
      },
      ...
      {
        path: '/user/:userid',
        component: user
      }
      ]
    
    ...
    ```

  * 在App.vue中使用：
  
    ```
    <template>
      <div id="app">
        <router-link to="/home">主页</router-link>
        <router-link to="/about">关于</router-link>
        <router-link :to="'/user/' + userId">用户</router-link>
        <router-view></router-view>
      </div>
    </template>
    
    <script>
    export default {
      name: 'App',
      data() {
        return {
          userId: 'zhangsan'
        }
      }
    }
    </script>
    
    ```
  
* 额外知识：希望能够在user组件中获取到路径上的id并显示到user组件中，应该怎么做呢？

  ```
  <template>
    <div>
        <h2>我是用户界面</h2>
        <h2> {{userId}} </h2>
    </div>
  </template>
  
  <script>
  export default {
      name: "user",
      computed: {
        userId() {
          return this.$route.params.userid
          //拿到的这个route就是当前处于活跃状态的路由
          // 获取到的这个userId就是index.js中的path: '/user/:userid',
        }
      }
  }
  
  </script>
  <style>
  </style>
  ```

  ```
  // 或者直接这样
  <h2> {{$route.params.userid}} </h2>
  ```

###### 通过代码跳转路由

不用``<router-link>``来实现路由跳转，例如：

```
<template>
  <div id="app">
    <a @click="userClick">主页</a>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userId: 'zhangsan'
    }
  },
  methods: {
  	userClick() {
  	// 每一个组件都有$router属性，这个router就是index.js中new出来的router
  		this.$router.push('/user/' + this.userId)
  		// this.$router.replace('/user/' + this.userId)
  	}
  }
}
</script>

<style>

</style>
```

###### route和router的区别

涉及源码，暂时不学......

* 区别：
  * $router为VueRouter实例，想要导航到不同URL，则使用$router.push方法。
  * $route为当前route跳转对象(当前处于活跃状态的路由)，里面可以获取name、path、query、params等。

##### 路由的懒加载

* 当我们打包构建应用时，JS包会变得非常大（``npm run build``打包之后dist-static文件夹下的js文件夹），影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件（这就叫懒加载，用到时再加载），这样就更高效了。
* 首先，我们知道路由中通常会定义很多不同的页面，这个页面最后一般被打包在一个js文件中。但是页面这么多放在一个js文件中，必然会造成这个js文件非常的大。如果我们一次性从服务器请求下来这个页面，可能需要花费一定的时间，甚至用户的电脑上还出现了短暂空白的情况，用户体验就非常差。因此要想办法对js包中js文件中的代码进行分离。最常见的一个做法就是使用路由懒加载。
* 路由懒加载做了什么：
  * 主要作用就是将路由对应的组件打包成一个个的js代码块
  * 只有在这个路由被访问的时候才加载对应的组件

###### 懒加载的方式

在index.js文件中修改组件导入方式：

```
......

const home = () => import('../components/home.vue')
const about = () => import('../components/about.vue')
const user = () => import('../components/user.vue')

// 1.通过Vue.use()安装插件
Vue.use(Router)

// 2.创建VueRouter对象
const routes = [
  {
    path: '',
    redirect: '/home'
  },
  { //表示路径中只要出现了/home，那么就显示下面对应的组件
    path: '/home',
    component: home
  },
  {
    path: '/about',
    component: about
  },
  {
    path: '/user/:userid',
    component: user
  }
  ]

......
```

重新执行``npm run build``后发现dist-static文件夹下的js文件夹中多了几个js文件，一个路由懒加载对应一个js文件。

#### 嵌套路由(bug)

* 嵌套路由是一个很常见的功能
  * 比如在home页面中，我们希望通过/home/news和/home/msg访问内容
  * 一个/home路径映射一个组件，访问/home/news和/home/msg也分别映射两个子组件。
* 实现嵌套路由的步骤：
  * 创建对应的子组件(例如在components文件夹下创建homeNews.vue和homeMsg.vue两个子组件)
  
  * 在index.js配置路由映射关系 (这里也配置了重定向)。
  
    ```
    ......
    const home = () => import('../components/home.vue')
    const homeNews = () => import('../components/homeNews.vue')
    const homeMsg = () => import('../components/homeMsg.vue')
    ......
    const routes = [
      ...
      { //表示路径中只要出现了/home，那么就显示下面对应的组件
        path: '/home',
        component: home,
        children: [
          {
            path: '',
            redirect: 'news'
          },
          {
            path: 'news',
            component: homeNews
          },
          {
            path: 'msg',
            component: homeMsg
          }
        ]
      },
      ......
    
    ```
  
  * 在home组件内部使用``<router-link>``和``<router-view>``标签
  
    ```
    <template>
      <div>
          <h2>我是home</h2>
          <router-link to="/home/news">新闻</router-link>
          <router-link to="/home/msg">消息</router-link>
          <router-view></router-view>
      </div>
    </template>
    
    <script>
    export default {
        name: "home"
    }
    
    </script>
    <style>
    </style>
    ```
  
  * 注意事项：在给子组件命名的时候，我命名为homenews.vue和homemsg.vue, 结果报错：``[Failed to mount component: template or render function not defined. ](https://www.cnblogs.com/fq1017/p/13366578.html)``，下面还有一串报错信息，大意是找不到template和render function。
  
    结合百度资料，我猜想可能是文件命名导致找不到文件？于是改成驼峰式命名，只改了homeNews.vue文件，另一个子组件命名依然为homemsg.vue，但结果就好了。然后我也不知道这是什么原因了......为了防止后面的操作再报错，还是改为homeMsg.vue了。

#### 参数传递(不懂)

* 传递参数的方式

  * 传递参数主要有params和query这两种类型

  * params类型 (前面讲动态路由的时候讲过)：

    * 配置路由格式：/router/:id

      ```
      // index.js文件中
      {
          path: '/user/:userid',
          component: user
        }
      ```

    * 传递的方式：在path后面跟上对应的值（？？？

      ```
      // App.vue文件中
      <router-link :to="'/user/' + userId">用户</router-link>
      ```

    * 传递后形成的路径：/router/123或/router/abc等等

      ```
      // user.vue文件中
      computed: {
            userId() {
              return this.$route.params.userid
              //拿到的这个route就是当前处于活跃状态的路由
              // 获取到的这个userid就是index.js中的path: '/user/:userid',
            }
          }
      ```

  * query类型

    * 配置路由格式：/router，也就是普通配置
    * 传递的方式：对象中使用query的key作为传递方式
    * 传递后形成的路径： /router?id=123或者/router?id=abc

* 为了演示传递参数(query类型)，我们这里再创建一个组件，并将其配置好：

  * 第一步：创建新组建Profile.vue

  * 第二步：配置路由映射

  * 第三步：使用<router-link>``

    ```
    <router-link :to="{path:'/profile', query: {name: 'ting', age: '25'}}">档案</router-link>
    ```

  * 最后打开网页查看路径有什么不同。URL：``scheme://host:port/path?query#fragment``

  * 额外补充：希望能够在profile组件中获取到路径上的id并显示到profile组件中，应该怎么做呢？

    ```
    // 在Profile.vue文件中
    <template>
      <div>
          <h2>我是profile</h2>
          <h2>{{$route.query}}</h2>
          <h4>{{$route.query.name}}</h4>
          <h4>{{$route.query.age}}</h4>
      </div>
    </template>
    ```

###### 通过代码跳转路由

不用``<router-link>``来实现路由跳转，例如：

```
<template>
  <div id="app">
    <a @click="profilerClick">主页</a>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      userId: 'zhangsan'
    }
  },
  methods: {
  	profileClick() {
  	// 每一个组件都有$router属性，这个router就是index.js中new出来的router
  		this.$router.push({path:'/profile', query: {name: 'ting', age: '25'}})
  		// this.$router.replace('/user/' + this.userId)
  	}
  }
}
</script>

<style>

</style>
```



#### 导航守卫

* 我们来考虑一个需求：在一个SPA应用中，当我们切换不同页面时，如何改变网页的标题呢？

  * 利用前置守卫。在index.js文件中

    ```
    ......
    
     const router = new Router({
      routes,
      mode: 'history',
      linkActiveClass: "active"
    })
    
    // 前置守卫(guard)/前置钩子
    router.beforeEach( (to, from, next) => { //这个to其实是routes数组里的一个个route
      document.title = to.matched[0].meta.title
      //console.log(to) 查看to对象，有matched
      next()
    })
    ```

  * 其中，to是指即将要进入目标的路由对象，from是当前导航即将要离开的路由对象。meta意思是描述数据的数据。

  * 调用next函数是因为只有调用该方法后才能进入下一个钩子(hook，也有回调的意思)。但后置钩子就不需要主动调用next()

* 什么是导航守卫？

  * vue-router提供的导航守卫主要用来监听路由的进入和离开的。

  * vue-router提供了beforeEach(前置钩子)和afterEach(后置钩子)的钩子函数，它们会在路由即将改变前和改变后触发

  * 这两个导航守卫被称之为全局守卫，除了这两个，还有：

    * 路由独享的守卫（只有进入某个路由里才会进行回调）
    * 组件内的守卫

    具体怎么使用看官网

#### keep-alive(bug)

如果我们想要在返回上一个页面时页面呈现的是原来浏览的内容，那么可以用到keep-alive。例如home页面中有新闻和消息，其中我点击消息显示局部页面；然后再点用户页面；再点回home页面，此时home页面我希望显示的是我刚刚点过的消息这个局部页面。

* keep-alive是Vue内置的一个组件，可以使被包含的组件保留状态(而非在路由跳转的时候就被销毁，或者回来的时候又被创建)，或避免重新渲染。router-view也是一个组件(vue-router内置)，如果直接被包在keep-alive里面，所有路径匹配到的视图组件都会被缓存。

* 首先，在App.vue中用``<keep-alive>``把``<router-view>``包围：

  ```
  <keep-alive>
      <router-view></router-view>
  </keep-alive>
  ```

* 然后在home.vue中：

  ```
  export default {
      name: "home",
      data() {
        return {
          path: '/home/news'
        }
      },
      // 先利用这个组件内导航守卫、在路由跳转之前、保存当前这个页面的路径
      beforeRouteLeave(to, from, next) {
        console.log(this.$route.path);
        this.path = this.$route.path;
        next()
      },
      // 然后该页面激活时push之前保存的路径
      activated() {
        this.$router.push(this.path)
      },
      deactivated() {
        console.log('deactivated');
      }
  }
  ```

  其中，activated()和deactivated() 函数**只有在有keep-alive的时候**才能执行。总之，被keep-alive包含的组件不会被频繁销毁和创建，这可以通过在home等组件下添加生命周期函数created()和destroyed()进行验证，例如：

  ```
  export default {
      name: "home",
      ...
      created() {
        console.log('home created');
      },
      destroyed() {
        console.log('home destroyed');
      }
  }
  ```

* bug及解决：最后重新打开浏览器页面的时候发现虽然实现了最终效果，但有报错：*Avoided* *redundant* *navigation* *to* *current* *location*，百度解决方案：在index.js中加入下面的代码：

  ```
  // 解决ElementUI导航栏中的vue-router在3.0版本以上重复点菜单报错问题
  const originalPush = Router.prototype.push
  Router.prototype.push = function push(location) {
    return originalPush.call(this, location).catch(err => err)
  }
  ```

###### keep-alive属性(bug)

* keep-alive有两个非常重要的属性：

  * include -字符串或正则表达，只有匹配的组件会被缓存

  * exclude -字符串或正则表达，任何匹配的组件都不会被缓存

    ```
    // user和Profile是这两个组件的name
    // user和Profile中间不需要加空格！
    <keep-alive exclude="user, Profile">
          <router-view></router-view>
    </keep-alive>
    ```

    * 分别在user和profile组件中添加生命周期函数created()和destroyed()进行验证。
    * bug及解决：打开浏览器网页后发现，只有user组件在创建和销毁时控制台没有显示，于是猜想可能又和文件名有关，于是改为User.vue，最终验证可行。



## Promise

#### 简介

* Promise是异步编程的一种解决方案（异步编程的其他解决方案还有哪些？）。Promise俗称**链式调用**，它是es6中最重要的特性之一。

* 那么什么时候我们会来处理异步事件呢？

  * 一种很常见的场景就是网络请求了。
  * 我们封装一个网络请求的函数，因为不能立即拿到结果，所以往往我们会传入另外一个函数，在数据请求成功时，将数据通过传入的函数回调出去。
  * 如果只是一个简单的网络请求，那么这种方案不会有很大麻烦。

* 但是，当网咯请求非常复杂时，就会出现回调地域。

* 什么是回调地狱？我们来考虑下面的场景(有夸张成分)：

  * 我们需要通过一个url从服务器加载一个数据data1，data1包含了下一个请求的url2

  * 我们需要通过data1取出url2，从服务器加载数据data2，data2中包含了下一个请求的url3

  * 我们需要通过data2取出url3，从服务器加载数据data3,data3包含了下一个请求的url4

  * 发生网络请求url4，获取最终数据data4。

    ```、
    $.ajax('url', function(data1) {
      $.ajax(data1['url2'], function(data2) {
        $.ajax(data2['url3'], function(data3) {
          $.ajax(data3['url4'], function(data4) {
            console.log(data4)
          }
        }
      })
    })
    ```

* 正常情况下，上面的代码没有什么问题，可以正常运行并且获取我们想要的结果。但是这样的代码难看且不容易维护。我们更加期望一种优雅的方式来进行这种异步操作。

* 因此我们需要一个叫做Promise的东西，来解决这个问题

* 当然，除了回调地狱之外，还有一个非常重要的需求：**为了我们的代码更加具有可读性和可维护性，我们需要将数据请求与数据处理明确的区分开来**。上面的写法，是完全没有区分开，当数据变得复杂时，也许我们自己都无法轻松维护自己的代码了。这也是模块化过程中，必须要掌握的一个重要技能，请一定重视。

#### Promise写法

###### 基本写法

```
new Promise((resolve, reject) => {
        setTimeout(() => {
            // 成功的时候调用resolve
            resolve('helloWorld')
            // 失败的时候调用reject
            reject('error message')
        }, 1000)
    }).then((data) => {
        console.log(data);
        console.log(data);
        console.log(data)
      }).catch((error) => {
        console.log(error);
        })
```



###### 另外一种处理形式

```
// 另一种写法：then里面放两个函数，
    // 一个是成功时调用的函数，一个是失败时调用的函数
    new Promise((resolve, reject) => {
        setTimeout(() => {
            // 成功的时候调用resolve
            resolve('helloWorld')
            // 失败的时候调用reject
            reject('error message')
        }, 1000)
    }).then(data => {
        console.log(data);
    }, error => {
        console.log(error);
    })
```

###### 只有一次异步操作时

```
// 当你在Promise的链式调用中只有一次异步操作，
    // 那么可以这样简写：
    new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('Hello')
        },1000)
    }).then(data => {
        console.log(data, '1');
        // 1) 改写原来的return new Promise(resolve => {})
            // reject 也是同理的，写成Promise.reject
        return Promise.resolve(data + '222')
    }).then(data => {
        console.log(data, '2');
        // 2) 改变原来的Promise.reject，写成throw
        throw 'error msg'
    }).then(data => {
        console.log(data, '3');
    }).catch(err => {
        console.log(err);
    })
```

###### 两次请求的情况all()

```
// 需求要两次请求才能完成的情况
    Promise.all([
        /* new Promise((resolve, reject) => {
            $.ajax({
                url: 'url1',
                success: function(data) {
                    resolve(data)
                }
            })
        }),
        new Promise((resolve, reject) => {
            $.ajax({
                url: 'url2',
                success: function(data) {
                    resolve(data)
                }
            })
        }) */

        new Promise((resolve, reject => {
            setTimeout(() => {
                resolve('1')
            }, 1000)
        })),
        new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve('2')
            }, 1000)
        })
    ]).then(results => {
        // 拿到的这个数据(可以起任何名，如results)是一个数组
        console.log(results);
    })
```

* 假如需要发送两个网络请求，all方法可以判断两个请求是否都成功。

#### Promise三种状态

  首先, 当我们开发中有异步操作时, 就可以给异步操作包装一个Promise
  异步操作之后会有三种状态。如图：

![img](https://img2018.cnblogs.com/i-beta/1843694/201912/1843694-20191203192522292-1377750546.png)

* pending：等待状态，比如正在进行网络请求，或者定时器没有到时间。
* fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()
* reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()



## vuex

#### 简介

* 官方解释：Vuex是一个专为Vue.js应用程序开发的状态管理模式。
* 状态管理到底是什么？
  * 状态暂时理解为变量，状态管理可以简单地将其看成把需要多个组件共享的变量存储在一个对象里面。然后将这个对象放在顶层的Vue实例，让其他组件可以使用。那么，多个组件就可以共享这个对象中的所有变量。
* 管理什么状态呢？
  * 比如用户的登录状态、用户名称、头像、地理位置信息等等。
  * 比如商品的收藏、购物车中物品等等。

#### 从单界面到多界面

###### 单界面状态管理

* 如下案例：在App.vue组件中设计一个计数器，然后通过**父传子通信**让子组件helloVuex中展示App组件中的数据counter：

  * 在这个案例中，需要管理的状态就是数据counter
    * 它需要被记录下来，也就是State
    * counter目前的值被显示在界面中，也就是View部分
    * 界面发生某些操作时 (案例中是用户的点击，也可以是用户的imput)，需要去更新状态，也就四Actions
    * 以上就是Vue中单个界面的状态管理
  * App组件：

  ```
  <template>
    <div id="app">
      <h2>-------App里的counter-------</h2>
      <h2> {{counter}} </h2>
      <button @click="counterAdd">+</button>
      <button @click="counterReduce">-</button>
      <h2>-------Vuex组件里的counter-------</h2>
      <hello-vuex :excounter="counter"></hello-vuex>
    </div>
  </template>
  
  <script>
  import helloVuex from './components/helloVuex'
  export default {
    name: 'App',
    components: {
      helloVuex
    },
    data() {
      return {
        counter: 0
      }
    }
  }
  </script>
  
  <style>
  </style>
  
  ```

  * helloVuex子组件：

  ```
  <template>
    <h2> {{excounter}} </h2>
  </template>
  
  <script>
  export default {
      name: 'helloVuex',
      props:{
          excounter: Number
      }
  }
  
  </script>
  <style>
  </style>
  ```

###### 多界面状态管理

* Vue已经帮我们做好了单个界面的状态管理，但如果是多个界面呢？

  * 多个视图都依赖同一个状态（一个状态改了，多个界面需要进行更新）
  * 不同界面的Actions都想修改同一个状态
  * 也就是说某些状态是属于多个视图共同想要维护的，那我们希望这些状态交给一个大管家来统一管理。

* Vuex就是大管家角色的一个工具

  * 我们现在要做的就是将共享的状态抽取出来，交给我们的大管家统一进行管理。
  * 之后，每个视图按照规定进行访问、修改等操作
  * 这就是Vuex背后的基本思想

* 例如上面的案例，我们希望用Vuex进行管理，那么可以这样做：

  * 首先安装Vuex: ``npm install vuex --save``

  * 在src文件下创建名为store(仓库的意思)的文件夹，然后在store中创建index.js文件：

    ```
    import Vue from 'vue'
    import Vuex from 'vuex'
    
    // 1.安装插件
    Vue.use(Vuex)
    
    // 2.创建对象,
    const store = new Vuex.Store({
        state: {
            counter: 100
        },
        mutations: {
    
        },
        actions: {
    
        },
        getters: {
    
        },
        modules: {
    
        }
    })
    
    export default store
    ```

  * 这样的话App组件中的对应的地方也需要修改：

    ```
    <template>
      <div id="app">
        <h2>-------App里的counter-------</h2>
        <h2> {{$store.state.counter}} </h2>
        <button @click="counterAdd">+</button>
        <button @click="counterReduce">-</button>
        <h2>-------Vuex组件里的counter-------</h2>
        <hello-vuex></hello-vuex>
      </div>
    </template>
    
    <script>
    import helloVuex from './components/helloVuex'
    export default {
      name: 'App',
      components: {
        helloVuex
      },
      data() {
        return {
          counter: 0
        }
      }
    }
    </script>
    
    <style>
    </style>
    ```

  * helloVuex组件也不需要通过App组件来传递数据：

    ```
    <template>
      <h2> {{$store.state.counter}} </h2>
    </template>
    
    <script>
    export default {
        name: 'helloVuex'
    }
    
    </script>
    <style>
    </style>
    ```

  * 最后执行``npm run dev``发现显示成功

###### 单一状态树state

* vuex提出单一状态树(Single Source of Truth)，也可以翻译成单一数据源。
* 如果你的状态信息是保存到多个Store对象中，那么之后的管理和维护都会变得特别麻烦。所以Vuex使用了单一状态树来管理应用层级的全部状态。
* 单一状态树能够让我们以最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中也可以方便管理和维护。

#### vuex-mutations

* 继续上面那个案例，其中还有未完善的地方，就是App.vue组件中两个按钮中的事件。

* 这两个事件都想要改变状态(即变量counter)。**在vuex中，应该通过mutations来更改这个State (而不应该直接去修改State，否则devtools无法跟踪到是哪个组件更改了State)**

* 具体做法：

  * store下的index.js文件中，在mutations中添加方法:

    ```
    mutations: {
            incre(state) {
                state.counter++
            },
            decre(state) {
                state.counter--
            }
        }
    ```

  * App.vue中添加方法：表示给store提交某个事件，这个事件名就是mutations里定义的事件名

    ```
    methods: {
        counterAdd() {
            this.$store.commit('incre')
          },
          counterReduce() {
            this.$store.commit('decre')
          }
      }
    ```

###### mutations状态更新

* Vuex的store状态更新的唯一方式：提交mutation
* mutation主要包括两部分：
  * 字符串的事件类型type(如incre)
  * 一个回调函数handler(如incre() {...})，该回调函数的第一个参数就是state。

###### mutations携带参数

* 在更新状态的时候，有时候我么你希望携带一些额外的参数

  * 参数被称为是mutation的载荷(Payload)

* 比如如下案例：数字增加按钮、添加学生

  * App.vue文件中：

    ```
    <button @click="addCount(5)">+5</button>
    <button @click="addCount(10)">+10</button>
    <br/>
    姓名：<input v-model="stuName"/>
    <br/>
    年龄：<input v-model="stuAge"/>
    <br/>
    性别：<input v-model="stuSex"/>
    <br/>
    <button @click="addStu">+stu</button>
    <h4> {{$store.state.students}} </h4>
        
    ......
        methods: {
        counterAdd() {
          this.$store.commit('incre')
        },
        counterReduce() {
          this.$store.commit('decre')
        },
        addCount(count) {
          /* this.$store.commit('increCount', count) */
          this.$store.commit({
            type: 'increCount',
            count
          })
        },
        addStu() {
          var stu = {}
          stu.name = this.stuName
          stu.age = this.stuAge
          stu.sex = this.stuSex
          this.$store.commit('increStu', stu)
        }
    
      }
    ```

  * store下的index.js文件中：

    ```
    ......
    mutations: {
            .......
            increCount(state, count) {
                state.counter += count
            },
            increStu(state, stu) {
                state.students.push(stu)
            }
        },
    ```

###### mutations提交风格

* 上面的通过commit进行提交是一种普通的方式，Vue还提供了另外一种风格，commit的是一个对象：

  * App.vue中：

    ```
    addCount(count) {
          /* this.$store.commit('increCount', count) */
          this.$store.commit({
            type: 'increCount',
            count
          })
        },
    ```

  * store下的index.js中：

    ```
    mutations: {
      ......
      /* increCount(state, count) {
         state.counter += count
      }, */
      increCount(state, payload) {
        state.counter += payload.count
      }
      ......
    },
    ```

###### mutations响应规则

* Vuex的store中的state是响应式的，当state中国的数据发生改变时，Vue组件会自动更新。

* 这就要求我们必须遵守一些Vuex对应的规则：

  * 如果state中有一个变量是对象，那么需要提前在store中初始化好所需的属性

  * 当给state中的对象添加属性时，使用下面的方式：

    * 方式一：**使用Vue.set(obj, 'newProp', 123)** 

      （删除的属性的话用Vue.delete方法）

    * 方式二：用新对象给旧对象重新赋值

* 看如下例子：在state中有一对象info, 里面包含name、age、height属性，在App.vue中有一按钮，点击按钮后可触发修改info的事件。

  * store下的index.js文件的mutations中定义了修改info的事件：

  ```
  state: {
    ......
    info: {name: 'zhuzhu', age: 2, height: 56}
      },
    mutations: {
      updateInfo(state) {
      // 下面修改属性和添加属性的方法都只能更改数据而无法更新视图，即做不到响应式
      // state.info.name = 'baobao'
      // state.info['address'] = '洛杉矶'
      Vue.set(state.info, 'address', '洛杉矶')
      // 这种删除方式也做不到响应式
      // delete state.info.age
      Vue.delete(state.info, 'age')
    }
   },
  ```

######　mutations常量

* 我们来考虑下面的问题：在mutation中，我们定义了很多事件类型(也就是方法名称)

* 当我们项目增大时，Vuex管理的状态越来越多，需要更新状态的情况越来越多，那么意味着mutations中的方法越来越多

* 方法过多，使用者需要花费大量精力去记住这些方法甚至是多个文件间来回切换查看方法名称，甚至如果复制的时候，可能还会出现写错的情况。

* 那么最好把这个mutations事件类型(也就是方法名字)抽成常量。（官方推荐）

* 例如把mutations中的事件类型incre抽取成常量：

  * 在store中新建mutations-types.js文件：

    ```
    export const INCREMENT = 'increment'
    ```

  * 分别在App.vue和index.js中导入该文件：

    ```
    import { INCREMENT } from '......'
    // 因为mutations-types.js文件中的导出并没有default，所以导入的时候要加大括号
    ```

  * store下的index.js文件中：

    ```
    mutations: {
      [INCREMENT](state) {
        state.counter++
      },
      ...
    }
    ```

  * App.vue中：

    ```
    methods: {
      counterAdd() {
        this.$store.commit(INCREMENT)
      },
      ....
    }
    ```

    

#### vuex-actions

* 通常情况下，Vuex要求我们mutations中的方法**必须是同步方法**

  * 主要原因是当我们使用devtools时，可以捕捉mutation的快照（哪个组件更改了state）

  * 但如果是异步操作，那么devtools将不能很好地追踪这个操作什么时候会被完成。

    * 比如上述案例中修改信息的那段代码，devtools中可以跟踪到操作：

      ```
      mutations: {
          updateInfo(state) {
          // 下面修改属性和添加属性的方法都只能更改数据而无法更新视图，即做不到响应式
          // state.info.name = 'baobao'
          // state.info['address'] = '洛杉矶'
          Vue.set(state.info, 'address', '洛杉矶')
          // 这种删除方式也做不到响应式
          // delete state.info.age
          Vue.delete(state.info, 'age')
        }
      ```

    * 如果updateInfo函数里是一个回调函数：

      ```
      mutations: {
          updateInfo(state) {
            setTimeout(() => {
              state.info.name = 'baobao'
            }, 1000)
          }
        }
      ```

      执行后发现，点击修改信息，视图进行了更新，但devtools中并没有跟踪。

* 如果确实希望在Vuex中进行一些异步操作，比如网络请求，这个时候怎么处理呢？

  * Action类似于Mutation，但是是用来代替Mutation进行异步操作的。

  * 上述回调函数的案例可以这么写：

    * 关键词：commit，dispatch, payload, Promise
    * store下的index.js文件中：

    ```
    ......
    mutations: {
      updateInfo(state) {
        state.info.name = 'baobao'
      }
    },
    actions: {
      // context在actions中表示store, payload表示组件中传过来的参数
      /*aupdateInfo(context, payload) {
        setTimeout(() => {
          context.commit('updateInfo')
          console.log(payload.msg)
          payload.success()
        }, 1000)
      } */
      
      // Promise方法
      aupdateInfo(context, msg) {
        return new Promise((resolve) => {
          setTimeout(() => {
            context.commit('updateInfo')
            console.log(msg);
            resolve('已经更改成功！')
          }, 1000)
        })
      }
    ```

    * App.vue中：

    ```
    methods: {
      updateInfo() {
        /* this.$store.dispatch('aupdateInfo', {
          msg: '我是携带的信息',
          success: () => {
            console.log('已经更改成功')
          }
        }) */
    	
    	// Promise方法
        this
          .$store.dispatch('aupdateInfo',  '我是携带的信息')
          .then(data => {
          console.log(data)
        })
     }
    }
    ```

    

#### vuex-getters

* 相当于vue中的计算属性。

* 用法看如下案例： state中有一个数组，里面存储多个对象(学生的信息)，现在我需要提取出符合某个条件(如年龄大于20)的所有对象。提示：用es6中的filter数组方法

  * store的index.js文件：

  ```
  ......
  state: {
    counter: 100,
    students: [
      {name: 'zhuwen', age: 56, sex: '男'},
      {name: 'qimei', age: 57, sex: '女'},
      {name: 'xiting', age: 25, sex: '女'},
      {name: 'yuqi', age: 15, sex: '女'},
      {name: 'zihuai', age: 56, sex: '男'},
      {name: 'dazhuti', age: 28, sex: '男'}
          ]
  },
  
  getters: {
    out20stu(state) {
      return state.students.filter(s => s.age > 20)
    }
  },
  ```

  * App.vue文件：

  ```
  <h2> {{$store.getters.out20stu}} </h2>
  ```

* 以上就是getters的用法。除此之外，getters也可以作为参数进行传递，例如我需要展示这个筛选出来的学生的个数：

  * store的index.js文件：

  ```
  ......
  state: {
    ......
  },
  
  getters: {
    ......
    out20stuLength(state, getters) {
      return getters.out20stu.length
    }
  },
  ```

  * App.vue文件：

  ```
  <h2> {{$store.getters.out20stuLength}} </h2>
  ```

* 此外，getters里的函数还可以返回一个函数。比如上述案例中的筛选条件age是动态的，比如App.vue中传入14这个年龄条件``<h2> {{$store.getters.out20stu(14)}} </h2>``，那么又如何筛选出符合这个条件的学生呢？提示：getters定义一个返回函数的函数。

  * store的index.js文件：

  ```
  ......
  state: {
    ......
  },
  
  getters: {
    ......
    outAgeStu(state) {
      // 奇怪，这里不能使用箭头函数
      return function(age) {
        return state.students.filter(s => s.age > age)
      }
    }
  }
  ```

  * App.vue文件：

  ```
  <h2> {{$store.getters.outAgeStu(14)}} </h2>
  ```

#### vuex-module(略记)

* 略记并不是说这块内容不重要，只是表示在这里省略了笔记，具体的知识点要去看代码

###### 认识Module

* 为什么在Vuex中我们要使用模块呢？
  * Vuex使用单一状态树，那么也意味着很多状态都会交给Vuex来管理。
  * 当应用变得非常复杂时，store对象就有可能变得相当臃肿
  * 为了解决这个问题，Vuex允许我们将store分割成模块，而每个模块拥有自己的state、mutations、actions、getters等

###### state

###### mutations

###### getters

###### actions

#### store文件夹目录组织



## 网络模块封装axios(略记)

* 略记并不是说这块内容不重要，只是表示在这里省略了笔记，具体的知识点要去看代码
* 为什么要封装网络请求模块？（摘录老师口语：）
  * 在开发前端应用程序的过程中，我们要**向服务器请求数据、对请求过来的数据进行展示**，这个时候就必然要用到网络请求。
  * 网络请求有很多选择。**即使选择第三方框架来进行网络请求，我们也要对它进行封装**。然后也不是用第三方框架来进行网络请求的，而是使用我们自己已经封装好的模块。
  * 为什么不使用第三方框架呢，因为担心它有一天不被维护或出现严重的漏洞。到时再换另一个第三方框架，也是非常麻烦的事情。
* Vue中发送网络请求有非常多的方式：
  * 传统的Ajax是基于XMLHttpRequest。为什么不用它呢？因为它配置和调用方式等非常混乱，编码起来就非常麻烦。所以真实开发中很少直接使用，而是使用JQuery-Ajax。
  * 但Vue中为什么也不使用JQuery-Ajax呢？首先，我们先明确一点，在Vue的整个开发中都是不需要使用JQuery了。因为它是一个重量级框架(代码1w+，而Vue的代码才1w+)。
  * 官方在Vue1.x的时候推出了Vue-resource，但作者表示不再继续更新和维护。同时，作者还推荐了一个框架：Axios。Axios有非常多的优点，并且用起来也非常方便。
    * axios功能特点：
      * 在浏览器中放XMLHttpRequest请求
      * 在node.js中发生http请求
      * 支持Promise API
      * 拦截请求和响应
      * 转换请求和响应数据
      * ......

#### JSONP的原理和封装 

* 在前端开发中，常见的一种网络请求方式就是JSONP。使用JSONP最主要的原因往往是为了解决跨域访问的问题。

###### JSONP的原理

* JSONP的核心在于通过script标签的src来帮助我们请求数据。将数据当做一个js的函数来执行，并在执行的过程中传入我们需要的json。
* 所以，封装jsonp的核心就在于我们监听window上的JSONP进行回调时的名称。

###### JSONP封装(看不懂)

```
let count = 1
export default function originPJSONP(option) {
  // 1.从传入的option中提取URL
  const url = option.url
  
  // 2.在body中添加script标签
  const body = document.getElementByTagName('body')[0]
  const script = document.createElement('script')
  
  // 3.内部生产一个不重复的callback
  const callback = 'jsonp' + count++
  
  // 4.监听window上的jsonp的调用
  return new Promise((resolve), reject) => {
    try {
      window[callback] = function(result) {
        body.removeChild(script);
        resolve(result)
      }
      const params = handleParam(option.data)
      script.src = url + '?callback' + callback + params
      body.appendChild(script)
    }catch(e) {
      body.removeChild(script)
      reject(e)
    }
  })
  
}
```



```
function handleParam(data) {
  let url = ''
  for (let key in data) {
    let value = data[key] !== undefined ? data[key] : ''
    url += '&${key} = ${encodeURIComponent(value)}'
  }
  return url
}
```





#### axios框架的基本使用

* 准备工作：用脚手架2初始化一个项目，把Components文件夹下的helloworld.vue及相关代码删除。
* 安装axios: ``npm install axios``
* 教程中老师服务器的地址：http://123.207.32.32:8000/home/multidata



#### axios发送并发请求

* 有时候我们可能需要同时发送两个请求，两个请求成功了才执行下一步操作。
* axios中可以使用axios.all

#### axios全局配置

* 事实上，在开发中可能很多参数都是固定的。这个时候我们可以进行一些抽取，也可以利用axios的全局配置。
* 常见的配置选项：
  * 请求类型 ``method: 'get/post'``
  * url查询对象 ``params: {id: 12}``
  * request body ``data: {key: 'aa'}``



#### axios实例

* 为什么要创建axios的实例呢？
  * 当我们从axios模块中导入对象时，使用的实例是默认的实例。
  * 当给该实例设置一些默认配置时，这些配置就被固定下来了。但是在后续开发中，某些请求和其他请求的配置可能会不太一样。比如某些请求需要使用特定的baseURL或者timeout或者content-Type等，而其他请求就不需要。
  * 这个时候，我们就可以创建新的实例，并且传入**属于该实例**的配置信息。



#### axios模块封装

* 为什么要封装模块呢？
  * 只要引用了第三方东西，例如axios，就不能在每个组件都去引用这个第三方的东西，不能让组件对第三方的东西产生依赖，否则一旦第三方的东西不再被维护，那么后期就得一个一个组件文件去改。因此，必须要对第三方的东西做一个封装。
  * 封装首先在src下单独建一个文件夹network, 专门存放网络层的封装。



#### axios拦截器

* axios提供了拦截器，用于我们在放每次请求或者得到响应后，进行对应的处理。例如请求成功/失败后进行拦截，响应成功/失败后进行拦截
* 过程：**拦截请求**、发送请求、**拦截响应**、处理响应到的数据 (即main.js中.then里的语句)