# 第二章：jquery选择器


### JQuery的优势：
#### 1.简洁的写法
#### 2.支持从CSS1到CSS3的选择器
#### 3.完善的处理机制
``` html
<body>

<div class="one" id="one" >
    id为one,class为one的div
    <div class="mini">class为mini</div>
</div>

<div class="one"  id="two" title="test" >
    id为two,class为one,title为test的div.
    <div class="mini"  title="other">class为mini,title为other</div>
    <div class="mini"  title="test">class为mini,title为test</div>
</div>

<div class="one">
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini"></div>
</div>

<div class="one">
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini">class为mini</div>
    <div class="mini"  title="tesst">class为mini,title为tesst</div>
</div>


<div style="display:none;"  class="none">
    style的display为"none"的div
</div>

<div class="hide">class为"hide"的div</div>

<div>
    包含input的type为"hidden"的div<input type="hidden" size="8"/>
</div>

<span id="mover">正在执行动画的span元素.</span>

</body>
```

### JQuery选择器

- 基本选择器

$("#one")    //获取id为one的元素
$(".mini")       //获取class为mini的元素
$("div")       //获取元素名为<div>的元素
$("*")       //获取所有元素
$("span #one")     //获取所有<span> 和id为one 的元素


- 层次选择器

$("body div") //获取<body>内所有<div>的后代元素 
$("body > div") //获取<body>内子<div>的元素
$(".one + div") //获取class为one的下一个<div>元素
$("#two ~ div")  //获取id为two的元素后面所有<div>同辈元素
$(".one +div") 等于$(".one").next("div")
$("#prev ~div") 等于$("#prev").nextAll("div")


- 过滤选择器

1.基本过滤选择器
$("div :first") //获取第一个<div>的元素
$("div:last")     //获取最后一个<div>元素
$("div:not(.one)")      //获取没有class为one的div元素
$("div:even")     //获取索引值为偶数的div元素
$("div:eq(3)")     //获取索引值等于3的div元素
$("div:gt(3)")     //获取索引值大于3的div元素
$("div:lt(3)")     //获取索引值小于3的div元素
$(":header")     //获取所有标题的元素
$(":animated")     //获取正在执行动画的元素
$("focus")     //获取正在获取焦点的元素
2.内容过滤选择器
$("div:contains(di)")     //获取包含文本有di的<div>元素
    $("div:empty")     //获取不包含子元素（包括文本元素）的div元素
    $("div:has('.mini')")     //获取有class为mini的div元素
    $("div:parent")     //获取有子元素的div元素（div）
3.可见性过选择滤器
$("div:visible")     //获取所有可见的元素
    $("div:hidden")     //获取隐藏的元素
4.属性过滤选择器
$("div[title]")    //获取含有title属性的div元素
    $("div[title=text]")     //获取含有title=text属性的div元素
    $("div[title!=test]")     //获取含有title！=text属性的div元素
    $("div[title^=te")     //获取含有以te为开头的title属性的div元素
    $("div[title$=est]")     //获取含有以est为结尾的title属性的div元素
    $("div[title*=es]")     //获取含有es的title属性的div元素
    $("div[id][title*=es]")     //获取含有id 并且 title含有es的属性的div元素
5.子元素过滤选择器
$("div .one:nth-child(2)")     //获取每个class为one的div父元素下的第二个子元素
    $("div .one:first-child")     //获取每个class为one的div父元素下的第一个子元素
    $("div .one:last-child")     //获取每个class为one的div父元素下的最后一个子元素
    $("div .one:only-child")     //获取每个class为one的div父元素下的只有一个子元素
6.表单对象属性过滤选择器
$("#form1 input:enabled")     //获取可用的input元素
    $("#form1 input:disabled")     //获取不可用的input元素
    $("input:checked")     //获取多选框的input元素
    $("input:selected")     //获取下拉框的input元素


- 表单选择器

$("#form1 :text")
$("#form1 :password")
$("#form1 :radio")
$("#form1 :checkbox")
$("#form1 :submit")
$("#form1 :image")
$("#form1 :reset")
$("#form1 :button")
$("#form1 :file")
$("#form1 :hidden")
$("#form1 select")
$("#form1 textarea")
$("#form1 :input")
$("#form1 input")

**一些注意事项：**
1.选择器中含有“.”“#”，“（）”等特殊字符需要转义
```
var $id_a  = $('#id.a');//jQuery对象
            var $id_b  = $('#id#b');//jQuery对象
            var $id_c =  $('#id[1]');     //jQuery对象

            var $id_right_a  = $('#id\\.a');//jQuery对象,对特殊字符,我们转义一下
            var $id_right_b  = $('#id\\#b');//jQuery对象,对特殊字符,我们转义一下
            var $id_right_c  = $('#id\\[1\\]');             //对特殊字符，我们转义一下
```
2.属性选择器的@号问题
```
$(function(){
            //var $id_a = $("div[@title='test']"); //错误
            var $id_a = $("div[title='test']");  //正确
            alert( $id_a.html() );
        })
```
3.选择器中含有空格的注意事项
```
<body>
    <div class="test">
       <div style="display:none;">aa</div>
       <div style="display:none;">bb</div>
       <div style="display:none;">cc</div>
       <div class="test" style="display:none;">dd</div>
    </div>
    <div class="test" style="display:none;">ee</div>
    <div class="test" style="display:none;">ff</div>
</body>


$(function(){
          //注意区分类似这样的选择器
          //虽然一个空格，却截然不同的效果.
           var $t_a = $('.test :hidden');     //选取class为test的元素里面的隐藏元素
           var $t_b = $('.test:hidden');     //选取隐藏的class为test的元素
           var len_a = $t_a.length;
           var len_b = $t_b.length;
           $("<p>$('.test :hidden')的长度为"+len_a+"</p>").appendTo("body");
           $("<p>$('.test:hidden')的长度为"+len_b+"</p>").appendTo("body");
        })
```

