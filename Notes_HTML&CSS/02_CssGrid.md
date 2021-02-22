## 概述



## 基本概念



## 3 容器属性

### 3.1 display



### 3.2 列宽行高grid-template-columns/rows

* 列宽和行高

#### 1) repeat(num1, num2)

* `repeat()`接受两个参数，第一个参数是重复的次数，第二个参数是所要重复的值。

#### 2) repeat(auto-fill, num)

* `auto-fill`关键字表示自动填充，第二个参数表示列宽/行高

#### 3) fr: 倍数

* 如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。

#### 4) minmax()长度范围

#### 5) num auto num

* > grid-template-columns: 100px auto 100px;

  上面代码中，第二列的宽度，基本上等于该列单元格的最大宽度，除非单元格内容设置了`min-width`，且这个值大于最大宽度。



### 3.3 间隔grid-gap



### 3.4 grid-template-areas



### 3.5 子元素排列顺序grid-auto-flow



### 3.6 单元格内容在单元格的位置

* `justify-items`属性设置单元格内容的水平位置（左中右）
* `align-items`属性设置单元格内容的垂直位置（上中下）

* `place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式



### 3.7 整个内容在容器的位置

* `justify-content`属性是整个内容区域在容器里面的水平位置（左中右），`align-content`属性是整个内容区域的垂直位置（上中下）。

* ```
  .container {
    justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
    align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
  }
  ```

* `place-content`属性是`align-content`属性和`justify-content`属性的合并简写形式。