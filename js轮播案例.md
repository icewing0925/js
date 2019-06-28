# 轮播案例

## 1.动画函数案例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        div {
            width: 150px;
            height: 100px;
            background-color: lightblue;
            position: absolute;
            left: 50px;
        }
    </style>
</head>
<body>
<!--div移动案例-->
<input type="button" value="移动400px" id="btn1">
<input type="button" value="移动800px" id="btn2">
<div id="dv"></div>

<script>
    let btn = document.getElementById("btn1");
    let btn2 = document.getElementById("btn2");
    let dv = document.getElementById("dv");
    /*
    * div想要移动 必须要脱离文档流
    * 要获取div所在的位置
    *
    * 在style标签中设置的值是无法使用style.属性获取到的;
    * */
    btn.onclick = function () {
        animate(dv, -50)
    }
    btn2.onclick = function () {
        animate(dv, 800)
    }

    function animate(element, target) {
        clearInterval(element.timeID);//先清理定时器
    
        element.timeID= setInterval(function () {
            let current = element.offsetLeft;//获取当前位置
            let move = 10;
           move = current < target ? move : -move;
           current += move; //每次加10px
            if (Math.abs(target - current) > Math.abs(move)) { //如果 当前位小于400
              element.style.left = current + "px";
           } else {
              clearInterval(element.timeID);
                element.style.left = target + "px";
            }
        }, 20)
   }
</script>
</body>
</html>

```

## 2.简单小按钮案例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>

        *{
            margin: 0px;
            padding: 0px;
        }
        ul li{
            list-style: none;
        }

        .box{
            width: 768px;
            height: 287px;
            margin: 100px auto;
            border: 1px solid skyblue;
            padding: 5px;
        }
        .inner{
            width: 768px;
            height: 287px;
            position: relative;
            overflow: hidden;
        }
        .inner ul{
            width: 1000%;
            position: absolute;
            top: 0;
            left: 0;
        }
        img{
            vertical-align: top;
        }
        .inner ul li{
            float: left;
        }
        .inner img{
            width: 768px;
            height: 287px;
        }
        .square{
            position: absolute;
            top: 93%;
            width: 250px;
            height: 20px;
            left: 50%;
            margin-left: -125px;
        }
        .square ul li{
            width: 28px;
            height: 18px;
            float: left;
            margin-right: 20px;
            background-color: rgba(128,128,128,0.8);
            text-align: center;
            font-size: 14px;
            cursor:pointer;
            border: 1px solid #000000;
        }
        .square ul .current{
            color: white;
            cursor: pointer;
            background-color: rgba(248,128,8,1);
        }
    </style>
</head>
<body>
<!--js轮播案例-->
<div id="box" class="box">
    <div class="inner">
        <ul>
            <li><a href=""><img src="img/banner/banner1.jpg" alt=""></a></li>
            <li><a href=""><img src="img/banner/banner2.jpg" alt=""></a></li>
            <li><a href=""><img src="img/banner/banner3.jpg" alt=""></a></li>
            <li><a href=""><img src="img/banner/banner4.jpg" alt=""></a></li>
            <li><a href=""><img src="img/banner/banner5.jpg" alt=""></a></li>
        </ul>
        <div class="square">
            <ul>
                <li class="current">1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
            </ul>
        </div>
    </div>
</div>

<script>
     let box = document.getElementById("box");
     let inner = box.children[0]; //获取相框
     let innerUl = inner.children[0];
     let imgWidth = inner.offsetWidth; //获取相框宽度
     let square = inner.children[1];
     let ulObj = square.children[0]; //获取ul
     let liObj = ulObj.children; //获取li
    for(let i=0; i<liObj.length;i++){
        //设置索引值
        liObj[i].setAttribute("index",i);

        //设置鼠标移入事件
        liObj[i].onmouseover = function () {
            //移出所有li的类样式
            for(let j=0;j<liObj.length;j++){
                liObj[j].removeAttribute("class");
            }

            this.className="current";
            //获取当前的索引值
            let index = this.getAttribute("index");


            animate(innerUl,-index*imgWidth);

        }
    }

     function animate(element, target) {
         clearInterval(element.timeID);//先清理定时器

         element.timeID= setInterval(function () {
             let current = element.offsetLeft;//获取当前位置
             let move = 10;
             move = current < target ? move : -move;
             current += move; //每次加10px
             if (Math.abs(target - current) > Math.abs(move)) { //如果 当前位小于400
                 element.style.left = current + "px";
             } else {
                 clearInterval(element.timeID);
                 element.style.left = target + "px";
             }
         }, 20)
     }

</script>
</body>
</html>

```

## 3.左右焦点按钮控件

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>左右焦点轮播图</title>
</head>
<style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    ul,li,ol{
        list-style: none;
    }
    img{
        vertical-align: top;
    }
    .box{
        width: 768px;
        height: 287px;
        padding: 5px;
        margin: 100px auto;
        position: relative;
        border: 1px solid #cccccc;
    }
    .inner{
        width: 768px;
        height: 287px;
        position: relative;
        overflow: hidden;
    }
    .inner ul{
        width: 1000%;
        position: absolute;
        top: 0;
        left: 0;
    }
    .inner ul li{
        float: left;
    }
    .inner img{
        width: 768px;
        height: 287px;
    }
    .box .slides{
        display: none;
    }
    .box .slides .slides-left{
        left: 5px;
    }
    .box .slides .slides-right{
        right: 5px;
    }
    .slides .slides-left, .slides .slides-right{
        position: absolute;
        top: 50%;
        margin-top: -34px;
        width: 41px;
        height: 69px;
        z-index: 10;
    }
    .slides-left{
        background: url("img/icon-slides.png") no-repeat -84px 50%;
    }
    .slides-right{
        background: url("img/icon-slides.png") no-repeat -125px 50%;
    }
    .slides .slides-left:hover {
        background: url("img/icon-slides.png") no-repeat 0px 50%;
    }
    .slides .slides-right:hover {
        background: url("img/icon-slides.png") no-repeat -41px 50%;
    }
</style>
<body>
<div id="box" class="box">
    <div class="inner">
        <ul>
            <li><a href=""><img src="img/banner/banner1.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner2.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner3.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner4.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner5.jpg" alt="" ></a></li>
        </ul>
    </div><!--相框-->
    <!--左右焦点按钮-->
    <div class="slides">
        <a href="javascript:void(0)" class="slides-left" id="upper"></a>
        <a href="javascript:void(0)" class="slides-right" id="next"></a>
    </div>
</div>

<script type="text/javascript">
    //获取盒子
    let box = document.getElementById("box");
    //获取相框
    let inner = box.children[0];
    //获取相框宽度
    let imgWidth = inner.offsetWidth;
    //获取ul
    let ulObj = inner.children[0];
    //获取左右焦点按钮
    let slides = document.getElementsByClassName("slides"); //class获取的是一个数组
    //左右焦点的显示和隐藏
    let upper = document.getElementById("upper");
    let next = document.getElementById("next");

    box.onmouseover = function () { //鼠标移入事件
        slides[0].style.display="block";

    }

    box.onmouseout = function () { //鼠标移出事件
        slides[0].style.display="none";
    }
    let index = 0;
    //右焦点事件
    next.onclick = function () {
        if(index < ulObj.children.length-1){
            index++;
            animate(ulObj,-index*imgWidth);
        }
    }

    //左焦点事件
    upper.onclick = function () {
        if(index>0){
            index--;
            animate(ulObj,-index*imgWidth);
        }
    }


    function animate(element, target) {
        clearInterval(element.timeID);//先清理定时器

        element.timeID= setInterval(function () {
            let current = element.offsetLeft;//获取当前位置
            let move = 10;
            move = current < target ? move : -move;
            current += move; //每次加10px
            if (Math.abs(target - current) > Math.abs(move)) { //如果 当前位小于400
                element.style.left = current + "px";
            } else {
                clearInterval(element.timeID);
                element.style.left = target + "px";
            }
        }, 20)
    }
</script>
</body>
</html>

```

## 4.自动切换及无缝切换案例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>无缝轮播切换</title>
</head>
<style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    ul,li,ol{
        list-style: none;
    }
    img{
        vertical-align: top;
    }
    .box{
        width: 768px;
        height: 287px;
        padding: 5px;
        margin: 100px auto;
        position: relative;
        border: 1px solid #cccccc;
    }
    .inner{
        width: 768px;
        height: 287px;
        position: relative;
        overflow: hidden;
    }
    .inner ul{
        width: 1000%;
        position: absolute;
        top: 0;
        left: 0;
    }
    .inner ul li{
        float: left;
    }
    .inner img{
        width: 768px;
        height: 287px;
    }
    .box .slides{
        display: none;
    }
    .box .slides .slides-left{
        left: 5px;
    }
    .box .slides .slides-right{
        right: 5px;
    }
    .slides .slides-left, .slides .slides-right{
        position: absolute;
        top: 50%;
        margin-top: -34px;
        width: 41px;
        height: 69px;
        z-index: 10;
    }
</style>
<body>
<div id="box" class="box">
    <div class="inner">
        <ul>
            <li><a href=""><img src="img/banner/banner1.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner2.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner3.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner4.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner5.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner1.jpg" alt="" ></a></li>
        </ul>
    </div><!--相框-->
</div>

<script type="text/javascript">

    let box = document.getElementById("box");
    let inner = box.children[0];
    let ulObj = inner.children[0];
    let current = 0;
   let timeID =   setInterval(move,20);


    function  move() { //无缝切换
        current-=10;
        if(current< -3840){
            ulObj.style.left = 0 +"px";
            current=0;
        }else{
            ulObj.style.left = current +"px";
        }
    }

    //鼠标移入
    box.onmouseover = function () {
        clearInterval(timeID);
    }
    //鼠标移出
    box.onmouseout = function () {
       timeID =   setInterval(move,20);
    }

    function animate(element, target) {
        clearInterval(element.timeID);//先清理定时器

        element.timeID= setInterval(function () {
            let current = element.offsetLeft;//获取当前位置
            let move = 10;
            move = current < target ? move : -move;
            current += move; //每次加10px
            if (Math.abs(target - current) > Math.abs(move)) { //如果 当前位小于400
                element.style.left = current + "px";
            } else {
                clearInterval(element.timeID);
                element.style.left = target + "px";
            }
        }, 20)
    }
</script>
</body>
</html>

```

## 5.完整案例整合

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>完整轮播案例</title>
</head>
<style type="text/css">
    * {
        margin: 0;
        padding: 0;
    }
    ul,li,ol{
        list-style: none;
    }
    img{
        vertical-align: top;
    }
    .box{
        width: 768px;
        height: 287px;
        padding: 5px;
        margin: 100px auto;
        position: relative;
        border: 1px solid #cccccc;
    }
    .inner{
        width: 768px;
        height: 287px;
        position: relative;
        overflow: hidden;
    }
    .inner ul{
        width: 1000%;
        position: absolute;
        top: 0;
        left: 0;
    }
    .inner ul li{
        float: left;
    }
    .inner img{
        width: 768px;
        height: 287px;
    }
    .box .slides{
        display: none;
    }
    .box .slides .slides-left{
        left: 5px;
    }
    .box .slides .slides-right{
        right: 5px;
    }
    .slides .slides-left, .slides .slides-right{
        position: absolute;
        top: 50%;
        margin-top: -34px;
        width: 41px;
        height: 69px;
        z-index: 10;
    }
    .slides-left{
        background: url("img/icon-slides.png") no-repeat -84px 50%;
    }
    .slides-right{
        background: url("img/icon-slides.png") no-repeat -125px 50%;
    }
    .slides .slides-left:hover {
        background: url("img/icon-slides.png") no-repeat 0px 50%;
    }
    .slides .slides-right:hover {
        background: url("img/icon-slides.png") no-repeat -41px 50%;
    }
    .square{
        position: absolute;
        top: 90%;
        width: 250px;
        height: 20px;
        right: 0;
    }
    .square  li{
        width: 28px;
        height: 18px;
        float: left;
        margin-right: 20px;
        background-color: rgba(128,128,128,0.8);
        text-align: center;
        font-size: 14px;
        cursor:pointer;
        border: 1px solid #000000;
    }
    .square  .current{
        color: white;
        cursor: pointer;
        background-color: rgba(248,128,8,1);
    }
</style>
<body>
<div id="box" class="box">
    <div class="inner">
        <ul>
            <li><a href=""><img src="img/banner/banner1.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner2.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner3.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner4.jpg" alt="" ></a></li>
            <li><a href=""><img src="img/banner/banner5.jpg" alt="" ></a></li>
        </ul>
        <ol class="square">

        </ol>
    </div><!--相框-->
        <!--左右焦点按钮-->
        <div id="arr" class="slides">
            <a href="javascript:void(0)" class="slides-left" id="upper"></a>
            <a href="javascript:void(0)" class="slides-right" id="next"></a>
        </div>
</div>

<script type="text/javascript">
    //box
    let box = document.getElementById("box");
    //inner 相框
    let inner = box.children[0];
    //获取相框宽度
    let imgWidth = inner.offsetWidth;
    //获取ul
    let ulObj = inner.children[0];
    //获取ul中所有的li
    let list = ulObj.children;
    //获取ol
    let olObj = inner.children[1];
    //获取焦点 div
    let arr = document.getElementById("arr");

    //左焦点
    let upper = document.getElementById("upper");
    //右焦点
    let next = document.getElementById("next");
    //记录图片所在位置
    let index = 0;


    /*
    * 小按钮轮播功能
    * */
    //创建小按钮
    for (let i=0; i<list.length;i++){
        let liObj = document.createElement("li");
        olObj.appendChild(liObj);
        //设置索引值
        liObj.setAttribute("index",i);
        liObj.innerHTML=(i+1);

        //注册鼠标移入事件
        liObj.onmouseover = function (){
            //移除olObj上的所有class;
            for(let j=0; j<olObj.children.length;j++){
                olObj.children[j].removeAttribute("class");
            }
            //设置当前属性class
            this.className ="current";
            //获取当前的索引值
            index = this.getAttribute("index");
            animate(ulObj,-index*imgWidth)
        }
    }
    olObj.children[0].className = "current";
    //克隆ul中的第一个li追加到最后
    ulObj.appendChild(ulObj.children[0].cloneNode(true));

/***********************分割线************************************/
    /*
   * 自动播放
   * */
    let timeID = setInterval(clickHandle,3000);



    //鼠标移入显示左右焦点按钮
    box.onmouseover = function () {
        arr.style.display = "block";
        clearInterval(timeID);
    }

    //鼠标移入隐藏左右焦点按钮
    box.onmouseout = function () {
        arr.style.display = "none";
        timeID = setInterval(clickHandle,3000);
    }
    //右焦点点击事件
    next.onclick = clickHandle;
    function  clickHandle () {
        if(index==list.length-1){ //当索引值等于5时 ，显示的是第一张图片;
            index=0;
            ulObj.style.left = 0 +"px";
        }
        index++;
        animate(ulObj,-index*imgWidth);

        //index =5 时 此时图片显示的是第6张
        if(index==list.length-1){
            olObj.children[olObj.children.length-1].className="";//将最后一个的class清除
            olObj.children[0].className="current";//设置ol——li的第一个的class
        }else{
            for(let i=0; i<olObj.children.length;i++){
                //移除其他li上的样式
                olObj.children[i].removeAttribute("class");
            }
            //设置当前的class 形成联动
            olObj.children[index].className="current";
        }
    }

    //左焦点点击事件
    upper.onclick = function () {

        if(index == 0){ //当 当前图片就是第一个时候; 点击向左,直接跳第5张图片的位置
            index = 5;
            ulObj.style.left = -index*imgWidth +"px";
        }
        index--;
        animate(ulObj,-index*imgWidth);

        //同步小按钮颜色
        for(let i=0; i<olObj.children.length;i++){
            //移除其他li上的样式
            olObj.children[i].removeAttribute("class");
        }
        olObj.children[index].className="current";
    }


    /**
    * 动画函数
    * */
    function animate(element, target) {
        clearInterval(element.timeID);//先清理定时器

        element.timeID= setInterval(function () {
            let current = element.offsetLeft;//获取当前位置
            let move = 10;
            move = current < target ? move : -move;
            current += move; //每次加10px
            if (Math.abs(target - current) > Math.abs(move)) { //如果 当前位小于400
                element.style.left = current + "px";
            } else {
                clearInterval(element.timeID);
                element.style.left = target + "px";
            }
        }, 10)
    }
</script>
</body>
</html>

```

