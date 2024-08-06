# level-1

## vite

- 新建项目`npm create vite@latest .`
- `@`路径别名

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue(),],
    resolve: {
    alias: [
      {
        find: '@',
        replacement: '/src'
      }
    ]
  }
})
```

## element框架

导入方法参考[快速开始 | Element Plus (element-plus.org)](https://element-plus.org/zh-CN/guide/quickstart.html)


## Pinia 

`npm install pinia`

```js
// vue加载pinia 
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'

createApp(App).use( createPinia() ).mount('#app')
```



```javascript
//Pinia store 模板
// ./src/stores/dialogVisiable.js

import { defineStore } from 'pinia';
import { ref } from 'vue'

export const useDialogVisiableStore = defineStore('dialogVisiable', () => {
    
    const upload = ref(false)//原名ImageDialog
    const analyse = ref(false)
    const loading = ref(false)
    const params = ref(false)
    const edit = ref(false)
    const showCropper = ref(false)
    
    return { upload, analyse, loading, params, edit, showCropper }
})
```

```vue
<script setup>
import { useDialogVisiableStore } from '@/stores/dialogVisiable'
const dialogVisiableStore = useDialogVisiableStore()
</script>

<template>
<button @click="dialogVisiableStore.upload = true"> </button>
</template>

```

## vue router

安装vue router：[安装 | Vue Router (vuejs.org)](https://router.vuejs.org/zh/installation.html)

```js
// ./src/router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'

const router = createRouter(
    {
        history:createWebHistory(),
        routes:[
            {
                path:'/',
                name:'home',
                component: Home
            }
        ]
)
export default router;
```

```js
// ./src/main.js
import { createApp } from 'vue'
import router from './router'


const app = createApp(App)
    .use(router)
    .mount('body')
```

```vue
<template>
    <router-view />
</template>
```

## sass

1. 安装

   1. 使用sass：`npm install node-sass sass-loader --save-dev`，注意node-sass是node环境的sass编译器，sass-loader是webpack对sass的加载器，他们只在编译时发生作用，所以用--save-dev，不要装到运行时环境去了。
   1. node-sass似乎要改成sass？

2. `/deep/`报错

   1. 把`/deep/`改成`::v-deep`或者`:deep`。
   2. 最新版本是`:deep()`


## node

1. 高版本没有require的问题

   1. 原来是这样导入的`const ImageEditor = require("tui-image-editor");`

   2. 去node_moudules找到这个包，是这样导出的：

      ```js
      declare module 'tui-image-editor' {
        export = tuiImageEditor.ImageEditor;
      }
      ```

   3. 所以改成这样的导入：`import {ImageEditor} from 'tui-image-editor'`
