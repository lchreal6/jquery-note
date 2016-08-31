# 第五章：jQuery对表单，表格的操作及更多应用

## 5.1表单应用
一个表单有3个基本组成部分
- 1.表单标签：包含处理表单数据的服务器端程序URL以及数据数据提交到服务器的方法
- 2.表单域：包含文本框，密码框，隐藏框，多行文本框，复选框等
- 3.表单按钮：包含提交按钮、复位按钮和一般按钮。

### 单行文本框的应用

当文本框获得焦点时，它的背景颜色改变，当它失去焦点时，则要恢复为原来的样式。
可以用css代码来实现

    input:focus ,textarea:focus{
        border:1px solid #f00;
        background:#fcc;
    }
IE6不支持出：hover之外的伪类，可以使用js来实现

    //css
    .focus { 
     border: 1px solid #f00;
     background: #fcc;}
     //js
     $(function(){
        $(":input").focus(function(){
              $(this).addClass("focus");
        }).blur(function(){
              $(this).removeClass("focus");
        });
    })

### 多行文本框的应用

#### 1.高度变化

设置多行文本框的的高度变化

    $(function(){
        var $comment = $('#comment');  //获取评论框
        $('.bigger').click(function(){ //放大按钮绑定单击事件
              if( $comment.height() < 500 ){ 
                   $comment.height(  $comment.height() + 50 ); //重新设置高度，在原有的基础上加50
              }
        })
        $('.smaller').click(function(){//缩小按钮绑定单击事件
                if( $comment.height() > 50 ){
                    $comment.height( $comment.height() - 50 ); //重新设置高度，在原有的基础上减50
                }
        });

#### 2.滚动条高度变化
控制文本框的滚动条

    $(function(){
        var $comment = $('#comment');//获取评论框
        $('.up').click(function(){ //向上按钮绑定单击事件
           if(!$comment.is(":animated")){//判断是否处于动画
                $comment.animate({ scrollTop  : "-=50" } , 400);
            }
        })
        $('.down').click(function(){//向下按钮绑定单击事件
           if(!$comment.is(":animated")){
                $comment.animate({ scrollTop  : "+=50" } , 400);
            }
        });

### 复选框的选用

  

```
  <form method="post" action="">
   你爱好的运动是？
   <br/>
    <input type="checkbox" name="items" value="足球"/>足球
    <input type="checkbox" name="items" value="篮球"/>篮球
    <input type="checkbox" name="items" value="羽毛球"/>羽毛球
    <input type="checkbox" name="items" value="乒乓球"/>乒乓球
   <br/>
    <input type="button" id="CheckedAll" value="全　选"/>
    <input type="button" id="CheckedNo" value="全不选"/>
    <input type="button" id="CheckedRev" value="反　选"/> 

    <input type="button" id="send" value="提　交"/> 
</form>
```

当点击全选框时，要求四个选项都选中：

    $("#CheckedAll").click(function(){
         $('[name=items]:checkbox').attr('checked', true);
     });
点击全不选框，四个选项都不选中：

    $("#CheckedNo").click(function(){
        $('[type=checkbox]:checkbox').attr('checked', false);
     });
反选：

    $("#CheckedRev").click(function(){
          $('[name=items]:checkbox').each(function(){
            //此处用JQ写法颇显啰嗦。体现不出JQ飘逸的感觉。
            //$(this).attr("checked", !$(this).attr("checked"));
            //$(this).prop("checked", !$(this).prop("checked"));
            
            //直接使用JS原生代码，简单实用
            this.checked=!this.checked;
          });
     });

- 复选框

   

```
 <form>
   你爱好的运动是？<input type="checkbox" id="CheckedAll" />全选/全不选<br/>
    <input type="checkbox" name="items" value="足球"/>足球
    <input type="checkbox" name="items" value="篮球"/>篮球
    <input type="checkbox" name="items" value="羽毛球"/>羽毛球
    <input type="checkbox" name="items" value="乒乓球"/>乒乓球<br/>
    <input type="button" id="send" value="提　交"/> 
</form>
```

全选或全不选

    $("#CheckedAll").click(function(){
            if(this.checked){                //如果当前点击的多选框被选中
                 $('input[type=checkbox][name=items]').attr("checked", true );
            }else{                              
                 $('input[type=checkbox][name=items]').attr("checked", false );
            }
     });
     //更简便写法
     $("#CheckedAll").click(function(){
            //所有checkbox跟着全选的checkbox走。
            $('[name=items]:checkbox').attr("checked", this.checked );
     });
    

其他选项选了或不选跟复选框生成联系：

    $('input[type=checkbox][name=items]').click(function(){
               var flag=true;
               $('input[type=checkbox][name=items]').each(function(){
                    if(!this.checked){
                         flag = false;
                    }
               });

               if( flag ){
                     $('#CheckedAll').attr('checked', true );
               }else{
                     $('#CheckedAll').attr('checked', false );
               }
     });

     // //或者用累计选了的框的数量是否等于总数量来反馈到复选框中
     $('[name=items]:checkbox').click(function(){
                //定义一个临时变量，避免重复使用同一个选择器选择页面中的元素，提升程序效率。
                var $tmp=$('[name=items]:checkbox');
                //用filter方法筛选出选中的复选框。并直接给CheckedAll赋值。
                $('#CheckedAll').attr('checked',$tmp.length==$tmp.filter(':checked').length);

            /*
                //一行做过多的事情需要写更多注释。复杂选择器还可能影响效率。因此不推荐如下写法。
                $('#CheckedAll').attr('checked',!$('[name=items]:checkbox').filter(':not(:checked)').length);
            */
     });

### 下拉框应用

    <body>
    <div class="centent">
        <select multiple="multiple" id="select1" style="width:100px;height:160px;">
            <option value="1">选项1</option>
            <option value="2">选项2</option>
            <option value="3">选项3</option>
            <option value="4">选项4</option>
            <option value="5">选项5</option>
            <option value="6">选项6</option>
            <option value="7">选项7</option>
        </select>
        <div>
            <span id="add" >选中添加到右边&gt;&gt;</span>
            <span id="add_all" >全部添加到右边&gt;&gt;</span>
        </div>
    </div>

    <div class="centent">
        <select multiple="multiple" id="select2" style="width: 100px;height:160px;">
            <option value="8">选项8</option>
        </select>
        <div>
            <span id="remove">&lt;&lt;选中删除到左边</span>
            <span id="remove_all">&lt;&lt;全部删除到左边</span>
        </div>
    </div>
    </body>
左边选中添加到右边

    $('#add').click(function() {
    //获取选中的选项，删除并追加给对方
        $('#select1 option:selected').appendTo('#select2');
    });
左边全部选中添加到右边

    $('#add_all').click(function() {
        //获取全部的选项,删除并追加给对方
        $('#select1 option').appendTo('#select2');
    });
左边双击添加到右边

    ('#select1').dblclick(function(){ //绑定双击事件
        //获取全部的选项,删除并追加给对方
        $("option:selected",this).appendTo('#select2'); //追加给对方
    });


### 表单验证
以一个简单的用户注册为例：

    <form method="post" action="">
    <div class="int">
        <label for="username">用户名:</label>
        <input type="text" id="username" class="required" />
    </div>
    <div class="int">
        <label for="email">邮箱:</label>
        <input type="text" id="email" class="required" />
    </div>
    <div class="int">
        <label for="personinfo">个人资料:</label>
        <input type="text" id="personinfo" />
    </div>
    <div class="sub">
        <input type="submit" value="提交" id="send"/><input type="reset" id="res"/>
    </div>
    </form>

表单内class属性为"required"的文本框时必须填写的，因此需要将它与其他的非必须填写表单元素加以区别，即在文本框后面追加一个红色的小星星标识，可以使用append()方法来完成：

    $(function(){
        //如果是必填的，则加红星标识.
        $("form :input.required").each(function(){
            var $required = $("<strong class='high'> *</strong>"); //创建元素
            $(this).parent().append($required); //然后将它追加到文档中
        });
    });

判断当前失去焦点的元素是"用户名"还是"邮箱"，然后分别处理

    $('form :input').blur(function(){  // 为表单元素添加失去焦点事件
             var $parent = $(this).parent();
             //验证用户名
             $parent.find(".formtips").remove();//删除以前的提醒元素
             if( $(this).is('#username') ){
                    if( this.value=="" || this.value.length < 6 ){
                        var errorMsg = '请输入至少6位的用户名.';
                        $parent.append('<span class="formtips onError">'+errorMsg+'</span>');
                    }else{
                        var okMsg = '输入正确.';
                        $parent.append('<span class="formtips onSuccess">'+okMsg+'</span>');
                    }
             }
             //验证邮件
             if( $(this).is('#email') ){
                if( this.value=="" || ( this.value!="" && !/.+@.+\.[a-zA-Z]{2,4}$/.test(this.value) ) ){
                      var errorMsg = '请输入正确的E-Mail地址.';
                      $parent.append('<span class="formtips onError">'+errorMsg+'</span>');
                }else{
                      var okMsg = '输入正确.';
                      $parent.append('<span class="formtips onSuccess">'+okMsg+'</span>');
                }
             }
        })


**如果用户无视错误的提醒，要单击“提交”按钮时，为了使表单填写准确，在表单提交之前，需要对表单的必须填写元素进行一次整体的验证，可以直接用trigger()来触发blur事件。**

```
 $('#send').click(function(){
                $("form :input.required").trigger('blur');
                var numError = $('form .onError').length;
                if(numError){
                    return false;
                } 
                alert("注册成功,密码已发到你的邮箱,请查收.");
         });
```

如果要求用户在输入或得到焦点时获得提醒，可以用keyup方法和focus方法

    $("form:input").blur(function(){
        //失去焦点处理函数
    }).keyup(function(){
           $(this).triggerHandler("blur");
        }).focus(function(){
           $(this).triggerHandler("blur");
        });//end blur

tigger()触发事件且绑定元素，triggerHandler()只会触发事件，不绑定元素。

## 表格应用

### 表格变色

#### 1.普通的隔行变色

首先定义css代码：

    .even       { background:#FFF38F;}  /* 偶数行样式*/
    .odd        { background:#FFFFEE;}  /* 奇数行样式*/

选择奇偶函数分别添加样式

    $(function(){
        $("tr:odd").addClass("odd");  /* ÆæÊýÐÐÌí¼ÓÑùÊ½*/
        $("tr:even").addClass("even"); /* Å¼ÊýÐÐÌí¼ÓÑùÊ½*/
    })

#### 2.单选框控制表格行高亮
定义css代码：

    .selected       { background:#FF6500;color:#fff;}

给单击的当前行添加高亮样式：

    $('tbody>tr').click(function() {
            $(this)
                .addClass('selected')
                .siblings().removeClass('selected')
                .end()
                .find(':radio').attr('checked',true);
        });

**end()函数表示返回当前的this**

#### 3.复选框控制表格行高亮
单击当前行数添加高亮和勾选多选框

```
$('tbody>tr').click(function() {
            if ($(this).hasClass('selected')) {
                $(this)
                    .removeClass('selected')
                    .find(':checkbox').attr('checked',false);
            }else{
                $(this)
                    .addClass('selected')
                    .find(':checkbox').attr('checked',true);
            }
        });
```

更简洁的写法：

    $('tbody>tr').click(function() {
            //ÅÐ¶Ïµ±Ç°ÊÇ·ñÑ¡ÖÐ
            var hasSelected=$(this).hasClass('selected');
            //Èç¹ûÑ¡ÖÐ£¬ÔòÒÆ³öselectedÀà£¬·ñÔò¾Í¼ÓÉÏselectedÀà
            $(this)[hasSelected?"removeClass":"addClass"]('selected')
                //²éÕÒÄÚ²¿µÄcheckbox,ÉèÖÃ¶ÔÓ¦µÄÊôÐÔ¡£
                .find(':checkbox').attr('checked',!hasSelected);
        });

#### 表格展开关闭

    <table>
        <thead>
            <tr><th>ÐÕÃû</th><th>ÐÔ±ð</th><th>ÔÝ×¡µØ</th></tr>
        </thead>
        <tbody>
            <tr class="parent" id="row_01"><td colspan="3">Ç°Ì¨Éè¼Æ×é</td></tr>
            <tr class="child_row_01"><td>ÕÅÉ½</td><td>ÄÐ</td><td>Õã½­Äþ²¨</td></tr>
            <tr class="child_row_01"><td>ÀîËÄ</td><td>Å®</td><td>Õã½­º¼ÖÝ</td></tr>

            <tr class="parent" id="row_02"><td colspan="3">Ç°Ì¨¿ª·¢×é</td></tr>
            <tr class="child_row_02"><td>ÍõÎå</td><td>ÄÐ</td><td>ºþÄÏ³¤É³</td></tr>
            <tr class="child_row_02"><td>ÕÒÁù</td><td>ÄÐ</td><td>Õã½­ÎÂÖÝ</td></tr>

            <tr class="parent" id="row_03"><td colspan="3">ºóÌ¨¿ª·¢×é</td></tr>
            <tr class="child_row_03"><td>Rain</td><td>ÄÐ</td><td>Õã½­º¼ÖÝ</td></tr>
            <tr class="child_row_03"><td>MAXMAN</td><td>Å®</td><td>Õã½­º¼ÖÝ</td></tr>
        </tbody>
    </table>

单击父行，子行关闭隐藏：

    $(function(){
    $('tr.parent').click(function(){   // »ñÈ¡ËùÎ½µÄ¸¸ÐÐ
            $(this)
                .toggleClass("selected")   // Ìí¼Ó/É¾³ý¸ßÁÁ
                .siblings('.child_'+this.id).toggle();  // Òþ²Ø/ÏÔÊ¾ËùÎ½µÄ×ÓÐÐ
    });})

#### 表格内容筛选

    $(function(){
       $("#filterName").keyup(function(){
          $("table tbody tr")
                    .hide()
                    .filter(":contains('"+( $(this).val() )+"')")
                    .show();
       }).keyup()//加载DOM完成后，绑定事件完成之后立即触发; })

### 其他应用
#### 1. 网页字体大小
#### 2.选项卡
#### 3.网页换肤




