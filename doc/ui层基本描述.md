ui层基本描述
=
本页目录
-
* 用到的插件
* 基本描述
* 功能简介
* 改写事件
* 扩展方式

用到的插件
-
* jQuery，dom操作、动画等。参考：<http://jquery.com>
* jsmart，模版引擎。参考：<https://code.google.com/p/jsmart/w/list>
* flotr2，图表生成。参考：<http://www.humblesoftware.com/flotr2/documentation>
* iScroll，滚动拖动。参考：<http://cubiq.org/iscroll-4>

基本描述
-

ui层即视图层。
视图层的设计主要用于实现html模版到最终页面呈现的过程。视图层内部一般不处理业务逻辑，**只提供一系列接口及功能方法供外层调用**。<br>
特例：当某类视图需要简化对外提供的接口时，可封装一些简单的事件处理逻辑。例如弹窗视图可统一封装几个接口对外调用。
###html解析流程、ui层内部工作流程
1. ui层初始化时，会在app目录下template目录中寻找同名的.html模版文件并自动加载。
2. 用list属性中储存的数据替换html模版中的标签。
3. 替换数据后，将html转换为一个jQuery对象。
4. 为转换后的jQuery对象绑定html元素中设置的事件。
5. 将最终的jQuery对象插入到页面，并最终显示。

###创建及初始化

####创建

* 使用`my.ui.create(name,parentName,func,prop)`方法创建一个ui视图类。

####初始化

* 只能使用`var uiLogin = my.ui.load(name,target,list);`方法实例化一个ui视图。
* 视图实例化后，必须调用初始化方法才能在页面上显示，例如：`uiLogin.init();`

功能简介
-
**所有和页面显示相关的功能都将在ui层实现。**其中包括：

* 对自身的基本操作。包括：初始化方法`init()`、隐藏`hide()`、显示`show()`、移除`remove()`、刷新`fresh()`。
* 对dom的操作。包括：获取jQuery对象方法`$(selector)`。
* 对表单的操作。包括：获取目的元素内部所有表单元素的值`form()`、验证字段方法`vali()`。
* 动画效果。项目暂时集成了jQuery的三个动画相关的方法，`show()`、`hide()`、`animate()`，使用方式和jQuery中同名方法一致。
* 复合视图（子视图）。包括：添加一个子视图`addChild()`、获取某个子视图的引用`child()`、移除子视图`removeChild()`。
* 在当前视图使用热键。包括：绑定热键`hotkey()`、清除热键`clearHotkeys()`。
* 扩展方法。包括：扩展自身方法`extend()`、扩展验证方式`addVali()`。
* 其他方法。包括：获取当前绑定的控制器引用`control()`、重新实例化一个自身`copy()`、插入到目的元素`insert()`。

改写事件
-
视图层在初始化并渲染到页面时，会按顺序触发一定的事件，分别为：

#### onReady() 
此时视图类已经实例化为一个对象，但模版尚未加载，dom也没有转换。这时，可以对视图对象做一些基础的属性设置、修改等。 
#### onLoad() 
dom初始化及页面成功渲染后（如果设置了动画，此时动画可能尚未结束）触发该事件。可在此事件中对dom进行操作。
#### onResize() 
html窗口大小发生变化时触发该事件。
#### onUnload() 
视图卸载移除事件。可在此事件中，对定时器、热键等对象进行移除操作。

扩展方式
-
所有通过`my.ui.create(name,parentName,func,prop)`方式创建的ui视图，均默认继承自名为base的基础视图，即第二个参数parentName为空时均继承自base视图。<br>
当然，第二个参数也可以手动指定为'base'。所以，系统无法再次创建名为'base'的视图。<br>
创建视图时，一般无需改写系统自带的方法。主要通过改写事件、处理html元素中触发的事件两种方式实现扩展。
