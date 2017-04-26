# svg是个好东西

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
		两者呈现方式：preserveAspectRatio="xMidYMid slice">后者参数slice为视野包括视窗meet视窗包括视野，前者参数为两者对齐方式
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







