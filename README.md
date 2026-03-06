### qmyd-project

通过页面创建海报，并支持导出。目前前端基于 **Vue 2 + vue-cli-service** 搭建。

---

### 技术栈

- **框架**：Vue 2.6.x
- **构建工具**：`@vue/cli-service`（基于 webpack）
- **语言**：JavaScript (ES2015+，Babel 转译)
- **兼容性**：使用 `core-js` 做按需 polyfill
- **路由**：预置 `vue-router` 依赖，后续可按需接入

---

### 目录结构（简要）

- `public/`：静态模板与资源
  - `index.html`：应用入口 HTML 模板
- `src/`
  - `main.js`：应用入口，创建并挂载 Vue 实例
  - `App.vue`：根组件
  - `components/`
    - `HelloWorld.vue`：示例组件（可按业务需要替换）
- `babel.config.js`：Babel 配置，使用 `@vue/cli-plugin-babel/preset`
- `package.json`：项目依赖与脚本

---

### 本地开发

#### 环境要求

- Node.js 建议版本：**>= 12**, 推荐与本机一致的 14/16 LTS
- npm（或其他兼容的包管理工具）

#### 安装依赖

```bash
npm install
```

#### 启动开发环境

```bash
npm run serve
```

启动后按照终端提示，在浏览器中访问类似 `http://localhost:8080/` 的地址即可预览。

#### 生产构建

```bash
npm run build
```

构建产物会输出到 `dist/` 目录，可用于部署到生产环境。

#### 代码检查

```bash
npm run lint
```

---

### 后续规划（建议）

- 接入 `vue-router`，搭建多页面路由结构
- 按业务设计实现海报编辑页面（画布、组件拖拽、配置面板等）
- 支持导出高清图片（如使用 `html2canvas`、`dom-to-image` 等方案）

---

### 参与贡献

1. Fork 本仓库
2. 新建 `feat_xxx` 分支
3. 提交代码并确保通过 `npm run lint`
4. 提交 Pull Request
