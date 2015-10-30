# canvasTest
<head>
	<style type="text/css">
		p{text-indent:2em;}
	</style>
</head>
<h1>本文为实现canvas刮刮卡特效的一个心得笔记</h1>

<h2>首先从绘图开始，canvas能根据图片资源的src，绘制出canvas的图片</h2>
定义和用法<br/>
drawImage() 方法在画布上绘制图像、画布或视频。<br/>
drawImage() 方法也能够绘制图像的某些部分，以及/或者增加或减少图像的尺寸。<br/>
JavaScript 语法 1<br/>
在画布上定位图像：<br/>
context.drawImage(img,x,y);<br/>
JavaScript 语法 2<br/>
在画布上定位图像，并规定图像的宽度和高度：<br/>
context.drawImage(img,x,y,width,height);<br/>
JavaScript 语法 3<br/>
剪切图像，并在画布上定位被剪切的部分：<br/>
context.drawImage(img,sx,sy,swidth,sheight,x,y,width,height);<br/>
参数值<br/>
参数	描述<br/>
img	规定要使用的图像、画布或视频。<br/>
sx	可选。开始剪切的 x 坐标位置。<br/>
sy	可选。开始剪切的 y 坐标位置。<br/>
swidth	可选。被剪切图像的宽度。<br/>
sheight	可选。被剪切图像的高度。<br/>
x	在画布上放置图像的 x 坐标位置。<br/>
y	在画布上放置图像的 y 坐标位置。<br/>
width	可选。要使用的图像的宽度。（伸展或缩小图像）<br/>
height	可选。要使用的图像的高度。（伸展或缩小图像）<br/>
<h2>通过drawImage绘制出理想的遮罩之后接下来是对抠图效果的实现</h2>
通过 canvas.getContext("2d").strokeStyle  设置canvas绘线时线条的颜色。<br/>

定义和用法<br/>
strokeStyle 属性设置或返回用于笔触的颜色、渐变或模式。<br/>
默认值：	#000000<br/>
JavaScript 语法：	context.strokeStyle=color|gradient|pattern;<br/>

这位看官问了！刮刮卡不是要刮掉遮罩么？！你弄个有色线条搞毛啊！？<br/>

问到重点了！我们刮卡时就是对手指活动路径进行的绘制，那这是怎么回事呢？<br/>

接下来是重量级嘉宾<br/>

<h2>globalCompositeOperation</h2>

定义和用法<br/>
globalCompositeOperation 属性设置或返回如何将一个源（新的）图像绘制到目标（已有）的图像上。<br/>
源图像 = 您打算放置到画布上的绘图。<br/>
目标图像 = 您已经放置在画布上的绘图。<br/>
默认值：	source-over<br/>
JavaScript 语法：	context.globalCompositeOperation="source-in";<br/>
属性值<br/>
值	描述<br/>
source-over	默认。在目标图像上显示源图像。<br/>
source-atop	在目标图像顶部显示源图像。源图像位于目标图像之外的部分是不可见的。<br/>
source-in	在目标图像中显示源图像。只有目标图像内的源图像部分会显示，目标图像是透明的。<br/>
source-out	在目标图像之外显示源图像。只会显示目标图像之外源图像部分，目标图像是透明的。<br/>
destination-over	在源图像上方显示目标图像。<br/>
destination-atop	在源图像顶部显示目标图像。源图像之外的目标图像部分不会被显示。<br/>
destination-in	在源图像中显示目标图像。只有源图像内的目标图像部分会被显示，源图像是透明的。<br/>
destination-out	在源图像外显示目标图像。只有源图像外的目标图像部分会被显示，源图像是透明的。<br/>
lighter	显示源图像 + 目标图像。<br/>
copy	显示源图像。忽略目标图像。<br/>
source-over	使用异或操作对源图像与目标图像进行组合。<br/>
具体效果如下：http://www.w3school.com.cn/tiy/t.asp?f=html5_canvas_globalcompop_all<br/>
<img src="http://dl.iteye.com/upload/picture/pic/71054/9f086ab0-932c-3195-884b-cfc8aa3d8ce4.jpg">

其中destination-out就是今天实现刮卡效果的主要功臣，结合lineWidth设置绘制透明线的宽度，就可以实现我们手指宽度刮卡的效果了。<br/>

然后呢？？？<br/>

<code>ctx.beginPath();<br/>
ctx.moveTo(10,10)<br/>
ctx.lineTo(100,100);<br/>
ctx.stroke()<br/></code>

beginPath() 方法在一个画布中开始子路径的一个新的集合<br/>

moveTo(10,10) lineTo(100,100)开始一条路径，移动到位置 10,10。创建到达位置 100,100 的一条线<br/>

最后的最后<br/>
stroke() 方法会实际地绘制出通过 moveTo() 和 lineTo() 方法定义的路径<br/>
实现我们刮卡的路径，不然一切都是然并卵的<br/>
以上就构成了我们刮卡效果特效的理论基础，然后我们就可以开始开工了<br/>
