在 Vue.js 应用中，路由管理是通过 Vue Router 实现的。Vue Router 是官方提供的路由解决方案，专为 Vue.js 设计。它可以帮助你在单页应用（SPA）中实现页面导航和视图切换。

### Vue Router 的核心概念

1. **路由配置**：定义应用中各个页面的路径和对应的组件。通过配置路由，你可以指定当用户访问某个 URL 时，应该渲染哪个组件。

2. **路由视图（`<router-view>`）**：这是一个 Vue 组件，用于在页面中显示当前路由匹配的组件。你可以在主组件的模板中放置 `<router-view>`，以渲染匹配的组件。

3. **路由链接（`<router-link>`）**：用于创建导航链接。点击这些链接会根据路由配置切换到相应的组件。

### 基本用法

1. **安装 Vue Router**

   如果你使用 Vue CLI 创建项目，Vue Router 通常会作为一个选项被包括在内。如果没有，你可以通过 npm 或 yarn 安装：

   ```bash
   npm install vue-router
   ```

2. **创建路由配置**

   创建一个 `router/index.js` 文件（或其他你喜欢的文件名），并设置路由配置：

   ```javascript
   import Vue from 'vue';
   import Router from 'vue-router';
   import Home from '@/components/Home.vue';
   import About from '@/components/About.vue';

   Vue.use(Router);

   export default new Router({
     mode: 'history', // 使用 HTML5 History 模式（可选）
     routes: [
       {
         path: '/',
         name: 'Home',
         component: Home
       },
       {
         path: '/about',
         name: 'About',
         component: About
       }
     ]
   });
   ```

3. **在主应用中使用路由**

   在 `main.js` 文件中引入并使用路由：

   ```javascript
   import Vue from 'vue';
   import App from './App.vue';
   import router from './router';

   new Vue({
     router,
     render: h => h(App)
   }).$mount('#app');
   ```

4. **使用路由视图和链接**

   在你的主组件（如 `App.vue`）中使用 `<router-view>` 和 `<router-link>`：

   ```html
   <template>
     <div id="app">
       <nav>
         <router-link to="/">Home</router-link>
         <router-link to="/about">About</router-link>
       </nav>
       <router-view></router-view>
     </div>
   </template>
   
   <script>
   export default {
     name: 'App'
   }
   </script>
   ```

### 路由导航

Vue Router 支持多种导航模式，比如：

- **编程式导航**：通过调用 `this.$router.push` 或 `this.$router.replace` 方法进行导航。

  ```javascript
  this.$router.push('/about');
  ```

- **路由守卫**：用于在路由跳转前或跳转后进行一些操作，比如权限验证。可以在全局、路由级别或组件级别设置路由守卫。

  ```javascript
  router.beforeEach((to, from, next) => {
    // 逻辑处理
    next();
  });
  ```

### 动态路由

你可以在路由中使用动态参数：

```javascript
{
  path: '/user/:id',
  name: 'User',
  component: User
}
```

在组件中，你可以通过 `$route.params` 访问这些动态参数：

```javascript
computed: {
  userId() {
    return this.$route.params.id;
  }
}
```

Vue Router 是一个功能强大的工具，它不仅支持基本的路由配置，还提供了许多高级功能来处理复杂的路由需求。