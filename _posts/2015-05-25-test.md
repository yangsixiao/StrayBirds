---
layout: post
title: jq写的碰撞游戏
category: 技术
comments: true
---


## 用jq写的小游戏

* 要点

鼠标跟随，碰撞计算

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
	<meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
	<title></title>
	<script type="text/javascript" src = "jquery-1.10.2.js"></script>
	<style type="text/css">
	body{
		position:relative;
		cursor:pointer;
	}
	.one{
		width:50px;
		height:50px;
		position:absolute;
		left:0px;
		top:0px;
		border-radius: 25px; 
		-moz-border-radius: 25px; /* Mozilla浏览器的私有属性 */
		-webkit-border-radius: 25px; /* Webkit浏览器的私有属性 */
		border-radius: 25px 25px 25px 25px; 
	}
	.style{
		display:none;
	}
	.style2{
		font-size:16px;color:#f44;
	}
	.span{
		color:#ad8;
		font-size:300px;
		position:absolute;
		top:220px;
		left:125px;
	}
	</style>
</head>
<body>
	<div id = "one" class = "one"></div>
	<div id = "two" class = "one"></div>
	<p id="show" class="style style2"></p>
	<script type="text/javascript">
	/*
	*game:两个小球碰撞
	*author:jax
	*/
	s = true;
	var num = 100;
	function showColor(){
		colorRun = setInterval(function(){
			if(s == true){
				num++;
			}else{
				num--;
			}
			if(num == 100 || num == 999){
				s = !s;
			}
			one.style.background = "#"+num+"";
			two.style.background = "#"+num+"";
		 }
	 ,24)
	}
	showColor();
	//闪烁事件
	function beginNum(a,b){
		setTimeout(function(){
			$('#app').css({fontSize:"220px",opacity:1}).text(a);
			$("#app").animate({
				fontSize: "320px",
				opacity:0,
			}, 400 );
		},b)
	}
		$('#show').after('<span id="app" class="span">3</span>');
		beginNum('2',800);
		beginNum('1',1600);
		beginNum('GO!',2400);
	//鼠标跟随事件
	$(document).mousemove(function(e){
		var x = parseInt($("#one").width())/2;
		var y = parseInt($("#one").height())/2;
		$("#one").css({left:e.pageX-x+'px',top:e.pageY-y+'px'});
		touchCheck();
	});
	var count = 0;
	$("#one").click(function(){
		if(count%2 == 0){
			$(this).animate({opacity:0.2},0);
		}else{
			$(this).animate({opacity:1},0);			
		}
		count++;
	})
	//碰撞函数
	function touchCheck() {
		//得到块一的高度和宽度以及（距离左侧和上端的距离）
		var oneOffset = $("#one").offset();
		var oneWidth = parseInt($("#one").width());
		var oneHeight = parseInt($("#one").height());
		//得到块二的高度和宽度以及（距离左侧和上端的距离）
		var twoOffset = $("#two").offset();
		var twoWidth = parseInt($("#two").width());
		var twoHeight = parseInt($("#two").height());
		//左侧和上方相交的bool值
		var leftBool;
		var topBool;
		//处理左侧相交
		if (oneOffset.left > twoOffset.left) {
			leftBool = oneOffset.left - twoOffset.left - twoWidth < 0;
		} else {
			leftBool = twoOffset.left - oneOffset.left - oneWidth < 0;
		}
		//处理上方相交
		if (oneOffset.top > twoOffset.top) {
			topBool = oneOffset.top - twoOffset.top - twoHeight < 0;
		} else {
			topBool = twoOffset.top - oneOffset.top - oneHeight < 0;
		}
		//上方和左侧共同相交则代表碰撞
		if (leftBool && topBool) {
			//碰到则增加长宽
			$("#one").animate({width:$("#one").width()+15+"px",height:$("#one").height()+15+"px"},1);
			if($("#one").width()>2000 && $("#one").width()<2080){
				alert("你赢了！");
			}else if($("#one").width()>2080){
				alert("请刷新！");
			}
			beginNum('GOOD!',0);
			num1 = Math.round(Math.random()*1920);
			num2 = Math.round(Math.random()*654);
			$("#two").css({left:num1+"px",top:num2+"px"});
			//防止超出屏幕
			if($("#two").position().top>$(window).height() || $("#two").position().left>$(window).width()){
				$("#two").css({left:"10px",top:"10px"});
			}
		}

        }

	</script>
</body>
</html>
```

一个简单的碰撞的游戏，需要引入jq库
