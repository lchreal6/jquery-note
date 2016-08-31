# 第十一章 jquery性能优化和技巧

## jquery性能优化
###  1.使用最新版本的jquery类库
jquery每一个新的版本都会较上一个版本进行bug修复和一些优化。

### 2.使用合适的选择器

$("#id")
使用id来定位DOM元素时最佳的提高性能的方式，因为jquery底层将直接调用本地方法document.getElementById()

 \$ ( "p " ),$ ("div"),$("input")
它是性能优化的第二选择，jquery底层将直接调用本地方法document.getElementsByTagName();

$(".class")
支持本地方法调用document.getElementsByClassName()

$("[attribute=value]")
对于利用属性来定位DOM元素，本地JavaScript方法并没有直接地实现，大多都是使用DOM搜索方法来达到效果。

$(":hidden")
这种伪选择器也同样没有直接在本地JavaScript方法中实现，并且jquery需要搜索每一个元素来定位这个选择器，将对性能带来比较大的性能问题

以上是使用选择器的基本规则，性能自上而下依次下降。

### 3.缓存对象

在一些开发的过程中，有些开发人员会这样写：

```
    $("#traffic_light input on").bind("click",function(){

})
$("#traffic_light input on").css("border","1px solid black");
```

这样写的话jquery在创建每一个选择器的过程中，查找DOM，创建多个jquery对象，带来了性能的损耗，可以这样写：

```
    var $active_light = $("#traffic_light input on")
$active_light.bind("click",function(){

})

$active_light.css("border","1px solid black");
```

这样创建对应的选择器，只在DOM中查找一次，节省性能。

使用链式操作更加简洁：

```
  var $active_light = $("#traffic_light input on")
$active_light.bind("click",function(){

}).$active_light.css("border","1px solid black");
    
```

如果打算在其他函数中使用jquery对象，那么可以把它们缓存在全局环境中。

  

```
  window.$my={
    head : $(head);
    traffic_light: $("#traffic_light")
    traffic_button: $("#traffic_button")
};

function do_something(){

    //现在你可以引用存储的结果并操作它们
    var script = document.createElement("script");

    $my.head.append('script');
    //当你在函数内部操作时，可以继续将查询存入全局对象中去
    $my.cool_results = $("#some_ul li");
    $my.other_results = $("#some_table td")

    //将全局函数作为一个普通的jquery对象去使用
    $my.other_result.css("border-color","red");
    //也可以在其他函数中使用它
}
```

4.循环DOM操作

```
    var top_100_list =[....];
$mylist = $("#mylist");
for (var i = 0; i < top_100_list.length; i++) {
    top_100_list[i] += "<li> " + top_100_list[i] + "</li>";
}
```

以上代码，for执行一次 jquery都要历编top_100_list数组 ，带来了不低的性能损耗，更好的方法是减少DOM操作

```
    var top_100_list[....]
$mylist = $("#mylist")
top_100_li ="" //这个变量用来存储我们的列表元素。
for (var i = 0; i < top_100_li.length; i++) {
    top_100_li +="<li>" +top_100_list[i] + "</li>"

}

$mylist.html(top_100_li);

```

### 数组方式使用jquery对象
性能方面，建议使用简单for或者while替代each来循环处理。

```
$.each(array,function(i){
    array[i]=i
})
```

```
var array =new Array();
for(var i=0;i<array.length;i++){
    array[i]=i;
}
```

另外注意，检查长度也是一个检查jquery对象是否存在的方式，下面一段代码通过length属性检查页面是否含有id为"content" 元素：

```
var $content = $('#content');
if( $content){ //总是true

}
```

```
if($content.length){//拥有元素才有true

}
```

### 6.事件代理


假设有100td元素，在使用以下方式的时候，如果绑定了100事件，这将带来了负面的性能影响。
```
    $('#myTable td').click(function(){
    $(this).css("background","red")

})
```

代替这种效率差很多的元素事件监听的方法是，只需向它们的父节点绑定一次事件，然后通过event.target 获取到点击的当前元素

```
    $("#myTable").click(function(e){
    var $clicked = $(e.target);
    $clicked.css("background","red");
})
```

还新版本的jquery1.7中，还可以这样：

    $("#myTable").om("click","td",function(){
        $(this).css("background","red");
    })

### 7.将你的代码转化成juqery插件

如果你每次需要花上一定的事件去开发类似的jquery代码，那么你可以考虑将代码变成插件，它能够使你的代码有更好的重用性，兵器能够有效的帮助你组织代码：

```
(function($){
    $.fn.youPluginName = function (){
         //Your code goes here 
         return false
    }
})(jQuery)
```

### 8.使用join()来拼接字符串

    var array = [ ];
    for (var i =0;i<10000;i++){
        array[i] = '<li>' + i + '</li>'
    }
    $$('#this').html(array.join(''));

### 9.合理利用HTML5的Data属性

HTML5的data属性可以帮助我们插入数据，特别是前后端的数据交换。jquery的data()方法，有效的利用HTML5的属性，来自动得到数据。

```
    <div id = "dl" data-role="page" data-last-value="43"
data-options='{"name":"Join"}'> </div>
```

为了读取数据，你需要使用如下代码

```
$('#d1').data("role");    //page
$('#d1').data("lastValue")      //43
$('#d1').data('options').name   //Join
```
### 10.尽量使用原生的JavaScript方法

```
    var $cr = $("#cr");
$cr.click(function(){
    if($cr.is(":checked"))
        alert()
})
```
下面用JavaScript方法，很明显使用原生的JavaScript效率高于第一种

```
    var $cr = $("#cr");
var cr = $cr.get(0);
$cr.click(function(){
    if(cr.checked)
    {
        alert()
    }
})
```
### 11.压缩JavaScript


## jqeury技巧

### 1.禁用页面的邮件菜单

```
$(document).ready(function(){  
    $(document).bind("contextmenu",function(e){  
        return false;  
    });  
```

### 2.新窗口打开页面

    $(document).ready(function() {  
     //例子1: href=”http://”的超链接将会在新窗口打开链接 
     $('a[href^="http://"]').attr("target", "_blank");  
 
     //例子2: rel="external"的超链接将会在新窗口打开链接  
     $("a[rel$='external']").click(function(){  
          this.target = "_blank";  
     });  

### 3.判断浏览器类型

    $(document).ready(function() {  
    // Firefox 2 and above  
    if ($.browser.mozilla && $.browser.version >= "1.8" ){  
        // do something  
    }    
    // Safari  
    if( $.browser.safari ){  
        // do something  
    }    
    // Chrome  
    if( $.browser.chrome){  
        // do something  
    }    
    // Opera  
    if( $.browser.opera){  
        // do something  
    }    
    // IE6 and below  
    if ($.browser.msie && $.browser.version <= 6 ){  
        alert("ie6")
    }    
    // anything above IE6  
    if ($.browser.msie && $.browser.version > 6){  
        alert("ie6以上")
    }  

### 4.输入框文字获取和失去焦点

```
    $("input.text1").val("Enter your search text here.");  
    textFill( $('input.text1') );  
});  
function textFill(input){ //input focus text function  
    var originalvalue = input.val();  
    input.focus( function(){  
        if( $.trim(input.val()) == originalvalue ){
            input.val(''); 
        }  
    }).blur( function(){  
        if( $.trim(input.val()) == '' ){ 
            input.val(originalvalue); 
        }  
    });  
```

### 5.返回头部滑动动画

```
    jQuery.fn.scrollTo = function(speed) {
    var targetOffset = $(this).offset().top;
    $('html,body').stop().animate({scrollTop: targetOffset}, speed);
    return this;
}; 
// use
$("#goheader").click(function(){
    $("body").scrollTo(500);
    return false;
}); 
```

### 6.获取鼠标位置

```
$(document).mousemove(function(e){  
        $('#XY').html("X : " + e.pageX + " | Y : " + e.pageY);  
    });
```

### 7.判断元素是否存在

    if ($('#XY').length){  
       alert('元素存在!')
    }else{
       alert('元素不存在!')
    }

### 8.点击div也可以跳转

    $("div").click(function(){  
        window.location=$(this).find("a").attr("href"); 
        return false;  
    }); 

### 9.根据浏览器大小添加不同的样式

    $(document).ready(function() {  
    function checkWindowSize() {  
        if ( $(window).width() > 900 ) {  
            $('body').addClass('large');  
        }else{  
            $('body').removeClass('large');  
        }  
    }  
    $(window).resize(checkWindowSize);  });

### 10.设置div在屏幕中央

```
    $(document).ready(function() {  
   jQuery.fn.center = function () {  
      this.css("position","absolute");  
      this.css("top", ( $(window).height() - this.height() ) / 2+$(window).scrollTop() + "px");  
      this.css("left", ( $(window).width() - this.width() ) / 2+$(window).scrollLeft() + "px");  
      return this;  
    }  
    //use
    $("#XY").center();  
});
```

### 11.创建自己的选择器

```
    $(document).ready(function() {  
   $.extend($.expr[':'], {  
       moreThen500px: function(a) {  
           return $(a).width() > 500;  
      }  
   });  
  $('.box:moreThen500px').click(function() {  
      alert();  
  });  
});
```

### 12.关闭所有的动画效果

```
$(document).ready(function() {  
    $("#XY1").click(function(){
        animateIt();
    });
    $("#XY2").click(function(){
        jQuery.fx.off = true;
    });
    $("#XY3").click(function(){
        jQuery.fx.off = false;
    });
});
```


### 13.检测鼠标的邮件和左键

    $(document).ready(function() {  
    $("#XY").mousedown(function(e){
        alert(e.which)  // 1 = 鼠标左键 ; 2 = 鼠标中键; 3 = 鼠标右键
    })});


### 14.回车提交表单

    $(document).ready(function() {  
     $("input").keyup(function(e){
            if(e.which=="13"){
               alert("回车提交!")
            }
        })});

### 15.设置全局Ajax参数

    $(document).ready(function() { 
    $('#send1').click(function() {
        $.getJSON("http://api.flickr.com/services/feeds/photos_public.gne?tags=car&tagmode=any&format=json&jsoncallback=?",function(data){
                      $("#resText1").empty();
                      $.each(data.items, function( i,item ){
                            $("<img/> ").attr("src", item.media.m ).appendTo("#resText1");
                            if ( i == 3 ) { 
                                return false;
                            }
                      });
                 }
        ); });

### 16.获取选中额下拉框

```
function getObj(){
    var $obj = $('#someElement').find('option:selected');
    alert( $obj.val() );
}
```

### 17.切换复选框

```
var tog = false; 
$('button').click(function(){
    $("input[type=checkbox]").attr("checked",!tog);
    tog = !tog;
});
```

### 18.使用siblings()来选择同辈元素

```
/* 不这样做
$('#nav li').click(function(){
    $('#nav li').removeClass('active');
    $(this).addClass('active');
});
*/
//替代做法是
$('#nav li').click(function(){
    $(this).addClass('active')
           .siblings().removeClass('active');
});
```

### 19.个性化链接

```
$(document).ready(function(){
    $("a[href$='pdf']").addClass("pdf");
    $("a[href$='zip']").addClass("zip");
    $("a[href$='psd']").addClass("psd");
});
```

### 20.在一段时间之后自动隐藏或关闭元素

```
$(document).ready(function(){  
    $("button").click(function() {
        $("div").slideUp(300).delay(3000).fadeIn(400);
    });
    /*
    //这是1.3.2中我们使用setTimeout来实现的方式
    setTimeout(function() {
        $('div').fadeIn(400)
    }, 3000);
    */
    //而在1.4之后的版本可以使用delay()这一功能来实现的方式
    //$("div").slideUp(300).delay(3000).fadeIn(400);
});
```

### 21.在使用Firefox 和Firebug来记录事件日志

```
jQuery.log = jQuery.fn.log = function (msg) {
      if (console){
         console.log("%s: %o", msg, this);
      }
      return this;
};
$(document).ready(function(){  
    $("button").click(function() {
        $('#someDiv').hide().log('div被隐藏');
    });
});
```

### 22.为任何与选择器相匹配的元素绑定事件

```
$(document).ready(function(){  
    /*
    // 为table里面的td元素绑定click事件，不管td元素是一直存在还是动态创建的
    // jQuery 1.4.2之前使用的方式
    $("table").each(function(){ 
    　　$("td", this).live("click", function(){ 
    　　　　$(this).toggleClass("hover"); 
    　　}); 
    }); 
    // jQuery 1.4.2 使用的方式
    $("table").delegate("td", "click", function(){ 
    　　$(this).toggleClass("hover"); 
    });
    */
    // jQuery 1.7.1使用的方式
    $("table").on("click","td",function(){ 
    　　$(this).toggleClass("hover"); 
    });

});
```

### 23.使用css钩子

```
$('#rect').css('borderRadius',10);
```

### 24.$.proxy()使用

```
$('#panel').fadeIn(function(){
    // Using $.proxy :
    $('#panel button').click($.proxy(function(){
        // this 指向 #panel
        $(this).fadeOut();
    },this));
});
```

### 25.限制Text-Area域中的字符的个数

    jQuery.fn.maxLength = function(max){
    this.each(function(){
        var type = this.tagName.toLowerCase();
        var inputType = this.type? this.type.toLowerCase() : null;
        if(type == "input" && inputType == "text" || inputType == "password"){
            //应用标准的maxLength
            this.maxLength = max;
        }else if(type == "textarea"){
            this.onkeypress = function(e){
                var ob = e || event;
                var keyCode = ob.keyCode;
                var hasSelection = document.selection? document.selection.createRange().text.length > 0 : this.selectionStart != this.selectionEnd;
                return !(this.value.length >= max && (keyCode > 50 || keyCode == 32 || keyCode == 0 || keyCode == 13) && !ob.ctrlKey && !ob.altKey && !hasSelection);
            };
            this.onkeyup = function(){
                if(this.value.length > max){
                    this.value = this.value.substring(0,max);
                }
            };
        }
    });};

### 26.本地存储

### 27.解析json数据时报parseError错误

### 28.从元素中出去HTML

```
(function($) { 
$.fn.stripHtml = function() { 
　　var regexp = /<("[^"]*"|'[^']*'|[^'">])*>/gi; 
　　this.each(function() { 
　　　　$(this).html( $(this).html().replace(regexp,'') ); 
　　});
　　return $(this); 
} 
})(jQuery); 
//用法： 
$('div').stripHtml(); 
```

###  29.扩展String对象的方法

```
$.extend(String.prototype, {
isPositiveInteger:function(){
    return (new RegExp(/^[1-9]\d*$/).test(this));
},
isInteger:function(){
    return (new RegExp(/^\d+$/).test(this));
},
isNumber: function(value, element) {
    return (new RegExp(/^-?(?:\d+|\d{1,3}(?:,\d{3})+)(?:\.\d+)?$/).test(this));
},
trim:function(){
    return this.replace(/(^\s*)|(\s*$)|\r|\n/g, "");
},
trans:function() {
    return this.replace(/&lt;/g, '<').replace(/&gt;/g,'>').replace(/&quot;/g, '"');
},
replaceAll:function(os, ns) {
    return this.replace(new RegExp(os,"gm"),ns);
},
skipChar:function(ch) {
    if (!this || this.length===0) {return '';}
    if (this.charAt(0)===ch) {return this.substring(1).skipChar(ch);}
    return this;
},
isValidPwd:function() {
    return (new RegExp(/^([_]|[a-zA-Z0-9]){6,32}$/).test(this)); 
},
isValidMail:function(){
    return(new RegExp(/^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z0-9]+$/).test(this.trim()));
},
isSpaces:function() {
    for(var i=0; i<this.length; i+=1) {
    var ch = this.charAt(i);
    if (ch!=' '&& ch!="\n" && ch!="\t" && ch!="\r") {return false;}
    }
    return true;
},
isPhone:function() {
    return (new RegExp(/(^([0-9]{3,4}[-])?\d{3,8}(-\d{1,6})?$)|(^\([0-9]{3,4}\)\d{3,8}(\(\d{1,6}\))?$)|(^\d{3,8}$)/).test(this));
},
isUrl:function(){
    return (new RegExp(/^[a-zA-z]+:\/\/([a-zA-Z0-9\-\.]+)([-\w .\/?%&=:]*)$/).test(this));
},
isExternalUrl:function(){
    return this.isUrl() && this.indexOf("://"+document.domain) == -1;
}
});

$("button").click(function(){
    alert(   $("input").val().isInteger()  );
});
```
