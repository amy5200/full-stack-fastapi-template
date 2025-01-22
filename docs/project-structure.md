# 项目开发注意事项与结构说明

## 开发注意事项

### 前端开发注意事项

1. **环境配置**
   - 确保Node.js版本与项目要求一致（查看 `.nvmrc` 文件）
   - 首次运行前执行 `npm install` 安装依赖
   - 本地开发时使用 `npm run dev` 启动开发服务器

2. **API客户端更新**
   - 后端API变更后必须更新前端客户端代码
   - 使用 `npm run generate-client` 生成最新的API类型定义
   - 确保API文档与前端代码保持同步

3. **代码规范**
   - 遵循项目的TypeScript类型定义
   - 使用项目配置的代码格式化工具（Biome）
   - 组件开发遵循React最佳实践

### 后端开发注意事项

1. **环境配置**
   - 使用Python 3.8+版本
   - 使用uv管理Python依赖
   - 本地开发时使用 `fastapi dev` 启动服务器

2. **数据库操作**
   - 所有数据库变更通过Alembic迁移管理
   - 创建新的数据模型后执行迁移命令
   - 测试环境使用独立的数据库

3. **API开发**
   - 遵循RESTful API设计规范
   - 确保API文档的及时更新
   - 实现适当的错误处理和验证

## 项目结构说明

### 后端目录结构 (`/backend`)

```
backend/
├── app/                    # 主应用目录
│   ├── api/               # API路由和端点定义
│   ├── core/              # 核心配置和工具
│   ├── crud.py           # 数据库CRUD操作
│   ├── models.py         # SQLAlchemy数据模型
│   └── tests/            # 测试用例
├── alembic.ini           # Alembic配置文件
└── pyproject.toml        # Python项目依赖配置
```

#### 关键文件说明

- `app/main.py`: 应用程序入口点，FastAPI实例配置
- `app/core/config.py`: 全局配置管理
- `app/api/deps.py`: 依赖注入和中间件
- `app/crud.py`: 数据库操作封装
- `app/models.py`: 数据库模型定义

### 前端目录结构 (`/frontend`)

```
frontend/
├── src/                   # 源代码目录
│   ├── components/       # React组件
│   ├── hooks/           # 自定义React Hooks
│   ├── routes/          # 路由组件
│   └── client/          # 生成的API客户端代码
├── public/               # 静态资源
├── vite.config.ts        # Vite配置
└── package.json          # 项目依赖配置
```

#### 关键文件说明

- `src/main.tsx`: 应用程序入口点
- `src/theme.tsx`: 全局主题配置
- `src/utils.ts`: 通用工具函数
- `src/components/`: 可重用UI组件
- `src/routes/`: 页面路由组件

### 配置文件说明

1. **根目录配置文件**
   - `.env`: 环境变量配置
   - `docker-compose.yml`: Docker服务配置
   - `.pre-commit-config.yaml`: 代码提交检查配置

2. **前端配置文件**
   - `vite.config.ts`: Vite构建配置
   - `biome.json`: 代码格式化配置
   - `tsconfig.json`: TypeScript配置

3. **后端配置文件**
   - `alembic.ini`: 数据库迁移配置
   - `pyproject.toml`: Python项目配置

## 开发工作流程

1. **本地开发**
   - 使用Docker Compose启动完整环境
   - 前端修改实时热更新
   - 后端修改自动重载

2. **代码提交**
   - 运行测试确保功能正常
   - 执行代码格式化
   - 使用pre-commit进行代码检查

3. **部署流程**
   - 使用CI/CD自动化部署
   - 执行自动化测试
   - 生成API文档和客户端代码

## 最佳实践建议

1. **代码组织**
   - 遵循模块化原则
   - 保持代码简洁清晰
   - 编写详细的注释

2. **测试覆盖**
   - 编写单元测试和集成测试
   - 保持测试用例的独立性
   - 模拟外部依赖

3. **性能优化**
   - 使用适当的缓存策略
   - 优化数据库查询
   - 实现合理的前端状态管理

4. **安全性**
   - 实施proper的认证和授权
   - 防止常见的安全漏洞
   - 保护敏感数据

5. **文档维护**
   - 及时更新API文档
   - 记录重要的设计决策
   - 维护清晰的版本历史