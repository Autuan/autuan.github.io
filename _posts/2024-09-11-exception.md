---
layout:     post
title:      element-ui时间选择页面无变化
subtitle:   el-time-picker选择后值变化但是页面不变
date:       2024-09-11
author:     Autuan.Yu
header-img:
catalog: true
tags:
    - ElementUI
---

## 问题描述
el-time-picker 有时会出现选中了值,绑定的值也变更了，但是页面就是渲染不了的问题

## 快速解决方案

解决方案： `forceUpdate` 强制刷新 


### 示例
```vue
<el-form-item :label="item.label" :prop="item.prop">  
 <!-- 绑定@input 事件 -->
  <el-time-picker @input="timePickerChange"  
    is-range     value-format="HH:mm"  
    format="HH:mm"  
    v-model="timerange"  
    range-separator="至"  
    start-placeholder="开始时间"  
    end-placeholder="结束时间"  
    placeholder="选择时间范围">  
  </el-time-picker>
</el-form-item>
```


```js
// 强制刷新
timePickerChange(){  
  this.$forceUpdate();  
},
```
