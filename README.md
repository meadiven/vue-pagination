# vue-mobile-calendar

![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg) ![Github file size](https://img.shields.io/badge/size-7kb-brightgreen.svg)
![npm](https://img.shields.io/badge/npm-6.3.0-red.svg)
![node.js](https://img.shields.io/badge/node.js-v10.14.2-blue.svg)

>基于 Vue 的PC端分页组件

## How to use

```bash
npm i -S v-pc-pagination
```
Global regisiter
```js
import Vue from 'vue'
import VuePagination from 'v-pc-pagination'
Vue.use(VuePagination)
```

OR in component

```js
import VuePagination from 'v-pc-pagination'

// in vue component

export default {
  components: { VuePagination },
  methods: {
      pageChange (page) {
        console.log(page);
      }
  }
}
```
```xml
<vue-pagination :total="total" @change="pageChange"></vue-pagination>
```
## Props

| name    | type    | desc                                     | default  |
| ------- | ------- | ---------------------------------------- | -------  |
| total   | Number  | Total page number                        | required |
| current | Number  | Current page number                      | 1        |
| show    | Number  | Display Page number: odd number and >= 5 | 5        |

## Event

| name    | params | desc                              |
| ------- | ------ | --------------------------------- |
| @change | index  | The page index which user clicked |