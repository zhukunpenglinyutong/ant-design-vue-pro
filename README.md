# ant-design-vue-pro

学习版本，持续优化（此项目主要是Vue的全家桶的应该用，Vue-Cli，Vue-Router，Vuex，Axios）

---

### 1.初始化项目

- vue create ant-design-vue-pro
- 选中 babel, router, vuex, c2ss pre-precessors, linter/formatter, unit testing
- npm i ant-design-vue moment


---

###  2.自定义 webpack（更多配置，可以看Vue-Cli链接）

```js
// 引入ant-design-vue | src/main.js
import antd from 'ant-design-vue'
import 'ant-design-vue/dist/antd.less'

Vue.use(antd)
```

会报错，这时候就需要配置 webpack 配置了 [vue-cli链接](https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE)

```js
// 新建 vue.config.js，自定义 webpack 配置 : 
module.exports = {
  css: {
    loaderOptions: {
      less: {
        javascriptEnabled: true // 可以加载 .less
      }
    }
  }
}
```

---

### 3.如何设计一个搞拓展性的路由配置

**登录和注册路由**

```js
// router.js | 定义路由做路由匹配的
import Vue from "vue";
import Router from "vue-router";

Vue.use(Router);

const router = new Router({
    mode: "history",
    base: process.env.BASE_URL,
    routes: [
        {
            path: "/user",
            component: () => import(/* webpackChunkName: "layout" */ "./layouts/UserLayout"),
            children: [
                {
                path: "/user/login",
                name: "login",
                component: () =>
                    import(/* webpackChunkName: "user" */ "./views/User/Login")
                },
              ]
        }
    ]
})

```

---

```html
<!-- 匹配上路由规则的模板，放到这（路由占位符）里面渲染 -->
<router-view></router-view>

<!-- 路由标签跳转 -->
<router-link to="/"><a-button size="large">返回首页</a-button></router-link>

```

---


```js
// src/main.js
new Vue({
    router,
})

// 路由的内部（this是Vue实例）
this.$router // 能访问到所有的内容


```

---

**路由全局守卫**

```js
const router = new Router({...})

// 当路由切换的时候，会先经过路由守卫里面的逻辑
// to 我们将要去的路由，from 当前的路由，next 调用这个才会继续往下走

// 开始全局路由
router.beforeEach((to, from, next) => {
    xxx.start() // 页面加载等待动画 开始 示例
    next()
})

// 结束全局路由
router.afterEach(() => {
    xxx.end() // 页面加载等待动画 结束 示例
    next()
})
```

---

### 4.Vuex的设计

```js

```

---

### 5.如何高效的使用Mock数据进行开发

**新建文件夹 mock**

- 之前我是真用 mock + Vue-Cli 中的 vue.config.js 的devServer 里面的 proxy 来设置
- 后面直接启动一个node serve ，更加的方便和更真实

---

### 6.如何与服务端进行交互（axios）