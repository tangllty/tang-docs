# Vercel 网站分析快速开始

本指南将帮助您开始在项目中使用 Vercel 网站分析，向您展示如何启用它、将包添加到项目中、将应用部署到 Vercel 以及在仪表板中查看数据。

**选择您的框架以查看在项目中使用 Vercel 网站分析的说明**。

## 前提条件

- Vercel 账户。如果没有，可以[免费注册](https://vercel.com/signup)。
- Vercel 项目。如果没有，可以[创建新项目](https://vercel.com/new)。
- 安装 Vercel CLI。如果没有，可以使用以下命令安装：

:::tabs
== pnpm
```bash
pnpm i vercel
```

== yarn
```bash
yarn i vercel
```

== npm
```bash
npm i vercel
```

== bun
```bash
bun i vercel
```
:::

## 在 Vercel 中启用网站分析

在 [Vercel 仪表板](https://vercel.com/dashboard)上，选择您的项目，然后点击 **Analytics** 选项卡，并从对话框中点击 **Enable**。

> **💡 注意：**启用网站分析将在下次部署后添加新路由（范围在 `/_vercel/insights/*`）。

## 将 `@vercel/analytics` 添加到您的项目

使用您选择的包管理器，将 `@vercel/analytics` 包添加到您的项目：

:::tabs
== pnpm
```bash
pnpm i @vercel/analytics
```

== yarn
```bash
yarn i @vercel/analytics
```

== npm
```bash
npm i @vercel/analytics
```

== bun
```bash
bun i @vercel/analytics
```
:::

## 特定框架设置

在下方选择您的框架以查看具体的集成说明：

### Next.js（Pages 目录）

`Analytics` 组件是跟踪脚本的包装器，提供与 Next.js 的更无缝集成，包括路由支持。

如果您使用 `pages` 目录，请将以下代码添加到主应用文件：

:::tabs key:nextjs-pages
== TypeScript

```tsx {2, 8} filename="pages/_app.tsx"
import type { AppProps } from "next/app";
import { Analytics } from "@vercel/analytics/next";

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <>
      <Component {...pageProps} />
      <Analytics />
    </>
  );
}

export default MyApp;
```

== JavaScript

```jsx {1, 7} filename="pages/_app.js"
import { Analytics } from "@vercel/analytics/next";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Analytics />
    </>
  );
}

export default MyApp;
```
:::

### Next.js（App 目录）

`Analytics` 组件是跟踪脚本的包装器，提供与 Next.js 的更无缝集成，包括路由支持。

将以下代码添加到根布局：

:::tabs key:nextjs-app
== TypeScript

```tsx {1, 15} filename="app/layout.tsx"
import { Analytics } from "@vercel/analytics/next";

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  );
}
```

== JavaScript

```jsx {1, 11} filename="app/layout.jsx"
import { Analytics } from "@vercel/analytics/next";

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head>
        <title>Next.js</title>
      </head>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  );
}
```
:::

### Remix

`Analytics` 组件是跟踪脚本的包装器，提供与 Remix 的无缝集成，包括路由检测。

将以下代码添加到根文件：

:::tabs key:remix
== TypeScript

```tsx {9, 21} filename="app/root.tsx"
import {
  Links,
  LiveReload,
  Meta,
  Outlet,
  Scripts,
  ScrollRestoration,
} from "@remix-run/react";
import { Analytics } from "@vercel/analytics/remix";

export default function App() {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <Meta />
        <Links />
      </head>
      <body>
        <Analytics />
        <Outlet />
        <ScrollRestoration />
        <Scripts />
        <LiveReload />
      </body>
    </html>
  );
}
```

== JavaScript

```jsx {9, 21} filename="app/root.jsx"
import {
  Links,
  LiveReload,
  Meta,
  Outlet,
  Scripts,
  ScrollRestoration,
} from "@remix-run/react";
import { Analytics } from "@vercel/analytics/remix";

export default function App() {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <Meta />
        <Links />
      </head>
      <body>
        <Analytics />
        <Outlet />
        <ScrollRestoration />
        <Scripts />
        <LiveReload />
      </body>
    </html>
  );
}
```
:::

### Nuxt

`Analytics` 组件是跟踪脚本的包装器，提供与 Nuxt 的更无缝集成，包括路由支持。

将以下代码添加到主组件：

:::tabs key:nuxt
== TypeScript

```vue {2,6} filename="app.vue"
<script setup lang="ts">
import { Analytics } from '@vercel/analytics/nuxt';
</script>

<template>
  <Analytics />
  <NuxtPage />
</template>
```

== JavaScript

```vue {2,6} filename="app.vue"
<script setup>
import { Analytics } from '@vercel/analytics/nuxt';
</script>

<template>
  <Analytics />
  <NuxtPage />
</template>
```
:::

### SvelteKit

`injectAnalytics` 函数是跟踪脚本的包装器，提供与 SvelteKit 的更无缝集成，包括路由支持。

将以下代码添加到主布局：

:::tabs key:sveltekit
== TypeScript

```ts filename="src/routes/+layout.ts"
import { dev } from "$app/environment";
import { injectAnalytics } from "@vercel/analytics/sveltekit";

injectAnalytics({ mode: dev ? "development" : "production" });
```

== JavaScript

```js filename="src/routes/+layout.js"
import { dev } from "$app/environment";
import { injectAnalytics } from "@vercel/analytics/sveltekit";

injectAnalytics({ mode: dev ? "development" : "production" });
```
:::

### Astro

`Analytics` 组件是跟踪脚本的包装器，提供与 Astro 的更无缝集成，包括路由支持。

将以下代码添加到基础布局：

:::tabs key:astro
== TypeScript

```astro {2, 10} filename="src/layouts/Base.astro"
---
import Analytics from '@vercel/analytics/astro';
{/* ... */}
---

<html lang="en">
	<head>
    <meta charset="utf-8" />
    <!-- ... -->
    <Analytics />
	</head>
	<body>
		<slot />
  </body>
</html>
```

== JavaScript

```astro {2, 10} filename="src/layouts/Base.astro"
---
import Analytics from '@vercel/analytics/astro';
{/* ... */}
---

<html lang="en">
	<head>
    <meta charset="utf-8" />
    <!-- ... -->
    <Analytics />
	</head>
	<body>
		<slot />
  </body>
</html>
```
:::

#### Astro 旧版配置

`Analytics` 组件在 `@vercel/analytics@1.4.0` 及更高版本中可用。

如果您使用的是早期版本，必须在 `astro.config.mjs` 文件中配置 Vercel 适配器的 `webAnalytics` 属性，如下面的代码所示。

有关更多信息，请参阅 [Astro 适配器文档](https://docs.astro.build/en/guides/integrations-guide/vercel/#webanalytics)。

:::tabs key:astro-config
== TypeScript

```ts {7-9} filename="astro.config.mjs"
import { defineConfig } from "astro/config";
import vercel from "@astrojs/vercel/serverless";

export default defineConfig({
  output: "server",
  adapter: vercel({
    webAnalytics: {
      enabled: true, // 使用 @vercel/analytics@1.4.0 时设置为 false
    },
  }),
});
```

== JavaScript

```js {7-9} filename="astro.config.mjs"
import { defineConfig } from "astro/config";
import vercel from "@astrojs/vercel/serverless";

export default defineConfig({
  output: "server",
  adapter: vercel({
    webAnalytics: {
      enabled: true, // 使用 @vercel/analytics@1.4.0 时设置为 false
    },
  }),
});
```
:::

### Create React App

`Analytics` 组件是跟踪脚本的包装器，提供与 React 的更无缝集成。

> **💡 注意：**使用纯 React 实现时，没有路由支持。

将以下代码添加到主应用文件：

:::tabs key:cra
== TypeScript

```tsx {1, 7} filename="App.tsx"
import { Analytics } from "@vercel/analytics/react";

export default function App() {
  return (
    <div>
      {/* ... */}
      <Analytics />
    </div>
  );
}
```

== JavaScript

```jsx {1, 7} filename="App.jsx"
import { Analytics } from "@vercel/analytics/react";

export default function App() {
  return (
    <div>
      {/* ... */}
      <Analytics />
    </div>
  );
}
```
:::

### Vue

`Analytics` 组件是跟踪脚本的包装器，提供与 Vue 的更无缝集成。

> **💡 注意：**如果您使用 `vue-router`，路由支持会自动启用。

将以下代码添加到主组件：

:::tabs key:vue
== TypeScript

```vue {2,6} filename="src/App.vue"
<script setup lang="ts">
import { Analytics } from '@vercel/analytics/vue';
</script>

<template>
  <Analytics />
  <!-- your content -->
</template>
```

== JavaScript

```vue {2,6} filename="src/App.vue"
<script setup>
import { Analytics } from '@vercel/analytics/vue';
</script>

<template>
  <Analytics />
  <!-- your content -->
</template>
```
:::

### 其他 JavaScript 框架

从包中导入 `inject` 函数，这将把跟踪脚本添加到您的应用程序。**此函数在应用程序中只应调用一次，并且必须在客户端运行**。

> **💡 注意：**`inject` 函数没有路由支持。

将以下代码添加到主应用文件：

:::tabs key:other
== TypeScript

```ts filename="main.ts"
import { inject } from "@vercel/analytics";

inject();
```

== JavaScript

```js filename="main.js"
import { inject } from "@vercel/analytics";

inject();
```
:::

### HTML

对于纯 HTML 网站，您可以将以下脚本添加到 `.html` 文件：

:::tabs key:html
== TypeScript

```html filename="index.html"
<script>
  window.va = window.va || function () { (window.vaq = window.vaq || []).push(arguments); };
</script>
<script defer src="/_vercel/insights/script.js"></script>
```

== JavaScript

```html filename="index.html"
<script>
  window.va = window.va || function () { (window.vaq = window.vaq || []).push(arguments); };
</script>
<script defer src="/_vercel/insights/script.js"></script>
```
:::

> **💡 注意：**使用 HTML 实现时，无需安装 `@vercel/analytics` 包。但是，没有路由支持。

## 将您的应用部署到 Vercel

使用以下命令部署您的应用：

```bash
vercel deploy
```

如果您还没有，我们还建议[连接项目的 Git 仓库](https://vercel.com/docs/git#deploying-a-git-repository)，这将使 Vercel 能够在不使用终端命令的情况下部署您对主分支的最新提交。

部署应用后，它将开始跟踪访问者和页面浏览量。

> **💡 注意：**如果一切设置正确，您应该能够在访问任何页面时在浏览器的网络选项卡中看到来自 `/_vercel/insights/view` 的 Fetch/XHR 请求。

## 在仪表板中查看您的数据

部署应用并且用户访问您的网站后，您可以在仪表板中查看数据。

要这样做，请转到您的[仪表板](https://vercel.com/dashboard)，选择您的项目，然后点击 **Analytics** 选项卡。

几天的访问者数据后，您将能够开始探索数据，通过查看和[过滤](https://vercel.com/docs/analytics/filtering)面板。

专业版和企业版计划的用户还可以向其数据添加[自定义事件](https://vercel.com/docs/analytics/custom-events)，以跟踪用户交互，如按钮点击、表单提交或购买。

了解有关 Vercel 如何通过 Vercel 网站分析支持[隐私和数据合规标准](https://vercel.com/docs/analytics/privacy-policy)的更多信息。

## 下一步

现在您已经设置了 Vercel 网站分析，可以探索以下主题以了解更多信息：

- [了解如何使用 `@vercel/analytics` 包](https://vercel.com/docs/analytics/package)
- [了解如何设置自定义事件](https://vercel.com/docs/analytics/custom-events)
- [了解有关过滤数据的信息](https://vercel.com/docs/analytics/filtering)
- [阅读有关隐私和合规性的信息](https://vercel.com/docs/analytics/privacy-policy)
- [探索定价](https://vercel.com/docs/analytics/limits-and-pricing)
- [故障排除](https://vercel.com/docs/analytics/troubleshooting)
