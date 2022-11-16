---
title: Dom
lang: zh-CN
layout: doc
---

# Dom

## getScrollTop

> 获取滚动条距顶部的距离

```js
import { getScrollTop } from "ny-utils";

getScrollTop(); // 200
```

## offset

> 获取一个元素的距离文档(document)的位置，类似 JQ 中的 offset() ele.offset()

```js
import { offset } from "ny-utils";

offset(element); // { left: number, top: number } 距离文档左侧、顶部的距离
```

## scrollTo

> 在 ${duration} 时间内，滚动条平滑滚动到 ${to} 指定位置 scrollTo(to, duration)

```js
import { scrollTo } from "ny-utils";

scrollTo(1200, 2); // 滚动位置的值, 时间-毫秒数
```

## setScrollTop

> 设置滚动条距顶部的距离

```js
import { setScrollTop } from "ny-utils";

setScrollTop(100); // {Number} value 距顶部的距离的值
```

## windowResize

> H5 软键盘缩回、弹起回调

```js
import { windowResize } from "ny-utils";

// 当软键盘弹起后，缩回的回调
const downCallBack = function () {
  alert("down");
};
// 当软键盘弹起的回调
const upCallBack = function () {
  alert("down");
};
windowResize(downCallBack, upCallBack);
```
