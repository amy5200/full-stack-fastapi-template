# 全栈项目开发指南

## 目录

- [环境准备](#环境准备)
- [本地开发](#本地开发)
- [Docker环境](#docker环境)
- [前端开发](#前端开发)
- [后端开发](#后端开发)
- [API文档](#api文档)
- [测试](#测试)
- [部署](#部署)

## 环境准备

### 必需工具

- Docker 和 Docker Compose
- Node.js (推荐使用项目指定版本)
- Python 3.8+
- Git

### 推荐工具

- VS Code 或其他支持TypeScript/Python的IDE
- pre-commit (用于代码格式化和lint)

## 本地开发

### 1. 克隆项目

```bash
git clone <项目地址>
cd <项目目录>
```

### 2. 环境配置

- 复制并配置环境变量文件：
```bash
cp .env.example .env
```

- 安装pre-commit hooks：
```bash
uv run pre-commit install
```

## Docker环境

### 启动完整开发环境

```bash
docker compose watch
```

启动后可访问以下服务：

- 前端界面：http://localhost:5173
- 后端API：http://localhost:8000
- API文档(Swagger UI)：http://localhost:8000/docs
- 数据库管理(Adminer)：http://localhost:8080
- Traefik管理界面：http://localhost:8090

### 查看服务日志

```bash
# 查看所有服务日志
docker compose logs

# 查看特定服务日志
docker compose logs backend
docker compose logs frontend
```

## 前端开发

### 本地开发模式

1. 停止Docker中的前端服务：
```bash
docker compose stop frontend
```

2. 进入前端目录并启动开发服务器：
```bash
cd frontend
npm install
npm run dev
```

### API客户端生成

当后端API发生变化时，需要更新前端API客户端代码：

1. 确保后端服务运行中
2. 下载OpenAPI文档：
```bash
cd frontend
# 从后端获取最新的API文档
curl http://localhost:8000/api/v1/openapi.json > openapi.json
```

3. 优化API操作ID：
```bash
node modify-openapi-operationids.js
```

4. 生成客户端代码：
```bash
npm run generate-client
```

## 后端开发

### 本地开发模式

1. 停止Docker中的后端服务：
```bash
docker compose stop backend
```

2. 进入后端目录并启动开发服务器：
```bash
cd backend
fastapi dev app/main.py
```

### 数据库迁移

- 创建新的迁移：
```bash
alembic revision --autogenerate -m "migration description"
```

- 应用迁移：
```bash
alembic upgrade head
```

## API文档

项目使用OpenAPI(Swagger)自动生成API文档，可通过以下地址访问：

- Swagger UI：http://localhost:8000/docs
- ReDoc：http://localhost:8000/redoc

## 测试

### 后端测试

```bash
cd backend
./scripts/test.sh
```

### 前端测试

```bash
cd frontend
npm run test
```

## 部署

### 域名配置

项目支持两种域名配置模式：

1. 本地开发模式（默认）：
- 前端：http://localhost:5173
- 后端：http://localhost:8000

2. 子域名模式：
修改 `.env` 文件：
```dotenv
DOMAIN=localhost.tiangolo.com
```

此时可通过以下地址访问：
- 前端：http://dashboard.localhost.tiangolo.com
- 后端：http://api.localhost.tiangolo.com

### 生产环境部署

1. 配置Traefik：
```bash
mkdir -p /root/code/traefik-public/
rsync -a docker-compose.traefik.yml root@your-server.example.com:/root/code/traefik-public/
```

2. 启动Traefik：
```bash
cd /root/code/traefik-public/
docker compose -f docker-compose.traefik.yml up -d
```

3. 部署应用：
```bash
./scripts/deploy.sh
```

## 开发最佳实践

1. 代码提交前运行pre-commit检查：
```bash
uv run pre-commit run --all-files
```

2. 保持API文档和前端客户端代码同步

3. 使用Docker Compose进行集成测试

4. 遵循项目既定的代码风格和架构模式

5. 定期更新依赖并进行安全检查