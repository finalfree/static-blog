### **项目计划书：从零开发个人静态博客**

#### **1\. 项目概述**

* **项目名称**: My Static Blog  
* **项目目标**: 开发一个功能完整、样式现代、可高度定制的静态博客。项目将从零开始，不依赖现有的静态站点生成器 (SSG) 框架，以深入学习前端技术栈。  
* **核心特点**:  
  * 内容驱动：所有文章使用 Markdown 编写。  
  * 性能优先：生成纯静态文件，实现极速加载。  
  * 开发者友好：模块化设计，易于扩展和维护。  
  * 自动化部署：无缝集成 GitHub/Cloudflare Pages。  
* **最终交付物**:  
  1. 一个包含完整博客功能的代码仓库。  
  2. 一套部署在 GitHub Pages 或 Cloudflare Pages 上的线上博客。

#### **2\. 技术栈 (Technology Stack)**

| 类别 | 技术 | 用途 |
| :---- | :---- | :---- |
| **构建工具** | Vite | 提供极速的开发服务器和优化的生产构建。 |
| **核心框架** | Vue 3 (Composition API) | 构建用户界面的核心，使用组合式 API 进行逻辑组织。 |
| **语言** | TypeScript | 提供类型安全，提升代码质量和可维护性。 |
| **路由** | Vue Router | 管理应用的页面路由，支持动态路由匹配文章。 |
| **样式方案** | Tailwind CSS | 提供原子化 CSS 类，用于快速构建现代化的 UI。 |
| **内容处理** | gray-matter | 解析 Markdown 文件中的 Front-matter 元数据。 |
| **Markdown 渲染** | marked | 将 Markdown 文本转换为 HTML。 |
| **代码高亮** | highlight.js | 对渲染后代码块进行语法高亮。 |
| **代码规范** | ESLint \+ Prettier | 保证代码风格统一和质量。 |

#### **3\. 功能模块划分**

1. **文章处理核心 (Content Pipeline)**  
   * **职责**: 负责读取、解析和转换 Markdown 源文件。  
   * **子功能**:  
     * 读取 posts 目录下的所有 .md 文件。  
     * 使用 gray-matter 分离 Front-matter 和 Markdown 内容。  
     * 使用 marked 将 Markdown 转换为 HTML 字符串。  
     * 集成 highlight.js 实现代码块高亮。  
2. **数据层 (Data Layer)**  
   * **职责**: 在构建时收集和组织所有文章数据，供视图层使用。  
   * **子功能**:  
     * 创建一个全局可访问的文章列表（包含标题、日期、slug、标签等元数据）。  
     * 提供根据 slug 查找单篇文章数据的能力。  
     * 生成所有标签和分类的集合。  
3. **视图与布局 (View & Layout)**  
   * **职责**: 渲染 UI 界面和可复用的布局组件。  
   * **子功能**:  
     * MainLayout: 包含页头 (Navbar) 和页脚 (Footer) 的主布局。  
     * PostLayout: 文章详情页的专用布局。  
     * HomePage: 首页，展示文章列表和简介。  
     * PostPage: 文章详情页，展示文章内容。  
     * TagsPage: 标签列表页，展示所有标签。  
     * TagPostsPage: 展示特定标签下的所有文章。  
     * AboutPage: “关于我”页面。  
     * 可复用组件: PostCard, Tag, Navbar, Footer 等。  
4. **路由系统 (Routing System)**  
   * **职责**: 定义页面 URL 与视图组件的映射关系。  
   * **子功能**:  
     * 静态路由: /, /about, /tags。  
     * 动态路由: /post/:slug (用于文章详情), /tags/:tagName (用于标签文章列表)。  
5. **构建与部署 (Build & Deployment)**  
   * **职责**: 将整个应用打包成纯静态文件并实现自动化部署。  
   * **子功能**:  
     * 配置 Vite 在 npm run build 时生成静态 HTML、CSS、JS。  
     * 编写脚本在构建时基于路由动态生成每个文章的 HTML 页面。  
     * 集成 GitHub Actions 或 Cloudflare Pages 的 CI/CD 流程。

#### **4\. 详细开发步骤 (Step-by-Step Plan)**

**阶段一：项目初始化与环境配置 (预计用时: 2小时)**

1. **步骤 1.1: 创建项目**  
   * 执行 npm create vue@latest。  
   * 在交互式选项中，选择：Vue, TypeScript, Vue Router, Tailwind CSS, ESLint, Prettier。  
2. **步骤 1.2: 安装内容处理依赖**  
   * 执行 npm install gray-matter marked highlight.js。  
   * 安装类型定义：npm install \-D @types/marked @types/highlight.js。  
3. **步骤 1.3: 初始化目录结构**  
   * 在项目根目录创建 posts 文件夹。  
   * 在 posts 文件夹中创建两篇示例文章，如 hello-world.md 和 another-post.md，并填充好 Front-matter 和内容。  
   * 在 src 目录下，创建 layouts, pages, utils 目录。

**阶段二：实现核心内容管道 (预计用时: 4小时)**

1. **步骤 2.1: 创建文章数据获取逻辑**  
   * 在 src/utils/posts.ts 中创建核心逻辑。  
   * 使用 Vite 的 import.meta.glob 功能来动态导入 posts 目录下的所有 .md 文件。  
   * 编写一个函数 getPosts()，该函数处理 glob 导入的结果，对每篇文章进行 gray-matter 解析，并返回一个包含所有文章元数据和 slug 的数组，按日期降序排列。  
   * 编写另一个函数 getPost(slug)，用于根据 slug 获取单篇文章的完整内容（元数据 \+ 转换后的 HTML）。  
2. **步骤 2.2: 实现文章详情页**  
   * 在 src/pages 中创建 PostPage.vue。  
   * 配置 src/router/index.ts，添加动态路由: { path: '/post/:slug', name: 'post', component: PostPage }。  
   * 在 PostPage.vue 中，使用 useRoute() 获取 URL 中的 slug 参数，调用 getPost(slug) 获取文章数据，并使用 v-html 指令将文章 HTML 渲染到页面上。  
   * 配置 highlight.js 并应用到渲染后的代码块。

**阶段三：构建博客基础结构 (预计用时: 6小时)**

1. **步骤 3.1: 创建首页**  
   * 在 src/pages 中创建 HomePage.vue。  
   * 在 HomePage.vue 中，调用 getPosts() 获取所有文章的元数据列表。  
   * 创建一个 PostCard.vue 组件，用于展示单篇文章的摘要（标题、日期、简介、标签）。  
   * 在 HomePage.vue 中，遍历文章列表并使用 PostCard 组件渲染。  
2. **步骤 3.2: 创建主布局**  
   * 在 src/layouts 中创建 MainLayout.vue。  
   * 在 MainLayout.vue 中添加 \<Navbar\> 和 \<Footer\> 组件。  
   * 在 App.vue 中使用 MainLayout，并通过 \<router-view\> 来展示不同页面的内容。  
3. **步骤 3.3: 完善路由和导航**  
   * 在 Navbar.vue 中添首页、关于等页面的导航链接 (\<router-link\>)。  
   * 创建 AboutPage.vue 并配置相应路由。

**阶段四：高级功能与样式优化 (预计用时: 5小时)**

1. **步骤 4.1: 实现标签系统**  
   * 在 src/utils/posts.ts 中添加一个函数 getTags()，用于遍历所有文章并返回一个包含所有标签及其文章数量的集合。  
   * 创建 TagsPage.vue，展示所有标签。  
   * 创建 TagPostsPage.vue，并添加动态路由 /tags/:tagName，用于显示特定标签下的文章列表。  
2. **步骤 4.2: 全局样式**  
   * 使用 Tailwind CSS 的 @apply 功能在 src/assets/main.css 中定义基础文章样式（如 h1, p, blockquote 等），以便 v-html 渲染的内容也能应用样式。  
3. **步骤 4.3: SEO 优化**  
   * 利用 vue-head 或类似库为每个页面动态设置 \<title\> 和 \<meta description\>。  
   * 在构建流程中添加脚本，用于生成 sitemap.xml 和 rss.xml。

**阶段五：构建与部署 (预计用时: 3小时)**

1. **步骤 5.1: 配置静态站点生成 (SSG)**  
   * 研究并使用 vite-ssg 或编写自己的 build.js 脚本。  
   * 脚本逻辑：  
     1. 调用 getPosts() 获取所有文章。  
     2. 遍历文章列表，为每个 /post/:slug 路由生成一个对应的 post/slug.html 文件。  
     3. 为 /, /about, /tags 等静态路由生成 HTML。  
2. **步骤 5.2: 部署到 Cloudflare/GitHub Pages**  
   * 将代码推送到 GitHub 仓库。  
   * 在 Cloudflare Pages 或 GitHub Pages 设置中关联该仓库。  
   * 配置构建命令: npm run build。  
   * 配置输出目录: dist。  
   * 触发首次部署并验证线上效果。

---

#### **5\. 给 AI Agent 的提示 (Tips for AI Agent)**

在协助编码时，请遵循以下原则：

1. **遵循计划**: 严格按照上述的阶段和步骤进行，不要跳跃。一次只专注于一个步骤。  
2. **模块化和单一职责**: 创建的每个 Vue 组件或 TS 工具函数都应有明确的单一职责。例如，posts.ts 只负责数据处理，PostCard.vue 只负责展示文章卡片。  
3. **代码质量**: 生成的代码必须是类型安全的 TypeScript 代码，遵循项目已配置的 ESLint 和 Prettier 规则。  
4. **解释代码**: 在生成关键代码块（如 import.meta.glob 的使用、动态路由生成脚本）后，请附上简要的注释或解释，说明其工作原理。  
5. **优先使用 Composition API**: 所有 Vue 组件都应使用 \<script setup\> 语法和 Composition API。  
6. **无状态组件**: 尽可能创建无状态的展示组件，通过 props接收数据，通过 emits 发出事件。

这份计划书为你和 AI 协作提供了一个坚实的框架。祝你开发顺利！