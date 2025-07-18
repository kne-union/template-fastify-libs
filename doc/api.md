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
