---
title: 单元测试
date: 2018-12-29 15:31:51
tags:
- mocha
categories:
- mocha
comments: false
---
# mocha + chai 单元测试

参考链接： [测试框架 Mocha 实例教程](http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html)
断言库使用 [chai](https://www.chaijs.com/api/bdd/)

> 主要为个人的练习记录存放，下面会展示一些或者重点易错点。

## 通配符（建议，如果只有一个文件的时候，不要使用{}这种匹配模式）。
```js
$ mocha spec/{my,awesome}.js
$ mocha test/unit/*.js
$ mocha 'test/**/*.@(js|jsx)' // 也支持node的通配符，使用字符串包裹起来即可

提示： 如果只有一个文件的时候，也需要加上, 比如 mocha test/{cf,}.js 否则会报错。
Warning: Could not find any test files matching pattern: test/{cf}.js
No test files found

```

## 递归执行（mocha默认会执行当前目录下的test目录下的文件）

```js
如果想执行内嵌的其他文件，比如：
test/cf/**.js;
test/gg/tt/**.js;
这种默认使用mocha命令是不会执行的，因为他们不在test目录下。
可以使用：
mocha --recursive 即可以递归执行test目录下所有文件，包含深度嵌套的。
```

## 显示报告结果(`--reporter`参数用来指定测试报告的格式，默认是`spec`格式。)

```js
$ mocha
# 等同于
$ mocha --reporter spec
```

> `--reporters`参数可以显示所有内置的报告格式。

```js
$ mocha --reporters
```

## 生成报告结果（使用[`mochawesome`](http://adamgruber.github.io/mochawesome/)模块，可以生成漂亮的HTML格式的报告。）

```js
$ npm install --save-dev mochawesome
$ ../node_modules/.bin/mocha --reporter mochawesome
```

上面代码中，`mocha`命令使用了项目内安装的版本，而不是全局安装的版本，因为`mochawesome`模块是安装在项目内的。

然后，测试结果报告就在[`mochaawesome-reports`](https://github.com/ruanyf/mocha-demos/blob/master/demo02/mochawesome-reports)子目录生成。

## 配置文件mocha.opts

Mocha允许在`test`目录下面，放置配置文件`mocha.opts`，把命令行参数写在里面。请先进入[`demo03`](https://github.com/ruanyf/mocha-demos/tree/master/demo03)目录，运行下面的命令。

```js
$ mocha --recursive --reporter tap
```

上面这个命令有两个参数`--recursive`、`--reporter tap`。

然后，把这两个参数写入`test`目录下的[`mocha.opts`](https://github.com/ruanyf/mocha-demos/blob/master/demo03/test/mocha.opts)文件。

```js
--reporter tap
--recursive
```

然后，执行`mocha`就能取得与第一行命令一样的效果。

```js
$ mocha
```

如果测试用例不是存放在test子目录，可以在`mocha.opts`写入以下内容。

```js
otherDirTests
--recursive
```

上面代码指定运行`otherDirTests`目录及其子目录之中的测试脚本。

## ES6测试

如果测试脚本是用ES6写的，那么运行测试之前，需要先用Babel转码。进入[`demo04`](https://github.com/ruanyf/mocha-demos/tree/master/demo04)目录，打开[`test/add.test.js`](https://github.com/ruanyf/mocha-demos/blob/master/demo04/test/add.test.js)文件，可以看到这个测试用例是用ES6写的。

```js
import add from '../src/add.js';
import chai from 'chai';

let expect = chai.expect;

describe('加法函数的测试', function() {
  it('1 加 1 应该等于 2', function() {
    expect(add(1, 1)).to.be.equal(2);
  });
});
```

ES6转码，需要安装Babel。

```js
$ npm install babel-core babel-preset-es2015 --save-dev
```

然后，在项目目录下面，新建一个[`.babelrc`](https://github.com/ruanyf/mocha-demos/blob/master/demo04/.babelrc)配置文件。

```js
{
  "presets": [ "es2015" ]
}
```

最后，使用`--compilers`参数指定测试脚本的转码器。

```js
$ ../node_modules/mocha/bin/mocha --compilers js:babel-core/register
```

上面代码中，`--compilers`参数后面紧跟一个用冒号分隔的字符串，冒号左边是文件的后缀名，右边是用来处理这一类文件的模块名。上面代码表示，运行测试之前，先用`babel-core/register`模块，处理一下`.js`文件。由于这里的转码器安装在项目内，所以要使用项目内安装的Mocha；如果转码器安装在全局，就可以使用全局的Mocha。

> 可以结合mocha.opts使用，或者在package.json中自定义命令去实现。


### package.json（使用方式：`npm run test`）

```js
"scripts": {
    "test": "mocha --recursive --compilers js:babel-core/register"
 }
```

--compilers在使用过程中提示在未来版本中将被移除，推荐使用下面这种方式！

```js
"scripts": {
    "test": "mocha --recursive --require babel-core/register"
 }
```

### Mocha.opts（使用方式： `mocha`）

```js
--reporter spec
--recursive
--compilers js:babel-core/register
```

--compilers在使用过程中提示在未来版本中将被移除，推荐使用下面这种方式！

```js
--reporter spec
--recursive
--require babel-core/register
```