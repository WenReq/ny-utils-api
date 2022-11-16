---
title: Storage
lang: zh-CN
layout: doc
---

# Storage

## setStorage(name, value)

> 根据 name, value 添加 localStorage

```js
import { setStorage } from "ny-utils";

setStorage("name", "zs");
```

## getStorage(name)

> 根据 name 读取 localStorage

```js
import { getStorage } from "ny-utils";

getStorage("name");
```

## removeStorage(name)

> 根据 name 删除 localStorage

```js
import { removeStorage } from "ny-utils";

removeStorage("name");
```
