# 项目部署与 GitHub 仓库组织指南

## 目录
1. [GitHub 仓库结构](#github-仓库结构)
2. [推荐的仓库组织方式](#推荐的仓库组织方式)
3. [文档调整建议](#文档调整建议)
4. [开发环境搭建](#开发环境搭建)
5. [部署流程](#部署流程)
6. [CI/CD 配置](#cicd-配置)

---

## GitHub 仓库结构

### 方案一：Monorepo（单仓库，推荐）

**优点**：
- 代码统一管理，版本同步
- 共享配置和工具
- 便于协作和代码审查
- 适合中小型团队

**仓库结构**：
```
talent-insight/                          # 根仓库
├── .github/                             # GitHub 配置
│   ├── workflows/                       # CI/CD 工作流
│   │   ├── frontend-ci.yml
│   │   ├── backend-ci.yml
│   │   └── deploy.yml
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE.md
├── docs/                                # 📚 项目文档（核心）
│   ├── README.md                        # 文档总览
│   ├── product/                         # 产品文档
│   │   ├── product_charter.md
│   │   ├── user_persona.md
│   │   └── prd.md
│   ├── design/                          # 设计文档
│   │   ├── user_flow.md
│   │   ├── style-guide.md
│   │   └── screens/                     # 设计稿参考
│   │       ├── dashboard.png
│   │       └── ...
│   ├── architecture/                    # 架构文档
│   │   ├── system-architecture.md
│   │   ├── database-schema.md
│   │   └── api-specification.md
│   └── development/                     # 开发文档
│       ├── frontend-setup.md
│       ├── backend-integration.md
│       ├── testing-strategy.md
│       └── deployment-guide.md
├── frontend/                            # 前端应用
│   ├── public/
│   ├── src/
│   │   ├── api/
│   │   ├── assets/
│   │   ├── components/
│   │   ├── features/
│   │   ├── hooks/
│   │   ├── store/
│   │   ├── styles/
│   │   ├── types/
│   │   ├── utils/
│   │   ├── App.tsx
│   │   ├── main.tsx
│   │   └── router.tsx
│   ├── .env.development
│   ├── .env.production
│   ├── .eslintrc.json
│   ├── .prettierrc
│   ├── package.json
│   ├── tsconfig.json
│   ├── vite.config.ts
│   └── README.md
├── backend/                             # 后端应用
│   ├── app/
│   │   ├── api/                         # API 路由
│   │   │   ├── v1/
│   │   │   │   ├── talent.py
│   │   │   │   ├── risk.py
│   │   │   │   ├── team.py
│   │   │   │   └── employee.py
│   │   │   └── deps.py
│   │   ├── core/                        # 核心配置
│   │   │   ├── config.py
│   │   │   ├── security.py
│   │   │   └── database.py
│   │   ├── models/                      # 数据模型
│   │   │   ├── employee.py
│   │   │   ├── talent.py
│   │   │   └── risk.py
│   │   ├── repositories/                # 数据访问层
│   │   │   └── employee_repository.py
│   │   ├── services/                    # 业务逻辑层
│   │   │   ├── talent_service.py
│   │   │   ├── risk_service.py
│   │   │   └── ml_service.py
│   │   ├── schemas/                     # Pydantic schemas
│   │   │   └── talent.py
│   │   └── main.py
│   ├── alembic/                         # 数据库迁移
│   │   ├── versions/
│   │   └── env.py
│   ├── tests/
│   │   ├── unit/
│   │   ├── integration/
│   │   └── conftest.py
│   ├── .env.example
│   ├── requirements.txt
│   ├── Dockerfile
│   └── README.md
├── database/                            # 数据库脚本
│   ├── migrations/                      # SQL 迁移脚本
│   │   ├── 001_initial_schema.sql
│   │   └── 002_add_indexes.sql
│   ├── seeds/                           # 测试数据
│   │   └── sample_data.sql
│   └── README.md
├── infrastructure/                      # 基础设施配置
│   ├── docker/
│   │   ├── docker-compose.yml
│   │   ├── docker-compose.prod.yml
│   │   ├── frontend.Dockerfile
│   │   └── backend.Dockerfile
│   ├── kubernetes/                      # K8s 配置（可选）
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── terraform/                       # IaC（可选）
│       └── main.tf
├── scripts/                             # 实用脚本
│   ├── setup-dev.sh                     # 开发环境初始化
│   ├── seed-database.sh                 # 数据库初始化
│   └── deploy.sh                        # 部署脚本
├── .gitignore
├── .editorconfig
├── LICENSE
└── README.md                            # 项目总览
```

### 方案二：Multi-Repo（多仓库）

**适用场景**：
- 大型团队，前后端独立开发
- 需要独立版本控制
- 技术栈差异大

**仓库组织**：
```
talent-insight-frontend/                 # 前端仓库
talent-insight-backend/                  # 后端仓库
talent-insight-docs/                     # 文档仓库（共享）
talent-insight-infrastructure/           # 基础设施仓库
```

---

## 推荐的仓库组织方式

### 我的建议：**Monorepo + 清晰的目录分层**

**理由**：
1. 中小型项目，便于协作
2. 前后端版本同步
3. 文档与代码统一管理
4. CI/CD 更简单

### 立即行动：GitHub 仓库创建步骤

```bash
# 1. 在本地创建项目根目录
mkdir talent-insight
cd talent-insight

# 2. 初始化 Git 仓库
git init

# 3. 创建基础目录结构
mkdir -p .github/workflows
mkdir -p docs/{product,design,architecture,development}
mkdir -p frontend
mkdir -p backend
mkdir -p database/{migrations,seeds}
mkdir -p infrastructure/docker
mkdir -p scripts

# 4. 创建 .gitignore
cat > .gitignore << 'EOF'
# 依赖
node_modules/
__pycache__/
*.pyc
.venv/
venv/

# 环境变量
.env
.env.local
*.env.production

# IDE
.vscode/
.idea/
*.swp

# 构建产物
dist/
build/
*.log

# 数据库
*.db
*.sqlite

# 操作系统
.DS_Store
Thumbs.db
EOF

# 5. 创建根 README.md
cat > README.md << 'EOF'
# AI人才洞察平台

## 项目简介
AI驱动的人才管理与决策分析平台，帮助HRBP实现数据驱动的人才管理。

## 快速开始
- [前端开发指南](./frontend/README.md)
- [后端开发指南](./backend/README.md)
- [完整文档](./docs/README.md)

## 技术栈
- **前端**: React 18 + TypeScript + Vite + Redux Toolkit + Ant Design
- **后端**: FastAPI + PostgreSQL + Redis
- **部署**: Docker + Nginx

## 项目结构
见 [项目文档](./docs/README.md)
EOF

# 6. 在 GitHub 创建远程仓库（通过 GitHub CLI）
gh repo create talent-insight --public --description "AI人才洞察平台"

# 7. 关联远程仓库
git remote add origin https://github.com/your-username/talent-insight.git

# 8. 初始提交
git add .
git commit -m "chore: 初始化项目结构"
git branch -M main
git push -u origin main
```

---

## 文档调整建议

### 📋 需要调整的文档

#### 1. 重新组织文档目录结构

**当前状态**：
```
workspace/paraflow/
├── Global Context/
├── Feature Plan/
├── Style Guide/
├── Screen & Prototype/
├── README.md
├── DATABASE_SCHEMA.md
├── API_SPECIFICATION.md
├── BACKEND_INTEGRATION.md
├── FRONTEND_SETUP.md
└── TESTING_STRATEGY.md
```

**调整为**：
```
docs/
├── README.md                            # 📌 新增：文档导航总览
├── product/                             # 产品文档
│   ├── product-charter.md               # 从 Global Context 移动
│   ├── user-persona.md                  # 从 Global Context 移动
│   └── prd.md                           # 从 Feature Plan 移动
├── design/                              # 设计文档
│   ├── user-flow.md                     # 从 Feature Plan 移动
│   ├── style-guide-intelligent-tech.md  # 从 Style Guide 移动
│   └── screens/                         # 📌 新增：设计稿截图
│       ├── dashboard.png
│       ├── talent-recommend.png
│       └── ... (将HTML转为截图参考)
├── architecture/                        # 架构文档
│   ├── system-architecture.md           # 📌 新增：系统架构总览
│   ├── database-schema.md               # 原 DATABASE_SCHEMA.md
│   └── api-specification.md             # 原 API_SPECIFICATION.md
└── development/                         # 开发文档
    ├── getting-started.md               # 📌 新增：快速开始
    ├── frontend-setup.md                # 原 FRONTEND_SETUP.md
    ├── backend-integration.md           # 原 BACKEND_INTEGRATION.md
    ├── testing-strategy.md              # 原 TESTING_STRATEGY.md
    └── deployment-guide.md              # 📌 新增：本文档
```

#### 2. 新增必要文档

##### A. `docs/README.md` - 文档导航

```markdown
# 项目文档总览

## 📖 文档分类

### 产品文档
- [产品定位书](./product/product-charter.md) - 产品战略与核心价值
- [用户画像](./product/user-persona.md) - HRBP用户研究
- [产品需求文档](./product/prd.md) - 详细功能需求

### 设计文档
- [用户流程](./design/user-flow.md) - 页面导航与用户旅程
- [设计系统](./design/style-guide-intelligent-tech.md) - 视觉规范

### 架构文档
- [系统架构](./architecture/system-architecture.md) - 技术架构总览
- [数据库设计](./architecture/database-schema.md) - 数据模型详解
- [API规范](./architecture/api-specification.md) - 接口文档

### 开发文档
- [快速开始](./development/getting-started.md) - 5分钟上手
- [前端开发指南](./development/frontend-setup.md) - 前端项目搭建
- [后端开发指南](./development/backend-integration.md) - 后端开发与集成
- [测试策略](./development/testing-strategy.md) - 测试规范
- [部署指南](./development/deployment-guide.md) - 生产部署

## 🚀 根据角色查阅

**我是产品经理** → 阅读 `product/` 目录

**我是设计师** → 阅读 `design/` 目录

**我是前端开发** → 阅读 `development/frontend-setup.md` + `architecture/api-specification.md`

**我是后端开发** → 阅读 `development/backend-integration.md` + `architecture/database-schema.md`

**我是全栈开发** → 从 `development/getting-started.md` 开始
```

##### B. `docs/development/getting-started.md` - 快速开始

```markdown
# 快速开始指南

## 5分钟上手开发

### 前置要求

- Node.js 18+
- Python 3.11+
- PostgreSQL 15+
- Docker (可选)

### 方式一：Docker Compose（推荐）

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/talent-insight.git
cd talent-insight

# 2. 启动所有服务
docker-compose up -d

# 3. 访问应用
# 前端: http://localhost:3000
# 后端: http://localhost:8000
# API文档: http://localhost:8000/docs
```

### 方式二：本地开发

```bash
# 1. 启动后端
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# 编辑 .env 配置数据库连接
alembic upgrade head
uvicorn app.main:app --reload

# 2. 启动前端（新终端）
cd frontend
npm install
cp .env.development.example .env.development
# 编辑 .env.development 配置后端地址
npm run dev
```

### 验证安装

访问 http://localhost:3000，看到登录页面即成功。

### 下一步

- [前端开发指南](./frontend-setup.md)
- [后端开发指南](./backend-integration.md)
- [API 文档](../architecture/api-specification.md)
```

##### C. `docs/architecture/system-architecture.md` - 系统架构

```markdown
# 系统架构文档

## 整体架构

```
┌─────────────────────────────────────────────────────────────┐
│                     用户界面层 (Frontend)                      │
│  React 18 + TypeScript + Redux Toolkit + Ant Design         │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTPS / WebSocket
┌─────────────────────────────────────────────────────────────┐
│                      API 网关层 (Nginx)                        │
│  路由转发 + 负载均衡 + SSL终止 + 静态资源服务                   │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTP
┌─────────────────────────────────────────────────────────────┐
│                    应用服务层 (FastAPI)                        │
│  业务逻辑 + API路由 + 认证鉴权 + 后台任务                       │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                     数据层 (Databases)                        │
│  PostgreSQL (主库) + Redis (缓存) + Elasticsearch (搜索)      │
└─────────────────────────────────────────────────────────────┘
```

## 技术栈选型

详见各子系统文档。

## 部署架构

见 [部署指南](../development/deployment-guide.md)
```

#### 3. 调整现有文档

##### 需要修改的地方

**A. `frontend/README.md`**（新增）
```markdown
# 前端应用

## 快速开始
见 [前端开发指南](../docs/development/frontend-setup.md)

## 技术栈
- React 18
- TypeScript
- Vite
- Redux Toolkit
- Ant Design

## 项目结构
见文档
```

**B. `backend/README.md`**（新增）
```markdown
# 后端应用

## 快速开始
见 [后端开发指南](../docs/development/backend-integration.md)

## 技术栈
- FastAPI
- PostgreSQL
- Redis
- Celery

## API 文档
启动服务后访问: http://localhost:8000/docs
```

**C. 根目录 `README.md` 调整**
```markdown
# AI人才洞察平台

[![CI/CD](https://github.com/your-org/talent-insight/actions/workflows/ci.yml/badge.svg)](https://github.com/your-org/talent-insight/actions)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 🎯 项目简介

AI驱动的人才管理与决策分析平台，为HRBP提供：
- 🤖 AI人才推荐与匹配
- ⚠️ 智能人才风险预警
- 📊 团队能力分析盘点
- 📈 数据驱动决策支持

## 🚀 快速开始

```bash
# 使用 Docker Compose（推荐）
docker-compose up -d

# 访问应用
open http://localhost:3000
```

详细步骤见 [快速开始指南](./docs/development/getting-started.md)

## 📚 文档

- **产品文档**: [docs/product](./docs/product/)
- **设计文档**: [docs/design](./docs/design/)
- **架构文档**: [docs/architecture](./docs/architecture/)
- **开发文档**: [docs/development](./docs/development/)

## 🏗️ 项目结构

```
talent-insight/
├── frontend/          # React 前端应用
├── backend/           # FastAPI 后端应用
├── database/          # 数据库迁移脚本
├── docs/              # 📚 完整项目文档
└── infrastructure/    # Docker/K8s 配置
```

## 🛠️ 技术栈

- **前端**: React 18, TypeScript, Vite, Redux Toolkit, Ant Design
- **后端**: FastAPI, PostgreSQL, Redis, Celery
- **部署**: Docker, Nginx, GitHub Actions

## 📖 开发指南

### 前端开发
见 [前端开发指南](./docs/development/frontend-setup.md)

### 后端开发
见 [后端开发指南](./docs/development/backend-integration.md)

### 测试
见 [测试策略](./docs/development/testing-strategy.md)

## 🚢 部署

见 [部署指南](./docs/development/deployment-guide.md)

## 📄 License

MIT License - 见 [LICENSE](LICENSE) 文件
```

---

## 开发环境搭建

### 使用脚本自动化

创建 `scripts/setup-dev.sh`：

```bash
#!/bin/bash

echo "🚀 开始初始化开发环境..."

# 检查依赖
command -v node >/dev/null 2>&1 || { echo "❌ 需要安装 Node.js"; exit 1; }
command -v python3 >/dev/null 2>&1 || { echo "❌ 需要安装 Python 3"; exit 1; }
command -v docker >/dev/null 2>&1 || { echo "⚠️  建议安装 Docker"; }

# 启动数据库（Docker）
echo "📦 启动 PostgreSQL 和 Redis..."
docker-compose up -d postgres redis

# 等待数据库就绪
echo "⏳ 等待数据库启动..."
sleep 5

# 初始化后端
echo "🐍 设置后端环境..."
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
alembic upgrade head
cd ..

# 初始化前端
echo "⚛️  设置前端环境..."
cd frontend
npm install
cp .env.development.example .env.development
cd ..

echo "✅ 开发环境初始化完成！"
echo "📝 下一步："
echo "   1. 编辑 backend/.env 配置数据库连接"
echo "   2. 运行 'cd backend && uvicorn app.main:app --reload'"
echo "   3. 运行 'cd frontend && npm run dev'"
```

---

## 部署流程

### 生产环境部署

#### 1. 使用 Docker Compose

**infrastructure/docker/docker-compose.prod.yml**：

```yaml
version: '3.8'

services:
  frontend:
    build:
      context: ../../frontend
      dockerfile: ../infrastructure/docker/frontend.Dockerfile
    ports:
      - "80:80"
    environment:
      - VITE_API_URL=https://api.yourdomain.com
    depends_on:
      - backend

  backend:
    build:
      context: ../../backend
      dockerfile: ../infrastructure/docker/backend.Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@postgres:5432/talent_insight
      - REDIS_URL=redis://redis:6379
      - SECRET_KEY=${SECRET_KEY}
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=talent_user
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=talent_insight

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

  nginx:
    image: nginx:alpine
    ports:
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - /etc/letsencrypt:/etc/letsencrypt
    depends_on:
      - frontend
      - backend

volumes:
  postgres_data:
  redis_data:
```

#### 2. 部署脚本

**scripts/deploy.sh**：

```bash
#!/bin/bash

echo "🚀 开始部署到生产环境..."

# 拉取最新代码
git pull origin main

# 构建并启动服务
cd infrastructure/docker
docker-compose -f docker-compose.prod.yml up -d --build

# 等待服务启动
sleep 10

# 运行数据库迁移
docker-compose -f docker-compose.prod.yml exec backend alembic upgrade head

# 健康检查
curl -f http://localhost:8000/health || exit 1

echo "✅ 部署完成！"
```

---

## CI/CD 配置

### GitHub Actions

**.github/workflows/ci.yml**：

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  frontend-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json

      - name: Install dependencies
        working-directory: ./frontend
        run: npm ci

      - name: Lint
        working-directory: ./frontend
        run: npm run lint

      - name: Test
        working-directory: ./frontend
        run: npm run test:coverage

      - name: Build
        working-directory: ./frontend
        run: npm run build

  backend-test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: talent_insight_test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - uses: actions/checkout@v3

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        working-directory: ./backend
        run: |
          pip install -r requirements.txt
          pip install pytest pytest-cov

      - name: Lint
        working-directory: ./backend
        run: flake8 app/

      - name: Test
        working-directory: ./backend
        env:
          DATABASE_URL: postgresql://postgres:test@localhost/talent_insight_test
        run: pytest --cov=app --cov-report=xml

      - name: Upload coverage
        uses: codecov/codecov-action@v3

  deploy:
    needs: [frontend-test, backend-test]
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Deploy to production
        run: |
          # 这里添加实际的部署逻辑
          echo "Deploying to production..."
```

---

## 总结与行动清单

### ✅ 立即行动

1. **创建 GitHub 仓库**
   ```bash
   gh repo create talent-insight --public
   ```

2. **重组文档目录**
   - 将当前 `workspace/paraflow/` 下的文档按新结构迁移到 `docs/` 目录

3. **创建项目骨架**
   - 创建 `frontend/`、`backend/`、`database/` 目录
   - 添加 `.gitignore`、`docker-compose.yml`

4. **编写初始化脚本**
   - `scripts/setup-dev.sh`
   - `scripts/deploy.sh`

5. **配置 CI/CD**
   - 创建 `.github/workflows/ci.yml`

### 📋 文档调整清单

- [ ] 创建 `docs/README.md` 文档导航
- [ ] 移动并重命名现有文档到新目录结构
- [ ] 新增 `docs/development/getting-started.md`
- [ ] 新增 `docs/architecture/system-architecture.md`
- [ ] 新增 `frontend/README.md`
- [ ] 新增 `backend/README.md`
- [ ] 更新根目录 `README.md`
- [ ] 将 HTML 设计稿转换为截图放入 `docs/design/screens/`

### 🚀 开发建议

使用 VSCode 开发时，可以创建 `.vscode/settings.json`：

```json
{
  "files.associations": {
    "*.md": "markdown"
  },
  "editor.formatOnSave": true,
  "python.linting.enabled": true,
  "python.linting.flake8Enabled": true,
  "eslint.enable": true
}
```

---

**完成以上调整后，您的项目将具备完整的生产就绪能力！** 🎉
