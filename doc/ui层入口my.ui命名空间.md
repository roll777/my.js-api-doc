#my.ui命名空间

ui层的创建、加载均只能使用my.ui.create和my.ui.load方法实现。

###my.ui.create(name,parentName,func,prop)
```js
/**
 * 创建一个ui层
 * @method
 * @name my.ui.create
 * @param  {string} name       ui层的名称
 * @param  {string} parentName 继承的ui层名称
 * @param  {function} func       构造函数
 * @param  {Object} prop       自身的原型
 * @return {class}            该ui层
 */
```

###my.ui.load(name, target, list)
```js
/**
 * 实例化一个ui层
 * @method
 * @name my.ui.load
 * @param  {string} name   ui层的名称
 * @param  {jQuery|dom} [target] 目的元素
 * @param  {Object} [list]   要替换的目的数据
 * @return {ui}  实例化的ui对象
 */
```

###my.ui.extend(name,prop)
```js
/**
* 扩展一个ui层的原型方法
* @method
* @name my.ui.extend
* @param  {string} name ui层的名称
* @param  {object} prop 要扩展的方法
*/
```

###my.ui.addVali(type,test)

```js
/**
* 添加验证方式，第一个参数为object时可传入多个
* @method
* @name my.ui.addVali
* @param {string|json} type 验证方式名称|多个验证方式的json形式
* @param {string} [test] 验证的正则表达式
*/
```