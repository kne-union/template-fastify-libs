
# fastify-lib


### 描述

用于初始化一个fastify的插件库


### 安装

```shell
npm i --save @kne-template/fastify-lib
```


### 概述

### 项目概述

`@kne-template/fastify-lib` 是一个用于快速创建 Fastify 插件库的模板项目。它提供了一个预设的项目结构和基础功能，帮助开发者快速搭建基于 Fastify 的plugin项目。

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

#### 安装依赖

```bash
npm install
```

#### 开发

```bash
# 启动开发服务器
npm run dev

# 构建文档
npm run build:md

# 实时预览文档
npm run start:md
```

#### 代码格式化

```bash
# 格式化代码
npm run prettier
```

### 依赖项

#### 核心依赖

- `fastify-plugin`: Fastify 插件系统
- `@kne/fastify-namespace`: 命名空间管理
- `@kne/fastify-sequelize`: Sequelize ORM 集成

#### 开发依赖

- `@fastify/env`: 环境变量管理
- `fastify`: Web 框架
- `husky`: Git hooks 管理
- `nodemon`: 开发热重载
- `prettier`: 代码格式化
- `sqlite3`: 开发用数据库


### 示例

#### 示例代码



### API

### API 文档概述

本文档详细描述了 `@kne-template/fastify-lib` 提供的 API 接口、数据模型和服务函数。

### API 端点

#### 控制器端点

| 路径 | 方法 | 描述 | 参数 | 返回值 |
|------|------|------|------|--------|
| `/welcome` | GET | 欢迎页面 | 无 | `{ message: "Welcome to fastify-lib" }` |

### 数据模型

#### Example 模型

| 字段名 | 类型 | 描述 | 约束 |
|--------|------|------|------|
| name | STRING | 名称 | 非空 |

##### 模型选项

| 选项 | 值 | 描述 |
|------|------|------|
| comment | '示例表' | 表注释 |
| indexes | `[{ fields: ['name'] }]` | 索引配置 |

##### 关联关系

可以在 `associate` 函数中定义模型间的关联关系，例如：

```javascript
associate: function (models) {
  // 定义与其他模型的关联
  // this.belongsTo(models.OtherModel)
}
```

### 服务函数

#### Example 服务

| 函数名 | 描述 | 参数 | 返回值 |
|--------|------|------|--------|
| exampleService | 示例服务函数 | 无 | 包含 `message` 属性的对象 |

##### 使用示例

```javascript
const { services } = fastify.kne;
const result = await services.exampleService();
// result: { message: "This is an example service" }
```

### 使用命名空间

项目使用 `@kne/fastify-namespace` 创建命名空间，可以通过 `fastify.kne` 访问：

```javascript
// 访问模型
const { models } = fastify.kne;
const exampleModel = models.Example;

// 访问服务
const { services } = fastify.kne;
const result = await services.exampleService();

// 在插件中扩展
module.exports = fp(async function (fastify, opts) {
  const { models, services } = fastify.kne;
  
  // 添加新服务
  services.newService = async function() {
    return { data: "New service result" };
  };
});
```

### 自定义扩展

#### 添加新控制器

在 `libs/controllers` 目录中创建新的控制器文件，例如：

```javascript
// libs/controllers/user.js
module.exports = async function (fastify, opts) {
  fastify.get('/users', async function (request, reply) {
    return { users: [] };
  });
};
```

#### 添加新模型

在 `libs/models` 目录中创建新的模型文件，例如：

```javascript
// libs/models/user.js
module.exports = function (sequelize, DataTypes) {
  return sequelize.define('User', {
    username: {
      type: DataTypes.STRING,
      allowNull: false
    },
    email: {
      type: DataTypes.STRING,
      allowNull: false
    }
  }, {
    comment: '用户表'
  });
};
```

#### 添加新服务

在 `libs/services` 目录中创建新的服务文件，例如：

```javascript
// libs/services/user.js
const fp = require('fastify-plugin');

module.exports = fp(async function (fastify, opts) {
  const { models, services } = fastify.kne;
  const { Op } = fastify.Sequelize;
  
  services.getUserById = async function(id) {
    return await models.User.findByPk(id);
  };
});
```

