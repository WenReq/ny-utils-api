---
title: 一套代码对多环境的支持和 Rollup 的配置
lang: zh-CN
---

# 一套代码对多环境的支持和 Rollup 的配置

我们使用一套代码，可以实现对多环境的支持（EMS、UMD、CJS）。这要归功于 Rollup 。我们来看下 Rollup 具体的配置。

## 1. 可使用的场景

1. 该库是用 ES6+ 编写的，使用 `import` 和 `export` 关键字
2. 该库可以与 `<script>` 标记一起使用
3. 该库可以在使用现代包管理器（npm、yarn、pnpm）的 web 应用程序中使用。
4. 该库可以在 node 应用程序中使用。

这意味着库可以在以下上下文中工作:

### 1.1 使用 `<script>` 标签

```html
<html>
  <head>
    <script src="scripts/my-library.min.js"></script>
  </head>
  <body>
    <div id="app" />
    <script>
      myLibrary.helloWorld();
    </script>
  </body>
</html>
```

### 1.2 使用 RequireJS

```js
define(["my-library"], function (myLibrary) {});
// or
define(function (require) {
  var myLibrary = require("my-library");
});
```

### 1.3 在 web 应用中通过包管理器

```js
import { helloWorld } from "my-library";
helloWorld();
```

### 1.4 在 node 环境中的使用

```js
const myLibrary = require("my-library");
myLibrary.helloWorld();
// or
const { helloWorld } = require("my-library");
helloWorld();
```

### 1.5 注意

在 web 应用中通过包管理器，没有办法导入整个库并调用其中的单个函数。`import lib from 'library'; lib.sayHello();` 这完全是故意的。我们希望消费者只调用他们使用的部分，这样摇树（TreeShaking）就可以完成工作，并且在捆绑最终应用程序时消除死代码。记住，在使用现代捆绑器的应用程序的情况下，消费 Web 应用程序也将生成一个用于部署的捆绑包（Bundle），我们希望它尽可能小，这样我们就可以使消费者（即使用的代码），不必包含应用程序中没有使用的代码。

## 2. Rollup

为了实现这一切，我们将使用 rollup.js。主要原因是 `Rollup` 非常快(虽然不是最快的)，需要最小的配置，并且通过它方便的插件系统支持我们需要的一切。

一旦我们的库编写完成，我们将使用 Rollup 以下三种格式导出代码:

1. UMD(通用模块定义):这将支持使用脚本标记和 RequireJS。由于消费应用程序本身不会转译或捆绑代码，我们需要提供库的一个版本，该版本经过了精简和转译，以获得广泛的浏览器支持。
2. ESM (ES2015 模块):这将允许捆绑器（npm、yarn 和 pnpm）导入我们的应用程序，消除死代码，并将其编译到他们选择的级别。我们仍然在编译代码，但只是以一种方便消费者的格式提供它，让他们决定下一步做什么。我们可以 `import` 关键字引入使用。
3. CJS (CommonJS): Node.js 格式的选择。这里不需要摇树，因为代码大小并不那么重要，这种格式允许在节点应用程序中使用 `require` 关键字。

对于每种格式，我们还将提供一个**源映射**，以便用户可以在需要时调试库。

### 2.1 创建项目

```sh
mkdir my-library
cd my-library
npm init -y
```

### 2.2 添加依赖项

显然，我们需要 `rollup`

```sh
npm install rollup --save-dev
```

我们需要将代码转译为 UMD 格式，所以让我们安装 `babel`:

```sh
npm install @babel/core @babel/preset-env --save-dev
```

我们还需要 rollup 来使用 babel 和最小化代码，所以让我们安装必要的插件来使用 babel 和 `terser`:

```sh
npm install @rollup/plugin-babel @rollup/plugin-terser --save-dev
```

最后，我们希望能够在我们的库中以 node 的样式使用 import/export 语法: 这让我们可以使用 `import fn from './fn'` 替换 `import fn from './fn/index.js'`。当然，还有使用`node_modules`目录中的模块。

```sh
npm install @rollup/plugin-node-resolve --save-dev
```

库的最终依赖项列表应该如下所示:

```json
{
  "dependencies": {},
  "devDependencies": {
    "@babel/core": "^7.20.2",
    "@babel/preset-env": "^7.20.2",
    "@rollup/plugin-babel": "^6.0.2",
    "@rollup/plugin-node-resolve": "^15.0.1",
    "@rollup/plugin-terser": "^0.1.0",
    "rollup": "^3.3.0"
  }
}
```

完整的 package.json 如下

```json
{
  "name": "nyUtils",
  "version": "1.0.0",
  "description": "",
  "main": "dist/cjs/index.js",
  "module": "dist/esm/index.js",
  "files": ["dist"],
  "type": "module",
  "scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.20.2",
    "@babel/preset-env": "^7.20.2",
    "@rollup/plugin-babel": "^6.0.2",
    "@rollup/plugin-node-resolve": "^15.0.1",
    "@rollup/plugin-terser": "^0.1.0",
    "rollup": "^3.3.0"
  }
}
```

### 2.3 添加目录和配置文件

我们还需要一个用于源代码的目录，一个用于 `babel` 的配置文件，一个用于 `rollup` 的配置文件:

```sh
mkdir src
touch .babelrc.json
touch rollup.config.js
```

`.babelrc.json` 中的配置非常简单，我们只需要告诉 Babel 我们想要使用最新版本的 JavaScript:

```json
{
  "presets": [["@babel/env", { "modules": false }]]
}
```

对于 Rollup，我们需要导入必要的插件:

```js
import { nodeResolve } from "@rollup/plugin-node-resolve";
import { terser } from "rollup-plugin-terser";
import babel from "@rollup/plugin-babel";
```

我们还会导入这个 `package.json`。所以我们可以在导出 UMD Bundle 时使用 `name` 字段:

```js
import pkg from "./package.json" assert { type: "json" };
```

我们的 `rollup.config.js` 会做两件事:

对于 UMD：获取代码，处理它并通过 babel (transpile)转化器和 terser (minify)压缩器运行它，并将其导出为 UMD 可使用文件。

```js
{
  // UMD
  input: "src/index.js",
  plugins: [
    nodeResolve(),
    babel({
      babelHelpers: "bundled",
    }),
    terser(),
  ],
  output: {
    file: `dist/${pkg.name}.min.js`,
    format: "umd",
    name: "myLibrary",
    esModule: false,
    exports: "named",
    sourcemap: true,
  },
},
```

对于 CJS/ESM:获取代码，处理它，并将其导出为 ESM 模块和 CJS 模块。记住，在这种情况下，我们不需要转导或缩小。Node 不需要它，对于 ESM，由消费者（即使用函数）来完成。

```js
{
  input: ["src/index.js"],
  plugins: [nodeResolve()],
  output: [
    {
      dir: "dist/esm",
      format: "esm",
      exports: "named",
      sourcemap: true,
    },
    {
      dir: "dist/cjs",
      format: "cjs",
      exports: "named",
      sourcemap: true,
    },
  ],
},
```

然而，在所有情况下，我们都会生成一个源地图（sourcemap）。

注意所有配置中的 `exports: "named"` 选项，在 `rollup` 的[文档](https://rollupjs.org/guide/en/#outputexports)中有更长的解释，本质上这告诉 rollup 我们使用的是**命名导出**而不是默认导出。长话短说，这允许最广泛的兼容性，并使树摇动发生。如果使用 linter，请确保将其配置为**优先于命名导出**而不是默认导出(这不适用于应用程序，**只适用于库**，对于应用程序使用默认导出甚至混合使用默认/命名导出（default/named exports）完全可以)。

完整的 rollup 文件如下所示。而且因为名字是从 `package.json` 里取的。只要入口点是 `src/index.js`，并且在 UMD 模块的输出中相应地设置名称，你就可以实际使用这个文件。

### 2.4 完整配置

```js
import { nodeResolve } from "@rollup/plugin-node-resolve";
import { terser } from "rollup-plugin-terser";
import babel from "@rollup/plugin-babel";
import pkg from "./package.json" assert { type: "json" };
const input = ["src/index.js"];
export default [
  {
    // UMD
    input,
    plugins: [
      nodeResolve(),
      babel({
        babelHelpers: "bundled",
      }),
      terser(),
    ],
    output: {
      file: `dist/${pkg.name}.min.js`,
      format: "umd",
      name: "myLibrary", // this is the name of the global object
      esModule: false,
      exports: "named",
      sourcemap: true,
    },
  },
  // ESM and CJS
  {
    input,
    plugins: [nodeResolve()],
    output: [
      {
        dir: "dist/esm",
        format: "esm",
        exports: "named",
        sourcemap: true,
      },
      {
        dir: "dist/cjs",
        format: "cjs",
        exports: "named",
        sourcemap: true,
      },
    ],
  },
];
```
