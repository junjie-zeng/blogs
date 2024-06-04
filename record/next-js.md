###  Next.js：学习分享

### 目录

1. 什么是 Next.js？
2. Next.js的优势 ， 解决了什么问题
2. 创建第一个 Next.js 应用
3. 项目结构介绍 
4. 路由与导航
5. 数据获取（SSR 和 SSG）
6. API 路由
7. 样式与 CSS






### 1. 什么是 Next.js？
   <img src="https://raw.githubusercontent.com/junjie-zeng/blogs/master/assets/images/next-ssr.png" />

    Next.js 是一个基于 React 的框架，由 Vercel 开发和维护。它用于构建 Web 应用程序，特别是支持服务器端渲染（SSR）和静态网站生成（SSG）。  

### 2. Next.js的优势 ， 解决了什么问题
  ## 服务器端渲染（SSR）
    问题：React、Vue 应用默认是客户端渲染，这意味着整个页面在客户端生成。
    现象：请求一个url得到的不是一个完整的页面，页面中的内容是由js发起异步请求，然后拿到数据再做页面渲染
    Next.js 解决方案：使用服务器端渲染，页面在服务器上预先生成 HTML，然后发送到客户端(页面可以更快的呈现出来，搜索引擎可以更容易的抓取页面内容)。

  ## 静态生成（SSG）
    问题：需要在构建时预先生成静态 HTML 文件。
    Next.js 解决方案：内置静态生成功能，可以在构建时预先生成 HTML 页面。

  ## 路由配置
    问题：在传统 Vue、React 应用中，开发者需要手动配置路由（每开发一个页面需要配置一次路由）。
    Next.js 解决方案：使用基于文件系统的路由，每个页面文件自动对应一个 URL 路由，简化了路由配置和管理，能直观的体现这个url的结构。

  ## 代码分割和性能优化
    问题：手动配置代码分割和性能优化需要额外的工具和复杂的配置，如 Webpack、Vite。
    Next.js 解决方案：自动进行代码分割，只加载当前页面所需的代码，并内置多种性能优化功能，如图片优化、静态文件缓存等。


  ## API 路由
    问题：前后端分离的应用需要单独设置后端服务器来处理 API 请求
    Next.js 解决方案：提供内置的 API 路由功能，开发者可以在 pages/api 目录下创建 API 端点，从而在同一个项目中处理前端页面和后端 API 请求。




### 2. 创建第一个 Next.js 应用

#### 安装 Next.js

```bash
npx create-next-app my-next-app
cd my-next-app
npm run dev
```

#### 创建页面

在 `pages/` 目录下创建 `about.js`：

```jsx
// pages/about.js
import React from 'react';

const About = () => (
  <div>
    <h1>About Page</h1>
    <p>This is the about page.</p>
  </div>
);

export default About;
```

访问 `http://localhost:3000/about` 可以看到新页面。

### 3. 项目结构介绍

一个典型的 Next.js 项目结构如下：

```
my-next-app/
├── pages/
│   ├── index.js
│   ├── about.js
│   ├── _app.js
│   └── api/
│       └── hello.js
├── public/
│   └── images/
├── styles/
│   ├── globals.css
│   └── Home.module.css
├── package.json
└── next.config.js
```

- `pages/`：页面组件，每个文件对应一个路由。
- `public/`：静态资源目录。
- `styles/`：全局和模块化 CSS 文件。



### 4. 路由与导航

#### 基本路由

文件系统路由，`pages/index.js` 对应 `/`，`pages/about.js` 对应 `/about`。

#### 动态路由

```jsx
// pages/user/[id].js
import { useRouter } from 'next/router';

const User = () => {
  const router = useRouter();
  const { id } = router.query;

  return (
    <div>
      <h1>User ID: {id}</h1>
    </div>
  );
};

export default User;
```

访问 `http://localhost:3000/user/123`，显示 `User ID: 123`。

#### 链接导航

```jsx
// pages/index.js
import Link from 'next/link';

const Home = () => (
  <div>
    <h1>Home Page</h1>
    <Link href="/about">
      <a>Go to About Page</a>
    </Link>
  </div>
);

export default Home;
```

### 5. 数据获取（SSR 和 SSG）

#### 静态生成（SSG）

```jsx
// pages/posts/[id].js
export async function getStaticPaths() {
  const paths = [{ params: { id: '1' } }, { params: { id: '2' } }];
  return { paths, fallback: false };
}

export async function getStaticProps({ params }) {
  const post = { id: params.id, title: `Post ${params.id}` };
  return { props: { post } };
}

const Post = ({ post }) => (
  <div>
    <h1>{post.title}</h1>
    <p>This is post {post.id}</p>
  </div>
);

export default Post;
```

#### 服务端渲染（SSR）

```jsx
// pages/profile.js
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/profile');
  const profile = await res.json();
  return { props: { profile } };
}

const Profile = ({ profile }) => (
  <div>
    <h1>{profile.name}</h1>
    <p>{profile.bio}</p>
  </div>
);

export default Profile;
```

### 6. API 路由

在 `pages/api` 目录下创建 API 路由：

```jsx
// pages/api/hello.js
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello, world!' });
}
```

访问 `http://localhost:3000/api/hello`，返回 `{"message":"Hello, world!"}`。

### 7. 样式与 CSS

#### 使用全局 CSS

在 `styles/globals.css` 中定义全局样式：

```css
/* styles/globals.css */
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}
```

在 `pages/_app.js` 中引入：

```jsx
// pages/_app.js
import '../styles/globals.css';

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />;
}

export default MyApp;
```

#### 使用 CSS 模块

```jsx
// styles/Home.module.css
.container {
  margin: 0 auto;
  padding: 2rem;
  max-width: 800px;
  text-align: center;
}

// pages/index.js
import styles from '../styles/Home.module.css';

const Home = () => (
  <div className={styles.container}>
    <h1>Home Page</h1>
  </div>
);

export default Home;
```

#### 总结

1. **项目结构**：了解 Next.js 项目的基本目录和文件结构。
2. **页面创建**：创建基本的静态页面和动态页面。
3. **路由和导航**：使用文件系统路由和动态路由，掌握 Link 组件的使用。
4. **数据获取**：了解静态生成（SSG）和服务端渲染（SSR）的基本用法。
5. **API 路由**：在 Next.js 中创建和使用 API 路由。
6. **样式处理**：使用全局 CSS 和 CSS 模块进行样式管理。




















### log



next.js是做什么的，首先我们要理解一个概念：
什么是服务端渲染？
什么是客户端渲染？
我们通常编写的 Vue 和 React 应用大多数都是单页面应用（SPA）。开发完成后，实际上只有一个页面，这种模式会带来两个不友好的问题：
1. 首屏加载过慢：由于只有一个页面，所以需要将所有的资源加载到这个页面当中，这会导致首屏加载时间过长（后续加载正常）。
2. SEO 不佳：因为数据是动态加载的，搜索引擎无法有效地抓取和索引页面内容。因此，对于需要良好 SEO 的网站（如博客、门户网站），单页面应用并不适合。

那么 Next.js 是如何解决这些问题的呢？首先，它是一个基于 React 的框架，支持服务器端渲染（SSR）。通过在服务器端预先渲染页面，Next.js 能够显著提升首屏加载速度，并且优化 SEO 表现。

接着我说一下它的优点：
1. 搭建便捷：Next.js 基于 React，拥有完善的架构，搭建起来非常轻松。内部许多配置都集成好了，无需额外配置。

2. 自带数据同步：实现服务器端渲染（SSR）时，服务端渲染的数据需要在客户端重新渲染，确保前后端数据同步。没有框架时，这一过程非常复杂。而 Next.js 提供了非常好的解决方法，自动实现前后端数据同步，减少了开发复杂度。

3. 丰富的插件：Next.js 形成了自己的生态系统，提供了丰富的插件，减少了我们重复造轮子的步骤。

4. 灵活的配置：Next.js 允许根据配置项的修改，快速生成我们需要的应用形式。例如，通过配置实现静态站点生成（SSG）。



怎么让大家抛开传统的单应用框架去认识next.js
  1. 单页面应用，使用率比较高的框架 Vue、React
  2. 数据驱动视图 + 组件化开发 + 路由管理 + 状态管理
  3. next.js 数据驱动视图 + 组件化开发 + 路由管理 + 状态管理 + 服务端渲染

搭建方式
1. 手动配置
2. 脚手架

### 需要展示什么的效果
1. next.js 脚手架的使用（通过什么方式去安装）

2. 项目结构介绍（介绍各个文件是做什么的）

3. 关于文件系统的路由
  - 文件系统路由的优势在哪里？
    简化了路由配置
    路由配置更加直观

  - 和其它单页面应用框架有什么不一样 ？
    vue、react 都是通过路由配置来管理路由的

4. 展示api路由

4. SSR渲染 （对比一下vue单页面的加载情况）
  - 什么是SSR
  - 它的作用是什么
  - 解决了什么问题，什么场景下可以使用

5. SSG
    - 什么是SSG
    - 它的作用是什么
    - 解决了什么问题，什么场景下可以使用

6. 使用Next.js 连接接数据库

7. 验证代码分割




6. 用next.js开发的网站
  - Vercel





### 什么场景下可以使用next.js
    - 博客
      Next.js 的静态生成功能非常适合 CMS 和博客网站，因为这些网站通常具有大量的静态内容，需要良好的 SEO 支持。例如，使用 Next.js 构建的博客可以在构建时生成静态页面，确保快速加载和良好的搜索引擎排名。
    - 企业网站和营销页面
      企业网站和营销页面需要快速加载和 SEO 优化，以提升品牌形象和吸引潜在客户。Next.js 可以通过静态生成和服务器端渲染来优化这些页面的加载速度和搜索引擎可见性。

    - 动态 Web 应用和 SPA
      Next.js 支持混合静态生成和服务器端渲染，可以在同一个应用中处理静态内容和动态内容。这使得它非常适合构建复杂的单页应用（SPA），如社交媒体平台、在线教育平台和仪表盘应用。


 缺点：由于需要在服务器端进行页面渲染，因此会增加服务器的负载，可能需要更高的服务器性能来应对
           由于每次页面交互都需要向服务器请求新的HTML页面，因此在一些情况下，交互体验可能不如以客户端渲染为主的单页应用
