---
title: Url
lang: zh-CN
layout: doc
---

# Url

## parseQueryString

> url 参数转对象

```js
import { parseQueryString } from "ny-utils";

parseQueryString("?a=1&b=2&c=3"); // {a: '1', b: '2', c: '3'}
```

## stringifyQueryString

> 对象序列化

```js
import { stringifyQueryString } from "ny-utils";

stringifyQueryString({ a: "1", b: "2", c: "3" }); // 'a=1&b=2&c=3'
```
