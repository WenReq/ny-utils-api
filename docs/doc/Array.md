---
title: Array
lang: zh-CN
layout: doc
---

# Array

## equalityArray

> 判读两个数组是否相等

```js
import { equalityArray } from "ny-utils";

const = arr1 = [1, 2, 3];
const arr2 = [1, 2, 3];
const flag = equalityArray(arr1, arr2);
console.log(flag); // true
```

## mergeArrayDelRepeat

> 合并数组后去重

```js
import { mergeArrayDelRepeat } from "ny-utils";

const arr1 = [1, 2, 3];
const arr2 = [3, 4, 5];
const newArr = mergeArrayDelRepeat(arr1, arr2);
console.log(newArr); // [1, 2, 3, 4, 5]
```

## uniqueArray

> 数组去重

```js
import { uniqueArray } from "ny-utils";

const arr = [1, 2, 3, 2, 3, 4, 5];
const newArr = uniqueArray(arr);
console.log(newArr); // [1, 2, 3, 4, 5]
```

## subArray

> 数组相减

```js
import { subArray } from "ny-utils";

const arr1 = [1, 2, 3, 4, 5, 6];
const arr2 = [1, 2, 3];
const newArr = subArray(arr1, arr2);
console.log(newArr); // [4, 5, 6]
```
