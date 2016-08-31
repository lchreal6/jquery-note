# 第三章：jquery中的DOM操作
@(锋利的jquery)

1.DOMCore
- 并不专属与JavaScript，任何一种支持DOM的程序设计语言都可以使用它。
-  使用DOM来获取表单对象的方法
  

```
document.getElementsByTagName("form");
```

- 使用DOMCore来获取某元素的src属性的方法：

   

```
 element.getAttribute("src");
```


----------
2.HTML-DOM
- HTML-DOM的出现比DOM Core还要早，它提供了一些更简明的记号来描述各种html
- 使用HTML-DOM来获取表单对象的方法：

   

```
 document.form;
```

- 使用HTML-DOM来获取某元素的src属性的方法：

```
element.src
```

3.CSS-DOM
- 是针对css的操作
- 设置某元素style对象字体颜色的方法：

 

```
   element.style.color = "red";
```

##JQuery中的DOM操作
HTML代码如下：

    <body>

    <p title="选择你最喜欢的水果." >你最喜欢的水果是?</p>
    <ul>
      <li title='苹果'>苹果</li>
      <li title='橘子'>橘子</li>
      <li title='菠萝'>菠萝</li>
    </ul>

    </body>

### 查找节点
#### 1.查找元素节点

    var $li = $ ("ul li:eq(1)") //获取<ul>里第2个<li>节点
    var li-txt = $li.text //获取第2个<li>节点的文本内容
#### 2.查找属性节点

    var $para = $("p");   //获取<p>节点
    var p_txt = $para.attr("title");  //获取<p>元素节点属性title

#### 创建属性节点

    var $li_1 = $("<li title = '香蕉'>香蕉</li>") //创建一个li元素
            //包括元素节点、文本节点和属性节点，其中title=‘香蕉’就是创建的属性节点
    var $li_2 = $("<li title = '雪梨'>雪梨</li>") //创建一个li元素
    //包括元素节点、文本节点和属性节点
    $("ul").append($li_1);
    $("ul").append($li_2);     //添加到ul节点，使之能在网页显示

#### 插入节点
- append() 
向每个匹配的元素内容追加内容

```
<p>我想说：</p>
$("p").append("<b>你好</b>");
//结果
<p>我想说：<b>你好</b></p>
```
- appendTo()
将所有匹配的元素追加到指定的元素中，实际上，使用该方法是颠倒了常规的append()操作。

```
<p>我想说：</p>
$("<b>你好</b>").appendTo("p");
//结果
<p>我想说：<b>你好</b></p>
```
- prepend()
向每个匹配的元素内部前置内容

   <p>我想说：</p>
$("p").prepend("<b>你好</b>");
//结果
   <p><b>你好</b>我想说：</p>
- prependTo
    跟appendTo类似

- after
在每个匹配的元素之后插入内容

    <p>我想说：</p>
$("p").append("<b>你好</b>");
//结果
<p>我想说：</p><b>你好</b>

- insertAfter
    跟appendTo类似
- before
跟after类似
- insertBefore
跟appendTo类似

#### 删除节点

1.remove()方法

    $("ul li:eq(1)").remove(); //获取第2个<li>元素节点后，将它从网页中删除

用remove()方法删除后，该节点所包含的所有后代节点将同时被删除。这个方法的返回值是一个纸箱已被删除的节点的引用

2.detach()方法

    $("ul li").click(functtion(){
    alert($(this).html());
    })
    var$li = $("ul li:ea(1)".detach();) //删除元素
    $li.appenTo("ul");  //重新追加元素，发现它之前绑定的事件还在，如果使用remove()方法删除元素，那么它之前绑定的事件将失效。

3.empty()方法

    $("ul li:eq(1)").empty();
    //获取第2个元素节点后，清空此内容元素，注意是元素里。

#### 复制节点

1.clone()方法

    $("ul li").click(function(){
    $(this).clone().appendTo("ul") //复制当前单击的节点，并追加到ul元素
    })

**复制节点后，被复制的新元素并不具有任何行为。如果需要新元素也具有复制功能（单击事件），可以这样：**

    $(this).clone(true).appendTo("body"); //注意参数true

####替换节点
1.replaceWith()方法
    

    $("p").replaceWith("<strong>你最不喜欢的水果？<strong>");
    
2.replaceAll()方法
跟appendTo类似

#### 包裹节点
1.wrap()方法

    $("strong").wrap(<b></b>);
    //结果
    <b><strong><strong><b>
    <b><strong><strong><b>
    
2.wrapAll()方法

    $("strong").wrapAll(<b></b>);
    //结果
    <b>
    <strong></strong>
    <strong></strong>
    </b>
3.wrapInner()方法

    $("strong").wrapInner(<b></b>);
    //结果
    <strong><b></b><strong>

###  属性操作
####   1.获取属性和设置属性

    var $para = $("p"); //获取p节点
    var p_txt = $para.attr("title")  //获取p元素节点属


    $("p").attr("title","your title");  //设置单个的属性值

####   删除属性

    $("p").removeAttr("title");  //删除p元素的属性title
###   样式操作
####   1.获取样式和设置样式
    var p_class = $("p").attr("class");  //获取p元素的class
    $("p").attr("class","high");  //设置p元素的class为high
    
####   2.追加样式

    $("P").addClass("another") //给p元素追加"another"类

**如果给一个元素添加了多个class值，那么久相当于合并了它们的样式**
**如果有不同的class设定了同一样式属性，则后者覆盖前者**

####   3.移去样式

    $("p").removeClass("high");  //移除p元素"high"类
    $("p").removeClass();  //移除p元素所有的类

####  4.切换样式

    $("p").toggleClass("another");  //重复切换类名"another"

####   5.判断是否含有某个样式

    $("p").hasClass("another");  //判断p元素是否含有another类 ，返回布尔值

**为了增强代码的可读性，可以调用.is()方法**

###   设置和获取HTML，文本和值
####   1.html()方法
**返回的是HTML的文本，含有节点元素**

    var p_html= $("p").html();
    alert(p_html);  
    //结果
    <strong>你好<strong>
####   2.text()方法
**返回的是某个元素内的文本内容**

    var p_text = $("p").text();
    alert(p_text);
    //结果
    你好

####   3.val()方法
**此方法类似于JavaScript中的value属性，可以用来设置和获取元素的值。无论元素时文本框，下拉列表还是单选框，都可以返回元素的值，如果元素为多选，则返回一个数组**

    $element.val("text");  //设置element的某个内容
    $(":checkbox").val("[check2]","[check3]");  //使多选框被选中
    $(":raido").val("radio1")  //使单选框被选中

###   遍历节点
####    1.children()方法
- 使用childer()方法来获取元素的所有子元素
**只考虑子元素而不考虑其他后代元素**

####   2.next()方法
- 用于取得匹配元素后面紧邻的同辈元素

####   3.prev()方法
- 用于取得匹配元素前面金陵的同辈元素

####   4.siblings()方法
- 用于取得匹配元素前后所有的同辈元素
####   5.closest()方法
- 用于取得最近的匹配元素
- 首先检查当前元素是否匹配，如果匹配则直接返回元素本身，如果不匹配则向上查找父元素。
####   6.parent()方法，parents()方法，cloest()方法的区别

| 方法      |     描述 |   数量   |
| :-------- | --------:| :------: |
| parents()方法    |   获得匹配元素的祖先元素 |  可以有多个  |
| parent()方法      |     获得匹配元素的父级元素 |   只有一个   |
| cloest()方法 | 获得匹配元素最近的父级元素 | 只有一个|

###  CSS-DOM操作

    $("p").css("cloor") //获取p元素的css样式；
    $("p").css("color") //设置p元素的css样式color为red；

####   1.offset()方法
- 是获取元素在当前视窗的相对偏移，其中返回的对象包含两个属性，即top和left，只对可见元素有效。

  

```
  var offset = $("p").offset();
    var left = offset.left //获取左偏移
    var top = offset.top //获取上偏移
```

####   2.position()方法
- 获取元素相对于最近的一个position样式属性设置为relative或者absolute的祖父节点的相对偏移

```
var position = $("p").position();
var left = position.left;  //获取左偏移
var top = position.top; //获取上偏移
```

####   scrollTop()方法，scrollLeft()方法

    var p = $("p");
    var scrollTop = $p.ScrollTop(); //获取元素的滚动条距顶端的距离
    var scrollLeft =$p.scrollLeft(); //获取元素的滚动条距左端的距离

