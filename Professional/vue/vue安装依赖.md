## 1、vue-Router

在 Vue 后面加载 `vue-router`，它会自动安装的：

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vue-router.js"></script>
```

### [#](https://router.vuejs.org/zh/installation.html#npm)NPM

```bash
npm install vue-router
```

如果在一个模块化工程中使用它，必须要通过 `Vue.use()` 明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

## 2、axios

Using npm:

```
$ npm install axios
```

Using bower:

```
$ bower install axios
```

Using cdn:

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

## 3、sass-loader

```bash
npm install sass-loader node-sass webpack --save-dev
```

## 4、vuex

在 Vue 之后引入 `vuex` 会进行自动安装：

```html
<script src="/path/to/vue.js"></script>
<script src="/path/to/vuex.js"></script>
```

### [#](https://vuex.vuejs.org/zh/installation.html#npm)NPM

```bash
npm install vuex --save
```

### [#](https://vuex.vuejs.org/zh/installation.html#yarn)Yarn

```bash
yarn add vuex
```

在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex：

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

