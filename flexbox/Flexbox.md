# Flexbox 布局  
  作为一名新手，在团队里的技术分享会上，我分享了flexbox(弹性盒子)的布局方式，这仅仅是我学习这种灵活的布局方式的一种理解与总结，供大家参考。
## 什么是弹性盒子
 以前，我们布局都是用的盒子模型，基础的用padding和magin,用的比较多的布局方式应该就是floats和positioning,这个大家应该都比较熟悉了，在大部分需求里还是很方便实现的，但是总会遇到一些类似于垂直居中的看是简单的需求但实现起来却总是遇到无数的坑的需求，一下是用floats和positioning，较为难以实现的3种情况：
 
 * 垂直居中父内容的一块内容
 * 使容器的所以子项占用等量的可用宽度/高度，而不管有多少宽度/高度可用
 * 使多列布局中的所有列采用同样的高度，即使他们包含的内容量不同。

 ## 如何使用弹性布局
 **指定元素的布局为flexible**,我们需要给需要flexible的元素的**父元素**`display`设置一个特定值。
 ```
 section{
    display: flex;
 }
 ```
 **注意**如果想设置行内元素为flexible box，也可以设置`display`为`inline-flex`
 
 ## flex的布局方式
 ### flex 布局的两轴
 
  * **主轴（main axis）**是沿着flex元素防止的方向延伸的轴（比如页面上的横向的行、纵向的列），该轴的开始和结束被称为mian start 和 mian end。
  * **交叉轴（cross axis）**是垂直于flex元素放置方向的轴，该轴的开始和结束被称为cross start 和 cross end。
  * 设置了`dispalay:flex`的父元素被称为flex容器（flex container）。


## flex container
  主要是对各个flex item的排列方向、排列顺序、对齐方式进行设置
  
|     属   性     |      含  义     |     取  值     |
|:--------------- |:-----------------:| -----------:|
| flex-direction | 排列方向 |row(default)/row-reverse/column/column-reverse|
| flex-wrap  | 是否换行 |nowrap(default)/wrap/wrap-reverse|
| flex-flow | fles-direction 和flex-wrap的简写 |            row nowrap(default)|
|justity-content|主轴对齐方式|flex-start/flex-end/center/space-between/space-around|
|align-items|侧轴对齐方式|flex-start/flex-end/center/baseline/stretch(default）|
|align-content|多行/列内容对齐方式|flex-start/flex-end/center/stretch/space-between/space-around|


###flex item
  flex iten为项目，主要对item 本身的一些特性进行设置
   
|     属   性     |      含  义     |     取  值     |
|:--------------- |:-----------------:| -----------:|
|order|排列顺序|数值，默认为0，可为负值。flex item按照order从小到大排列|
|flex-grow|空间过多时增长比例|默认为0：即使存在剩余空间也不放大；数值不同：按比例划分|
|flex-shrink|空间不够时缩小比例|默认为1：等比例缩小；某项为0：不缩小|
|flex-basis|分配多余空间之前，项目占据的主轴空间|<length>/auto(default)|
|flex|flex-grow、 flex-shrink、 flex-basis的缩写|默认为0 1 auto|
|align-self|自身对齐方式|auto/flex-start/flex-end/center/stretch/baseline|

## 遇到的那些坑
### 坑1 `margin:auto`
  
  * 当在flex项目上使用`margin:auto`,值为auto的方向会占据所有剩余空间
  * 当在一个flex项目上使用自动外边距`margin:auto`时，justity-content属性就不起作用了
  * 在有些浏览器中，会有一个 bug，允许Flex项目收缩后比其内容尺寸小。解决这个 bug 的变通方案是把 flex-shrink 的属性值设置为 0，而不是默认值 1，同时，把 flex-basis 属性设置为 auto

### 坑2 `flex-direction: column;`

 * 在切换 flex-direction 时，影响Main-Axis的每一个属性现在会影响新Main-Axis。像 flex-basis 这种会影响Main-Axis上Flex项目宽度的属性，现在会影响项目的高度，而不是宽度。
* 同理：flex-grow 属性，它也是影响高度。本质上，每个作用于横向轴（即Main-Axis）上的 flex 属性，现在都会作用于纵向上的新Main-Axis。它只是在方向上的一个切换。

### 坑3 flex container的高度问题
忘记给flex container设置高度，导致使用align-items属性的时候，总是得不到我想要的结果

## 兼容性问题
 * 旧版本：display:box;
 * 过渡版本：display:flex box;
 * 新版本： display:flex;
 
####Android:
    * 2.3开始支持旧版本  display:-webkit-box
    * 4.4开始支持标准版本  diaplay:flex
    
###IOS：
    * 6.1开始支持旧版本  display: webkit-box
    * 7.1开始支持标准版本  display：flex
    
####PC:
    * IE10开始支持，但是IE10是-ms0形式
    
###兼容性写法-父元素(flex container)
因为所有都是向下兼容的，所以写法的顺序一定要写好了才起作用。就是把旧语法写在底下，个别不兼容的移动设置才会识别，

~~~
.box{
     display : -webkit-flex; /*新版本语法: Chrome 21+ */
     display :  flex;       /*新版本语法: Opera 12.1, Firefox 22+ */
     display : -webkit-box; /*老版本语法: Safari, iOS, Android browser,   older WebKit browsers. */
     display : -moz-flex;   /*老版本语法: Firefox (buggy) */
     display : -ms-flexbox; /*混合版本语法IE10*/
}
~~~

### 兼容性写法-子元素(flex item)
~~~
.flex1 { 
      -webkit-box-flex: 1;/* 老版 - iOS 6-, Safari 3.1-6 */ 
      -moz-box-flex: 1; /*老版- Firefox 19- */ 
       width: 20%;      /* For old syntax, otherwise collapses. */ 
     -webkit-flex: 1;   /* Chrome */ 
     -ms-flex: 1;       /* IE 10 */ 
       flex: 1;         /* 新版, Spec - Opera 12.1, Firefox 20+ */
   }
~~~
### 常用css例子-1
#### flex：定义布局为盒模型
```
.flex { 
      display: -webkit-box; 
      display: -webkit-flex; 
      display: -ms-flexbox; 
      display: flex;
 }
```
#### flex-v：盒模型垂直布局
```
.flex-v {
     -webkit-box-orient: vertical;
     -webkit-flex-direction: column; 
     -ms-flex-direction: column; 
     flex-direction: column; 
  }
```
### 常用css例子-2
#### flex-1：子元素占据剩余的空间
```
.flex-1 { 
     -webkit-box-flex: 1; 
     -webkit-flex: 1; 
     -ms-flex: 1; 
     flex: 1; 
}
```
#### flex-align-center：子元素垂直居中
```
.flex-align-center {
      -webkit-box-align: center; 
      -webkit-align-items: center; 
      -ms-flex-align: center; 
      align-items: center;
 }
```
### 常用css例子-3
#### flex-pack-center：子元素水平居中
```
.flex-pack-center { 
    -webkit-box-pack: center;
    -webkit-justify-content: center;
    -ms-flex-pack: center; 
    justify-content: center; 
}
```
#### flex-pack-justify：子元素两端对齐
```
.flex-pack-justify {
   -webkit-box-pack: justify; 
   -webkit-justify-content: space-between; 
   -ms-flex-pack: justify; 
    justify-content: space-between;
 }
```
### 常见兼容性问题
 * 旧版本支持属性有限，比如不支持`flex:wrap`(eg:Android4.2.2)
 * 旧版本使用比例伸缩布局时，会导致盒子内容大小不等，即无法等分布局，需要设置width:0%(把原始大小设为0)
 * 旧版本box item要求属性是块级结构，所以很多inline元素需要设置display:block.
 * `Text-overflow:ellipsis`在`display:flex`元素上无效,需要在最外层嵌套一层父元素，设置`display:flex`即可。


 


  
