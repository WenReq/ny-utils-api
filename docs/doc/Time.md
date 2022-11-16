---
title: Time
lang: zh-CN
layout: doc
---

# Time

## dateFormate

> 格式化时间

```js
import { dateFormate } from "ny-utils";

dateFormate("YYYY-MM-DD HH:mm"); // '2022-08-07 14:40'
dateFormate("YYYY-MM-DD", "2020.11.29"); // '2020-11-29'
dateFormate("YYYYMMDDHHmm", "2020-11-29 18:10:07"); // '202011291810'
```

## dateStrFormat

> 将指定字符串由一种时间格式转化为另一种。

`dateStrFormat(str, from, to)`

参数：

- str：原始的日期字符串
- from：匹配原始字符串的格式
- to：想要转换的日期字符串格式

```js
import { dateStrFormat } from "ny-utils";

dateStrForma("20220807", "YYYYMMDD", "YYYY 年 MM 月 DD 日"); //  2022 年 08 月 07 日
dateStrForma("2022 年 08 月 07 日", "YYYY 年 MM 月 DD 日", "YYYYMMDD"); // 20220807
```

## formatRemainTime

> 计算一个时间到现在过去了多久，如：'451 天 15 小时 17 分钟 25 秒'

```js
import { formatRemainTime } from "ny-utils";

formatRemainTime("2022-02-24"); // '265天17小时24分钟13秒'
```

## formatPassTime

> 计算一个时间到现在过去了多久，如：'1 年前' '6 个月前' '45 分钟前'

```js
import { formatPassTime } from "ny-utils";

formatPassTime("2020-11-29"); // '1年前'
```

## isLeapYear

> 判断是否为闰年

```js
import { isLeapYear } from "ny-utils";

isLeapYear(2020); // true
isLeapYear(2021); // false
isLeapYear(2022); // false
```

## isSameDay

> 判断是否为同一天

```js
import { isSameDay } from "ny-utils";

isSameDay("2022-08-06", "2022-08-06"); // true
```

## monthDays

> 获取指定日期月份的总天数

```js
import { monthDays } from "ny-utils";

monthDays("2022-06"); // 30
monthDays("2022-08"); // 31
```

## timeLeft

> 计算${startTime - endTime}的剩余时间

```js
import { timeLeft } from "ny-utils";

timeLeft("2022-08-06 10:10:10", "2022-08-10 11:15:15"); // {d: 4, h: 1, m: 5, s: 5} // 剩余 4 天 1 小时 5 分 5 秒
```

## twoDateBetweenAllDay

> 根据指定的两个日期，计算并返回中间的所有日期 。

```js
import { twoDateBetweenAllDay } from "ny-utils";

twoDateBetweenAllDay("2022-06-01", "2022-06-09"); // ['2022-06-01', '2022-06-02', '2022-06-03', '2022-06-04', '2022-06-05', '2022-06-06', '2022-06-07', '2022-06-08', '2022-06-09']
```

## twoDateBetweenAllDay

> 计算两个日期之间的天数。 使用场景：距今天已有 N 天 。

```js
import { twoDateBetweenAllDay } from "ny-utils";

twoDaysBetweenNum("2022-06-16", "2022-06-20"); // 4
```
