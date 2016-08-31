# 第六章 jquery与ajax的应用

Ajax全称为“Asynchronous JavaScript and XML"（异步JavaScript和XML）。

### Ajax的优势
- 不需要插件支持
- 优秀的用户体验
- 提高web程序的性能
- 减轻服务器和带宽的负担

### Ajax的不足
- 浏览器对XMLHttpRequest对象支持度不足
- 破坏浏览器的前进，后退按钮的正常功能
- 对搜索引擎的支持的不足
- 开发和调试工具的缺乏

<font color=red>**Ajax的核心是XMLHttpRequest对象，发送异步请求，接受响应及执行回调是通过它来完成的**</font>

由于Ajax方法需要与web服务器端进行交互，可以下载Appserv工具包来搭建本地的服务器环境，完成网站架构。


----------
用传统的JavaScript来实现Ajax

#### 1.定义一个函数，通过该函数来异步获取信息：

    function Ajax(){
    }

#### 2.声明一个空对象用来装入XMLHttpRequest对象

    var xmlHttpReq =null

#### 3.给XMLHttpRequest对象赋值
    //检查浏览器的兼容性
    if(widow.ActiveXObject){
    xmlHtpReq =new Active XObjet("Microsoft.XMLHTTP")
    }
    else if(window.XMLHttpReques){
    xmlHttpReq = new XMLHttpRequest();
    }

#### 4.赋值成功后，使用open()方法初始化XMLHttpRequest对象，指定Http方法和URL

    xmlHttpReq.open("GET","test.php",true);

#### 5.弄一个回调函数，当服务器解析数据响应完成后，执行回调函数。

    xmlHttpReq.onreadystatechange = RequestCallBack;

#### 6.使用send方法发送该请求

    xmlHttpReq.send(null);
    
#### 7.请求完成（readyState的值为4），并且响应成功（HTTP状态值为200），执行回调函数

    function RequestCallBack(){
        if(xmlHttpReq.readyState == 4){
            if(xmlHttpReq.status == 200){
            //执行的代码
            }
        }
        else{
          alert("error");
        }
    }

### jquery中的Ajax
jquery对Ajax操作进行了封装，在jquery中 $.ajax()方法属于底层方法，第2层是load()，$.get(),$.post()方法，第3层是$.getScript()和$.getJSON()方法.

#### load()方法
1.载入HTML文档
load()方法是jquery中最为简单和常用的Ajax方法，能载入远程HTML代码并插入DOM中

     $("#className").load("test.html")
2.筛选载入的HTML文档

    $("#className").load("test.html" .para)
只插入test.html文档里的 id为para的元素

3.传递方式
如果没有参数传递，怎用GET方法，有参数传递则用POST方法

    $("#className").load("test.php",function(){
    })
    $("#className").load("test.php".{name:"rain",age"22"},function(){
    })

4.回调函数
加载完成后执行的操作

    $("#className").load("test.html",function(responseText,testStatus,XMLHttpRequest)){
    //responseText ;请求返回的内容
    // textStatus ;请求状态， success,error ,notmodified,timeout
    //XMLHttpRequest ;XMLHttpRequest 对象
    }

#### $.get()方法和$.post()方法

load()方法通常用来从Web服务器上获取静态的数据文件，然而如果需要传递一些参数给服务器，那么可以使用以上两种方法

1.$.get()方法
有一段HTML代码

```
    <body>
<form id="form1" action="#">
<p>评论:</p>
 <p>姓名: <input type="text" name="username" id="username" /></p>
 <p>内容: <textarea name="content" id="content"  rows="2" cols="20"></textarea></p>
 <p><input type="button" id="send" value="提交"/></p>
</form>


<div  class='comment'>已有评论：</div>
<div id="resText" >
</div>

</body>
```

用get（）方法来传递参数

    $(function(){
       $("#send").click(function(){
            $.get("get1.php", { 
                        username :  $("#username").val() , 
                        content :  $("#content").val()  
                    }, function (data, textStatus){
                        $("#resText").html(data); // 把返回的数据添加到页面上
                    }
            );
       })
    })
$.get()方法的回调函数只有两个参数：

    function (data, textStatus){
         //data 返回的内容，可以使 XML文档,JSON文档，HTML文档
         //textStatus 请求状态：success,error,notmodified,timeout
                    }

- HTML片段
不需要经过处理就可以插入

   

```
 $(function(){
       $("#send").click(function(){
            $.get("get1.php", { 
                        username :  $("#username").val() , 
                        content :  $("#content").val()  
                    }, function (data, textStatus){
                        $("#resText").html(data); // 把返回的数据添加到页面上
                    }
            );
       })
    })
```

- XML文档
需要对返回的数据进行处理

  

```
  $(function(){
       $("#send").click(function(){
            $.get("get2.php", { 
                        username :  $("#username").val() , 
                        content :  $("#content").val()  
                    }, function (data, textStatus){
                        var username = $(data).find("comment").attr("username");
                        var content = $(data).find("comment content").text();
                        var txtHtml = "<div class='comment'><h6>"+username+":</h6><p class='para'>"+content+"</p></div>";
                        $("#resText").html(txtHtml); // 把返回的数据添加到页面上
                    });
       })
    })
```
返回的实现过程比HTML稍微复杂些，但是这种格式提供的数据的重用性将极大提高

- JSON文档
XML文档在很多程度上文档的体积大，和难以解析，JSON能够解决这些问题，而也可以很方便地被重用。

  

```
  $(function(){
       $("#send").click(function(){
            $.get("get3.php", { 
                        username :  $("#username").val() , 
                        content :  $("#content").val()  
                    }, function (data, textStatus){
                        var username = data.username;
                        var content = data.content;
                        var txtHtml = "<div class='comment'><h6>"+username+":</h6><p class='para'>"+content+"</p></div>";
                        $("#resText").html(txtHtml); // 把返回的数据添加到页面上
                    },"json");
       })
    })
```

3中数据格式的优缺点进行分析：
在不需要与其他应用程序共享数据的时候，使用HTML片段来提供返回数据一般来说是最简单的，如果数据需要重用，那么JSON文件是个不错的选择，它在性能和文件大小方面具有优势，而当远程应用程序未知时，XML文档是明智的选择，它是web服务领域的“世界语”。

2.$.post()方法
post方法与get方法的结构和使用方式相同，不过它们之间仍然有以下区别
- get请求会将参数跟在URL后进行传递，而post请求则是作为HTTP消息的实现内容发生给web服务器。
- get方式对传输的数据有大小限制，而使用post方式传递的数据要比get大得多
- get方式请求的数据会被浏览器缓存起来，因此就可以从浏览器的历史中取到这些数据，如账号和密码，某种情况下，get方法会带来严重的安全性问题
- get和post传递的数据在服务端的获取也不相同，在php中，get的数据可以用$_GET[]获取，而post用$_POST[]方法获取



####  $.getScript()方法和$.getJson()方法
1.$.getScript()方法
有时候，在页面初次加载时就取得所需的全部JavaScript文件是完全没有必要的，可以需要哪个JavaScript文件时，动态地创建< script >标签

    $(function(){
        $('#send').click(function() {
             $.getScript('test.js');
        });})
加载完成后，还可以添加回调函数

    $(function(){
             $.getScript('jquery.color.js',function(){
                  $("<p>加载JavaScript完毕</p>").appendTo("body");
                  $("#go").click(function(){
                       $(".block").animate( { backgroundColor: 'pink' }, 1000)
                                  .animate( { backgroundColor: 'blue' }, 1000);
                  });
             });})

2.$.getJson()方法

跟getscript的原因差不多

    $(function(){
        $('#send').click(function() {
             $.getJSON('test.json', function(data) {
                 $('#resText').empty();
                  var html = '';
                  $.each( data  , function(commentIndex, comment) {
                      html += '<div class="comment"><h6>' + comment['username'] + ':</h6><p class="para">' + comment['content'] + '</p></div>';
                  })
                 $('#resText').html(html);
            })
       })})

使用JSONP形式的回调函数可以来加载其他网站JSON数据

    $(function(){
        $('#send').click(function() {
            $.getJSON("http://api.flickr.com/services/feeds/photos_public.gne?tags=car&tagmode=any&format=json&jsoncallback=?",
                      function(data){
                          $.each(data.items, function( i,item ){
                                $("<img class='para'/> ").attr("src", item.media.m ).appendTo("#resText");
                                if ( i == 3 ) { 
                                    return false;
                                }
                          });
                     }
            ); 
       })})

JSON可以实行跨域访问，打破了由于同源策略的原因，XMLHttpRequest对象只能实现同域访问的限制。

#### $.ajax()方法
$.ajax()方法是jquery最底层的Ajax实行

只有一个参数，但在这个对象里包含了$.ajax()方法所需要的请求设置以及回调函数等信息，参数以key/value的形式存在，所有参数都是可选的

    $(function(){
        $('#send').click(function() {
            $.ajax({
              type: "GET",
              url: "test.json",
              dataType: "json",
              success : function(data){
                     $('#resText').empty();
                      var html = '';
                      $.each( data  , function(commentIndex, comment) {
                          html += '<div class="comment"><h6>' + comment['username'] + ':</h6><p class="para">' + comment['content'] + '</p></div>';
                      })
                     $('#resText').html(html);
              }
            }); 
        })；})

### 序列化元素
#### 1.serialize()方法
可以将表单的数据序列化，可以将DOM元素内容转化为字符串，减少了编写数据的过程和对字符编码的问题。

    $(function(){
       $("#send").click(function(){
            $.get("get1.php", $("#form1").serialize() , function (data, textStatus){
                        $("#resText").html(data); // 把返回的数据添加到页面上
                    }
            );
       })
    })
用字符方式时，注意字符编码（中文问题）

    $.get("get1.php", "username="+encodeURIComponent($('#username').val())+"&content="+encodeURIComponent($('#content').val()), 

#### serializeArray()方法
是将DOM元素序列化，返回JSON格式

   

```
$(function(){
         var fields = $(":checkbox,:radio").serializeArray();
         console.log(fields);// Firebug输出
         $.each( fields, function(i, field){
            $("#results").append(field.value + " , ");
        }); 
   })
```

#### 3.$.param()方法
用来对一个数组或对象按照key/value的方式序列化

    (function(){
        var obj={a:1,b:2,c:3};
        var  k  = $.param(obj);
        alert(k)        // 输出  a=1&b=2&c=3
    })
    
### jquery中的ajax全局事件
jquery在调用ajax()方法时，会触发一些全局函数，即不论ajax()在何时何处，只要触发，就可以调用这些函数
ajaxStart() :在ajax请求开始前触发
ajaxStop(): 在ajax请求结束后触发

此外还有
ajaxComplete()  Ajax请求完成时触发
ajaxError()     Ajax请求发生错误时触发
ajaxSend()  Ajax请求发生前触发
ajaxSuccess()   Ajax请求成功时触发




