#ui层事件处理机制
##基本描述
需要处理绑定dom元素的事件时，有以下三种方式：

1. **html模版中设置事件标志** 
2. **ui层手动绑定**
3. control层手动改写ui层的onload方法，并在onload方法中手动绑定事件

推荐使用第一种方式。<br>
不推荐使用第三种方式，即不建议在控制器层手动绑定事件，容易造成代码混乱，同时清除事件侦听器也非常麻烦。<br>
不推荐使用第三种方式，即建议在以下情况下使用第二种方式：

* 需要动态绑定事件时
* 使用某个模版引擎，需要单独添加特殊的事件时（不推荐，需要时同样可在模版中设置事件标志）

##html模版中设置事件标志

### data-event属性

在需要设置事件的html元素内部设置data-event属性，值为以下一种：

* click 单击
* change 内容变化
* blur 失去焦点
* dbclick 双击
* focus 焦点
* keydown 键盘按下
* keypress 按键
* keyup 键盘松开
* mousedown 鼠标按下
* mouseover 鼠标经过
* mouseout 鼠标移除
* mouseup 鼠标松开
* submit 表单提交

### 指定处理函数

当html元素设置data-event属性时，ui层会按下列顺序查找元素的以下几个属性：

1. data-func属性
2. id属性
3. 第一个className

当任意一个值不为空时，则停止查找，根据下述规则设置处理该事件的回调函数，并将该事件绑定。

* data-func属性不为空时直接查找同名函数，如data-func="test"，则绑定至test函数。
* id属性或className时，则查找该值 + 下划线 + 事件名称的函数，如：id_click

```js
/**
 * @description  根据html模版中设置的data-event属性，单独绑定事件，并检测ui及ui绑定的model对象中是否存在对应的处理函数。<br>
 * 触发的处理函数名称可以通过3种方式设置，只会取优先级较高的一个函数名称，然后判定ui层是否存在该处理函数，然后判定model层是否存在该处理函数。<br>
 * 优先级顺序为：data-func属性直接指定处理函数的名称 > 元素的id值 + 下划线 + 事件类别  > 元素的第一个classname + 下划线 + 事件类别
 * @example
 * <div data-event="click" data-func="test" id="test1" class="test2">test text</div>
 * 则只会判定是否存在test函数。如不设置data-func属性，则会判定是否存在test1_click函数，否则判定test2_click函数。
 */
```

###ui层处理函数

例html模版中代码如下：
```html
<div data-event="click" data-func="test" id="test1" class="test2">test text</div>
```
则，处理函数参数如下：
```js
my.ui.create('list',null,null,{
	//参数为：触发事件的dom元素，事件对象
	test:function(element,event){
		
		return 1;
	}
});

```

###ui层的事件接口

ui层的事件处理之后，可通过以下几种方式将接口暴露给控制器层：

* 控制器层设置ui层的同名处理函数
* 控制器层直接改写ui层的事件处理函数
* 使用回调函数、链式调用
* 使用观察者对象暴露接口


例：设置同名处理函数
```js
my.control.create('list',null,null,{
	//设置同名处理函数
	//参数为：ui层返回的结果(上个例子返回的是1)，触发事件的dom元素，事件对象。
	test:function(objResult,element,event){
		console.log(objResult);
	}
});

```

例：直接改写事件处理函数
```js
my.control.create('list',null,null,{
	init:function(){
		this.uiList = my.ui.load('list');
		//参数：触发事件的dom元素，事件对象
		this.uiList.test = function(element,event){
			//处理过程
		}
	}
});
```

例：ui层将接口改写为ok函数。
```js
my.ui.create('list',null,null,{
	test:function(element,event){
		this.testCallback =='function' && this.testCallback();
	},
	ok:function(fn){
		this.testCallback = fn;
		return this;
	}
});

//使用方法：
my.control.create('list',null,null,{
	init:function(){
		this.uiList = my.ui.load('list');
		//设置ok接口的回调
		this.uiList.ok(function(){
			console.log('okok');
		}).init();
	}
});
```

