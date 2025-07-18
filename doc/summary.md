`@kne-template/fastify-libs` 是一个用于快速创建 Fastify 插件库的模板项目。它提供了一个预设的项目结构和基础功能，帮助开发者快速搭建基于 Fastify 的plugin项目。

### 项目结构

```
template/
├── index.js              # 插件主入口文件
├── package.json          # 项目配置和依赖
└── libs/                 # 核心库文件
    ├── controllers/      # API 控制器
    │   └── main.js       # 主控制器
    ├── models/           # 数据模型
    │   └── example._js   # 示例模型
    └── services/         # 业务服务
        └── example._js   # 示例服务
```

### 核心功能

#### 插件系统

项目基于 Fastify 的插件系统构建，使用 `fastify-plugin` 创建可复用的插件模块。

#### 命名空间管理

使用 `@kne/fastify-namespace` 创建命名空间，有效组织和管理控制器、模型和服务。

#### ORM 集成

集成 Sequelize ORM，提供强大的数据库操作能力，支持多种数据库类型。

#### MVC 架构

采用 MVC (Model-View-Controller) 架构设计：
- **Model**: 使用 Sequelize 定义数据模型
- **Controller**: 处理 HTTP 请求和响应
- **Service**: 实现业务逻辑

### 使用方法

```bash
npx @kne/npm-tools init 项目名
```
选择 Fastify Libs
