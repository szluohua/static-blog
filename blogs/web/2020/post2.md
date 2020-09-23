---
title: vue深度作用选择器使用
date: 2020-09-09
tags:
 - Vue
categories:
 -  Web
---

如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 >>> 操作符：

```html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```
上述代码将会编译成：

```html
.a[data-v-f3f3eg9] .b { /* ... */ }
```
有些像 Sass 之类的预处理器无法正确解析 >>>。这种情况下你可以使用 /deep/ 或 ::v-deep 操作符取而代之——两者都是 >>> 的别名，同样可以正常工作。

但是在chrome等浏览器，会提示/deep/即将被弃用。因为可以使用 `::v-deep` 。