---
title: Cookie
lang: zh-CN
layout: doc
---

# Cookie

## setCookie

> 根据 name, value, days 设置 Cookie

```js
import { setCookie } from "ny-utils";

setCookie("username", "ny", 7);
```

## getCookie

> 根据 name 读取 Cookie

```js
import { getCookie } from "ny-utils";

getCookie("username"); // ny
```

## removeCookie

> 根据 name 删除 Cookie

```js
import { removeCookie } from "ny-utils";

removeCookie("username");
```
