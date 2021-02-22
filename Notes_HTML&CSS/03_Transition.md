## vue里的过渡

* 方法1: 点击触发@click事件、改变元素样式
* 方法2:  
  * 给要过渡的元素包裹上``<transition>``组件，并设置name
  * 在CSS中设置``<transition>``组件的样式
    * .name-enter-active
    * .name-leave-active
    * .name-enter-to
    * .name-leave-to