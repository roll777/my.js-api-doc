ui层api方法
=
属性成员
-

###公共属性

* name: 名称
* target: 容器dom元素的htmlElement引用或jQuery引用
* enable : 是否可用。该属性暂时不用。
* url:html模版的路径
* dom: 当前视图的jQuery引用
* childs: 子视图数组
* parent: 父ui的引用
* index: 在父ui中的引用索引
* dataList: html模版中要替换的数据
* html: dom的html字符串
* hotkeys: 当前ui绑定的快捷键数组

###私有属性

* _template : 模版字符串

公共方法
-
###基础方法

* init([target],[list]) 初始化函数，参数：容器元素,要替换的数据。
* hide([time]) 隐藏dom，等同jQuery的hide()方法。参数：消失的时间
* show([time]) 显示dom，等同jQuery的show()方法。参数：显示的时间。
* remove() 移除该ui层，会触发onUnload()事件
* fresh() 重新加载数据，解析模版，刷新ui层。会触发onUnload()及onLoad()事件。
* insert([target], [list]) 插入到目的元素。触发onLoad()及**onUnload()[如果之前已经初始化]**。

### dom操作

* $(selector) 在当前视图查询符合条件的dom元素。返回一个jQuery对象。参数：jQuery选择器。

###表单操作

* form([selector]) 获取表单中所有内容的值
* vali(value,type) 验证表单内容，参数：要验证的内容，验证的格式。验证格式见附录。
* addVali(type,[test]) 添加验证方式，第一个参数为object时可传入多个。参数：验证方式名称|多个验证方式的json形式，验证的正则表达式

###动画效果

* show()
* hide()
* animate()

###子视图

* addChild(name,[target],[list]) 添加一个子ui，返回该子ui的索引。
* child(index) 根据子ui的索引获取该ui的引用
* removeChild([index]) 根据索引移除子ui，不传index参数时移除所有子ui。

###热键

* hotkey(key,func,o) 绑定一个热键。参数：热键名称，回调函数，回调的作用域（默认为this）
* clearHotkeys() 移除当前ui绑定的所有热键

###扩展方法

* extend(prop) 扩展当前ui对象的方法。（不是扩展原型方法，要扩展原型方法直接使用my.ui.extend(uiname,prop)）。主要方便在控制器层单独给ui扩展方法。

###其他方法

* control([ctrl]) 获取当前绑定的控制器对象的引用|设置绑定的控制器对象。参数：控制器对象
* copy() 重新实例化一个ui，等同于my.ui.load()
* template(str) 手动指定模版的html字符 

###内部方法

* parseHTML() 解析模版中的标签。
* getHTML() 远程加载模版的html字符串

使用示例
-
### init(target,list)
```js
 /**
 * 初始化函数。设置子ui为空数组，默认target为document.body，替换的数组为空。
 * @param  {jQuery|dom} [target] 目的元素
 * @param  {Object} [list]   要替换的目的数据
 * @return {ui}        自身的引用
 * @example
 */
var ui = my.ui.load('base');
var data = [
	{
		id:1,
		title:'测试产品一'
	},
	{
		id:2,
		title:'测试产品二'
	}
];
ui.init(document.body,data);
```

###control(control)
```js
/**
* 获取当前绑定的控制器对象的引用|设置绑定的控制器对象
* @param {control} [control] 控制器对象
* @return {control|ui} 控制器对象的引用|当前视图的引用
*/
```
