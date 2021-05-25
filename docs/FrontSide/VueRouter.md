# Vue-Router

---

?> 用 Vue.js + Vue Router 创建单页应用，感觉很自然：使用 Vue.js ，我们已经可以通过组合组件来组成应用程序，当你要把 Vue Router 添加进来，我们需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。

不过，首先来介绍一下**SPA**和**MPA**的区别吧。

<div style="margin-left:10%">

|                  | 单页面应用(SinglePage Web Application)  | 多页面应用(MultiPage Web Application) |
| :--------------: | :-------------------------------------: | :-----------------------------------: |
|       组成       |     一个外壳页面和多个页面片段组成      |           多个完整页面组成            |
| 资源公共(css,js) |        公用，只需要在外壳中加载         |       不共用，每个页面都要加载        |
|     刷新方式     |              页面局部刷新               |               整页刷新                |
|     url 模式     |    `a.com/#/pageone,a.com/#/pagetwo`    |   `a.com/one.html, a.com/two.html`    |
|     用户体验     |      页面片段间切换快，用户体验好       |       页面切换较慢，流畅度不够        |
|     转场动画     |                可以实现                 |               无法实现                |
|     数据传递     |           组件间数据传输容易            |      依赖缓存实现，或者 url 传参      |
|   搜索引擎优化   | 需要单独方案，实现较难，不利于 SEO 检索 |               容易实现                |
|     试用范围     |             追求页面流畅度              |         追求高度支持搜索引擎          |
|  开发、维护成本  |   开发成本较高，常借助框架；维护简单    |  开发成本低，但重复代码多；维护复杂   |

</div>

## router 基本配置

---

我们可以打开 Vue-CLI 创建的项目框架中的 router 文件夹中的`index.js`，

```js
import Vue from "vue";
import VueRouter from "vue-router"; //引入基础的模块
Vue.use(VueRouter); //注册成为路由模块
const route = [{}, {}]; //用于定义路由
const router = new VueRouter({
  route,
  /* 生成路由实例 */
});
export default router;
```

在这里举一个最简单的定义路由的例子：

```js
// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
const Foo = { template: "<div>foo</div>" };
const Bar = { template: "<div>bar</div>" };

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
const routes = [
  { path: "/foo", component: Foo },
  { path: "/bar", component: Bar },
];
```

## 声明式导航

---

那么在页面的 HTML 中，我们也有 2 个内置组件可以支持路由操作：

- ?> `<router-view>`: 路由出口,路由匹配到的组件将渲染在这里 (类似于 slot 插槽)

- ?> `<router-link>`: 路由导航，类似`<a>`标签，通过传入`to`属性来指定跳转路径；它默认被渲染成`<a>`，但是可以通过传入`tag`属性来改变；通过传入`active-class`属性来指定路径跳转后，该组件被加入的类名，可以通过该属性实现跳转高亮功能。

例子：

```html
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

## 路由重定向和别名

---

重定向也是通过 routes 配置来完成,下面例子是实现在路由不匹配的时候重定向：

```js
{
    path: '*',//通配符优先级最低
    redirect: '/film'
  }
```

“重定向”的意思是，当用户访问 /a 时，URL 将会被替换成 /b，然后匹配路由为 /b，那么“别名”又是什么呢？

**/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。**也就是说，访问`/b`相当于访问`/a`，`/b`是其绰号。

例子：

```js
const router = new VueRouter({
  routes: [{ path: "/a", component: A, alias: "/b" }],
});
```

## 嵌套路由

---

实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

```
/film/nowplaying                    /film/comingsoon
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Nowplaying   | |  +------------>  | | comingsoon  | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

那么借用嵌套路由`children`属性就可以实现，同时子组件在对应的 FILM 组件中的`<router-view>`中被渲染：

```js
{
    path: '/film',
    name: 'film',
    component: Film,
    children: [
      {
        /* 绝对路径写法 */
        path: '/film/nowplaying',
        component: nowplaying
      },
      {
        /* 相对路径写法：前面不能加斜杠 */
        path: 'comingsoon' ,
        component: comingsoon
      },
      {
        path: '',
        redirect: 'nowplaying'
      }
    ]
  },
```

## 编程式导航

---

除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

##### **#`router.push(Path)`方法**

?> **注意：在 Vue 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`。**

想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

##### **#`router.replace(Path)`方法**

跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

## 动态路由

---

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 Details 组件，对于所有 ID 各不相同的详情页面，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 来达到这个效果：

```js
{
    path: '/details/:id',
    name: 'details',
    component: Details
  },
```

一个“路径参数”使用冒号 : 标记。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。你可以在一个路由中设置多段“路径参数”，对应的值都会设置到 `$route.params` 中。例如：

<div style="margin-left:10%">

|             模式              |      匹配路径       |             $router.params             |
| :---------------------------: | :-----------------: | :------------------------------------: |
|        /user/:username        |     /user/evan      |         `{ username: 'evan' }`         |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |

</div>

## 命名路由

---

在跳转路由的时候，除了通过路径来识别路由，还可以通过名字来识别。我们可以在 `routes` 配置中给某个路由设置名称。

```js
const router = new VueRouter({
  routes: [
    {
      path: "/user/:userId",
      name: "user",
      component: User,
    },
  ],
});
```

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象：

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

`router.push`也一样：

```js
router.push({ name: "user", params: { userId: 123 } });
```

## 路由模式

---

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。即带一个**#**锚点。

如果不想要很丑的 hash，我们可以用路由的 history 模式，配置如下：

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

我们可以比对一下两种模式的路由原理：

<div style="margin-left:10%">
|          |      Hash 模式      |   History 模式    |
| :------: | :-----------------: | :---------------: |
| 切换页面 |    location.hash    | history.pushState |
|   监听   | window.onhashchange | window.onpopstate |
</div>

## 路由拦截

---

在项目中，我们经常会遇到这样的一种场景。比如在没有权限的情况下，不可以进入某些页面。那么在这种情况下，我们就需要用到路由拦截守卫了。`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。

##### 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫(在每一次路由跳转之前都会经过这个守卫)：

```js
const router = new VueRouter({
  routes,
});
router.beforeEach((to, from, next) => {
  /* 设置路由守卫 */
  //do something
});
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 `resolve` 完之前一直处于 等待中。

每个守卫有 3 个参数：

- ?> `to: Route`: 即将要进入的目标 _路由对象_

- ?> `from: Route`: 当前导航正要离开的路由

- ?> `next: Function`: 一定要调用该方法来 resolve 这个钩子。next 即放行，里面可以传入一个 url 来实现跳转。

##### 局部前置守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: "/foo",
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      },
    },
  ],
});
```

##### 组件内守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  },
};
```

## 路由懒加载

---

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

例子如下：

```js
{
    path: '/login',
    name: 'login',
    component: () => import('../views/Login.vue')
 }
```
