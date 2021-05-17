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
