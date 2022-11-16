---
title: Support
lang: zh-CN
layout: doc
---

# Support

## isSupportWebP

> 判断浏览器是否支持 webP 格式图片

```js
import { isSupportWebP } from "ny-utils";

isSupportWebP(); // true
```

## downloadFile

> base64 数据文件导出，文件下载。

参数：

- filename：文件名
- data：Base64 数据

```js
import { downloadFile } from "ny-utils";

downloadFile("xx报表", data);
```
