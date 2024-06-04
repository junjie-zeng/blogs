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



今天分享的主题是nextjs
在知道next是做什么之前，首先我们要回顾一个概念：
什么是服务端渲染？
什么是客户端渲染？

--- 展示图片

服务端渲染（Server-Side Rendering，SSR）是指在服务器端生成页面内容，然后将最终的HTML发送给客户端进行显示。
客户端渲染（Client-Side Rendering，CSR）是指在客户端浏览器中使用JavaScript动态地构建页面中的内容。

--- 展示对应例子

我们通常编写的 Vue 和 React 应用大多数都是单页面应用（SPA）。开发完成之后，它实际上只有一个页面，那么这种模式会带来两个不友好的问题：
1. 首屏加载过慢：由于只有一个页面，所以需要将所有的资源都加载到这个页面当中，这会导致首屏加载时间过长这么一个现象（后续加载正常）。
2. SEO 不佳：因为数据是动态加载的，搜索引擎无法有效地抓取页面的内容。因此，对于需要良好 SEO 的网站，单页面应用并不适合。

那么 Next.js 是如何解决这些问题的呢？首先next，它是一个基于 React 的框架，支持服务器端渲染（SSR）。
通过在服务器端预先渲染页面，能够提升首屏加载速度，并且优化 SEO的 表现。



### 1. 什么是 Next.js？
   <img src="https://raw.githubusercontent.com/junjie-zeng/blogs/master/assets/images/next-ssr.png" />

   Next.js 是一个基于 React 的框架，用于构建高性能的 Web 应用。它提供了服务器端渲染（SSR）、静态生成（SSG）和其他现代 Web 开发所需的功能。  

### 2. Next.js的优势 ， 解决了什么问题
  ## 服务器端渲染（SSR）
    问题：React、Vue 应用默认是客户端渲染，这意味着整个页面在客户端生成（请求一个url得到的不是一个完整的页面，页面中的内容是由js发起异步请求，然后拿到数据再做页面渲染）。
    Next.js 解决方案：使用服务器端渲染，页面在服务器上预先生成 HTML，然后发送到客户端(页面可以更快的呈现出来，搜索引擎可以更容易的抓取页面内容)。

  ## 静态生成（SSG） ？？？
    问题：需要在构建时预先生成静态 HTML 文件。
    Next.js 解决方案：内置静态生成功能，可以在构建时预先生成 HTML 页面。

  ## 文件系统路由
    问题：在传统 Vue、React 应用中，开发者需要手动配置路由（每开发一个页面需要配置一次路由）。
    Next.js 解决方案：使用基于文件系统的路由，每个页面文件自动对应一个 URL 路由，它简化了路由配置，并且能直观的体现这个url的结构。

  ## 代码分割和性能优化 ？？？
    问题：手动配置代码分割和性能优化需要额外的工具和复杂的配置，如 Webpack、Vite。
    Next.js 解决方案：自动进行代码分割，只加载当前页面所需的代码，并内置多种性能优化功能，如图片优化、静态文件缓存等。


  ## API 路由 ？？？
    问题：前后端分离的应用需要单独设置后端服务器来处理 API 请求
    Next.js 解决方案：提供内置的 API 路由功能，开发者可以在 pages/api 目录下创建 API 端点，从而在同一个项目中处理前端页面和后端 API 请求。




### 2. 创建第一个 Next.js 应用

使用官方命令创建新项目：

```bash
npx create-next-app@latest my-next-app
cd my-next-app
npm run dev
```

这样，我们就有了一个基础的 Next.js 应用，可以在 `http://localhost:3000` 上访问。

### 4. 项目结构介绍

创建的 Next.js 项目包含以下目录和文件：

```
my-next-app/
├── .next
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── about/
│   │   └── page.tsx
├── pages/
│   ├── api/
│   │   └── hello.ts
├── public/
├── styles/
├── .gitignore
├── next.config.js
├── package.json
```

- **.next**：Next.js构建输出的默认目录，包含了编译后的页面、静态资源以及构建相关的文件。
- **app/**：存放应用的页面和组件，基于文件系统自动生成路由。
- **pages/api/**：用于创建 API 路由。
- **public/**：存放静态资源，如图片、字体等。
- **styles/**：存放样式文件。
- **next.config.js**：Next.js 配置文件。
- **package.json**：项目依赖和脚本。

### 5. 路由与导航

Next.js 使用文件系统路由。`app` 目录中的每个文件和文件夹都对应一个路由。

在Next.js中，页面路由是基于文件系统的，每个位于app目录下的React文件都会对应一个路由。例如，创建一个app/about/page.tsx文件，就会自动创建/about这个路由。通过这种方式可以方便地创建新页面，并且创建之后不需要手动配置路由表。

#### 基本用法

在 `app` 目录中创建文件和文件夹来定义路由：

文件系统路由，`app/page.tsx` 对应 `/`，`app/about/page.tsx` 对应 `/about`。

```bash
mkdir app
cd app
touch page.tsx about/page.tsx
```

#### app/page.tsx

```tsx
import React from 'react';

const HomePage = () => {
  return (
    <div>
      <h1>Welcome to Next.js!</h1>
      <p>This is the home page.</p>
    </div>
  );
};

export default HomePage;
```

#### app/about/page.tsx

```tsx
import React from 'react';

const AboutPage = () => {
  return (
    <div>
      <h1>About Us</h1>
      <p>This is the about page.</p>
    </div>
  );
};

export default AboutPage;
```

#### 动态路由

创建一个带有用户ID参数的用户详情页面。可以在app目录下创建一个文件夹，命名为user，并在该文件夹内创建一个文件，命名为[id].js。这里的方括号[id]表示一个动态参数，通过这种方式可以通过URL传递不同的用户ID


#### app/user/[id].tsx
```tsx
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

#### 导航

对于导航，可以使用内置的Link组件，这个组件会自动处理页面之间的导航，比如下面这个示例当用户点击Home和About链接时，Next.js会自动处理页面之间的切换和加载，而无需进行完整的页面刷新。 ？？？ 验证会不会重新刷新页面

使用 `Link` 组件进行导航：

```tsx
import Link from 'next/link';

const Navigation = () => {
  return (
    <nav>
      <ul>
        <li>
          <Link href="/">Home</Link>
        </li>
        <li>
          <Link href="/about">About</Link>
        </li>
      </ul>
    </nav>
  );
};

export default Navigation;
```

### 6. 数据获取（SSR 和 SSG）

Next.js 提供了多种数据获取方法，包括 SSR 和 SSG。

#### 使用 `getServerSideProps` 进行 SSR

getServerSideProps 是一个异步函数，使用时将它导出到你的页面组件中。当一个页面包含了 getServerSideProps 函数时，Next.js 将会在每次请求页面时调用此函数，并在服务器端执行它。在 getServerSideProps 函数内部，可以执行数据获取的逻辑，例如从数据库中获取数据、调用外部 API 等。然后，可以将获取到的数据作为 props 返回，这些 props 将会被注入到页面组件中进行渲染。

```tsx
import React from 'react';

export async function getServerSideProps() {
  const response = await fetch('https://api/data');
  const data = await response.json();

  return {
    props: { data },
  };
}

const SSRPage = ({ data }) => {
  return (
    <div>
      <h1>Server Side Rendered Page</h1>
      <p>Data: {JSON.stringify(data)}</p>
    </div>
  );
};

export default SSRPage;
```

#### 使用 `getStaticProps` 进行 SSG

getStaticProps 是一个异步函数，使用时可以将它导出到你的页面组件中。当一个页面包含了 getStaticProps 函数时，Next.js 将会在构建项目时调用此函数，并执行获取数据的逻辑。

在 getStaticProps 函数内部，你可以执行数据获取的逻辑，例如从数据库中读取数据、调用外部 API 等。然后，你可以将获取到的数据作为 props 返回，这些 props 将会被注入到预渲染的静态页面中。

```tsx
import React from 'react';

export async function getStaticProps() {
  const response = await fetch('https://api/data');
  const data = await response.json();

  return {
    props: { data },
    revalidate: 10, // 页面重新生成的时间间隔（秒）
  };
}

const SSGPage = ({ data }) => {
  return (
    <div>
      <h1>Static Generated Page</h1>
      <p>Data: {JSON.stringify(data)}</p>
    </div>
  );
};

export default SSGPage;
```

### 7. API 路由

Next.js 内置了 API 路由，可以轻松创建 API 端点。

#### 创建 API 路由

通过 API 路由，你可以轻松地与外部服务或第三方 API 进行交互。比如，你可以创建一个 API 路由来代理客户端请求，并将请求发送到外部服务以获取数据或执行其他操作。

在 `pages/api` 目录下创建 API 端点：

#### pages/api/hello.ts

```ts
export default function handler(req, res) {
  res.status(200).json({ message: 'Hello, world!' });
}
```

访问 `http://localhost:3000/api/hello`，可以看到返回的 JSON 数据。



#### 总结
1. **关于next**：了解 Next.js 是什么，解决了什么问题。
1. **项目结构**：了解 Next.js 项目的基本目录和文件结构。
3. **路由和导航**：使用文件系统路由和动态路由，掌握 Link 组件的使用。
4. **数据获取**：了解静态生成（SSG）和服务端渲染（SSR）的基本用法。
5. **API 路由**：在 Next.js 中创建和使用 API 路由。




















### log


今天分享的主题是nextjs
在知道next是做什么之前，首先我们要回顾一个概念：
什么是服务端渲染？
什么是客户端渲染？
我们通常编写的 Vue 和 React 应用大多数都是单页面应用（SPA）。开发完成后，实际上只有一个页面，这种模式会带来两个不友好的问题：
1. 首屏加载过慢：由于只有一个页面，所以需要将所有的资源加载到这个页面当中，这会导致首屏加载时间过长这么一个现象（后续加载正常）。
2. SEO 不佳：因为数据是动态加载的，搜索引擎无法有效地抓取和索引页面内容。因此，对于需要良好 SEO 的网站（如博客、门户网站），单页面应用并不适合。

那么 Next.js 是如何解决这些问题的呢？首先，它是一个基于 React 的框架，支持服务器端渲染（SSR）。通过在服务器端预先渲染页面，Next.js 能够显著提升首屏加载速度，并且优化 SEO 表现。

然后就是它的优点：
1. 搭建便捷：Next.js 基于 React，拥有完善的架构，搭建起来非常轻松。可以直接使用官方的脚手架，它的内部许多配置都集成好了，无需额外配置。

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
