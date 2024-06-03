## 探索 Next.js：从入门到精通

### 目录

1. 什么是 Next.js？
2. 创建第一个 Next.js 应用
3. 项目结构介绍 
4. 路由与导航
5. 数据获取（SSR 和 SSG）
6. API 路由
7. 样式与 CSS
8. 部署 Next.js 应用


怎么让大家抛开传统的单应用框架去认识next.js
  1. 单页面应用，使用率比较高的框架 Vue、React
  2. 数据驱动视图 + 组件化开发 + 路由管理 + 状态管理
  3. next.js 数据驱动视图 + 组件化开发 + 路由管理 + 状态管理 + 服务端渲染

### 1. 什么是 Next.js？

1. 什么是 Next.js
  Next.js 是一个基于 React 的开源框架，由 Vercel 开发和维护。它用于构建 Web 应用程序，特别是支持服务器端渲染（SSR）和静态网站生成（SSG）。



2. next.js的优势 ， 解决了什么问题
  - 服务器端渲染（SSR）
    问题：React 应用默认是客户端渲染，这意味着整个页面在客户端生成。这可能导致较慢的初始加载时间和不佳的 SEO 性能，因为搜索引擎爬虫在抓取页面时可能无法执行 JavaScript。
    Next.js 解决方案：使用服务器端渲染功能，使得页面在服务器上预先生成 HTML，然后发送到客户端。这不仅提高了初始加载速度，还改善了 SEO 性能

  - 静态生成（SSG）
    问题：为了提高性能和减少服务器负载，需要在构建时预先生成静态 HTML 文件，但手动实现这个过程复杂且容易出错。
    Next.js 解决方案：内置静态生成功能，允许在构建时预先生成 HTML 页面，并在请求时直接提供静态内容。

  - 路由配置
    问题：在传统 React 应用中，开发者需要手动配置路由（每开发一个页面需要配置一次路由）。
    Next.js 解决方案：使用基于文件系统的路由，每个页面文件自动对应一个 URL 路由，简化了路由配置和管理，能直观的体现。

  - 代码分割和性能优化
    问题：手动配置代码分割和性能优化需要额外的工具和复杂的配置，如 Webpack。
    Next.js 解决方案：自动进行代码分割，只加载当前页面所需的代码，并内置多种性能优化功能，如图片优化、静态文件缓存等，减少开发者的配置负担。

    为什么next支持这些？？？

  - API 路由
    问题：前后端分离的应用需要单独设置后端服务器来处理 API 请求
    Next.js 解决方案：提供内置的 API 路由功能，开发者可以在 pages/api 目录下创建 API 端点，从而在同一个项目中处理前端页面和后端 API 请求，简化了开发和维护。


5. 什么场景下可以使用next.js
    - 博客
      Next.js 的静态生成功能非常适合 CMS 和博客网站，因为这些网站通常具有大量的静态内容，需要良好的 SEO 支持。例如，使用 Next.js 构建的博客可以在构建时生成静态页面，确保快速加载和良好的搜索引擎排名。
    - 企业网站和营销页面
      企业网站和营销页面需要快速加载和 SEO 优化，以提升品牌形象和吸引潜在客户。Next.js 可以通过静态生成和服务器端渲染来优化这些页面的加载速度和搜索引擎可见性。

    - 动态 Web 应用和 SPA
      Next.js 支持混合静态生成和服务器端渲染，可以在同一个应用中处理静态内容和动态内容。这使得它非常适合构建复杂的单页应用（SPA），如社交媒体平台、在线教育平台和仪表盘应用。

6. 用next.js开发的网站
  - Vercel

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

### 8. 部署 Next.js 应用

#### 部署到 Vercel

Vercel 是 Next.js 团队提供的平台，可以无缝部署 Next.js 应用。

1. 安装

Vercel CLI：

```bash
npm install -g vercel
```

2. 部署应用：

```bash
vercel
```

按照提示进行操作，几秒钟后你的应用就会在 Vercel 上部署完毕。

### 结语

通过这篇博客，我们快速浏览了 Next.js 的基本概念和功能，从安装、路由、数据获取到 API 路由和样式处理，再到最后的部署。Next.js 是一个强大且灵活的框架，适合从小型项目到大型企业级应用的开发。

#### 总结

1. **项目结构**：了解 Next.js 项目的基本目录和文件结构。
2. **页面创建**：创建基本的静态页面和动态页面。
3. **路由和导航**：使用文件系统路由和动态路由，掌握 Link 组件的使用。
4. **数据获取**：了解静态生成（SSG）和服务端渲染（SSR）的基本用法。
5. **API 路由**：在 Next.js 中创建和使用 API 路由。
6. **样式处理**：使用全局 CSS 和 CSS 模块进行样式管理。
7. **部署**：将 Next.js 应用部署到 Vercel 平台。
