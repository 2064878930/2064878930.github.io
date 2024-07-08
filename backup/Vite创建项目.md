# Vite创建项目



1.首先，确保你已经安装了 Node.js 和 npm（Node.js 的包管理器）。

2.安装 Vite。如果你还没有安装 Vite，可以通过以下命令全局安装：

```
npm install -g create-vite
```

3.创建新项目。安装完 Vite 后，你可以使用以下命令创建一个新的 Vite 项目：

```
# 使用 npm
npm create vite@latest my-vite-project --template vue
```

这里的 `my-vite-project` 是你想要创建的项目名称，`--template vue` 指定了项目模板，这里以 React 为例。Vite 支持多种模板，包括但不限于 `vanilla`（纯 JavaScript）、`vue`、`react`、`preact`、`lit`、`svelte` 等。

4.进入项目目录并启动开发服务器：

```
cd my-vite-project
npm install
npm run dev
```

执行 `npm run dev` ，Vite 将启动开发服务器，并且通常会在默认的浏览器中打开项目。

以上步骤将帮助你快速搭建起一个基于 Vite 的前端项目。记得根据你的项目需求选择合适的模板。