---
title: Number
lang: zh-CN
layout: doc
---

# Number

## standardAmount

> 将数字（整数逢三一断）含小数转换成标准的金额模式，最多保留三位小数 。

```js
import { standardAmount } from "ny-utils";

standardAmount(999999999.9991); // '999,999,999.999'
```

## standardIntegerAmount

> 将 "整数" 数字（整数逢三一断）。

```js
import { standardIntegerAmount } from "ny-utils";

standardIntegerAmount(99999999999); // '99,999,999,999'
```
