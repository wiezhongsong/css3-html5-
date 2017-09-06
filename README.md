# css3-html5-

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0" />
<script src="jquery.min.js"></script>
<script src="rotaryDraw.js"></script>
<title>只要指针转jQuery手机端转盘抽奖插件</title>
<style>
*{padding:0;margin:0;}
body{
	background:#f03200;
}
.box{
	position:relative;
	max-width:750px;
	margin:0 auto;
}
.box .img01{
	width:100%;
}
.box .drawBtn{
	position:absolute;
	width:10rem;
	height:auto;
	left:50%;
	margin-left:-5rem;
}

</style>
</head>
<body>
<div class="box">
	<img src="images/img_01.png" class="img01" alt="">
	<img src="images/img_03.png" class="drawBtn">
</div>
<script>
$(".box .img01").load(function() {
	var obj = $(".drawBtn");
	var hei = $(this).height();
	var hei2 = obj.height();
	obj.css("top",(hei - hei2)/2);
});

</script>
<script>
//share份额[数字没有默认],
//speed速度[单位s,最小0.1s],
//velocityCurve速度曲线[linear匀速，ease慢快慢，ease-in慢慢开始，ease-out慢慢结束，ease-in-out慢快慢等，用的是css3的速度曲线],可以不写，ease默认值；
//callback回调函数
//weeks几周[默认2周，可以不写]

//几份和回调函数这两个参数是必填


function callbackA(ind)
{
	alert("第一个回调"+ind);
};
function callbackB(ind)
{
	alert("第二个回调"+ind);
};
var newdraw =new turntableDraw('.drawBtn',{
	share:8,
	speed:"3s",
	velocityCurve:"ease",
	weeks:6,
	callback:function(num)
	{
		callbackA(num);
	},
});
var newdraw2 =new turntableDraw('.drawBtn2',{
	share:12,
	speed:"3s",
	velocityCurve:"ease",
	weeks:6,
	callback:function(num)
	{
		callbackB(num);
	},
});
$(".drawBtn").click(function(event) {
	//ajax
	newdraw.goto(parseInt(Math.random()*8)+1);
});
$(".drawBtn2").click(function(event) {
	//ajax
	newdraw2.goto(parseInt(Math.random()*12)+1);
});
</script>

 
</body>
</html>




//下面是js调用的方法
function turntableDraw(obj,jsn)
{
	"use strict";
	this.draw = {};
	this.draw.obj = $(obj);
	this.draw.objClass = $(obj).attr("class");
	this.draw.newClass = "rotary"+"new"+parseInt(Math.random()*1000);
	var _jiaodu = parseInt(360/jsn.share);
	var _yuan = 360*(jsn.weeks || 4);
	var _str = "";
	var _speed = jsn.speed || "2s";
	var _velocityCurve = jsn.velocityCurve || "ease";
	var _this = this;
	for(var i=1;i<=jsn.share;i++)
	{
		_str+="."+this.draw.newClass+i+"{";
		_str+="transform:rotate("+((i-1)*_jiaodu+_yuan)+"deg);";
		_str+="-ms-transform:rotate("+((i-1)*_jiaodu+_yuan)+"deg);";
		_str+="-moz-transform:rotate("+((i-1)*_jiaodu+_yuan)+"deg);";
		_str+="-webkit-transform:rotate("+((i-1)*_jiaodu+_yuan)+"deg);";
		_str+="-o-transform:rotate("+((i-1)*_jiaodu+_yuan)+"deg);";
		_str+="transition: transform "+_speed+" "+_velocityCurve+";";
		_str+="-moz-transition: -moz-transform "+_speed+" "+_velocityCurve+";";
		_str+="-webkit-transition: -webkit-transform "+_speed+" "+_velocityCurve+";";
		_str+="-o-transition: -o-transform "+_speed+" "+_velocityCurve+";";
		_str+="}";
		_str+="."+this.draw.newClass+i+"stop{";
		_str+="transform:rotate("+((i-1)*_jiaodu)+"deg);";
		_str+="-ms-transform:rotate("+((i-1)*_jiaodu)+"deg);";
		_str+="-moz-transform:rotate("+((i-1)*_jiaodu)+"deg);";
		_str+="-webkit-transform:rotate("+((i-1)*_jiaodu)+"deg);";
		_str+="-o-transform:rotate("+((i-1)*_jiaodu)+"deg);";
		_str+="}";
	};
	$(document.head).append("<style>"+_str+"</style>");
	_speed = _speed.replace(/s/,"")*1000;
	this.draw.startTurningOk = false;
	this.draw.goto=function(ind){
		if(_this.draw.startTurningOk){return false};
		_this.draw.obj.attr("class",_this.draw.objClass+" "+_this.draw.newClass+ind);
		_this.draw.startTurningOk = true;
		setTimeout(function(){
			_this.draw.obj.attr("class",_this.draw.objClass+" "+_this.draw.newClass+ind+"stop");
			if(jsn.callback)
			{
				_this.draw.startTurningOk = false;
				jsn.callback(ind);
			};
		},_speed+10);
		return _this.draw;
	};
	return this.draw;
};


