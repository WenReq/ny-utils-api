---
title: Random
lang: zh-CN
layout: doc
---

# Random

## randomColor

> 随机生成颜色

```js
import { randomColor } from "ny-utils";

randomColor(); // '#6369e7'
randomColor(); // '#313aa4'
randomColor(); // '#c831f2'
```

## randomNumber

> 生成指定范围（min - max）随机数

参数：

- min：最小值
- max：最大值

```js
import { randomNumber } from "ny-utils";

randomNumber(0, 50); // '7'
randomNumber(0, 50); // '23'
```
