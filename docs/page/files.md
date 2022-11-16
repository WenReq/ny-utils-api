---
title: 一套代码对多环境的支持和 Rollup 的配置
lang: zh-CN
---

# library 函数部分和项目脚本的配置

## 1. library 函数部分

现在我们有了依赖项、配置的 babel 和 rollup，是时候编写代码了。

我们将像这样布局文件:

```
src
├── goodbye
│   ├── goodbye.js
│   └── index.js
├── hello
│   ├── hello.js
│   └── index.js
└── index.js
```

```js
// src/index.js
export { default as hello } from "./hello";
export { default as goodbye } from "./goodbye";
```

```js
// src/hello/index.js
export { default } from "./hello";
```

```js
// src/hello/hello.js
export default function hello() {
  console.log("hello");
}
```

```js
// src/goodbye/index.js
export { default } from "./goodbye";
```

```js
// src/goodbye/goodbye.js
export default function goodbye() {
  console.log("goodbye");
}
```

## 2. 脚本的配置

接下来我们需要调用 rollup 并告诉它执行它的工作。为了方便起见，我们将创建两个 npm 脚本，一个用于构建库，另一个用于在每次更改时重新编译代码的 dev 任务:

```json
"scripts": {
  "build": "rollup -c",
  "dev": "rollup -c -w"
},
```

最后，我们需要描述如何导出应用程序，既使 `npm` 可用，又使使用者可以使用。

我们将在 `package.json` 中定义三个值:

files 选项告诉 npm 要打包什么(这可以使用 npm pack 进行测试)，指向 CJS 模块的主选项，以及模块选项，虽然不是标准的，但已经成为 ESM 模块的规范。

```json
// package.json
...
"main": "dist/cjs/index.js",
"module": "dist/esm/index.js",
...
files: [
  "dist"
]
```

就是这样!

要构建库，只需运行 `npm run build`，在开发时，你可以使用 `npm run dev`。可以使用 `npm pack` 对导出进行测试。

## 3. 测试库

### 3.1 script

使用 script 标记，只需创建一个 HTML 文件，并在浏览器中打开它。您将在控制台中看到“hello”字样。

```html
<html>
  <head>
    <script src="dist/my-library.min.js"></script>
  </head>
  <body>
    <script>
      myLibrary.hello();
    </script>
  </body>
</html>
```

### 3.3 web 应用 - React/Vue

从一个使用 webpack 的 web 应用程序，比如 React 应用程序:

```sh
npx create-react-app my-library-cra
cd my-library-cra
```

在 package.json 的 dependencies，只需添加这一行:

```json
"my-library": "../my-library/"
```

运行 `yarn install`

在 `src/App.js`，导入和调用 hello 的函数：

```js
import { hello } from "my-library";
hello();
```

使用 `yarn start` 运行 React 应用程序并打开 JavaScript 控制台，您应该会看到打印出的 "hello" 字。

现在，为了确保摇树（tree-shaking）工作，运行`yarn build`。React 应用程序将被捆绑并放入构建目录中。如果你在文件中搜索 hello 关键字，你会看到它在一个 js 文件中，有一个长而复杂的名字，但是关键字 goodbye 却找不到。这表明 webpack 只**拉入必要的代码**。由于我们在库中使用命名导出，库的消费者不能从 'my-library' 写入 `import myLibrary`;并且错误地导入了整个包，而只使用了其中的一部分。
