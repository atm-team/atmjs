# 模块化
> 模块化是一种处理复杂系统分解成为更好的可管理模块的方式，
它可以把系统代码划分为一系列职责单一，
高度解耦且可替换的模块，
系统中某一部分的变化将如何影响其它部分就会变得显而易见，
系统的可维护性更加简单易得。

# javascript模块化
把javascript代码按照模块化思想进行拆分和组织,从而使代码的开发和维护变得更容易。

## javascript模块化规范分类
* [commonJs规范](http://wiki.commonjs.org/wiki/Modules/1.1)
* [amd规范](https://github.com/amdjs/amdjs-api/wiki/AMD)
* [cmd规范](https://github.com/cmdjs/specification/blob/master/draft/module.md)

## javascript模块化的必要元素
* 模块定义 `define(id, deps, factory)`
* 模块标识 `id`
* 模块引用  `require(id)`
* 模块接口 `module.exports | exports`
* 模块的启动及入口id eg:`seajs.use(id[, callback])` 把此处的id称为 入口id

## 模块解析器原理

```js
// parser.js
var define;
var parser = {};
(function () {
    var modules = {};
    var defines = {}
    define = function (id, deps, factory) {
        defines[id] = {
            id: id,
            deps: deps,
            factory: factory
        };
    };

    function require (id) {
        if (modules[id]) {
            return modules[id].exports;
        } else {
            var module = {
                exports: {}
            };
            modules[id] = module;
            var factory = defines[id].factory;
            factory.apply(module.exports, [require, module.exports, module]);
            return module.exports;
        }
    }
    parser.use = function(id, callback) {
        callback && callback(require(id));
    }
})();
```

```js
// util.js
define('./util.js', [], function (require, exports, module) {
  module.exports = {
    log: function (str) {
      console.log(str);
    }
  }
});
```
```js
// dialog.js
define('./dialog.js', ['./util.js'], function (require, exports, module) {
  var util = require('./util.js');
  module.exports = {
    show: function () {
      // do something
      util.log('dialog is opened');
      // do something else
    }
  }
});
```

```js
// demo.js
define('./demo.js', ['./util.js', './dialog.js'], function (require, exports, module) {
  var dialog = require('./dialog.js');
  module.exports = {
    init: function () {
      // do something
      dialog.show();
      // do something else
    }
  }
});
```

```html
<html>
<head>
    <meta charset="UTF-8">
    <title>demo.html</title>
</head>
<body>

<script src="./parser.js"></script>
<script src="./dialog.js"></script>
<script src="./demo.js"></script>
<script>
    parser.use('./demo.js', function (demo) {
        demo.init();
    });
</script>
</body>
</html>
```

## javascript模块化给前端开发带来的好处

> 供参考文章
>
[Why SeaJS](http://chaoskeh.com/blog/why-seajs.html)
>
>[前端模块化开发的价值](https://github.com/seajs/seajs/issues/547)（作者这里的前端模块化其实是javascript模块化）

上面两篇文章都提到了模块化可以避免冲突,那么是如何做到的呢

* javascript模块化改变了javascript语言先定义后使用的特性，进而把命名权移交到引用层

```javascript
// 传统编程，我们需要先定义再使用
var util = {
  // other method or attributes
  log: function (str) {
    console.log(str);
  }
}
// 这样在引用层dialog.js中只能用变量名util来引用
```
* 只通过移交命名权并不足以避免命名冲突,因为标志名也存在同名的可能性

```js
// 模块1
define('util', [], function () {
  // some code
  method1: function () {}
});

// 模块2
define('util', [], function () {
    // some code
    method2: function () {}
});

// 如果同时引入这两个模块,那么 require('util') 就存在不确定性
```
* 不过模块最终的落脚点是文件,每个文件的路径是唯一的,
* 模块解析器正是通过模块标识与文件路径形成一一映射来避免冲突的
