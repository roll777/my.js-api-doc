ui层使用示例二
=
创建一个简单的内容列表页
-
需求分析：

* 每条内容可点击进入详情页。
* 视图至少拥有四个属性：当前页码、总条数、每页显示条数、内容数据。
* 点击页码可实现翻页。
* 对外的接口确定为2个：showDetail(id)和pageChange(page)

此需求涉及到数据，所以html模版中需要使用到模版引擎的语法。<br>
当前框架使用的是jsmart解析引擎，语法参考地址：<https://code.google.com/p/jsmart/w/list>

首先在template目录创建list.html文件，假设代码如下（css样式自行补上）：
```html
<ul class="list">
	{foreach $list as $k=>$v}
	<li class="rows" data-event="click" data-func="detail_click" data-id="{$v['id']}">
		<a class="id">{$v['id']}</a>
		<a class="title">{$v['title']}</a>
		<a class="more">查看详情</a>
	</li>
	{/foreach}
</ul>
<div class="page">
	{for $i=1 to $maxPage}
	<a {if $page==$i}class="current"{/if} data-event="click" data-func="page_click">{$i}</a>
	{/for}
</div>
```
然后是视图部分，在ui目录创建list.js文件，代码如下：
```js
my.ui.create('list',null,null,{
	onLoad:function(){
		this.dataList.page = this.dataList.page || 1;//默认页码为1
		this.dataList.maxPage = this.dataList.maxPage || 0;//默认总数为0
	},
	detail_click:function(element){
		//触发查看详情接口
		var id = this.$(element).data('id');
		typeof this.detailCallback=='function' && this.detailCallback(id);
	},
	page_click:function(element){
		var page = parseInt(this.$(element).html());//获取点击的页码
		typeof this.pageCallback=='function' && this.pageCallback(page);
	},
	//对外的接口
	showDetail:function(fn){
		this.detailCallback = fn;
		return this;
	},
	pageChange:function(fn){
		this.pageCallback = fn;
		return this;
	}
});
```

视图部分实现的功能较简单，测试代码如下：
```js

var data = [
	{
		id:1,
		title:'测试新闻一'
	},
	{
		id:2,
		title:'测试新闻二'
	},
	{
		id:3,
		title:'测试新闻三'
	}
];
var uiList = my.ui.load('list',null,{
	list:data,
	page:1,
	maxPage:5
}).showDetail(function(id){
	console.log('用户想查看id为'+id+'的内容详情，请处理之……');
}).pageChange(function(page){
	console.log('用户想跳转到第'+page+'页，请处理……');
});
```
请注意示例一中和本示例中传值的区别。<br>
本示例中，使用了my.ui.load方法中第三个参数，传入了可供html模版替换的数据格式。<br>
在ui层编码时，可使用在this.dataList的属性中获取到传入的值并进行预处理。<br>
而示例一中，直接使用了extend方法传入。这种方法的缺点是，extend参数中传入的值可能会覆盖ui层原型中定义的属性或方法。