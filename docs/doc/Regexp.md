---
title: Regexp
lang: zh-CN
layout: doc
---

# Regexp

## isColor

> 判断是否为 16 进制颜色，rgb 或 rgba

```js
import { isColor } from "ny-utils";

isColor("#6369e7"); // true
```

## isEmail

> 判断是否为邮箱地址

```js
import { isEmail } from "ny-utils";

isEmail("294491328@qq.com"); // true
```

## isIdCard

> 判断是否为身份证号

```js
import { isIdCard } from "ny-utils";

isIdCard(410329199301269696); // true
```

## isPhoneNum

> 判断是否为手机号

```js
import { isPhoneNum } from "ny-utils";

isPhoneNum(15371491293); // true
```

## isUrl

> 判断是否为 URL 地址

```js
import { isUrl } from "ny-utils";

isUrl("https://cn.bing.com/"); // true
```
