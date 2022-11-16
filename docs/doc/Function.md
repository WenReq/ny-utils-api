---
title: Function
lang: zh-CN
layout: doc
---

# Function

## debounce

> 函数防抖。在事件被触发 n 秒后再执行回调，如果在这 n 秒内又被触发，则重新计时。
> 函数防抖的应用场景：搜索框搜索输入，频繁操作点赞和取消点赞等等

参数：

- func: 逻辑函数
- wait: 执行逻辑的间隔（毫秒）
- immediate: 是否立即执行

```js
import { debounce } from "ny-utils";

const fn = function () {
  // 一些请求的逻辑
};
debounce(fn, 1000, false);
```

## throttle

> 函数节流。每隔一段时间，只执行一次函数。
> 函数节流的应用场景一般是 ，onscroll 或高频点击提交，表单重复提交这些频繁触发的函数

参数：

- func: 逻辑函数
- wait: 执行逻辑的间隔（毫秒）
- immediate: 是否立即执行

```js
import { throttle } from "ny-utils";

const fn = function () {
  // 一些请求的逻辑
};
throttle(fn, 1000, false); // { left: number, top: number } 距离文档左侧、顶部的距离
```
