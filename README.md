# Vue3CompositionAPI

## 第一章 最终效果演示

### 1. 下载依赖

```bash
npm install
```

### 2. 启动前端

```bash
npm run dev
```

### 3. 启动数据

```bash
json-server --watch data/db.json --port=3003
```

## 第二章 创建项目

### 1. vite 创建项目

```bash
npm init vite@latest vite-blog -- --template vue
```

### 2.App.vue

```vue
<script setup></script>

<template>
  <div>App.vue</div>
</template>

<style></style>
```

### 3. Home.vue

    1. 测试setup的执行周期

```vue
<template>
  <div class="home">Home</div>
</template>

<script>
export default {
  name: "Home",
  setup() {
    console.log("setup");
  },
  created() {
    console.log("setup");
  },
  mounted() {
    console.log("mounted");
  },
};
</script>
```

### 第三章 组件嵌套

### 1. 创建 views/Home.vue

### 2. App.vue

```vue
import "Home" from './views/Home.vue'
```

### Home.vue 定义属性

```vue
<template>
  <div class="home">
    <h1>Home</h1>
    <p>我的名字是: {{ name }} ,我的年龄是 {{ age }}</p>
    <button @click="handleClick">click me</button>
  </div>
</template>

<script setup>
console.log("setup");

let name = "米修在线";
let age = 30;

const handleClick = (e) => {
  console.log("you clicked me");
};
</script>
```

<div style="page-break-after: always;"></div>

## 第三章 Refs

### 1. Refs

​ \* 在 CompositionAPI 中,是不能使用 this 的, 所以也无法使用 this.$refs

```vue
<template>
  <div class="home">
    <h1>Home</h1>
    <p ref="p">我的名字是: {{ name }} 我的年龄是: {{ age }}</p>
    <button @click="handleClick">click me</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

console.log(this);

const p = ref(null);
// const p = ref("hello")

let name = "mario";
let age = 30;

const handleClick = (e) => {
  console.log(p, p.value);
  p.value.classList.add("test");
  p.value.textContent = "hello, world";
};

console.log(p.value);
</script>
```

<div style="page-break-after: always;"></div>

## 第四章 Refs 数据响应

### 1.Ref(数据驱动)

    * 未使用ref定义的数据 不能与DOM进行响应
    * 通过ref定义的数据 可以实现实时响应 跟(data)一样

```vue
<template>
  <div class="home">
    <h1>Home</h1>
    <p>My name is {{ name }} and my age is {{ age }}</p>
    <button @click="handleClick">click me</button>

    <input type="text" v-model="name" />
    <button @click="age++">add 1 to age</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

// // 非ref属性(不响应)
// const name = "mario";
// const age = 30;

// // 通过事件修改值(不响应)
// const handleClick = (e) => {
//   name = "luigi";
//   age = 35;
// };

// ref(响应)
const name = ref("mario");
const age = ref(30);

// 修改值(响应)
const handleClick = (e) => {
  name.value = "luigi";
  age.value = 35;
};
</script>
```

<div style="page-break-after: always;"></div>

## 第五章 Ref vs Reactive

### 1.引入 reactive

```js
import { reactive, ref } from "vue";
```

### 2.Home.vue

```html
<template>
  <div class="home">
    <h1>Home</h1>
    <h2>Refs</h2>
    <p>{{ promiseOne.name }} - {{ promiseOne.age }}</p>
    <button @click="updatePromiseOne">Update promise one</button>
    <h2>Reactive</h2>
    <p>{{ promiseTwo.name }} - {{ promiseTwo.age }}</p>
    <button @click="updatePromiseTwo">Update promise two</button>
  </div>
</template>

<script setup>
  import { reactive, ref } from "vue";

  const promiseOne = ref({ name: "mario", age: 30 });
  const promiseTwo = reactive({ name: "luigi", age: 35 });

  const updatePromiseOne = () => {
    promiseOne.value.age = 40;
  };
  const updatePromiseTwo = () => {
    promiseTwo.age = 45;
  };
</script>
```

### 3. Ref 和 Reactive 的区别

​ \* 个人建议是尽量使用 ref

```js
// ref和reactive的区别
const nameOne = ref("misterwu");
const nameTwo = reactive("promise");
// 对于基本数据类型来讲, reactive定义的,无法实现数据驱动的响应

// 实现值的更新(reactive)
const updatePromiseTwo = () => {
  nameTwo = "米修";
};

// 3. 获取测试
<p>{{ nameTwo }}</p>;
```

<div style="page-break-after: always;"></div>

## 第六章 计算属性

### 1. computed 的基本写法

```vue
<template>
  <div class="home">
    <h1>{{ name }}</h1>
  </div>
</template>

<script setup>
import { computed } from "vue";

// computed的基本语法
const name = computed(() => {
  return "米斯特吴";
});
</script>
```

### 2. 实现搜索功能

```vue
<template>
  <div class="home">
    <input type="text" v-model="search" />
    <p>search term - {{ search }}</p>

    <div v-for="name in matchingNames" :key="name">{{ name }}</div>
  </div>
</template>

<script setup>
import { computed, reactive, ref } from "vue";

const names = ref(["小猪", "小狗", "小猫", "小爱", "小狼", "小虎", "狮子王"]);
const search = ref("");

const matchingNames = computed(() => {
  return names.value.filter((name) => name.includes(search.value));
});
</script>
```

<div style="page-break-after: always;"></div>

## 第七章 watch & watchEffect

### 1. 监听 & 停止监听

```vue
<template>
  <div class="home">
    <h1>Home</h1>
    <input type="text" v-model="search" />
    <p>search term - {{ search }}</p>
    <div v-for="name in matchingNames" :key="name">{{ name }}</div>

    <button @click="handleClick">stop watching</button>
  </div>
</template>

<script setup>
import { computed, reactive, ref, watch, watchEffect } from "vue";

const search = ref("");
const names = ref(["小猪", "小狗", "小猫", "小爱", "小狼", "小虎", "狮子王"]);

const matchingNames = computed(() => {
  return names.value.filter((name) => name.includes(search.value));
});

watch(search, () => {
  console.log("watch function ran");
});

// watchEffect
watchEffect(() => {
  console.log("watchEffect ran"); // 仅初始化一次
  console.log("watchEffect ran", search.value); // 每次search属性发生变化都触发
});

// 停止监听 watch
const stopWatch = watch(search, () => {
  console.log("watch function ran");
});

// 停止监听 watchEffect
const stopEffect = watchEffect(() => {
  console.log("watchEffect ran", search.value);
  console.log(names.value);
});

const handleClick = () => {
  stopWatch();
  stopEffect();
};
</script>
```

<div style="page-break-after: always;"></div>

## 第八章 属性 Props

### 1.Home.vue

​ 配置 Home.vue 的基本形态

```html
<template>
  <div class="home">
    <h1>Home</h1>
  </div>
</template>

<script setup>
  import { ref } from "vue";
  const posts = ref([
    { title: "跟老吴学前端", body: "Lorem ipsum .", id: 1 },
    { title: "不跟老吴学前端", body: "lorem ipsum", id: 2 },
  ]);
</script>
```

### 2. PostList.vue

​ \*创建 components/PostList.vue

```vue
<template>
  <div class="post-list"></div>
</template>

<script setup></script>

<style></style>
```

### 3. Home.vue (属性 props)传值

```vue
<script setup>
import PostList from "../components/PostList.vue";
import { ref, reactive, computed, watch, watchEffect } from "vue";

const posts = ref([
  { title: "跟米斯特吴学习全新的Vue.js", body: "vue", id: 1 },
  {
    title: "跟米斯特吴学习全新的React.js",
    body: "react Lorem ipsum!",
    id: 2,
  },
]);
</script>

<template>
  <div class="home">
    <PostList :posts="posts" />
  </div>
</template>

<style></style>
```

### 6. PostList.vue 使用 SinglePost 组件

```vue
<script setup>
import SinglePost from "./SinglePost.vue";

defineProps({
  posts: Array,
});
</script>

<template>
  <div class="post-list">
    <div v-for="post in posts" :key="post.id">
      <SinglePost :post="post" />
    </div>
  </div>
</template>

<style></style>
```

### 7. SinglePost.vue 渲染数据

```vue
<script setup>
import { computed } from "vue";

const props = defineProps({
  post: Object,
});

const snippet = computed(() => {
  return props.post.body.substring(0, 100) + "...";
});
</script>

<template>
  <div class="post">
    <h3>{{ post.title }}</h3>
    <p>{{ snippet }}</p>
  </div>
</template>

<style></style>
```

<div style="page-break-after: always;"></div>

## 第九章 生命周期钩子函数

### 1. PostList.vue

```vue
<script setup>
import SinglePost from "./SinglePost.vue";
import { onMounted, onUnmounted, onUpdated } from "vue";

onMounted(() => {
  //   alert("页面渲染之前执行,执行完,页面就出来了");
  /** 数据请求 */
});

// watchEffect()

onUnmounted(() => {
  //   alert("组件注销之前执行,执行完组件就不在页面显示了");
  // better-scroll 销毁掉三方插件实例的一些对象
});

onUpdated(() => {
  alert("当组件内的内容发生变化, 就会执行这个更新钩子函数");
});

defineProps({
  posts: Array,
});
</script>

<template>
  <div class="post-list">
    <div v-for="post in posts" :key="post.id">
      <SinglePost :post="post" />
    </div>
  </div>
</template>

<style></style>
```

### 2. Home.vue

```html
<script setup>
  import PostList from "../components/PostList.vue";
  import { ref, reactive, computed, watch, watchEffect } from "vue";

  const posts = ref([
    { title: "跟米斯特吴学习全新的Vue.js", body: "vue", id: 1 },
    {
      title: "跟米斯特吴学习全新的React.js",
      body: "react Lorem ipsum!",
      id: 2,
    },
  ]);

  const showPosts = ref(true);
</script>

<template>
  <div class="home">
    <PostList :posts="posts" v-if="showPosts" />
    <button @click="showPosts = !showPosts">显示/隐藏 PostList组件</button>

    <button @click="posts.pop()">删除一个博客信息</button>
  </div>
</template>

<style></style>
```

<div style="page-break-after: always;"></div>

## 第十章 模拟数据

### 1. data/db.json

```json
{
  "posts": [
    {
      "title": "跟米斯特吴学全新版本的Vue.js",
      "body": "本课程指南主要是为有 Vue 2 经验的、希望学习 Vue 3 的新功能和更改旧项目的用户而提供的。",
      "id": 1,
      "tags": ["vue3", "compositionApi", "blog"]
    },
    {
      "title": "跟米斯特吴学全新版本的React.js",
      "body": "用于构建用户界面的 JavaScript 库",
      "id": 2,
      "tags": ["react", "redux", "hooks"]
    },
    {
      "id": 3,
      "title": "跟米斯特吴学全新版本的Angular",
      "body": "学会用 Angular 构建应用，然后把这些代码和能力复用在多种多种不同平台的应用上 —— Web、移动 Web、移动应用、原生应用和桌面原生应用。",
      "tags": ["angular", "rxjs", "typescript"]
    },
    {
      "id": 4,
      "title": "跟米斯特吴学全新版本微信小程序",
      "body": "微信小程序，小程序的一种，英文名Wechat Mini Program，是一种不需要下载安装即可使用的应用，它实现了应用“触手可及”的梦想，用户扫一扫或搜一下即可打开应用。",
      "tags": ["微信", "小程序", "wechat"]
    },
    {
      "id": 5,
      "title": "跟米斯特吴学全新版本的uniapp",
      "body": "uni-app 是一个使用 Vue.js 开发所有前端应用的框架，开发者编写一套代码，可发布到iOS、Android、Web（响应式）、以及各种小程序（微信/支付宝/百度/头条/QQ/钉钉/淘宝）、快应用等多个平台。",
      "tags": ["uniapp", "vue", "小程序", "iOS", "安卓"]
    }
  ]
}
```

### 2. 安装 json-server

```bash
npm install json-server
```

```bash
json-server --watch data/db.json
```

## 第十一章 axios 请求数据

### 1. Home.vue

- 安装 axios

```bash
npm install axios
```

- 请求数据

```vue
<script setup>
import PostList from "../components/PostList.vue";
import { ref, reactive, computed, watch, watchEffect } from "vue";
import axios from "axios";

const posts = ref([]);

const load = async () => {
  try {
    let { data } = await axios("http://localhost:3003/posts");
    posts.value = data;
  } catch (error) {
    console.log(error);
  }
};

load();

const showPosts = ref(true);
</script>

<template>
  <div class="home">
    <div v-if="posts.length">
      <PostList :posts="posts" v-if="showPosts" />
    </div>
    <div v-else>加载中....</div>
  </div>
</template>

<style></style>
```

## 第十二章 复用组件 composables

### 1. 将 Home.vue 中的 load 抽离

    * src/composables 文件夹
    * composables/getPosts.js
    * 将Home.vue中setup的内容抽离过去

### 2. getPost.js

```js
import { ref } from "vue";

const getPosts = () => {
  const posts = ref([]);
  const error = ref(null);

  const load = async () => {
    try {
      let data = await fetch("http://localhost:3000/posts");
      if (!data.ok) {
        throw Error("no available data");
      }
      posts.value = await data.json();
    } catch (err) {
      error.value = err.message;
    }
  };

  return { posts, error, load };
};

export default getPosts;
```

### 3. Home.vue 实现复用

```vue
<script>
import PostList from "../components/PostList.vue";
import getPosts from "../composables/getPosts";

const { posts, load } = getPosts();

load();
</script>
```

## 第十三章 渲染标签

### 1. SinglePost.vue

```vue
<template>
  <div class="post">
    <h3>{{ post.title }}</h3>
    <p>{{ snippet }}</p>
    <span v-for="tag in post.tags" :key="tag"> #{{ tag }} </span>
  </div>
</template>
```

### 2. App.vue 的样式

```css
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
```

### 3. Home.vue 的样式

```html
<div v-if="posts.length" class="layout"></div>
```

```js
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}

.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
```

### 4. SinglePost.vue 的样式

```css
.post {
  margin: 0 40px 30px;
  padding-bottom: 30px;
  border-bottom: 1px dashed #e7e7e7;
}
.post h3 {
  display: inline-block;
  position: relative;
  font-size: 26px;
  color: white;
  margin-bottom: 10px;
  max-width: 400px;
}
.post h3::before {
  content: "";
  display: block;
  width: 100%;
  height: 100%;
  background: #ff8800;
  position: absolute;
  z-index: -1;
  padding-right: 40px;
  left: -30px;
}
```

## 十四章 创建导航

### 1. 安装路由

```bash
npm install vue-router
```

### 2. 创建路由文件 src/router/index.js

```js
import { createRouter, createWebHistory } from "vue-router";
import Home from "../views/Home.vue";

const routes = [{ path: "/", name: "Home", component: Home }];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

export default router;
```

### 3. main.js 挂载路由实例

```js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

createApp(App).use(router).mount("#app");
```

### 4. 创建导航 components/Navbar.vue

```vue
<template>
  <header>
    <h1>个人博客</h1>
    <nav>
      <router-link :to="{ name: 'Home' }">首页</router-link>
      <router-link :to="{ name: 'Home' }">新建</router-link>
    </nav>
  </header>
</template>

<script></script>

<style>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
header h1 {
  color: #dfdfdf;
  font-size: 48px;
}
header a {
  color: #bbb;
  text-decoration: none;
  margin-left: 20px;
}
header a.router-link-active {
  color: #444;
  font-weight: bold;
}
</style>
```

### 5. App.vue 渲染组件

```vue
<script setup>
import Navbar from "./components/Navbar.vue";
</script>

<template>
  <Navbar />
  <router-view></router-view>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
```

## 十五章 Details

### 1. SinglePost.vue 添加 router-link

```vue
<template>
  <div class="post">
    <router-link :to="{ name: 'Details', params: { id: post.id } }">
      <h3>{{ post.title }}</h3>
    </router-link>
    <p>{{ snippet }}</p>
    <span v-for="tag in post.tags" :key="tag"> #{{ tag }} </span>
  </div>
</template>
```

### 2. router/index.js 配置路由

```js
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
  },
  {
    path: "/posts/:id",
    name: "Details",
    component: Details,
    props: true,
  },
];
```

### 3. views/Details.vue

```vue
<template>Details</template>

<script>
export default {
  props: ["id"],
  setup(props) {},
};
</script>

<style scoped></style>
```

### 4. getPost.js

    * 详情页面获取单个数据信息

```js
import { ref } from "vue";

const getPost = (id) => {
  const post = ref(null);
  const error = ref(null);

  const load = async () => {
    try {
      let data = await fetch("http://localhost:3000/posts/" + id);
      if (!data.ok) {
        throw Error("That post does not exist");
      }
      post.value = await data.json();
    } catch (err) {
      error.value = err.message;
    }
  };

  return { post, error, load };
};

export default getPost;
```

### 5. Details.vue 引入并使用

```vue
<template>
  <div v-if="error">{{ error }}</div>
  <div v-if="post" class="post">
    <h3>{{ post.title }}</h3>
    <p class="pre">{{ post.body }}</p>
  </div>
  <div v-else>Loading...</div>
</template>

<script>
import getPost from "@/composables/getPost";

export default {
  props: ["id"],
  setup(props) {
    const { error, post, load } = getPost(props.id);

    load();

    return { error, post };
  },
};
</script>
```

### 6. 加入样式

```css
.tags a {
  margin-right: 10px;
}
.post {
  max-width: 1200px;
  margin: 0 auto;
}
.post p {
  color: #444;
  line-height: 1.5em;
  margin-top: 40px;
}
.pre {
  white-space: pre-wrap;
}
```

## 十六章 实现加载动画

### 1. 创建 components/Spinner.vue

```vue
<template>
  <div class="spin"></div>
</template>

<style>
.spin {
  display: block;
  width: 40px;
  height: 40px;
  margin: 30px auto;
  border: 3px solid transparent;
  border-radius: 50%;
  border-top-color: #ff8800;
  animation: spin 1s ease infinite;
}

@keyframes spin {
  to {
    -webkit-transform: rotateZ(360deg);
  }
}
</style>
```

### 2. Home.vue 引入 Spinner 加载动画

```vue
<script>
import Spinner from "../components/Spinner.vue";

export default {
  name: "Home",
  components: { PostList, Spinner },
};
</script>

<div v-else>
  <Spinner />
</div>
```

### 3.Details.vue 引入 Spinner 加载动画

```vue
<template>
  <div v-else>
    <Spinner />
  </div>
</template>

<script>
import Spinner from "../components/Spinner.vue";

export default {
  props: ["id"],
  components: { Spinner },
};
</script>
```

### 4. 代码控制延迟,显示加载动画

- 在 getPosts & getPost 里加入延迟代码

```js
const load = async () => {
    try {
      await new Promise(resolve => {
        setTimeout(resolve, 2000)
      })
    }
}
```

## 十七章 创建表单

### 1. 创建 views/Create.vue

### 2. router/index.js 配置路由

```js
{
  path: '/create',
  name: 'Create',
  component: Create
}
```

### 3. Create.vue 表单

```vue
<template>
  <div class="create">
    <form>
      <label>标题:</label>
      <input v-model="title" type="text" required />
      <label>内容:</label>
      <textarea v-model="body" required></textarea>
      <label>标签 (回车添加标签):</label>
      <input @keydown.enter.prevent="handleKeydown" v-model="tag" type="text" />

      <button>添加</button>
    </form>
  </div>
</template>
```

### 4. Create.vue 业务逻辑

```vue
<script>
import { ref } from "vue";

export default {
  setup() {
    const title = ref("");
    const body = ref("");
    const tags = ref([]);
    const tag = ref("");

    const handleKeydown = () => {
      if (!tags.value.includes(tag.value)) {
        tag.value = tag.value.replace(/\s/g, "");
        tags.value.push(tag.value);
      }
      tag.value = "";
    };

    return { body, title, tags, tag, handleKeydown };
  },
};
</script>
```

### 5.渲染 tag

```vue
<div v-for="tag in tags" :key="tag" class="pill">
  #{{ tag }}
</div>
<button>添加</button>
```

### 6.实现样式 Create.vue

```css
form {
  max-width: 480px;
  margin: 0 auto;
  text-align: left;
}
input,
textarea {
  display: block;
  margin: 10px 0;
  width: 100%;
  box-sizing: border-box;
  padding: 10px;
  border: 1px solid #eee;
}
textarea {
  height: 160px;
}
label {
  display: inline-block;
  margin-top: 30px;
  position: relative;
  font-size: 20px;
  margin-bottom: 10px;
}

button {
  display: block;
  margin-top: 30px;
  background: #ff8800;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 18px;
}
.pill {
  display: inline-block;
  margin: 10px 10px 0 0;
  color: #444;
  background: #ddd;
  padding: 8px;
  border-radius: 20px;
  font-size: 14px;
}
```

## 十九章 添加数据 fetch

### 1. 给 form 添加 submit 事件

```vue
<form @submit.prevent="handleSubmit">
```

### 2. setup 中实现 handleSubmit 方法

```js
const handleSubmit = async () => {
  const post = {
    id: Math.floor(Math.random() * 10000),
    title: title.value,
    body: body.value,
    tags: tags.value,
  };

  await fetch("http://localhost:3000/posts", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(post),
  });
};
return { handleSubmit };
```

### 3.useRouter 实现路由跳转

- Create.vue 引入并使用 useRouter

```vue
<script>
import { useRouter } from "vue-router";

export default {
  setup() {
    const router = useRouter();

    const handleSubmit = async () => {
      await fetch("http://localhost:3000/posts", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(post),
      });

      router.push({ name: "Home" });
    };
  },
};
</script>
```

### 4. useRoute

- Details.vue

```vue
<script>
import { useRoute } from "vue-router";

export default {
  props: ["id"],
  components: { Spinner },
  setup(props) {
    const route = useRoute();
    const { error, post, load } = getPost(route.params.id);

    load();

    return { error, post };
  },
};
</script>
```

## 二十章 展示侧边标签

### 1. 创建 components/TagCloud.vue

```vue
<template>
  <div class="tag-cloud">
    <h3>标签</h3>
    <div v-for="tag in tags" :key="tag">
      <router-link :to="{ name: 'Tag', params: { tag } }">
        #{{ tag }}
      </router-link>
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";

const tags = ref([]);
const tagSet = new Set();

props.posts.forEach((item) => {
  item.tags.forEach((tag) => tagSet.add(tag));
});

tags.value = [...tagSet];

const props = defineProps({
  posts: Array,
});
</script>

<style scoped>
.tag-cloud {
  padding: 10px;
}
.tag-cloud h3 {
  border-bottom: 1px solid #eee;
  padding: 16px 8px;
  color: #444;
}
.tag-cloud div {
  display: inline-block;
  padding: 10px;
}
.tag-cloud a {
  color: #ccc;
  text-decoration: none;
}
.tag-cloud a.router-link-active {
  color: #ff8800;
  font-weight: bold;
}
</style>
```

### 2. Home.vue

```vue
<script setup>
import PostList from "../components/PostList.vue";
import getPosts from "../composibles/getPosts";
import Spinner from "../components/Spinner.vue";
import TagCloud from "../components/TagCloud.vue";

const { posts, load } = getPosts();
load();
</script>

<template>
  <div class="home">
    <div v-if="posts.length" class="layout">
      <PostList :posts="posts" />
      <TagCloud :posts="posts" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<style scoped>
.home {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}

.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
</style>
```

## 第二十一章 创建 tag 标签路由跳转

- TagCloud.vue

```vue
 <router-link :to="{ name: 'Tag', params: { tag } }">
```

### 1. router/index.js

```js
{
  path: '/tags/:tag',
  name: 'Tag',
  component: Tag
}
```

### 2. 创建 views/Tag.vue

```vue
<script setup>
import Spinner from "../components/Spinner.vue";
import PostList from "../components/PostList.vue";

import getPosts from "../composibles/getPosts";
import { useRoute } from "vue-router";
import { computed } from "vue";

const route = useRoute();
const { posts, load } = getPosts();

load();

const postsWithTag = computed(() => {
  return posts.value.filter((p) => p.tags.includes(route.params.tag));
});
</script>

<template>
  <div class="tag">
    <div v-if="posts.length">
      <PostList :posts="postsWithTag" />
    </div>
    <div v-else>
      <Spinner />
    </div>
  </div>
</template>

<style>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>
```

## 第二十二章 展示底部的标签

    * 浏览器调试 点击标签 跳转到Tag页面 但Tag页面底部并没有 tag
    * 所以需要在Tag.vue中也引入TagCloud.vue

### 9. Tag.vue 引入 TagCloud.vue

```vue
<template>
  <div class="tag">
    <div v-if="posts.length">
      <PostList :posts="postsWithTag" />
      <TagCloud :posts="posts" />
    </div>
  </div>
</template>

<script>
import TagCloud from "../components/TagCloud.vue";

export default {
  components: { TagCloud },
};
</script>

<style>
.tag {
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px;
}
</style>
```

### 10. 将 tag 标签展示在右侧

    * Home.vue 添加class

```html
<div v-if="posts.length" class="layout">
  <PostList :posts="posts" />
  <TagCloud :posts="posts" />
</div>
```

- Home.vue 添加样式

```css
.layout {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: 20px;
}
```
