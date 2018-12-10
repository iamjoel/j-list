# vue-list
[![npm](https://img.shields.io/npm/v/@lucky-joel/vue-list.svg) ![npm](https://img.shields.io/npm/dm/@lucky-joel/vue-list.svg)](https://www.npmjs.com/package/@lucky-joel/vue-list)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)

基于 [Vant](https://youzan.github.io/vant/) List 组件封装的 Vue.js 插件。

## 目录
- [安装](#安装)
- [使用](#使用)

## 安装
```
npm install --save @lucky-joel/vue-list
```

## 使用
```
<template>
<ul>
  <lj-list 
    url="/singer/list"
  >
    <template slot-scope="scope" v-if="scope.data">
      <li>{{scope.data.name}}</li>
    </template>
  </lj-list>
</ul>

</template>
<script>
import Vue from 'vue'
import List from '@lucky-joel/vue-list'
Vue.use(List)

export default {
  ...
}
</script>
```


参数:  
* url: 接口地址。必填。
* page-limit: 每页条数。默认五条。
* search-condition: 搜索条件。 对象类型。
* order: 分页。数组类型。 形如 `['name', 'desc或asc']`。
* is-infinate: 是否无限加载。 默认是。

## 构建
```
npm run build
```

## 发布到 npm
```
npm run build
npm publish --access public
```

## License
[MIT](http://opensource.org/licenses/MIT)
