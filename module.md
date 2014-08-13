## exports vs module.exports

很多人看一些包里面的代码时常会看到：

```
exports = module.exports = ...
```


官方对于module.exports的说明：

```
The module.exports object is created by the Module system. 
Sometimes this is not acceptable; many want their module to be an instance of some class. 
To do this assign the desired export object to module.exports
```

官方给的例子：

示例文件1：

```
var EventEmitter = require('events').EventEmitter;

module.exports = new EventEmitter();
```

引用文件2：

```
var a = require('./a');
```



当我们用在属性上的时候：

```
//a.js
module.exports.job = "fe";

exports.name = function(){
	console.log('yaochun');
};
```

```
//b.js
var a = require('./a');
console.log(a); //{ job: 'fe', name: [Function] }
```

> 注释：这个时候，最好还是采用exports.props，节省字符而且避免歧义。




其实开始的时候：

```
module.exports = exports
```

当模块被require的时候，node用c++实现了包裹，会变成：

(function (exports, require, module, __filename, __dirname) {
	
	//...

});

而且require方法返回的是module.exports对象的指向。




如果同时定义了module.exports和exports：

```
//a.js
module.exports = function test(){

};

exports.name = function(){
	console.log('yaochun');
};
```

```
//b.js
var a = require('./a');
console.log(a); //[Function: test]
```

> 上面的情况,exports指向了一个，而module.exports指向另一个，看到b里面还是module.exports定义的

建议：

* 默认情况下,module.exports是一个{}
* 如果只是添加方法或者属性的化，只操作exports.****

