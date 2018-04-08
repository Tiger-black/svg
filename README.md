# svg是个好东西
Canvas 和 SVG 都允许您在浏览器中创建图形，但是它们在根本上是不同的。
	SVG
		SVG 是一种使用 XML 描述 2D 图形的语言。
		SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。
		在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。
	Canvas
		Canvas 通过 JavaScript 来绘制 2D 图形。
		Canvas 是逐像素进行渲染的。
		在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。
Canvas 与 SVG 的比较 下表列出了 canvas 与 SVG 之间的一些不同之处。
	Canvas
		依赖分辨率
		不支持事件处理器
		弱的文本渲染能力
		能够以 .png 或 .jpg 格式保存结果图像
		最适合图像密集型的游戏，其中的许多对象会被频繁重绘
	SVG
		不依赖分辨率
		支持事件处理器
		最适合带有大型渲染区域的应用程序（比如谷歌地图）
		复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
		不适合游戏应用

浏览器支持 IE9+ 、chrome 33.0+ 、Firefox 28.0+ 、Safari 7.0+
基本图形 6个
	<rect> <circle> <ellipse> <line> <polyline> <polygon>
	1、矩形 <rect> 
		x,y 纵横坐标值
		width,height 宽高
		rx,ry 圆角 如果只设置一个那么另一个会继承
	2、圆<circle> 
		cx,cy 圆心点的纵横坐标
		r,半径
	3、椭圆<ellipse> 
		cx,cy 圆心点的纵横坐标
		rx,ry 椭圆 纵横值 横坐标 纵坐标
	4、直线<line>
		x1 y1,x2 y2描述两个点 连起来就是一条直线
	5、折线<polyline>
		x1 y1,x2 y2,x3 y3,....描述n个点 连起来就是一条直线
		有一个points属性 以数组形式描述 points='x1 y1 x2 y2 x3 y3'
		同理可以绘制一个多边形 起点和终点相连
	6、多边形<polygon>
		x1 y1,x2 y2,x3 y3,....描述n个点  起点和终点相连 连起来就是一个多边形
		有一个points属性 以数组形式描述 points='x1 y1 x2 y2 x3 y3'
基本属性 4个 其实就是对图像样式的填充
	fill、 stroke、 stroke-width、 transform
	1、fill
		填充的颜色
	2、stroke
		描边的颜色
	3、stroke-width
		描边的粗细
	4、transform
		变形

基本操作API、
	创建图形的接口：
		document.createElementNS(ns,tagName)
		[SVG拥有独立的namespace，故使用js创建元素时使用createElementNS（ns,tagName）接口]
	添加图形的接口：
		element.appendChild(childElement)
	设置/获取属性：
		element.setAttribute(name,value)
		element.getAttribute(name)

坐标系统和坐标变换
	1、 世界 视野 视窗的概念
		世界是无穷大的，视野是观察世界的一个矩形区域
		视窗（能看到窗口）width，height控制大小 svg渲染的区域大小
		视野（拉远拉近）viewbox=‘0 0 400 400’控制大小
			viewbox设置0.5个像素的偏移会在整数坐标线更加锐利一点
		两者呈现方式：preserveAspectRatio="xMidYMid slice">
			前者参数为两者对齐方式 xMidYMid (x居中y居中)
			后者参数slice为视野包括视窗、meet视窗包括视野，
	2、 分组
		<g>标签用来创建分组
	    属性继承
	    transform属性定义坐标变换
	    可以嵌套使用
	3、 坐标系统
		svg中使用笛卡尔直角坐标系，为图形提供一个统一定位基准
		定义了一个原点和相互垂直的数轴
		与数学坐标不一样的地方是y轴向下 不是向上
		角度也变成了顺时针 从x的正方向到y的正方向
	4、 4个坐标系
		（1）User Coordinate--用户坐标系；(SVG标签的坐标系，世界坐标系，也被称为原始坐标系)
		（2）Current Corrdinate--自身坐标系；(图形绘制后自身携带的坐标系，用户自身宽高等定义均基于自身坐标系)
		（3）Previous Coordinate--前驱坐标系；(父容器的坐标系，经过元素属性transform进行变换的时候形成了图形的自身坐标系)
		（4）Reference Coordinate--参考坐标系；（主要用于定义自身坐标系和前驱坐标系的关系，对某个图形进行观察作用）
		元素自身没有transform变换的时候 自身坐标系和前驱坐标系重合
	5、 坐标变换
		原理 
			线性变换方程 还有看一下三角函数吧 有点忘了～。。。。。
			X' = aX + cY + e
			Y' = bX + dY + f
		transform属性
			前驱坐标系：父元素坐标系
			定义前驱坐标系到自身坐标系的线形变换
			多次变换中，第一次变换基于前驱坐标系，而后变换基于自身坐标系，属性写法的前后对位置会有影响，先进行的操作先生效比如先旋转在偏移和先偏移再旋转完全不一样
			语法
			‣ rotate(<deg>)* 旋转
			‣ translate(<x>,<y>)* 偏移
			‣ scale(<sx>,<sy>)* 缩放
			‣ matrix(<a>,<b>,<c>,<d>,<e>,<f>)*

颜色、渐变和笔刷
	1、认识rgb和hsl
		都是css3支撑的颜色
		颜色的hsl表示法
		h：色调，取值为0到359
		s：饱和度，百分比
		l：亮度，0%为黑色，100%为白色
		透明度可以用rgba hsla 或者opacity
		在svg中有fill和stroke属性应用到颜色
	2、线性渐变和径向渐变
		渐变让图形更加饱满
		线性渐变 linearGradient 和 stop、定义方向、关键点位置及颜色、gradientUnits
			默认是自身坐标系 gradientUnits = "userSpaceOnUse" 设置为世界坐标系
		径向渐变radialGradient 需要多定义一个参数 r 焦点位置
	3、使用笔刷
		1.pattern 笔刷。绘制纹理 笔刷用来定义一个可以在boundingbox上重复铺满的图案集合。
		2.patternUnits指定pattern在rect里的单位，patternContentUnits指定pattern的内容的单位是基于rect的
		patternUnits patternContentUnits 可取值 userSpaceOnUse objectBoundingBox 
			一个是指定pattern本身宽高的意义，一个是指定pattern里面的每个元素的宽高意义
		objectBoundingBox 模式下的比例均为相对于boundingbox。也就是不是根据父标签来定义比例。

path 强大的绘图工具 由命令及其参数组成的字符串
	<path d="M0,0L10,20C30-10,40,20,100,100" stroke="red">  这三个的含义一样
	<path d="M 0 0 L 10 20 C 30 -10 40 20 100 100" stroke="red"> 
	<path d="M 0 0, L 10 20, C 30 -10 40 20 100 100" stroke="red">
	区分大小写：大写表示坐标参数为绝对位置，小写为相对位置
	最终参数表示最终要到达的位置
	上一个命令结束的位置就是下一个命令开始的位置
	命令可以重复参数表示充分执行同一条命令
				
	1、移动和直线命令 m、l、h、v 使用相对位置绘制
		命令                      			含义							
		M/m (x,y)+							移动当前位置 移动画笔， 					后面如果有重复参数，会当做是 L 命令处理  重定义画笔起点
		L/l (x,y)+							从当前位置绘制线段到指定位置 				绘制直线到指定位置 
		H/h (x)+							从当前位置绘制 平线到达指定的x坐标 		绘制水平线到指定的 x 位置
		V/v (x)+							从当前位置绘制竖直线到达指定的y坐标 		绘制竖直线到指定的 y 位置 
	2、弧线命令
		A/a (rx,ry,xr,laf,sf,x,y)			从当前位置绘制弧线到指定位置 绘制弧线
			rx - (radius-x)弧线所在椭圆的 x 半轴长  
			ry - (radius-y)弧线所在椭圆的 y 半轴长  
			xr - (xAxis-rotation)弧线所在椭圆的长轴角度  
			laf - (large-arc-flag)是否选择弧长较长的那一段弧 		1大圈 0小圈
			sf - (sweep-flag)是否选择逆时针方向的那一段弧  	 	1顺时针 0逆时针
			x, y - 弧的终点位置
	3、贝塞尔曲线命令
		Q/q (x1,y1,x,y)+					从当前位置绘制两次 塞尔曲线到指定位置
		T/t (x,y)+							从当前位置光滑绘制两次塞尔曲线到指定位置
		C/c (x1,y1,x2,y2,x,y)+				从当前位置绘制三次贝塞尔曲线到指定位置 		
		S/s (x2,y2,x,y)+					从当前位置光滑绘制三次塞尔曲线到指定位置
			二次曲线 起始点 控制点  结束点
			三次曲线 起始点 控制点  控制点  结束点
			光滑贝塞尔曲线,上一段贝塞尔曲线的最后一个控制点的镜像,为下一段贝塞尔曲线的第一个控制点。若前一段曲线为直线,则控制点为起始点。

		Z/Z 								闭合当前路径


svg文本
	1、text和tspan标签
		x 和 y 属性 - 定位标准  
		dx 和 dy 属性 - 字形偏移  
		style 属性 - 设置样式
		text 中的dy 能被tspan 中的dy覆盖 dy具有向下传递的效果,text传递给下个字符,tspan传递给下个tspan(当无tspan 传递给字符)
	2、垂直居中问题
		text-anchor - 水平居中属性   start end middle
		dominant-baseline 属性  auto use-scrip no-change reset-size ideographc alphabetic hanging(下对齐)
			mathematical central middle text-before-edge text-after-edge text-bottom
		自己模拟
	3、textPath 路径文本
		使用方法 
			<text  dominant-baseline='alphabetic' font-size='14px'>
	            <textpath xlink:href='#www'>房里看见啊省</textpath>
	        </text>
		布局原理  基于每个文字的宽度找到曲线上的起始点&终点 算出中心点，做切线、法线对齐
		定位属性
			确认排列上的起始位置 
				x(路径上的偏移量), y（在路径文本上没有效果）,  
				startOffset(从路劲的哪里开始文本 百分比) 与text-anchor='end'属性一起使用由良好的控制位置的效果
			dx, dy（切线或法线上的偏移量）
			x、dx、startOffset 偏移基线&法线位置
		脚本控制
			setAttributeNS() 方法设置 xlink:href 属性  
			把文本节点替换为 <textpath> 节点
	4、超链接
		可以添加到任意的图形上  
		xlink:href 指定连接地址  
		xlink:title 指定连接提示  
		target 指定打开目标


图形的引用、裁切和蒙版
	超链接
		1. <use> 标签创建图形引用 
	  		例子:满天星星 
		2. <clipPath> 标签裁切图形 
	  		例子:绘制灯塔的光线 
		3. <mask> 标签创建蒙版 
	  		例子:绘制月牙及湖面倒影
动画
	动画原理  
		其实就是值关于时间的一个函数 通过一段的时长（duration）使值从form 到 to
	SMIL for SVG  
	一、五种动画元素<set>、<animate>、<animateColor>、<animateTransform>、<animateMotion>
		1、set （延迟几秒执行） 可以在特定时间之后修改某个属性值（也可以是CSS属性值
			//<set attributeName="x" attributeType="XML" to="60" begin="3s" />
		2、animate 基础动画元素。实现单属性的动画过渡效果
			//<animate attributeName="x" from="160" to="60" begin="0s" dur="3s" repeatCount="indefinite" />
		3、animateColor 颜色变化animate可以实现其功能与效果，因此，此属性已经被废弃。
		4、animateTransform 实现transform变换动画效果的。
			//<animateTransform attributeName="transform" begin="0s" dur="3s"  type="scale" from="1" to="1.5" repeatCount="indefinite"/>
		5、animateMotion 可以让SVG各种图形沿着特定的path路径运动  rotate="auto" 运动元素与路径平行
			//<animateMotion path="M10,80 q100,120 120,20 q140,-50 160,0" begin="0s" dur="3s" repeatCount="indefinite"/>
		6、自由组合  可以写多个动画元素 各干各的	 
	二、animation参数详解
		1、 attributeName ① 可以是元素直接暴露的属性 x、y  ② 可以是CSS属性 opacity
		2、 attributeType 默认auto 支持三个固定参数，CSS（css属性）/XML（svg属性）/auto（先当成CSS处理，如果发现不认识，直接XML类别处理）
		3、 from, to, by, values 你从哪里来？要到哪里去？
			from = '<value>' 动画的起始值。
			to = '<value>' 指定动画的结束值。
			by = '<value>' 动画的相对变化值。
			values = '<list>' 用分号分隔的一个或多个值，可以看出是动画的多个关键值点。
			a.  如果动画的起始值与元素的默认值是一样的，from参数可以省略。
			b. （不考虑values）to,by两个参数至少需要有一个出现。否则动画效果没有。to表示绝对值，by表示相对值。拿位移距离，如果from是100, to值为160则表示移动到160这个位置，但是，如果by值是160，则表示移动到100+160=260这个位置。
			c.  如果to,by同时出现，则by打酱油，只识别to.
			d.  如果to,by,values都没设置，自然没动画效果。如果任意（包括from）一个属性的值不合法，规范上说是没有动画效果。但是，据我测试，FireFox浏览器确实如此但是Chrome特意做了写容错处理。例如，本来是数值的属性，写了个诸如a这个不合法的值，其会当作0来处理，动画效果依然存在。
			e.  values可以是一个值或多值。根据我在Chrome浏览器下的测试，是一个值的时候是没有动画效果。多值时候有动画效果。当values值设置并能识别时候，from, to, by的值都会被忽略。那values属性是干什么的呢？别看名字挺大众的，其还是有些功力的。我们实现动画，不可能就是单纯的从a位置到b位置，有时候，需要去c位置过渡下。此时，实际上有3个动画关键点。而from, to/by只能驾驭两个，此时就是values大显身手的时候了！
				<animate attributeName="x" values="160;40;160" dur="3s" repeatCount="indefinite" />
		4、 begin, end
			begin 没有begin或者begin参数解析异常，都当作0处理。 begin的定义是分号分隔的一组值。看到没？是一组值，单值只是其中的情况之一。例如，beigin="3s;5s"表示的是3s之后动画走一下，6s时候动画再走一下（如果之前动画没走完，会立即停止从头开始）。所以，如果一次动画时间为3s, 即dur="3s"，同时没有repeatCount属性时候，我们可以看到动画似乎连续执行了2次。animation中的时间表示(也适用于dur, end属性)。常见单位有 "h"|"min"|"s"|"ms"还有一点，十进制的小数值是秒的浮点定义。什么意思呢？就是如果begin="1.5"没有单位，这里的小数点表示秒，
			begin的单值除了普通value，还有下面这些类别的value：offset-value | syncbase-value | event-value | repeat-value | accessKey-value | media-marker-value | wallclock-sync-value | "indefinite"
			① offset-value表示偏移值，数值前面有+或-. 应该指相对于 document 的begin值而言。
			② syncbase-value基于同步确定的值。语法为：[元素的id].begin/end +/- 时间值. 就是说借用其他元素的begin值再加加减减，这个可以准确实现两个独立元素的动画级联效果。
				<animate id="x" attributeName="x" to="60" begin="0s" dur="3s" fill="freeze" />
        		<animate attributeName="y" to="100" begin="x.end" dur="3s" fill="freeze" />
        	当然，我们还可以增加一些偏移值，例如begin="x.end-1s", 就表示id为x的元素动画结束前一秒开始纵向移动。
        	③ event-value这个表示与事件相关联的值。类似于PowerPoint动画的“点击执行该动画”。语法是：[元素的id].[事件类型] +/- 时间值
        		<circle id="circle" cx="100" cy="100" r="50"></circle>
			    <text font-family="microsoft yahei" font-size="120" y="160" x="160">马
			        <animate attributeName="x" to="60" begin="circle.click" dur="3s" />
			    </text>
			④ repeat-value指重复多少次之后干嘛干嘛。语法为：[元素的id].repeat(整数) +/- 时间值. 
				<animate id="x" attributeName="x" to="60" begin="0s" dur="3s" repeatCount="indefinite" />
        		<animate attributeName="y" to="100" begin="x.repeat(2)" dur="3s" fill="freeze" />
			⑤ accessKey-value定义快捷键。即按下某个按键动画开始。兼容性不太好 Chrome36版本以下好像都不行，语法为：accessKey(" character "). character表示快捷键所在的字符，举个例子，按下s键动画走起。 
			⑥ wallclock-sync-value指真实世界的时钟时间定义。时间语法是基于在ISO8601中定义的语法。例如上面提到的1997-07-16T19:20:30.45+01:00这个让人呵呵呵的时间表示。
			⑦ "indefinite"就是这个字符串值，表示“无限等待”。据说需要beginElement()方法触发或者指向该动画元素的超链接(SVG中的a元素)。
				html svg <animate attributeName="x" to="60" begin="indefinite" dur="3s" />
				js      animate.beginElement();
				或者 超链接 直接xlink
						<a xlink:href="#animate">
					          <text x="10" y="20" fill="#cd0000" font-size="30">点击我</text>
					    </a>

			end与begin除了名字和字面含义不一样，其值的种类与表意都是一模一样的，
		5. dur 动画执行时间
			dur属性值比begin简单了好几层楼，就后面两种：常规时间值 | "indefinite".

			“常规时间值”就是3s之类的正常值；"indefinite"指事件无限。试想下，动画时间无限，实际上就是动画压根不执行的意思。因此，设置为"indefinite"跟没有dur是一个意思，与dur解析异常一个意思。







