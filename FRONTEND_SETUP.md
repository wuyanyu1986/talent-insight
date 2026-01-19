# 前端项目初始化指南

本文档提供基于已有 HTML 设计稿的前端项目完整初始化步骤，帮助开发团队快速搭建可运行的应用框架。

---

## 目录
1. [技术栈选择](#技术栈选择)
2. [项目初始化](#项目初始化)
3. [项目结构设计](#项目结构设计)
4. [样式系统配置](#样式系统配置)
5. [路由配置](#路由配置)
6. [状态管理配置](#状态管理配置)
7. [API 集成配置](#api-集成配置)
8. [组件开发指南](#组件开发指南)
9. [HTML 设计稿转换策略](#html-设计稿转换策略)
10. [开发工作流](#开发工作流)

---

## 技术栈选择

### 推荐方案：React + TypeScript

```json
{
  "framework": "React 18.2+",
  "language": "TypeScript 5.0+",
  "build": "Vite 5.0+",
  "routing": "React Router v6",
  "state": "Redux Toolkit + RTK Query",
  "ui": "Ant Design 5.x",
  "charts": "ECharts 5.x",
  "http": "Axios",
  "form": "React Hook Form",
  "styling": "Tailwind CSS 3.x + CSS Modules"
}
```

**选择理由：**
- React 18 并发特性提升大数据渲染性能
- TypeScript 提供类型安全，减少运行时错误
- Vite 提供极速的开发体验
- Redux Toolkit 简化状态管理
- Ant Design 提供企业级 UI 组件
- ECharts 强大的数据可视化能力

---

## 项目初始化

### 1. 创建项目

```bash
# 使用 Vite 创建项目
npm create vite@latest talent-insight-frontend -- --template react-ts

cd talent-insight-frontend
```

### 2. 安装核心依赖

```bash
# 核心框架
npm install react@^18.2.0 react-dom@^18.2.0

# 路由
npm install react-router-dom@^6.20.0

# 状态管理
npm install @reduxjs/toolkit@^2.0.0 react-redux@^9.0.0

# UI 组件库
npm install antd@^5.12.0

# 数据可视化
npm install echarts@^5.4.3 echarts-for-react@^3.0.2

# HTTP 客户端
npm install axios@^1.6.2

# 表单处理
npm install react-hook-form@^7.49.0

# 样式
npm install tailwindcss@^3.4.0 postcss@^8.4.32 autoprefixer@^10.4.16

# 工具库
npm install dayjs@^1.11.10 lodash-es@^4.17.21 clsx@^2.0.0

# 类型定义
npm install -D @types/lodash-es@^4.17.12
```

### 3. 安装开发依赖

```bash
npm install -D \
  eslint@^8.55.0 \
  eslint-plugin-react@^7.33.2 \
  eslint-plugin-react-hooks@^4.6.0 \
  @typescript-eslint/eslint-plugin@^6.15.0 \
  @typescript-eslint/parser@^6.15.0 \
  prettier@^3.1.1 \
  eslint-config-prettier@^9.1.0 \
  eslint-plugin-prettier@^5.0.1
```

---

## 项目结构设计

```
talent-insight-frontend/
├── public/                          # 静态资源
│   ├── favicon.ico
│   └── logo.svg
├── src/
│   ├── api/                         # API 接口层
│   │   ├── index.ts                 # Axios 实例配置
│   │   ├── talent.ts                # 人才推荐相关接口
│   │   ├── risk.ts                  # 风险预警相关接口
│   │   ├── team.ts                  # 团队评估相关接口
│   │   └── employee.ts              # 员工档案相关接口
│   ├── assets/                      # 资源文件
│   │   ├── images/
│   │   └── icons/
│   ├── components/                  # 通用组件
│   │   ├── Layout/                  # 布局组件
│   │   │   ├── MainLayout.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── Header.tsx
│   │   ├── Charts/                  # 图表组件
│   │   │   ├── RadarChart.tsx
│   │   │   ├── HeatmapChart.tsx
│   │   │   └── LineChart.tsx
│   │   ├── Common/                  # 通用组件
│   │   │   ├── LoadingSpinner.tsx
│   │   │   ├── EmptyState.tsx
│   │   │   └── ErrorBoundary.tsx
│   │   └── index.ts
│   ├── features/                    # 功能模块（按业务划分）
│   │   ├── dashboard/               # 工作台
│   │   │   ├── components/
│   │   │   ├── DashboardPage.tsx
│   │   │   └── index.ts
│   │   ├── talent/                  # 人才推荐
│   │   │   ├── components/
│   │   │   ├── TalentRecommendPage.tsx
│   │   │   ├── RecommendResultsPage.tsx
│   │   │   ├── TalentProfilePage.tsx
│   │   │   ├── talentSlice.ts       # Redux slice
│   │   │   └── index.ts
│   │   ├── risk/                    # 风险预警
│   │   │   ├── components/
│   │   │   ├── RiskAlertsPage.tsx
│   │   │   ├── RiskDetailPage.tsx
│   │   │   ├── riskSlice.ts
│   │   │   └── index.ts
│   │   └── team/                    # 团队评估
│   │       ├── components/
│   │       ├── TeamAssessmentPage.tsx
│   │       ├── teamSlice.ts
│   │       └── index.ts
│   ├── hooks/                       # 自定义 Hooks
│   │   ├── useAuth.ts
│   │   ├── useDebounce.ts
│   │   └── useWebSocket.ts
│   ├── store/                       # Redux Store
│   │   ├── index.ts                 # Store 配置
│   │   ├── rootReducer.ts
│   │   └── api/                     # RTK Query API
│   │       ├── talentApi.ts
│   │       ├── riskApi.ts
│   │       └── teamApi.ts
│   ├── types/                       # TypeScript 类型定义
│   │   ├── api.ts
│   │   ├── employee.ts
│   │   ├── talent.ts
│   │   ├── risk.ts
│   │   └── team.ts
│   ├── utils/                       # 工具函数
│   │   ├── format.ts
│   │   ├── validation.ts
│   │   └── constants.ts
│   ├── styles/                      # 全局样式
│   │   ├── globals.css
│   │   ├── variables.css            # CSS 变量（来自设计系统）
│   │   └── tailwind.css
│   ├── App.tsx                      # 根组件
│   ├── main.tsx                     # 入口文件
│   ├── router.tsx                   # 路由配置
│   └── vite-env.d.ts
├── .env.development                 # 开发环境变量
├── .env.production                  # 生产环境变量
├── .eslintrc.json                   # ESLint 配置
├── .prettierrc                      # Prettier 配置
├── tailwind.config.js               # Tailwind 配置
├── tsconfig.json                    # TypeScript 配置
├── vite.config.ts                   # Vite 配置
└── package.json
```

---

## 样式系统配置

### 1. Tailwind CSS 配置

**tailwind.config.js**
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        // 智能科技感风格配色
        primary: {
          DEFAULT: '#2DD4BF',
          hover: '#22C1AE',
          active: '#1BA897',
        },
        accent: {
          green: '#4ADE80',
          gray: '#64748B',
          silver: '#C7D2FE',
        },
        background: {
          DEFAULT: '#F8FAFC',
          secondary: '#E8F4F4',
        },
        text: {
          primary: '#0F172A',
          secondary: '#475569',
          tertiary: '#94A3B8',
        },
        status: {
          success: '#10B981',
          warning: '#F59E0B',
          error: '#EF4444',
          info: '#3B82F6',
        }
      },
      borderRadius: {
        'sm': '6px',
        'md': '8px',
        'lg': '12px',
        'xl': '16px',
      },
      boxShadow: {
        'sm': '0 1px 2px 0 rgba(0, 0, 0, 0.05)',
        'md': '0 4px 6px -1px rgba(0, 0, 0, 0.08)',
        'lg': '0 10px 15px -3px rgba(0, 0, 0, 0.1)',
        'xl': '0 20px 25px -5px rgba(0, 0, 0, 0.1)',
      },
      fontFamily: {
        sans: ['-apple-system', 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'sans-serif'],
      },
    },
  },
  plugins: [],
  corePlugins: {
    preflight: false, // 禁用 Tailwind 基础样式重置，避免与 Ant Design 冲突
  },
}
```

### 2. CSS 变量配置

**src/styles/variables.css**
```css
:root {
  /* 主色 */
  --primary-color: #2DD4BF;
  --primary-hover: #22C1AE;
  --primary-active: #1BA897;

  /* 辅助色 */
  --accent-1: #4ADE80;
  --accent-2: #64748B;
  --accent-3: #C7D2FE;

  /* 背景 */
  --background: #F8FAFC;
  --background-gradient: linear-gradient(135deg, #F8FAFC 0%, #E8F4F4 100%);
  --surface: #FFFFFF;
  --surface-secondary: #F8FAFC;

  /* 文字 */
  --text-primary: #0F172A;
  --text-secondary: #475569;
  --text-tertiary: #94A3B8;

  /* 状态色 */
  --success: #10B981;
  --warning: #F59E0B;
  --error: #EF4444;
  --info: #3B82F6;

  /* 圆角 */
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;

  /* 阴影 */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.08);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);

  /* 字体 */
  --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  --font-size-xs: 12px;
  --font-size-sm: 14px;
  --font-size-base: 16px;
  --font-size-lg: 18px;
  --font-size-xl: 20px;
  --font-size-2xl: 24px;
  --font-size-3xl: 30px;
}
```

### 3. Ant Design 主题配置

**src/App.tsx**
```typescript
import { ConfigProvider } from 'antd';
import zhCN from 'antd/locale/zh_CN';

const antdTheme = {
  token: {
    colorPrimary: '#2DD4BF',
    colorSuccess: '#10B981',
    colorWarning: '#F59E0B',
    colorError: '#EF4444',
    colorInfo: '#3B82F6',
    borderRadius: 8,
    fontFamily: '-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif',
  },
};

function App() {
  return (
    <ConfigProvider theme={antdTheme} locale={zhCN}>
      {/* 应用内容 */}
    </ConfigProvider>
  );
}
```

---

## 路由配置

**src/router.tsx**
```typescript
import { createBrowserRouter, Navigate } from 'react-router-dom';
import MainLayout from './components/Layout/MainLayout';
import DashboardPage from './features/dashboard/DashboardPage';
import TalentRecommendPage from './features/talent/TalentRecommendPage';
import RecommendResultsPage from './features/talent/RecommendResultsPage';
import TalentProfilePage from './features/talent/TalentProfilePage';
import RiskAlertsPage from './features/risk/RiskAlertsPage';
import RiskDetailPage from './features/risk/RiskDetailPage';
import TeamAssessmentPage from './features/team/TeamAssessmentPage';

export const router = createBrowserRouter([
  {
    path: '/',
    element: <MainLayout />,
    children: [
      {
        index: true,
        element: <Navigate to="/dashboard" replace />,
      },
      {
        path: 'dashboard',
        element: <DashboardPage />,
      },
      {
        path: 'talent',
        children: [
          {
            path: 'recommend',
            element: <TalentRecommendPage />,
          },
          {
            path: 'recommend/results/:recommendationId',
            element: <RecommendResultsPage />,
          },
          {
            path: 'profile/:employeeId',
            element: <TalentProfilePage />,
          },
        ],
      },
      {
        path: 'risk',
        children: [
          {
            path: 'alerts',
            element: <RiskAlertsPage />,
          },
          {
            path: 'detail/:alertId',
            element: <RiskDetailPage />,
          },
        ],
      },
      {
        path: 'team',
        children: [
          {
            path: 'assessment',
            element: <TeamAssessmentPage />,
          },
        ],
      },
    ],
  },
]);
```

**src/main.tsx**
```typescript
import React from 'react';
import ReactDOM from 'react-dom/client';
import { RouterProvider } from 'react-router-dom';
import { Provider } from 'react-redux';
import { ConfigProvider } from 'antd';
import zhCN from 'antd/locale/zh_CN';
import { router } from './router';
import { store } from './store';
import './styles/globals.css';

const antdTheme = {
  token: {
    colorPrimary: '#2DD4BF',
    borderRadius: 8,
  },
};

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <Provider store={store}>
      <ConfigProvider theme={antdTheme} locale={zhCN}>
        <RouterProvider router={router} />
      </ConfigProvider>
    </Provider>
  </React.StrictMode>
);
```

---

## 状态管理配置

### 1. Store 配置

**src/store/index.ts**
```typescript
import { configureStore } from '@reduxjs/toolkit';
import { setupListeners } from '@reduxjs/toolkit/query';
import { talentApi } from './api/talentApi';
import { riskApi } from './api/riskApi';
import { teamApi } from './api/teamApi';
import talentReducer from '../features/talent/talentSlice';
import riskReducer from '../features/risk/riskSlice';
import teamReducer from '../features/team/teamSlice';

export const store = configureStore({
  reducer: {
    // RTK Query APIs
    [talentApi.reducerPath]: talentApi.reducer,
    [riskApi.reducerPath]: riskApi.reducer,
    [teamApi.reducerPath]: teamApi.reducer,

    // Feature slices
    talent: talentReducer,
    risk: riskReducer,
    team: teamReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(
      talentApi.middleware,
      riskApi.middleware,
      teamApi.middleware
    ),
});

setupListeners(store.dispatch);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### 2. RTK Query API 配置

**src/store/api/talentApi.ts**
```typescript
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';
import type {
  TalentRecommendation,
  JobRequirement,
  CreateRecommendationRequest
} from '../../types/talent';

export const talentApi = createApi({
  reducerPath: 'talentApi',
  baseQuery: fetchBaseQuery({
    baseUrl: import.meta.env.VITE_API_URL,
    prepareHeaders: (headers) => {
      const token = localStorage.getItem('access_token');
      if (token) {
        headers.set('Authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['Recommendation', 'Employee'],
  endpoints: (builder) => ({
    createRecommendation: builder.mutation<TalentRecommendation, CreateRecommendationRequest>({
      query: (data) => ({
        url: '/talent/recommendations',
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Recommendation'],
    }),
    getRecommendation: builder.query<TalentRecommendation, string>({
      query: (id) => `/talent/recommendations/${id}`,
      providesTags: ['Recommendation'],
    }),
    getEmployeeProfile: builder.query<any, string>({
      query: (id) => `/employees/${id}/profile`,
      providesTags: ['Employee'],
    }),
  }),
});

export const {
  useCreateRecommendationMutation,
  useGetRecommendationQuery,
  useGetEmployeeProfileQuery,
} = talentApi;
```

---

## API 集成配置

### 1. Axios 实例配置

**src/api/index.ts**
```typescript
import axios, { AxiosError, AxiosRequestConfig, AxiosResponse } from 'axios';
import { message } from 'antd';

// 创建 axios 实例
export const apiClient = axios.create({
  baseURL: import.meta.env.VITE_API_URL || 'http://localhost:8000/api/v1',
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// 请求拦截器
apiClient.interceptors.request.use(
  (config: any) => {
    // 添加 Token
    const token = localStorage.getItem('access_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error: AxiosError) => {
    return Promise.reject(error);
  }
);

// 响应拦截器
apiClient.interceptors.response.use(
  (response: AxiosResponse) => {
    return response.data;
  },
  (error: AxiosError<any>) => {
    // 错误处理
    if (error.response) {
      const { status, data } = error.response;

      switch (status) {
        case 401:
          message.error('认证失败，请重新登录');
          localStorage.removeItem('access_token');
          window.location.href = '/login';
          break;
        case 403:
          message.error('无权限访问');
          break;
        case 404:
          message.error('请求的资源不存在');
          break;
        case 429:
          message.error('请求过于频繁，请稍后再试');
          break;
        case 500:
          message.error('服务器错误，请稍后再试');
          break;
        default:
          message.error(data?.message || '请求失败');
      }
    } else if (error.request) {
      message.error('网络错误，请检查网络连接');
    } else {
      message.error('请求配置错误');
    }

    return Promise.reject(error);
  }
);

export default apiClient;
```

### 2. 环境变量配置

**.env.development**
```bash
VITE_API_URL=http://localhost:8000/api/v1
VITE_WS_URL=ws://localhost:8000/ws
VITE_APP_ENV=development
```

**.env.production**
```bash
VITE_API_URL=https://api.talent-insight.com/api/v1
VITE_WS_URL=wss://api.talent-insight.com/ws
VITE_APP_ENV=production
```

---

## 组件开发指南

### 1. 页面组件模板

**src/features/dashboard/DashboardPage.tsx**
```typescript
import React, { useEffect } from 'react';
import { Row, Col, Card, Statistic } from 'antd';
import { useAppDispatch, useAppSelector } from '../../hooks/redux';
import { fetchDashboardData } from './dashboardSlice';

const DashboardPage: React.FC = () => {
  const dispatch = useAppDispatch();
  const { loading, data, error } = useAppSelector((state) => state.dashboard);

  useEffect(() => {
    dispatch(fetchDashboardData());
  }, [dispatch]);

  if (loading) return <div>加载中...</div>;
  if (error) return <div>加载失败: {error}</div>;

  return (
    <div className="dashboard-page">
      <h1 className="text-2xl font-bold mb-6">工作台</h1>

      <Row gutter={[16, 16]}>
        <Col span={6}>
          <Card>
            <Statistic
              title="待处理风险"
              value={data?.riskCount || 0}
              valueStyle={{ color: '#EF4444' }}
            />
          </Card>
        </Col>
        {/* 更多统计卡片 */}
      </Row>
    </div>
  );
};

export default DashboardPage;
```

### 2. 通用组件示例

**src/components/Charts/RadarChart.tsx**
```typescript
import React, { useRef, useEffect } from 'react';
import * as echarts from 'echarts';
import type { EChartsOption } from 'echarts';

interface RadarChartProps {
  data: {
    indicator: Array<{ name: string; max: number }>;
    value: number[];
  };
  height?: number;
}

const RadarChart: React.FC<RadarChartProps> = ({ data, height = 300 }) => {
  const chartRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!chartRef.current) return;

    const chart = echarts.init(chartRef.current);

    const option: EChartsOption = {
      radar: {
        indicator: data.indicator,
        shape: 'circle',
        splitNumber: 4,
        name: {
          textStyle: {
            color: '#475569',
          },
        },
        splitLine: {
          lineStyle: {
            color: '#E2E8F0',
          },
        },
        splitArea: {
          areaStyle: {
            color: ['rgba(45, 212, 191, 0.1)', 'rgba(45, 212, 191, 0.05)'],
          },
        },
      },
      series: [
        {
          type: 'radar',
          data: [
            {
              value: data.value,
              name: '能力评分',
              areaStyle: {
                color: 'rgba(45, 212, 191, 0.3)',
              },
              lineStyle: {
                color: '#2DD4BF',
                width: 2,
              },
              itemStyle: {
                color: '#2DD4BF',
              },
            },
          ],
        },
      ],
    };

    chart.setOption(option);

    // 响应式
    const resizeObserver = new ResizeObserver(() => {
      chart.resize();
    });
    resizeObserver.observe(chartRef.current);

    return () => {
      chart.dispose();
      resizeObserver.disconnect();
    };
  }, [data]);

  return <div ref={chartRef} style={{ height }} />;
};

export default RadarChart;
```

---

## HTML 设计稿转换策略

### 转换原则

1. **结构分离**：HTML 结构 → React 组件
2. **样式提取**：内联样式 → CSS Modules / Tailwind
3. **数据驱动**：静态内容 → Props / State
4. **交互实现**：静态 UI → 事件处理

### 转换步骤

#### 1. 分析 HTML 设计稿

以 `dashboard-intelligent-tech.html` 为例：

```html
<!-- 原始 HTML -->
<div class="stat-card">
  <div class="stat-label">待处理风险</div>
  <div class="stat-value">12</div>
</div>
```

#### 2. 创建 React 组件

```typescript
// StatCard.tsx
interface StatCardProps {
  label: string;
  value: number;
  color?: string;
}

const StatCard: React.FC<StatCardProps> = ({ label, value, color }) => {
  return (
    <Card className="stat-card">
      <div className="text-sm text-gray-600">{label}</div>
      <div className="text-3xl font-bold" style={{ color }}>{value}</div>
    </Card>
  );
};
```

#### 3. 集成到页面

```typescript
// DashboardPage.tsx
const DashboardPage: React.FC = () => {
  const { data } = useDashboardData();

  return (
    <Row gutter={16}>
      <Col span={6}>
        <StatCard label="待处理风险" value={data.riskCount} color="#EF4444" />
      </Col>
    </Row>
  );
};
```

### 批量转换工具

可以开发简单的脚本辅助转换：

```javascript
// html-to-jsx-converter.js
const fs = require('fs');
const { parse } = require('node-html-parser');

function convertHtmlToJsx(htmlFilePath) {
  const html = fs.readFileSync(htmlFilePath, 'utf-8');
  const root = parse(html);

  // 提取主体内容
  const mainContent = root.querySelector('.main-content');

  // 转换为 JSX（简化示例）
  const jsx = mainContent.toString()
    .replace(/class=/g, 'className=')
    .replace(/for=/g, 'htmlFor=');

  console.log(jsx);
}
```

---

## 开发工作流

### 1. 启动开发服务器

```bash
npm run dev
```

### 2. 代码规范检查

```bash
# ESLint 检查
npm run lint

# Prettier 格式化
npm run format

# TypeScript 类型检查
npm run type-check
```

### 3. 构建生产版本

```bash
npm run build

# 预览生产构建
npm run preview
```

### 4. 推荐的 VSCode 扩展

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "dsznajder.es7-react-js-snippets"
  ]
}
```

### 5. Git Hooks（可选）

安装 husky 和 lint-staged：

```bash
npm install -D husky lint-staged

# 初始化 husky
npx husky-init
```

**package.json**
```json
{
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
}
```

---

## 开发优先级建议

### Phase 1：基础框架搭建（1周）
- [x] 项目初始化
- [x] 路由配置
- [x] 样式系统
- [x] API 集成
- [x] 状态管理
- [x] 布局组件

### Phase 2：核心页面开发（2-3周）
- [ ] 工作台首页
- [ ] AI人才推荐页
- [ ] 推荐结果列表
- [ ] 人才画像详情
- [ ] 风险预警列表
- [ ] 风险详情页
- [ ] 团队能力盘点

### Phase 3：功能完善（1-2周）
- [ ] 表单验证
- [ ] 错误处理
- [ ] Loading 状态
- [ ] 权限控制
- [ ] WebSocket 实时通知

### Phase 4：优化与测试（1周）
- [ ] 性能优化
- [ ] 单元测试
- [ ] E2E 测试
- [ ] 代码审查

---

## 常见问题

### Q1: Ant Design 和 Tailwind CSS 冲突如何解决？

在 `tailwind.config.js` 中禁用 preflight：

```javascript
corePlugins: {
  preflight: false,
}
```

### Q2: 如何处理图表自适应？

使用 ResizeObserver 监听容器尺寸变化：

```typescript
useEffect(() => {
  const resizeObserver = new ResizeObserver(() => {
    chart.resize();
  });
  resizeObserver.observe(containerRef.current);
  return () => resizeObserver.disconnect();
}, []);
```

### Q3: 如何优化大列表渲染性能？

使用虚拟滚动库：

```bash
npm install react-window
```

---

## 参考资源

- **React 官方文档**：https://react.dev/
- **Vite 文档**：https://vitejs.dev/
- **Ant Design**：https://ant.design/
- **Redux Toolkit**：https://redux-toolkit.js.org/
- **ECharts 文档**：https://echarts.apache.org/zh/index.html
- **Tailwind CSS**：https://tailwindcss.com/

---

## 下一步

完成前端项目初始化后，建议按照以下顺序开发：

1. **先开发布局和导航** → 让整体结构跑起来
2. **再开发单个页面** → 从简单到复杂（Dashboard → 列表页 → 详情页）
3. **逐步集成 API** → 先用 Mock 数据，后接真实接口
4. **持续重构优化** → 提取公共组件，优化性能

祝开发顺利！🚀
