1. Mint-UI
2. 9 宫格布局：

* 在 src/components/home 中使用

> Home.vue

```
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
  <li>6</li>
</ul>

.grid ul {
  margin: 0;
  padding: 0;
}
.grid li {
  list-style: none;
  float: left;
  width: 33.3%;
  backgorund-color: yellowgreen;
  text-align: center;
  height: 30px;
}
```

3. 组件封装

* 在 components 文件夹下新建 common
* 在 common 下新建 MyUl.vue

> MyUl.vue

```
<template>
  <ul>
    <slot></slot>
  </ul>
</template>
<script>
  export default {
    name: 'my-ul',
    data() {
      return {

      }
    }
  }
</script>
<style scoped>
ul {
  margin: 0;
  padding: 0;
}
</style>
```

MyLi.vue
```
<template>
  <li>
    <slot></slot>
  </li>
</template>
<script>
  export default {
    name: 'my-li',
    data() {
      return {

      }
    }
  }
</script>
<style scoped>
li {
  list-style: none;
  float: left;
  width: 33.3%;
  backgorund-color: yellowgreen;
  text-align: center;
  height: 30px;
}
</style>
```

* main.js 中引用自己的组件

> main.js

```
// 引入自己的 ul 和 li 组件
import MyUl from '@/components/Common/MyUl'
import MyLi from '@/components/Common/MyLi'

// 注册全局组件
Vue.component(MyUl.name, MyUl);
Vue.component(MyLi.name, MyLi);
```

* 在页面中使用

> Home.vue

```
<div class="grid">
  <my-ul>
    <!-- :key 相当于给数据 grids 的每个元素做标识，如果 Vue 监听到某个元素不在了，单纯对这个元素做操作，而不是重新渲染加载 DOM，即先操作虚拟 DOM，最后再渲染到真实 DOM 上面 -->
    <my-li v-for="(grid, index) in grids" :key="index">
      <a href="grid.url">
        <span :class="grid.className"></span>
        <span>{{grid.title}}</span>
      </a>
    </my-li>
  </my-ul>
</div>

data() {
  return {
    grids: [
      { className: "cms-news", title: "新闻资讯", url: "baidu.com" },
      { className: "cms-photo", title: "图文分享", url: "jsliang.top" },
    ]
  }
}
```

4. 重定向路由：

> router/index.js

routes: [
  {
    path: '/',
    redirect: { name: 'home' },
  }
]

5. 通过 `<router-link>` 导向页面：

> Home.vue

```
<router-link :to="grid.router">首页</router-link>

data() {
  return {
    grids: [
      {className: "cms-news", title: "新闻", router:{name: 'new.list'}}
    ]
  }
}
```

> router/index.js

```

import NewsList from "@/components/News/NewsList"

routers: [
  {
    name: "news.list",
    path: "/news/list",
    component: NewsList
  }
]
```

> News/NewsList.vue

```
……省略
```

6. 关于 `:key="**"`，如果数据有对应的 id，则用它的 id。

7. 时间过滤器：[Moments.js](http://momentjs.com/)

* 安装 Monents.js：`npm install moment --save`

> main.js

```
// 定义 moment 全局日期过滤器
import Moment from 'moment';

// {{ 'xxx' | converTime('YYYY-MM-DD') }}
// {{ 'xxx' | converTime('YYYY年MM月DD日') }}
Vue.filter("converTime", function(data, formatStr) {
  return Moment(data).format(formatStr);
});
```

> NewsList.vue

```
{{ news.add_time | converTime('YYYY-MM-DD') }}
```

8. 封装头部

> components/common/NavBar.vue

```
<template>
  <div>
    <span @click="goBack">返回</span>
    <span>{{title}}</span>
  </div>
</template>
<script>
  export default {
    name: 'nav-bar',
    props: ['title'],
    methods: {
      goBack() {
        this.$router.go(-1);
      }
    }
  }
</script>
<style scoped>

</style>
```

> src/main.js

```
// 引入自己的 ul 和 li 组件
import NabBar from '@/components/Common/NabBar'

// 注册全局组件
Vue.component(NabBar.name, NabBar);
```

> NewsList.vue

```
<nav-bar :title="新闻列表"></nav-bar>
```

9. 点击切换不同页面

> App.vue

```
@click = "changeHash"

methods: {
  changeHash() {
    console.log(this.selected);
  }
}
```