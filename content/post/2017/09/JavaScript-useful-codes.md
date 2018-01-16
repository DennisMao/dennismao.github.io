+++
title = "JavaScript常用小功能收集"
date = 2017-08-29T00:07:13+08:00
draft = true
slug = ""
categories = ["前端"]
tags = ["JavaScript"]
toc = true
+++

```go
var i int64
func main(){
 i = 2 //XXXAA
}
```

### 1．让文字不停地滚动
```
＜MARQUEE＞滚动文字＜/MARQUEE＞
```
### 2．记录并显示网页的最后修改时间
```html
＜script language=JavaScript＞
　　document.write("最后更新时间: " + document.lastModified + "")
　　＜/script＞
```  
### 3．关闭当前窗口
```html
＜a href="/"onClick="javascript:window.close();return false;"＞关闭窗口＜/a＞
```
### 4．5秒后关闭当前页
```html
　　＜script language="Javascript"＞
　　＜!--
　　setTimeout('window.close();',5000);
　　--＞
　　＜/script＞
```  
### 5．2秒后载入指定网页
```html
＜head＞
　　＜meta http-equiv="refresh" content="2;URL=http://你的网址"＞
　　＜/head＞
```  
### 6．添加到收藏夹
```html
＜script Language="Javascript"＞
　　function bookmarkit()
　　{
　　window.external.addFavorite('http://你的网址','你的网站名称')
　　}
　　if (document.all)document.write('＜a href="#" onClick="bookmarkit()"＞加入收藏夹＜/a＞')
　　＜/script＞
```  
### 7．让超链接不显示下划线
```html
＜style type="text/css"＞
　　＜!-
　　a:link{text-decoration:none}
　　a:hover{text-decoration:none}
　　a:visited{text-decoration:none}
　　-＞
　　＜/style＞
```  
### 8．禁止鼠标右键的动作
```html
＜script Language = "Javascript"＞
　　function click() { if (event.button==2||event.button==3)
　　{
　　alert('禁止鼠标右键');
　　}
　　document.onmousedown=click // --＞
　　＜/script＞
```
### 9．设置该页为首页
```html
＜body bgcolor="#FFFFFF" text="#000000"＞
　　＜!-- 网址：http://你的网址--＞
　　＜a class="chlnk" style="cursor:hand" HREF
　　onClick="this.style.behavior='url(#default#homepage)';
　　this.setHomePage('你的网站名称);"＞＜font color="000000" size="2" face="宋体"＞设为首页＜/font＞＜/a＞
　　＜/body＞
```
### 10．节日倒计时
```html
　　＜script Language="Javascript"＞
　　var timedate= new Date("December 25,2003");
　　var times="圣诞节";
　　var now = new Date();
　　var date = timedate.getTime() - now.getTime();
　　var time = Math.floor(date / (1000 * 60 * 60 * 24));
　　if (time ＞= 0)
　　document.write("现在离"+times+"还有: "+time +"天")＜/script＞
```
### 11．单击按钮打印出当前页
```html
　　＜script Language="Javascript"＞
　　＜!-- Begin
　　if (window.print) {
　　document.write('＜form＞'
　　+ '＜input type=button name=print value="打印本页" '
　　+ 'onClick="javascript:window.print()"＞＜/form＞');
　　}
　　// End --＞
　　＜/script＞
```  
### 12．单击按钮‘另存为’当前页
```html
　　＜input type="button" name="Button" value="保存本页"
　　onClick="document.all.button.ExecWB(4,1)"＞
　　＜object id="button"
　　width=0
　　height=0
　　classid="CLSID:8856F961-340A-11D0-A96B-00C04FD705A2"＞
　　＜embed width="0" height="0"＞＜/embed＞
　　＜/object＞
```  
### 13．显示系统当前日期
```html
　　＜script language=Javascript＞
　　today=new Date();
　　function date(){
　　this.length=date.arguments.length
　　for(var i=0;i＜this.length;i++)
　　this[i+1]=date.arguments }
　　var d=new date("星期日","星期一","星期二","星期三","星期四","星期五","星期六");
　　document.write(
　　"＜font color=##000000 style='font-size:9pt;font-family: 宋体'＞ ",
　　today.getYear(),"年",today.getMonth()+1,"月",today.getDate(),"日",
　　d[today.getDay()+1],"＜/font＞" );
　　＜/script＞
```  
### 14．不同时间段显示不同问候语
```html
　　＜script Language="Javascript"＞
　　＜!--
　　var text=""; day = new Date( ); time = day.getHours( );
　　if (( time＞=0) && (time ＜ 7 ))
　　　　text="夜猫子，要注意身体哦！ "
　　if (( time ＞= 7 ) && (time ＜ 12))
　　　　text="今天天气……哈哈哈，不去玩吗？"
　　if (( time ＞= 12) && (time ＜ 14))
　　　　text="午休时间哦，朋友一定是不习惯午睡的吧？！"
　　if (( time ＞=14) && (time ＜ 18))
　　　　text="下午茶的时间到了，休息一下吧！ "
　　if ((time ＞= 18) && (time ＜= 22))
　　　　text="您又来了，可别和MM聊太久哦！"
　　if ((time ＞= 22) && (time ＜ 24))
　　　　text="很晚了哦，注意休息呀！"
　　document.write(text)
　　//---＞
　　＜/script＞
```  
### 15．水中倒影效果
```html
　　＜img id="reflect" src="你自己的图片文件名" width="175" height="59"＞
　　＜script language="Javascript"＞
　　function f1()
　　{
　　　　setInterval("mdiv.filters.wave.phase+=10",100);
　　}
　　if (document.all)
　　{
　　　　document.write('＜img id=mdiv src="'+document.all.reflect.src+'"
　　　　style="filter:wave(strength=3,freq=3,phase=0,lightstrength=30) blur() flipv()"＞')
　　　　window.onload=f1
　　}
　　＜/script＞
```  
### 16．慢慢变大的窗口
```js
　　var Windowsheight=100
　　var Windowswidth=100
　　var numx=5
　　function openwindow(thelocation){
　　temploc=thelocation
　　if
　　(!(window.resizeTo&&document.all)&&!(window.resizeTo&&document.getElementByIdx_x))
　　{
　　　　window.open(thelocation)
　　　　return
　　}
　　windowsize=window.open("","","scrollbars")
　　windowsize.moveTo(0,0)
　　windowsize.resizeTo(100,100)
　　tenumxt()
　　}
　　function tenumxt(){
　　if (Windowsheight＞=screen.availHeight-3)
　　　　numx=0
　　windowsize.resizeBy(5,numx)
　　Windowsheight+=5
　　Windowswidth+=5
　　if (Windowswidth＞=screen.width-5)
　　{
　　　　windowsize.location=temploc
　　　　Windowsheight=100
　　　　Windowswidth=100
　　　　numx=5
　　　　return
　　}
　　setTimeout("tenumxt()",50)
　　}
```
```html
＜p＞＜a href="javascript:openwindow(https://dennismao.github.io)"＞进入＜/a＞
  ```
### 17．改变IE地址栏的IE图标
```html
　　//我们要先做一个16*16的icon（图标文件），保存为index.ico。把这个图标文件上传到根目录下并在首页＜head＞＜/head＞之间加上如下代码：
　　＜link REL = "Shortcut Icon" href="index.ico"＞
  ```
### 18．在窗口的状态栏显示滚动信息 
```js
//(1) 在BODY中加入代码 
<script language="javascript">  
var msg="欢迎访问建站资源网，在这里有你会有所收获的!";  
var i=1  
function scroll()  
{  
mess=msg.substring(i,msg.length)+" "+msg.substring(0,i)  
window.status=mess  
i++;  
if (i>=msg.length) i=1; //设置不停滚动 
setTimeout("scroll()",200); //设置滚动速度 
}  
</script>  
```
```
//(2)在BODY标签中:  
<body onload="scroll()"> 
```
### 19、在页面加入当前时间 
```html
<script language="javascript">  
tdy=new Date();  
document.write("当前时间:",tdy.getHours());  
document.write(":",tdy.getMinutes());  
document.write(":",tdy.getSeconds());  
</script>  
```
### 21、加入页面最后修改日期  
```html
<script language="javascript">  
document.write("本页最后编辑日期：");  
document.write(document.lastModified)  
</script>  
```
### 22、前进、后退按钮 
```html
<font onclick="history.go(-1)"> 前一页</font>  
<font onclick="history.go(-2)"> 前两页</font>  
<font onclick="history.go(-3)"> 前三页</font>  
<font onclick="history.go(1)"> 后一页</font>  
<font onclick="history.go(2)"> 后两页</font>  
<font onclick="history.go(3)"> 后三页</font>  
也可设置退后、前进多步 
```
### 23、鼠标事件 
```html
<A HREF="MAILTO:raymond_2008@yahoo.com" onmouseover="alert("给我写信"); return true">点击发送邮件</A>  
```
### 24、获得浏览器的属性 
```html
navigator.appCodename=undefinednavigator.appName=Microsoft Internet Explorernavigator.appVersion=4.0 (compatible; MSIE 5.0; Windows 98; DigExt)navigator.appAgent=undefined  
```
### 25、打印整个页面 
```html
<font onClick="javascript:window.print()">打印本页</font> 
```
### 26、查看源码 
```html
<input TYPE="button" NAME="view" value="查看本页的源码" onClick="window.location="view-source:" +window.location.href" class="pt9"> 
```
### 27、刷新页面 
```html
<font onClick="history.go(0)">刷新本页</font> 
```
### 28、背景色变换 
```html
<input TYPE="button" value="背景色变换" onClick="BgButton()"> 
<script>function BgButton() 
{ 
if (document.bgColor==#00ffff)  
{ 
document.bgColor=#ffffff; 
} 
else{document.bgColor=#00ffff; 
} 
} 
</script>
```
### 29、Title上显示信息 
```html
<script language="javascript1.2"> 
document.title="今天是星期天" 
</script>
```

