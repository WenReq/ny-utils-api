---
title: Object
lang: zh-CN
layout: doc
---

# Object

## deepClone

> 对象的深拷贝

```js
import { deepClone } from "ny-utils";

deepClone({
  name: "zs",
  age: 18,
  sayHello: () => {
    console.log("Hello");
  },
  days: new Date("2020-11-29"),
});
```

## shallowClone

> 对象的浅拷贝

```js
import { shallowClone } from "ny-utils";

shallowClone({ name: "zs", age: 18 });
```

## isEmptyObject

> 判断对象是否为空

```js
import { isEmptyObject } from "ny-utils";

isEmptyObject({}); // true
```

## getFormData

> 将对象转换为 formData 对象

```js
import { getFormData } from "ny-utils";

getFormData({ name: "zs", age: 18 }); // FormData {...}
```
