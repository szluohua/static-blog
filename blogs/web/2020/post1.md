---
title: vue 解决css 打包插件 [mini-css-extract-plugin]报错warning Conflicting order
date: 2020-09-09
tags:
 - Vue-cli
categories:
 -  Web
---

在使用`mini-css-extract-plugin`这个插件，默认下可能你部分组件的css顺序加载顺序可能会不一致，所以会报出警告 `warning Conflicting order` ，然而在实际开发中，很多东西不能按理想去实现。所以最简单的方法是直接屏蔽掉警告。

判断环境，主要是因为这个设置会影响到热加载

```js
module.exports = {
	css: {
		extract: { ignoreOrder: process.env.NODE_ENV === 'production' }
}
```

参考链接：

https://github.com/webpack-contrib/mini-css-extract-plugin/issues/250

https://github.com/vuejs/vue-cli/issues/3771