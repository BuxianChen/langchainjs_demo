# langchainjs demo

## How to run

```bash
# install node and yarn, see below
yarn install
yarn build
node dist/app.js
```

## Step-by-Step development

### Install Node.js

```bash
# download from https://nodejs.org/en/download/
wget https://nodejs.org/dist/v20.11.1/node-v20.11.1-linux-x64.tar.xz
mv node-v20.11.1-linux-x64.tar.xz ~/software
tar -xvf node-v20.11.1-linux-x64.tar.xz

# vim ~/.bashrc
# export PATH=~/software/node-v20.11.1-linux-x64/bin:$PATH
```

这样一来 `node`, `npm`, `npx` 命令将变得可用, 其中 npm 的地位相当于是 pip 之于 python. 简要介绍一下命令:

```bash
npm install <package-name> # 安装在当前目录的 node_modules 目录下, 同时会包含 package.json 及 package-lock.json 记录依赖
npm list     # 列出当前目录的依赖
# 加上 -g 选项表示全局安装
npm install -g <package-name>
npm list -g
```

### Install yarn and TypeScript

一些最佳实践:

- `yarn` 与 `npm` 的关系类似于 `poetry` 与 `pip` 的关系, 通常推荐全局安装.
- `typescript` 局部安装或者全局安装则各有优劣, 这里对 `typescript` 使用局部安装.
- `vue` 和 `react` 推荐用局部安装.
- `npx` 主要用于运行 `node_modules/.bin` 下的命令

```bash
npm install -g yarn  # 后续将不会用到 npm 了, 这条命令会使得 yarn 命令变得全局可用
mkdir fastapi_react_demo
cd fastapi_react_demo
yarn add typescript --dev  # 加不加 --dev 的区别只在于在 package.json 里是写在 dependencies 里还是 devDependencies 里
```

安装 `typescript` 的主要目的是获取 `tsc` 命令, 这个命令可以用于将 typescript 脚本编译为 javascript 脚本, 由于这里使用局部安装 `typescript`, 所以只能在项目根目录下使用 `npx tsc` 才能起作用.

#### TypeScript Hello World program in node.js

typescript 的 helloworld 程序 `app.ts`:

```typescript
// app.ts
let message: string = 'Hello, World!';
console.log(message);
```

运行:

```bash
npx tsc app.ts  # 编译得到 app.js
node app.js     # 使用 node 运行
```

#### TypeScript Hello World program in Web Browsers

`app.ts` 文件内容

```typescript
// app.ts
let message: string = 'Hello, World!';
// create a new heading 1 element
let heading = document.createElement('h1');
heading.textContent = message;
// add the heading the document
document.body.appendChild(heading);
```

`index.html` 文件内容

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TypeScript: Hello, World!</title>
</head>
<body>
    <script src="app.js"></script>
</body>
</html>
```

运行:

```bash
npx tsc app.ts
```

然后直接用浏览器打开 index.html 即可

#### TypeScript with yarn Hello World program in node.js

项目最终的目录结构

```
dist/
  - app.js  # 由 yarn build 编译得到, 不要手动修改
src/
  - app.ts
node_modules/
package.json
tsconfig.json  # yarn install 时的编译选项
yarn.lock  # 依赖包的精确版本, 自动生成, 不要手动修改
```

step by step 执行:

```bash
mkdir tshello
cd tshello
yarn add typescript --dev
npx tsc --init  # 或者直接新建 tsconfig.json 进行修改

# 将代码像后面那样写好

# 编译
yarn build      # 使用 yarn build 会同时读取 package.json 和 tsconfig.json 文件
# 在这个例子中也可以只用 npx tsc 命令进行编译, 且只依赖 tsconfig.json

# 运行
node dist/app.js
```

`package.json` 文件内容

```json
{
  "scripts": {
    "build": "tsc"
  },
  "devDependencies": {
    "typescript": "^5.4.5"
  }
}
```

`tsconfig.json` 文件内容

```json
{
  "compilerOptions": {
    "target": "es2016",
    "module": "commonjs",
    "esModuleInterop": true,
    "strict": true,
    "outDir": "./dist"
  },
  "include": ["src/**/*"]
}
```

`src/app.ts` 文件内容

```
console.log("Hello, World!");
```

### Install langchain related packages

```bash
yarn add langchain
yarn add @langchain/community
yarn add @langchain/core
yarn add @langchain/openai
```

然后参照 [https://js.langchain.com/docs/expression_language/get_started](https://js.langchain.com/docs/expression_language/get_started) 编写 `src/app.js`, 注意修改好 `package.json`, `tsconfig.json` (目前不太确定 `tsconfig.json` 的设置)

### build & run

```bash
yarn build
OPENAI_API_KEY=sk-xxx node dist/node.js
```