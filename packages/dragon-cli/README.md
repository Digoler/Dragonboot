# dragon-cli

`dragon-cli` 是前端编码规范化工具，可以为项目一键接入规范，保障项目的编码规范和代码质量。

## CLI 使用

### 安装

在终端执行：

```bash
npm install dragon-cli -g
```

安装完成后，可执行 `dragon-cli -h` 以验证安装成功。

### 使用

#### `dragon-cli init`：一键接入

在项目根目录执行 `dragon-cli init`，即可一键接入规范，为项目安装规范 `Lint` 所需的依赖和配置。

具体会做以下事情：

- 安装各种依赖：包括 `Linter` 依赖，如 [ESLint](https://eslint.org/)、[stylelint](https://stylelint.io/)、[commitlint](https://commitlint.js.org/#/)、[markdownlint](https://github.com/DavidAnson/markdownlint) 等；配置依赖，如 [eslint-config-dragon](https://www.npmjs.com/package/eslint-config-dragon)、[stylelint-config-dragon](https://www.npmjs.com/package/stylelint-config-dragon)、[commitlint-config-dragon](https://www.npmjs.com/package/commitlint-config-dragon) 等
- 写入各种配置文件，包括：
  - `.eslintrc.js`、`.eslintignore`：ESLint 配置（继承 `eslint-config-dragon`）及黑名单文件
  - `.stylelintrc.js`、`.stylelintignore`：stylelint 配置（继承 `stylelint-config-dragon`）及黑名单文件
  - `commitlint.config.js`：commitlint 配置（继承 `commitlint-config-dragon`）
  - `.prettierrc.js`：符合规范的 [Prettier 配置](https://prettier.io/docs/en/configuration.html)
  - `.editorconfig`：符合规范的 [editorconfig](https://editorconfig.org/)
  - `.vscode/extensions.json`：写入规范相关的 [VSCode 插件推荐](https://code.visualstudio.com/docs/editor/extension-gallery#_workspace-recommended-extensions)，包括 `ESLint`、`stylelint`、`prettier` 等
  - `.vscode/settings.json`：写入规范相关的 [VSCode 设置](https://code.visualstudio.com/docs/getstarted/settings#_settings-file-locations)，设置 `ESLint` 和 `stylelint` 插件的 `validate` 及**保存时自动运行 fix**，如果选择使用 `Prettier`，会同时将 `prettier-vscode` 插件设置为各前端语言的 defaultFormatter，并配置**保存时自动格式化**
  - `dragonboot-lint.config.js`dragon-cli 包的一些配置，如启用的功能等

> 注 1：如果项目已经配置过 ESLint、stylelint 等 Linter，执行 `dragon-cli init` 将会提示存在冲突的依赖和配置，并在得到确认后进行覆盖：
>
> 注 2：如果项目的 .vscode/ 目录被 .gitignore 忽略，可以在拉取项目后单独执行 `dragon-cli init --vscode` 命令写入 `.vscode/extensions.json` 和 `.vscode/settings.json` 配置文件

## Node.js API 使用

### 安装

```bash
npm install dragon-cli --save
```

### API

#### init：初始化

- dragon-cli.init(config)：将项目一键接入规范，效果等同于 `dragon-cli init`

示例：

```js
await dragon-cli.init({
    eslintType: 'react',
    enableESLint: true,
    enableStylelint: true,
    enablePrettier: true,
    disableNpmInstall: false,
});
```

config 参数如下：

| 参数               | 类型       | 默认值 | 说明                                                                                                                |
| ------------------ | ---------- | ------ | ------------------------------------------------------------------------------------------------------------------- |
| cwd                | string     | -      | 项目绝对路径                                                                                                        |
| eslintType         | ESLintType | -      | 语言和框架类型，如果不配置，等同于 dragon-cli init，控制台会出现选择器，如果配置，控制台就不会出现选择器        |
| enableESLint       | boolean    | true   | 是否启用 ESLint，如果不配置默认值为 true，即默认启用 ESLint                                                         |
| enableStylelint    | boolean    | -      | 是否启用 stylelint，如果不配置，等同于 dragon-cli init，控制台会出现选择器，如果配置，控制台就不会出现选择器    |
| enablePrettier     | boolean    | -      | 是否启用 Prettier                                                                                                   |
| disableNpmInstall  | boolean    | false  | 是否禁用自动在初始化完成后安装依赖                                                                                  |

##### ESLintType

- `default`: JavaScript 项目（未使用 React 和 Vue 的 JS 项目）
- `react`: JavaScript + React 项目
- `vue`: JavaScript + Vue 项目
- `typescript/default`: TypeScript 项目（未使用 React 和 Vue 的 TS 项目）
- `typescript/react`: TypeScript + React 项目
- `typescript/vue`: TypeScript + Vue 项目
- `es5`: ES5 及之前版本的 JavaScript 老项目

