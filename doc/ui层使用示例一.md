ui层使用示例一
=
创建一个msgbox对话框
-
需求分析：

* 对话框处理整个页面正中。
* 除了对话框外，其他页面元素均不可点击。
* 对话框拥有四个属性：标题、提示内容、宽度、高度。
* 对话框拥有三个可点击元素：关闭、确认、取消。

首先在template目录创建msgbox.html文件，假设代码如下（css样式自行补上）：
```html
<div class="msgbox">
	<div class="title">警告信息</div>
	<div class="close" data-event="click" data-func="close_click">X</div>
	<div class="content">是否确认删除改信息？</div>
	<div class="button ok" data-event="click" data-func="ok_click">确认</div>
	<div class="button cancel" data-event="click" data-func="cancel_click">取消</div>
</div>
```
之后，在ui目录下创建msgbox.js文件，实现代码如下：
```js
//创建一个是否提示对话框
my.ui.create('msgbox', null, null, {
    onLoad: function () {
    	//设置默认宽高，防止外部不传入宽高时出错
		this.width = this.width || 500;
		this.height = this.height || 300;
		//设置默认内容
		this.title = this.title || '提示';
		this.content = this.content || '请确认操作！';

        //填充内容
        this.$(".title").html(this.title);
        this.$(".content").html(this.content);
        //设置宽高
        this.$(".msgbox").css("width", this.width).css("height", this.height);
        //设置居中
        winSize = my.getEleSize();//获取整个窗体的宽高
        domSize = my.getEleSize(this.$('.msgbox'));//获取当前窗体的宽高。
		
		this.$(".msgbox").css({
			left:(winSize.width - domSize.width)/2,
			top:(winSize.height - domSize.height)/2,
			'z-index':99999
		});
		//设置透明层
		this.opacityBg = document.createElement('div');
		this.opacityBg.style.cssText = "position:abusolute;width:100%;height:100%;z-index:99998;opacity:0.7;background:#fff;";
		document.body.appendChild(this.opacityBg);
    },
    onUnload:function(){
		//卸载时，将透明层去掉
		document.body.removeChild(this.opacityBg);
		this.opacityBg = null;
    },
    close_click:function(){
    	this.remove();//关闭
    },
    ok_click:function(){
    	typeof this.okCallback =='function' && this.okCallback();
    	this.remove();
    },
    cancel_click:function(){
    	typeof this.cancelCallback =='function' && this.cancelCallback();
    	this.remove();
    },
    //提供对外的接口
    ok:function(fn){
		this.okCallback = fn;
		return this;//返回自身引用，方便外部链式调用
    },
    cancel:function(fn){
		this.cancelCallback = fn;
		return this;//返回自身引用，方便外部链式调用
    }
});

```


至此，msgbox这个视图已经创建完毕，可以正常使用了。测试代码如下：
```js

var uiMsgbox = my.ui.load('msgbox').extend({//使用extend方法传入参数。
	width:600,
	height:400,
	title:'测试标题',
	content:'测试内容'
}).ok(function(){
	console.log('你点击了确认按钮！');
}).cancel(function(){
	console.log('你点击了取消按钮！');
});
uiMsgbox.init();//调用初始化方法，使ui层显示。

```