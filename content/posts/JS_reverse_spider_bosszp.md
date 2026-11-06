+++
date = '2026-09-02T23:48:47+08:00'
title = 'JS逆向、爬虫和boss直聘'
+++

## 如何组织JS/TS项目结构

爬虫不像前端项目有严格的项目结构，但也不能随便应付。所以使用一些简单的项目结构。

```
- root
	- lib             # 可以被抽离出来的库，比如Proxy和Hook等等
	  - src
		- ... 
	- website
	  - some-website	# 具体的被爬网站
		- lib           # 使用软连接连接到lib，方便使用库方法
```

## 方法论

总的方法论即对比浏览器生成的数据与nodejs生成的数据是否一致，确保数据一致。简而言之，能骗过服务器即可。

但存在不一致的数据时，可通过不同手段对比与浏览器生成的数据。解析状态机的实际意图。这就引出了很多手段。

## 手段

1. patch源代码，打印函数调用的返回值，有必要时也可打印入参。分别对比结果在nodejs和浏览器中是否一致。
2. 打印状态机的状态，追踪状态机运行轨迹。
3. Hook原本的函数，在Javascript中就是直接对函数赋值，这里不讨论逆向原生应用。

## 结果

目前使用Boss直聘的__zp_stoken__可以正常访问，但是大多数结果需要登录后才能获得，所一下一步需要解决登录的问题。了解boss直聘是如何维护登录Cookie的。

## 知识点

1. Javascript原型链可以通过`Object.getPrototypeOf`获得，也可以使用`Object.SetPrototypeOf`设置。也可使用`Object.create`直接在某个对象上创建一个对象，该对象原型默认是`Object.create`指定的对象。
2. 在模拟DOM中，需要批量的创建Property，就可以使用`Object.defineProperty`或`Object.definedProperties`来批量创建Property。
3. ES6提供的Proxy有许多有用的API，虽然可以有效拦截操作。但有时还是需要`Object.defineProperty`和`Object.defineProperties`来定义或修改属性。

## 注意事项

1. iframe被创建后contentWindow为null，被appendChild之后变为window，removechild之后变回null。
2. 在DOM中有一个document.all，因为历史遗留问题，他被称作“异常对象”（Exotic Object）。在在Javascript中使用宽松相等（==）比较中，表现的和undefined和null一样。

## 方案

### 解决如何模拟document.all的方案：

使用nodejs提供的不可检测对象。

``` js
const v8 = require("v8");
const vm = require("vm");
// 允许使用 V8 内置函数（需启用 --allow-natives-syntax 标志）
v8.setFlagsFromString('--allow-natives-syntax');
// 创建不可检测对象
let undetectable = vm.runInThisContext('%GetUndetectable()');
// 恢复标志禁用（可选）
v8.setFlagsFromString('--no-allow-natives-syntax');
function HTMLAllCollection() {
	return undetectable;
};
Object.defineProperties(HTMLAllCollection.prototype, {
	[Symbol.toStringTag]: {
		value: 'HTMLAllCollection',
		configurable: true
	},
});
undetectable.length = 3;
```