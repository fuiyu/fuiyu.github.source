---
title: 前端面试碰到的问题
date: 2017-03-05 16:16
tags:
---

css方面
-------------
#### 一、盒子模型
![盒子模型](http://omc3jlwz8.bkt.clouddn.com/box-model.gif)
* Margin(外边距) - 清除边框外的区域，外边距是透明的。
* Border(边框) - 围绕在内边距和内容外的边框。
* Padding(内边距) - 清除内容周围的区域，内边距是透明的。
* ontent(内容) - 盒子的内容，显示文本和图像

1.盒子模型的兼容性
&ensp;标准盒子模型中width属性值指的是content的width，设置正确的文档声明（DTD）后，大多数浏览器可以正确显示内容。但在IE5和IE6的非标准盒子模型中，width属性包含了padding和border，IE8及更早版本不支持填充的宽度和边框的宽度属性设置。
2.解决方法：
   * 不给指定元素添加内边距，给父元素添加内边距，或这指定元素添加外边距。
   * 在HTML页面声明`<!DOCTYPE html>`

#### 二、css优先级规则
1.就近原则
  * 内联样式（inline style）< 内部样式（internal style sheet）和 外部样式（external style sheet）
  * 内部样式和外部样式遵从就近原则（离元素近者覆盖远者）

2.选择器优先权 

| 内联样式   |   id    | class  |  tag   |
|-----------|---------|--------|--------|
|   1000    |   100   |   10   |   1    |

根据上面对应的权值去计算样式的总权值，权值越大优先，权值相等时按就近原则。

3.!important
标有!important时，有最大优先权。


#### 三、div垂直水平居中
1.absolute，绝对定位
``` css  
div{  
      width:100px;  
      height:100px;   
      position:absolute;   
      left:50%;  
      top:50%;  
      margin:-100px 0 0 -100px; 
}
```
2.flex布局，使内部的元素垂直水平居中
``` css  
.box{
  display:flex;
  justify-content: center;//水平居中
  align-items:center;//垂直居中
}
```
------ps:兼容性写法
``` css  

.box{
  /* 老版本语法 */
  display: -webkit-box; 
  /* 老版本语法: Firefox (buggy) */ 
  display: -moz-box;  
  /* 混合版本语法: IE 10 */  
  display: -ms-flexbox; 
   /* 新版本语法： Chrome 21+ */ 
  display: -webkit-flex; 
  /* 新版本语法*/
  display: flex;        
   /*垂直居中*/   
  /*老版本语法*/
  -webkit-box-align: center;
  -moz-box-align: center;
  /*混合版本语法*/
  -ms-flex-align: center;
  /*新版本语法*/
  -webkit-align-items: center;
  align-items: center;
 
  /*水平居中*/
  /*老版本语法*/
  -webkit-box-pack: center;
  -moz-box-pack: center;
  /*混合版本语法*/
  -ms-flex-pack: center;
  /*新版本语法*/
  -webkit-justify-content: center;
  justify-content: center;
 
  margin: 0;
  height: 100%;
  width: 100% /* needed for Firefox */
}
```

#### 四、布局问题
Q:使数个div块在容器内，垂直水平居中，且在拉伸情况下左右边距固定，不能使用绝对定位和JavaScript控制。
A:使用flex布局，代码如下:

``` css  
.box{
  display:box;
  justify-content:center;
  align-items:center;
} 
.item{
  margin-left:10px;
  border: 1px solid #333;
  flex-basis: 30%;//总和大于100%
}
```



<br>

javascript方面
-------------
#### 一、函数调用方法有哪几种
1.作为对象的方法调用
``` javascript  
var obj = {
      getA:function(){
            alert('A')
      }
};

obj.getA();//调用
```

2.作为普通函数调用
``` javascript  
function func(){
      alert('调用')
}

func();//调用
```

3.通过构造器调用
``` javascript  
var class = function(){
      this.name = '调用'
      this.getName = function(){
            alert(this.name)
      }
}

var func = new class()
func.getName()
```

4.通过Function.prototype.call 或 Function.prototype.apply 调用
``` javascript  
var obj1 = {
      getName:function(){
            alert(this.name)
      }
}

var obj2 = {
      name:'调用'
}

obj1.getName.call(obj2)
```
apply和call的不同在于参数代入的写法，call后面是一个一个参数，apply参数是一个数组或者arguments对象。
``` javascript  
call([thisObj[,arg1[, arg2[, [,.argN]]]]]) //call语法  

apply([thisObj[,argArray]]) //apply语法  
```

#### 二、理解js中的闭包
>闭包是指可以包含自由（未绑定到特定对象）变量的代码块；这些变量不是在这个代码块内或者任何全局上下文中定义的，而是在定义代码块的环境中定义（局部变量）。

可以从它几个有关概念和作用去理解这句话：

  1.变量的作用域。
  当在函数内部定义变量时，只有在内部才能访问这个变量，外部无法访问，可以防止变量命名冲突。

  2.变量的生存周期。
  如果没有被人为的去销毁，全局变量的生存周期是永久的；而在函数中定义的变量，在函数调用结束后，变量将会被销毁，但如果函数还被访问时，其局部变量就不会被销毁：
``` javascript  
var func = function(){  
      var i = 1;  
      return function(){  
            i++;  
            alert(i);  
      }  
}  
var f = func()  
f();//2  
f();//3  
f();//4  
f();//5  
//变量i一直未被销毁
```
上例中形成了一个闭包结构，使变量一直未被销毁。

3.将变量变成函数的私有变量。

4.利用上例的闭包结构原理延续变量寿命




html 方面
-------------
#### 一、seo优化
1.网站结构优化
 * 控制首页链接数，不能太少，不要超过100个，尽量全面直达内页；
 * 目录层次扁平化，级数在三级以内，即点击三次可到达网站的任何一个内页
 * 导航优化，尽量使用文字做导航，`<img>`标签必须加上'alt'和title，每个页面最好加上面包屑导航
 * 网站的布局，头部->主体->底部，减少页面大小，提高加载速度。

2.网页代码优化
 * `<title>`关键词要放前面，不要重复，每个页面的标题也不要相同；
 * `<meta keywords>`列举页面的关键字；
 * `<meta description>`网页的描述，不要太长；
 * `<body>`中的标签，尽量做到标签语义化，适当的位置使用适当的标签；
 * `<a>`需要加title，链接外网时需要加上el="nofollow"，防止"蜘蛛"爬了外部链接，不再回来；
 * 尽量做到正文标题用`<h1>`标签，副标题用`<h2>`标签, 而其它地方不应该随便乱用 h 标题标签；
 * 表格应该使用`<caption>`表格标题标签；
 * `<strong>`标签在搜索引擎中能够得到高度的重视，它能突出关键词，表现重要的内容，`<em>`标签强调效果仅次于`<strong>`标签。`<b>`、`<i>`标签: 只是用于显示效果时使用，在SEO中不会起任何效果；
 * 重要内容不要用JS输出；
 * 尽量少使用iframe框架；
 * 对于不想显示的文字内容，应当设置z-index或设置到浏览器显示器之外。因为搜索引擎会过滤掉display:none其中的内容；
 * js代码如果是操作DOM操作，应尽量放在body结束标签之前，html代码之后；
