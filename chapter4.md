# 第四章 jquery中的事件和动画

@(锋利的jquery)


## jquery中的事件

###  加载DOM

- 在页面加载完毕后，浏览器会通过JavaScript使用window.onload方法为DOM元素添加事件，而在jquery中使用$(document).ready()方法来加载

#### 1.执行时机

window.onload方法是在网页中所有的元素（包括元素的所有关联文件）完全加载到浏览器后才执行，而在jquery中，通过$(document).ready()方法不用等待浏览器全部加载所有元素，可以通过对应的DOM元素加载后就可以执行。并不意味着要等待所有元素下载完才执行。

#### 2.多次使用
有两个函数one()和two(),执行下面的代码

    window.onload = one;
    window.onload = two;

结果是只执行第二个函数，及是window.onload方法会吧后面执行的函数覆盖前面的函数。
为了达到两个函数按顺序地执行触发，可以这样：

    window.onload=function(){
        one();
        two();
    }

虽然这样编写代码能解决某些问题，但是不能满足某些需求，例如有多个JavaScript文件，每个文件都要用到window.onload方法.这样比较麻烦。可以通过jquery中的$(document).ready()方法，每次调用都能在现有的行为上追加新的行为，并按顺序执行。

    $(document).ready(function(){
        one();
    })
     
    $(document).ready(function(){
        two();
    }) 
两个函数都能按顺序来调用。

#### 简写方式

    $(document).ready(function(){
        //
    })

    $(function(){
        //
    })

    $().ready(function(){
        //
    })

### 事件绑定
在元素装载完成后，需要打算为某个元素绑定事件来完成某些行为，可以使用bind()方法。

    bind(type[,data],fn)

#### 1.基本效果

单击“标题”链接来显示内容

    <div id="panel">
    <h5 class="head">什么是jQuery?</h5>
    <div class="content">
        jQuery是继Prototype之后又一个优秀的JavaScript库，它是一个由 John Resig 创建于2006年1月的开源项目。jQuery凭借简洁的语法和跨平台的兼容性，极大地简化了JavaScript开发人员遍历HTML文档、操作DOM、处理事件、执行动画和开发Ajax。它独特而又优雅的代码风格改变了JavaScript程序员的设计思路和编写程序的方式。
    </div>

    $(function(){
    $("#panel h5.head").bind("click",function(){
        $(this).next().show();
    })


### 2.加强效果
单击“标题”显示内容，再次单击“标题”隐藏内容

    $(function(){
    $("#panel h5.head").bind("click",function(){
        var $content = $(this).next();
        if($content.is(":visible")){
            $content.hide();
        }else{
            $content.show();
        }
    })
    })


#### 3.改变绑定事件类型
鼠标滑进“标题”显示内容，滑出“标题”隐藏内容

    $(function(){
    $("#panel h5.head").bind("mouseover",function(){
         $(this).next().show();
    }).bind("mouseout",function(){
         $(this).next().hide();
    })})

#### 4.简写绑定事件

    $(function(){
    $("#panel h5.head").mouseover(function(){
        $(this).next().show();
    }).mouseout(function(){
        $(this).next().hide();
    })})

### 合成事件
#### 1.hover()方法

hover()方法用于模拟光标悬停事件。当光标移动到元素上时，会触发第一个函数，移出这个元素时，会触发第二个函数。

    $(function(){
    $("#panel h5.head").hover(function(){
        $(this).next().show();
    },function(){
        $(this).next().hide();   
    })})

#### 2.toggle()方法
toggle()方法用于模拟光标连续单击事件，当光标单击元素时，会触发第一个函数，再次单击时会触发第二个函数，如果有更多的函数会不断地循环。

    $(function(){
    $("#panel h5.head").toggle(function(){
            $(this).next().show();
    },function(){
            $(this).next().hide();
    })})
toggle()方法还有另一个作用，可以切换元素的可见状态，相当于show()和hide()方法。

    $(function(){
    $("#panel h5.head").toggle(function(){
            $(this).next().toggle();
    },function(){
            $(this).next().toggle();
    })})

#### 3.再次加强效果

    $(function(){
    $("#panel h5.head").toggle(function(){
            $(this).addClass("highlight");  //添加高亮样式
            $(this).next().show();
    },function(){
            $(this).removeClass("highlight");  //移除高亮样式
            $(this).next().hide();
    });})

### 事件冒泡
#### 1.什么是冒泡
- 在页面上多个不同的元素同时绑定了一个相同的事件，并且元素层层嵌套，这样但执行某个元素时，会先从该元素执行行为，然后再向上父级元素执行事件，称之为冒泡。

#### 2.事件冒泡引起的问题
- 当想执行某个元素时，却不仅触发了该元素的事件，还触发了上级父元素的相同事件。
- 事件对象
为函数添加一个参数指定为事件对象。
  


```
  $("element").bind("click",function(event){
        //
    })
```



- 停止事件冒泡
可以阻止事件中的其他对象的事件处理函数被执行。



```
 $('span').bind("click",function(event){
        var txt = $('#msg').html() + "<p>内层span元素被点击.<p/>";
        $('#msg').html(txt);
        event.stopPropagation();    //  阻止事件冒泡
    });
```


- 阻止默认行为
网页中有自己的默认行为，例如，单击超链接时会跳转，单击“提交”按钮后会提交数据，有时需要阻止元素的默认行为

可以用preventDefault()方法

    $("element").bind("click",function(event){
      event.preventDefault()  //阻止默认行为
    })

- 事件捕获
与事件冒泡相反，是从该元素触发事件后，在向其子元素触发相同的事件。

### 事件对象的属性
jquery对事件对象的常用属性进行封装，使得事件处理在浏览器下都可以正常运行而不需要进行浏览器类型判断。
#### event.type

    $(function(){
    $("a").click(function(event) {
      alert(event.type);//获取事件类型
      return false;//阻止链接跳转
    });})
    //结果
    "click"

#### event.preventDefault()方法
上面介绍过。主要的作用是阻止默认的事件行为。

#### event.stopPropagation()方法
该方法的作用是阻止事件冒泡

#### event.target
获取到触发事件的元素

    
```
$(function(){
    $("a[href='http://google.com']").click(function(event) {
      var tg = event.target;  //获取事件对象
      alert( tg.href ) ;
      return false;//阻止链接跳转
    });
})
//结果
“http：//google.com”
```

#### event.relateTarget
相关元素的触发事件是通过event.relateTarget来访问的。event.relateTarget在mouseover中相当于IE浏览器的event.fromElement,在mouseout中相当于IE浏览器的event.toElement.

#### event.pageX和event.pageY
获取到光标相对于页面的x坐标和y坐标

    $(function(){
    $("a").click(function(event) {
      alert("Current mouse position: " + event.pageX + ", " + event.pageY );//获取鼠标当前相对于页面的坐标
      return false;//阻止链接跳转
    });})
#### event.which
在鼠标单击事件中获取到鼠标的左，中，右键；在键盘事件中获取键盘的按键。

    $(function(){
    $("a").mousedown(function(e){
        alert(e.which)  // 1 = 鼠标左键 ; 2 = 鼠标中键; 3 = 鼠标右键
        return false;//阻止链接跳转
    })})

#### event.metaKey
针对不同浏览器对键盘中的< ctrl >按键解释不同，jquery规定event.metaKey为键盘事件中获取< ctrl >键

### 移除事件
#### 1.移除按钮元素上以前注册得事件

   

```
 enter code here<script type="text/javascript">
    $(function(){
       $('#btn').bind("click", function(){
                     $('#test').append("<p>我的绑定函数1</p>");
              }).bind("click", function(){
                     $('#test').append("<p>我的绑定函数2</p>");
              }).bind("click", function(){
                     $('#test').append("<p>我的绑定函数3</p>");
              });
       $('#delAll').click(function(){
              $('#btn').unbind("click");
       });
    })
</script>
</head>
<body>
<button id="btn">点击我</button>
<div id="test"></div>
<button id="delAll">删除所有事件</button>
</body>
</html>
```
可以用unbind()方法直接移除元素上所有的方法

#### 2.移除< button >元素中的一个事件

    $('#delTwo').click(function(){
              $('#btn').unbind("click",myFun2);
       });

one()方法是只触发元素的事件一次，然后立即解除绑定事件。

### 模拟操作
#### 1.常用模拟
用trigger()方法可以用来模拟操作；

    $("#btn").trigger('click');
页面装载完成后自动执行#btn元素的click行为

简化为：

    $("#btn").click();

#### 2.触发自定义事件

trigger()方法可以触发自定义事件

    $(function(){
       $('#btn').bind("myClick", function(){
            $('#test').append("<p>我的自定义事件.</p>");
        });
       $('#btn').click(function(){
            $(this).trigger("myClick");
       }).trigger("myClick");
    })

### 3.传递数据

    $(function(){
       $('#btn').bind("myClick", function(event, message1, message2){
            $('#test').append( "<p>"+message1 + message2 +"</p>");
        });
       $('#btn').click(function(){
            $(this).trigger("myClick",["我的自定义","事件"]);
       }).trigger("myClick",["我的自定义","事件"]);
    })

#### 4.执行默认操作
trigger()方法触发事件后，会执行浏览器的默认操作，如果不想执行浏览器的默认操作可以用triggerHandler()方法，仅仅绑定事件。
    

### 其他用法
### 1.绑定多个事件类型

    $(function(){
     $("div").bind("mouseover mouseout", function(){
        $(this).toggleClass("over");
     });
  })
### 2.添加事件命名空间，便于管理

    $(function(){
    $("div").bind("click.plugin",function(){
           $("body").append("<p>click事件</p>");
    });
    $("div").bind("mouseover.plugin", function(){
           $("body").append("<p>mouseover事件</p>");
    });
    $("div").bind("dblclick", function(){
           $("body").append("<p>dblclick事件</p>");
    });
    $("button").click(function() {
        $("div").unbind(".plugin");  
    })
    /*
        click,mouseover 事件被删除，
    */})

单击button元素后，“plugin”命名空间被删除，而不再“plugin”的命名空间的事件依然存在

### 3.相同事件名称，不同命名空间执行方法

    $(function(){
    $("div").bind("click",function(){
           $("body").append("<p>click事件</p>");
    });
    $("div").bind("click.plugin", function(){
           $("body").append("<p>click.plugin事件</p>");
    });
    $("button").click(function() {
          $("div").trigger("click!");    // 注意click后面的感叹号
    });})

单击div元素会触发click和click.plugin事件，而单击button元素则只会触发click事件

click！ 后面感叹号表示匹配所有不包含命名空间的click方法。

## jquery中的动画
### show()方法和hide()方法
#### 1.show()方法和hide()方法
show()方法和hide()方法是jquery中最基本的动画方法。

    $("element").hide(); 
    //相当于
    $("element").css("display","none");
    
而show()方法相当于css里的display属性的inline和block，取决于原本的属性。

#### 2.show()方法hide()方法让元素动起来

    $(function(){
    $("#panel h5.head").toggle(function(){
         $(this).next().hide(600);
    },function(){
         $(this).next().show(600);
    })})

连续单击元素触发行为使元素在1秒钟显示或1秒钟隐藏。

### fadeIn()方法和fadeOut()方法
 fadeIn()方法和fadeOut()方法只改变元素的透明度显示和隐藏。

### slideUp()方法和slideDown()方法
slideUp()方法是由上而下地延伸显示，而slideDown()方法是由下而上地延伸隐藏。

### 自定义动画animate()方法
animate()方法可以同时满足多个动画进行的需求，可以对多个动画进行控制
语法：

    animate(params,speed,callback);

1.params:一个包含样式属性的值的映射。
2.speed:速度参数，可选。
3.callback:在动画完成时执行的函数，可选。

#### 1.自定义简单的动画
是元素在3秒内，向右移动500像素。

    $(function(){
    $("#panel").click(function(){
       $(this).animate({left: "500px"}, 3000);
    })})

#### 2.累加累减动画
使元素在当前位置添加或减少移动的像素

    $(function(){
        $("#panel").click(function(){
            $(this).animate((left:"+=500px"),300)
        })
    })
#### 多重动画
1.同时执行多个动画
元素向右滑动时，同时放大

    $(function(){
    $("#panel").click(function(){
        $(this).animate({left: "500px",height:"200px"}, 3000);
    })})

2.按顺序执行多个动画

    $(function(){
    $("#panel").click(function(){
         $(this).animate({left: "500px"}, 3000)
                .animate({height: "200px"}, 3000);
    })})
#### 4.综合动画

    $(function(){
        $("#panel").css("opacity", "0.5");//ÉèÖÃ²»Í¸Ã÷¶È
        $("#panel").click(function(){
              $(this).animate({left: "400px", height:"200px" ,opacity: "1"}, 3000)
                     .animate({top: "200px" , width :"200px"}, 3000 )
                     .fadeOut("slow");
        });
    });

### 动画回调函数
在上例中，如果想在最后一步添加css样式的改变，写成

    .fadeOut("slow").css("....")

实际并不能得到预想的效果，而是在再开始执行动画的时候，css()已经被执行。

可以将css()写在最后一个动画的回调函数里即可

    $(function(){
        $("#panel").css("opacity", "0.5");//ÉèÖÃ²»Í¸Ã÷¶È
        $("#panel").click(function(){
              $(this).animate({left: "400px", height:"200px" ,opacity: "1"}, 3000)
                     .animate({top: "200px" , width :"200px"}, 3000 ，function(){   $(this).css("...")})
                     .fadeOut("slow");
        });
    });

### 停止动画和判断是否处于动画状态
#### 1.停止元素的动画
stop()方法
语法:

    stop([clearQueue],[gotoEnd]);
clearQueue和gotoEnd都是可选的参数，其值为布尔值。clearQueue代表是否要清空未执行完的动画队列，gotoEnd代表是否直接将正在执行的动画跳转到末状态。

stop()代表停止当前动画，并执行队列中下一个动画
stop(true)代表停止当前动画并清空未执行队列的所有动画
stop(false,true)代表让当前的动画，直接到达动画的末状态。
stop(true,true)代表停止当前动画并直接到达动画的末状态。

**注意：jquery只能设置正在执行的动画的最终状态，而没有提供直接到达未执行动画队列最终状态的方法。**

#### 2.判断元素是否处于动画状态

    $("button").click(function(){
            if(!$('#mover').is(":animated")){       //判断元素是否正处于动画状态
                 //如果当前没有进行动画，则添加新动画 
                $('#mover').fadeToggle("slow", animateIt2);
            }else{
                $('#mover').stop();
            }
        });

#### 3.延迟动画
对动画进行延迟操作，可以用dela()方法

    $(function(){
        $("#panel").css("opacity", "0.5");//设置不透明度
        $("#panel").click(function(){
              $(this).animate({left: "400px", height:"200px" ,opacity: "1"}, 3000)
                     .delay(1000)
                     .animate({top: "200px" , width :"200px"}, 3000 )
                     .delay(2000)
                     .fadeOut("slow");
        });
    });

### 其他动画方法
#### 1.toggle()方法


    $(function(){
    $("#panel h5.head").click(function(){
         $(this).next().toggle();
    })})
在隐藏和显示中切换

#### 2.slideToggle()方法

    $(function(){
    $("#panel h5.head").click(function(){
         $(this).next().slideToggle();
    })})

通过改变高度，在隐藏和显示中切换

#### 3.fadeTo()方法

    $(function(){
    $("#panel h5.head").click(function(){
         $(this).next().fadeTo(600, 0.2);
    })})
以不透明度渐进到指定的值的方式，在隐藏和显示中切换

#### 4.fadeToggle()方法

    $(function(){
    $("#panel h5.head").click(function(){
         $(this).next().fadeToggle();
    })})
以不透明度渐进的方式，在隐藏和显示中切换

### 动画队列
1.一组元素上的动画效果
- 当在一个animate()方法中应用多个属性，动画是同时发生的
- 当以链式的写法应用动画方式时，动画是按照顺序发生的


2.多组元素山的动画效果
- 默认情况下，动画是同时发生的
- 当以回调的形式应用动画方式时，动画是按照回调顺序发生的