# j-list
[![npm](https://img.shields.io/npm/v/@joel007/list.svg) ![npm](https://img.shields.io/npm/dm/@joel007/list.svg)](https://www.npmjs.com/package/@joel007/list)
[![vue2](https://img.shields.io/badge/vue-2.x-brightgreen.svg)](https://vuejs.org/)

基于 [Vant](https://youzan.github.io/vant/) List 组件封装的 Vue.js 插件。

## Table of contents

- [安装](#安装)
- [使用](#使用)

## 安装

```
npm install --save @joel007/list
```

## 使用

```
<template>
<ul>
  <j-list 
    url="/singer/list"
  >
    <template slot-scope="scope" v-if="scope.data">
      <li>{{scope.data.name}}</li>
    </template>
  </j-list>
</ul>

</template>
<script>
import Vue from 'vue'
import jList from '@joel007/list'
Vue.use(jList)

export default {
  ...
}
</script>
```

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
