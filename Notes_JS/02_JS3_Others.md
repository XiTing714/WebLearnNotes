

#### JS中的事件循环

* setTimeout即使延迟时间为设计为0，但实际上并不会立即执行其中的代码，还是会延迟那么一点点时间再执行。

#### apply的应用

https://www.jb51.net/article/164140.htm

本文实例讲述了JS中apply()的应用。分享给大家供大家参考，具体如下：

先从`Math.max()`函数说起，`Math.max`后面可以接收任意个参数，最后返回所有参数中的最大值。

比如：

```
alert(Math.max(5,8));``//8``alert(Math.max(5,7,3,1,9,2));``//9
```

但是在很多情况下，我们需要找出数组中最大的元素。

比如：

```
/*`` ``* 找出数组中最大的数`` ``*/``var` `arr = [1,4,9,6];``//alert(Math.max(arr));//NaN，这种用法不对``function` `max(arr){`` ``var` `arrLen = arr.length;`` ``var` `maxValue = arr[0];`` ``for``(``var` `i=0;i<arrLen;i++){``  ``var` `maxValue = Math.max(maxValue,arr[i]);  `` ``}`` ``return` `maxValue;``}``alert(max(arr));``//9
```

上面的写法麻烦而且低效。我们用apply()试试。

```
/*`` ``* 用apply()找出数组中最大的数`` ``*/``var` `arr = [1,4,9,6];``function` `getMax1(arr){`` ``return` `Math.max.apply(Math,arr);``//第一个参数也可以填this或null``}``alert(getMax1(arr));``//9
```

这两段代码达到了相同的效果，但是getMax1()却优雅，简洁，而且高效。



#### setTimeout用法

```
setTimeout(func,time)
```

这个func，要么是一个函数，要么是一段包含着要执行代码的字符串

* 函数：

  ```
  function debounce(func, delay) {
        var timer = null
        return function() {
          clearTimeout(timer)
          timer = setTimeout(func,delay) //其实为了避免错误不要这么封装
        } 
      }
  
  window.onresize = debounce(function() {
        console.log('aaa');
      }, 3000)
  ```

* 字符串：

  ```
  function debounce(func, delay) {
        var timer = null
        return function() {
          clearTimeout(timer)
          timer = setTimeout(func,delay)
        }
      }
  
  window.onresize = debounce("console.log('aaa')", 3000)
  ```

  