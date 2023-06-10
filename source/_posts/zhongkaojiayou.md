---
abbrlink: ''
categories:
- - 日常
date: '2023-06-10T21:04:10.403259+08:00'
tags:
- 日常
- 中考
title: 中考加油
updated: 2023-6-10T21:7:53.418+8:0
---
# 中考加油！

## 来自各处的祝福

![2](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/2.2mo1wbmmjjy0.jpg)

![3](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/3.ph3z010l8m8.jpg)

![6](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/6.1ehnu3zldo00.jpg)

![7](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/7.7dz8wbdmztk0.jpg)

![8](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/8.zxmx5id2as0.jpg)

![5](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/5.1ln58k58dvb4.jpg)

![4](https://cdn.staticaly.com/gh/histcat/static@master/rawimg/4.byehzihdgdc.jpg)

## 倒数日

<div id="h1"></h1>

<script>

window.onload=function starttime(){
time(h1,'2023/6/13');
ptimer = setTimeout(starttime,1000); // 添加计时
}
function time(obj,futimg){
var nowtime = new Date().getTime(); // 现在时间转换为时间戳
var futruetime =  new Date(futimg).getTime(); // 未来时间转换为时间戳
var msec = futruetime-nowtime; // 毫秒 未来时间-现在时
var time = (msec/1000);  // 毫秒/1000
var day = parseInt(time/86400); // 天  24*60*60*1000
var hour = parseInt(time/3600)-24*day;    // 小时 60*60 总小时数-过去的小时数=现在的小时数
var minute = parseInt(time%3600/60); // 分 -(day*24) 以60秒为一整份 取余 剩下秒数 秒数/60 就是分钟数
var second = parseInt(time%60);  // 以60秒为一整份 取余 剩下秒数
obj.innerHTML="<br>距中考还有：<br style="color:red">"+day+"天"+hour+"小时"+minute+"分"+second+"秒"+"<br><span>加油！</span>"
}

</script>

