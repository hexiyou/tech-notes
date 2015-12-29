JavaScript学习笔记

> 29. 微信内部的webview不能跳转到app store，微信给禁用了

# 28. 移动端scroll事件丢失
    
    在pc端一个页面快速滚动到页面底部（滚动距离为9200）大概执行42-50次scroll事件
    在webview中，同样的距离，快速滚到到页面底部，执行的scroll事件次数为8-9次，丢失了很多次scroll事件！！！

27. 不使用框架，node js  路由怎么返回html文件给 客户端渲染?
    
    server.on('request',function(req,res){
        
        var urlStr = url.parse(req.url);
        switch(urlStr.pathname){
            
            case '/' :
            // 首页
            sendData(htmlDir+'index.html',req,res);
            break;
            
            case '/user':
            // 用户首页
            sendData(htmlDir+'user.html',req,res);
            break;
            
            case '/login':
            // 用户登录
            sendData(htmlDir+'login.html',req,res);
            break;
            
        }
        
    })
    
    总结：就是写好一个html文件，然后sentData() 把文件路径传进去就可以了。

26. 手机webview是一个节省资源的版本，不同于手机浏览器，手机webview和手机浏览器大概有20-30%的差异
    
    最明显的是，在safari下，快速滚动页面，scroll事件不会丢失
    但是在webview下，快速滚动页面，会导致中间可能有几个scroll事件没有被触发。
    问题见于：调调分类listView

25. 属性选择器

    a、span[data-id="18399639900348"]{ // data-id属性的值等于18399639900348的span元素
        color:red;
       }
       
    b、[data-id="18399639900348"]{ // 所有data-id属性的值等于18399639900348的元素
        color:red;
       }
       
    c、span[data-id]{ // 包含data-id属性的span元素
        color:red;
       }
24. python搭建http服务器

    python -m SimpleHTTPServe 8088
    
    一个简易的http服务器就搭建好了，在浏览器访问 http://localhost:8088 如果命令执行的目录下有index.html，自动打开
    
    index.html 非常方便！！

23. offsetLeft 和 offsetTop

    相对于有定位（relative/absolute/fixed）的最近的祖先元素（直到找到body为止）的偏移（并不一定非是left 和 top的偏移）。

22. em的计算规则：
    
    1、自己有font-size的，em乘自己的font-size，比如 div {font-size:20px;width:10em;} 此时width为 20*10 = 200px
    2、自己没有font-size的（其实也有，继承于父元素），比如 body {font-size:30px;} div{width:20em;}，此时width为30*20=600px
    
    一个大例子：
    
    <--html结构-->
    
     <body>
        <div class="d">
            <div class="d2">d2</div>
        </div>
     </body>
     
     <--css-->
     
     <style>
        body{
            font-size: 20px;
        }
        .d{
            /* 继承了body的20px的font-size，此时.d的font-size为20px，所以width为20*60=1200px */
            width: 60em;
            background: #333;
            height: 200px;
        }
        .d2{
          color:white;
          line-height: 1;
          /* 继承了.d的20px的font-size（.d继承自body），此时.d2的font-size为 20*2 = 40px */
          font-size: 2em;
          /* 经过上面的运算，font-size为40px，所以height为 40*3 = 120px */
          height: 3em;
        }
      </style>
      
      记住一个技巧：EM乘自己。

21.
    chrome下在父页面操作ifrmae时，必须得在服务器环境下，使用文件协议打开的页面是无法操作iframe的。报错信息：
    Uncaught SecurityError: Failed to read the 'contentDocument' property from 'HTMLIFrameElement': Blocked a frame with origin "null" from accessing a frame with origin "null". Protocols, domains, and ports must match.

     safari和firefox下用文件协议打开的页面可以操作iframe

20.pushState 和 replaceState 不会导致hashchange事件触发；

   调用pushState 和 replaceState方法并不会导致 onpopstate 事件触发，
   
   该事件是在用户点击浏览器的“前进”、“后退”按钮在 ps 和 rs方法改变的history队列中的元素切换时才会触发。


19.iframe里与父框架共享history。。

   可以在iframe中验证history.length 与 父框架的history.length，他俩永远相等，
   
   在iframe页中调用history的go、back、forward方法与在父框架页里调用是一样的，
   
   而
   
   window.location.hash 与 window.location.href
   
   不与父框架共享
   
   iframe里的_self链接还是在iframe里打开，而_blank链接则是新开一个浏览器tab。
   
   

18.如果一个网页里有iframe，并且使用js来改变src的话，会改变这个页面的history

   实验：
   
   一个页面只有一个a标签，点击a标签时，改变页面中iframe中的src（http://www.baidu.com），

   a.onclick = function(){

       var ifr = document.getElementById('if');
       
       ifr.src = "http://www.imooc.com"; // 改变src为慕课网
     
   }

   在a标签被点击之前，使用 history.length 发现 history 队列的长度为2 （一个空地址、一个当前地址本身）

   点击之后，history.length = 3

   说明了，页面中的iframe也会改变父框架的history
   
   这也就导致了我不能使用iframe来模拟native app的页面左滑入场，因为要结合window.location.hash 和 onhashchange 来管理
   
   前进和后退按钮，如果iframe也能改变history的话，会导致history会一次入队2个元素（一次是iframe引起的，一次是hash改变引起的）


17.关于replaceChild

   例如 div.replaceChild(em,span);

   replaceChild函数有两个参数，
   第一个参数是要替换的新元素（一般是使用createElement新建出来的元素）；
        如果是已经存在于文档中的节点作为第一个参数，那么会先清空div，然后把这个em节点 剪切 到div下
   第二个参数必须是div元素的已存在的子元素，即span元素已经存在于div下了否则报错，
       报错信息：Uncaught DOMException: Failed to execute 'replaceChild' on 'Node': The node to be replaced is not a child                   of this node.

16.关于history对象

   地址     地址栏为空    #1     #2    #3    #4    #5
   length       1          2      3     4     5     6

   F5刷新页面、地址栏刷新(即光标放在地址栏，然后回车)、history.reload()刷新不会改变history
   手动在地址栏修改hash、window.location.hash、location.href修改hash，不管修改后的地址是否存在history，都会改变history，方式    是往后append
   
   window.location.reload() // 相当于F5刷新
   window.location.replace() // 替换history队列最后一个元素的值。history.length 并不变

15.DOM对象不可序列化

14.喜讯、喜讯，产房传喜讯

   发现了深拷贝Object和Array对象的方法啦。。
   
   function deepCopy(obj){
    
     return JSON.parse(JSON.stringify(obj));
    
   }
   
   首先，把一个对象转成json格式字符串
   JSON.stringify(obj)
   接着，parse这个字符串，原样返回一个深层复制的对象拷贝
   JSON.parse(JSON.stringify(obj))
   
   这种方法太棒了，简单，高效！！唯一的不足就是兼容性，在IE67下并没有JSON对象。。但是！！这么老的浏览器考虑它干嘛。。。
   
   什么递归深拷贝，$.extends()都是浮云！！


13.发现了函数的第二种调用方式
   例如

   var obj = {
    say:function(data){
      console.log('say '+data);
    }
   }

   (obj.say)('李彦峰'); // 李彦峰

   
   function test(){
     alert('test');
   }

   (test)(); // test


   在jquery中的应用：
   globalEval: function( data ) {
        if ( data && jQuery.trim( data ) ) {
            // We use execScript on Internet Explorer
            // We use an anonymous function so that context is window
            // rather than jQuery in Firefox
            ( window.execScript || function( data ) {
                window[ "eval" ].call( window, data );
            } )( data );
        }
    }

   

12.jquery判断是否是数字isNumeric

   return  !jQuery.isArray( obj ) && (obj - parseFloat( obj ) + 1) >= 0;
   
   对于 var num = new Number(123);
        var str = new String('123');
      都返回true
      
      原理是 在parseFloat时，参数会调用toString方法
             num.toString() -> '123'
             str.toString() -> '123'
             
       数组[1,2].toString() -> '1,2' 然后parseFloat -> 1
       
       所以不能是数组。。
       
       如果传入的数字是NaN,应该返回false， 肿么办？
       
       NaN - NaN = NaN
       NaN - 1 = NaN
       NaN >= 0  = false
       
       obj - parseFloat(obj) + 1 是为了防止parseFloa损失精度精度损失最高不能超过1，所以
       不管obj - parseFloat(obj)的值是正是负（大部分都是0），再加上1，肯定大于等于0

11.可以new对象的内置构造函数（9个），一般需要用type函数来判断类型

   Number,Boolean,String,Array,Date,Function,Object,RegExp,Error
   
   其他的内置对象，比如JSON,Math并不能new对象

10.Object()函数具有最强的包装性

   可以把基本类型的值变成包装类型
   如果是传入的本身就是一个object对象，原样返回

    Object(124) -> new Number(124)
    Object(true) -> new Boolean(true)
    Object('liy') -> new String('liy')
    Object(null) -> new Object()
    Object(undefined) -> new Object()
    Object([1,2,3]) -> new Array(1,2,3)
    Object(new Date()) -> new Date()
    Object(\a\) -> new RegExp('a')
    
9.arrayLike
  
  var al = {
    0:1,
    1:2,
    2:3,
    length:3
  }
 object的key一定为string类型！！！
 
 for(var attr in al){
   console.log(typeof attr);// string
 }


8.判断一个对象是否是window对象

  由于window的特殊性（globel），所以，判断它用的方法也很特殊
  
  如果一个对象是window对象,那么其下有一个window属性，就是它本身
  
  return obj!= null && obj == obj.window
  
  

7.数组复制
 
   如 var arr = [1,2,3,4]
   a. var arr2 = arr.slice(0)
   b. var arr2 = arr.concat()

6.likeArray转Array

  如 var likeArray = {0:0,1:1,2:2,length:3}
   a.var arr = Array.prototype.slice.call(likeArray)
   b.var arr = [].slice.call(likeArray)

5.在函数内部定义与函数重名的变量

 a.
   function test(){
     alert(test)
   }
   test() // function test(){ alert(test) }
   test() // function test(){ alert(test) }
 b.
   function test(){
     var test = 2;
     alert(test);
   }
   test() // 2
   test() // 2
 c.
   function test(){
     test = 2;
     alert(test);
   }
   test() // 2
   test() // 报错。因为此时全局下的test已经变为2，并不是一个函数了。Uncaught TypeError: test is not a function
   
  结论：在函数内部定义跟函数名一致的变量要注意，一定使用var来使得同函数名的变量变为局部变量。。
  否则，定义的函数只有第一次执行时是正确的，后续的调用会出问题 xxx is not a function
 
4.
  var name = 'liyanfeng';
  (function(){
    alert(name); // liyanfeng
  })()
  相当于
  var name = 'liyanfeng';
  function any(){
    alert(name);
  }
  any(); // liyanfeng
  

3. NaN === NaN  --> false
  
   在js中只有NaN不等于自己，可以利用这个特性来判断一个字符串是否是数字

   如
 
   var str = '123';

   str = +str;

   if(str === str){
  
       // 说明不是NaN，也就是说 使用 + 号进行类型转换并没有问题，所以str肯定是数字格式的字符串
       
   }

2.可以把一个构造函数当成一个普通函数来执行

  var Person = function(){
    this.name = 'liyanfeng';
    this.age = 24;
    console.log('person constructor');
    return 123;
  }
 
  var p = new Person(); // is a new object not a number 123
  
  var p2 = p.constructor(); // is a number 123 not a new object
 

1.带返回值的构造函数

  
  function return123(){
    
      console.log('return123 execute...');     
    
      return 123;
  
  }
  
  
  
  var Person = function(name,age){
    
     this.name = name;
     
     this.age = age;
     
     return return123();

  }

  var p = Person('liyanfeng',24);
  
  // 首先打印出 return123 execute...
 
  console.log(p) // 123
  
  console.log(p.name) // undefined
 
  console.log(p.age) // undefined
 
  name 和 age 绑定到了全局环境中。。


  var Person = function(name,age){
    
     this.name = name;
     
     this.age = age;
     
     return return123();

  }

  var p2 = new Person('liyanfeng',24);
  
  // 首先打印出 return123 execute...
 
  console.log(p2) // Person {name: "liyanfeng", age: 24}
  
  console.log(p2.name) // liyanfeng
 
  console.log(p2.age) // 24


  可以看出来，若是前面有new操作符，return后的语句虽然会执行，但是忽略掉了返回值，因为js引擎要把new出的对象的应用赋值给p2。。。p2并不能接收2个返回值。。。！！！！！


  




