---
title: Filter
lang: zh-CN
layout: doc
---

# Filter

## emptyFn

> 处理 Table 表格中列中的空数据和支持单位的添加

参数：

- value: 逻辑函数
- unit: 单位，如果不需要可以不用传

```vue
<template>
  <el-table :data="tableData">
    <el-table-column label="完成率">
      <template slot-scope="scope">
        <span>{{ emptyFn(scope.row.rate, %) }}</span>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import { emptyFn } from "ny-utils";
data() {
  return {
    tableData:[
      { name: 'zs', rate: null }, // -
      { name: 'ls', rate: 77 } // 77%
    ]
  }
},
methods: { emptyFn, }
</script>
```
