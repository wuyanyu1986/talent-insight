# 前后端数据连接指南

## 目录
1. [架构概览](#架构概览)
2. [前端技术栈](#前端技术栈)
3. [后端技术栈](#后端技术栈)
4. [API通信方案](#api通信方案)
5. [数据流示例](#数据流示例)
6. [状态管理](#状态管理)
7. [代码示例](#代码示例)
8. [部署方案](#部署方案)

---

## 架构概览

```
┌─────────────────────────────────────────────────────────────┐
│                         前端层 (Frontend)                      │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  React/Vue.js + TypeScript                           │   │
│  │  ├─ Pages (基于HTML设计稿)                            │   │
│  │  ├─ Components (UI组件库)                            │   │
│  │  ├─ State Management (Redux/Pinia)                  │   │
│  │  └─ API Client (Axios/Fetch)                        │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTPS / WebSocket
┌─────────────────────────────────────────────────────────────┐
│                       API网关层 (API Gateway)                 │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Nginx / Kong / AWS API Gateway                      │   │
│  │  ├─ 路由转发                                          │   │
│  │  ├─ 认证鉴权 (JWT验证)                                │   │
│  │  ├─ 限流熔断                                          │   │
│  │  └─ 日志监控                                          │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕ HTTP
┌─────────────────────────────────────────────────────────────┐
│                      后端服务层 (Backend Services)            │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  FastAPI / Express / Spring Boot                     │   │
│  │  ├─ RESTful API Controllers                         │   │
│  │  ├─ Business Logic Layer                            │   │
│  │  ├─ AI/ML Service (推荐算法、风险预测)                 │   │
│  │  └─ Data Integration Layer (多源数据同步)             │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                       数据层 (Data Layer)                     │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │ PostgreSQL │  │   Redis    │  │  Elasticsearch│          │
│  │  (主数据库)  │  │  (缓存)    │  │   (全文搜索)  │          │
│  └────────────┘  └────────────┘  └────────────┘            │
│  ┌────────────┐  ┌────────────┐                            │
│  │  消息队列   │  │  对象存储   │                            │
│  │ RabbitMQ   │  │   S3/OSS   │                            │
│  └────────────┘  └────────────┘                            │
└─────────────────────────────────────────────────────────────┘
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                    外部系统 (External Systems)                │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │  HR系统     │  │  业务系统   │  │  文件存储   │            │
│  │ (SAP/北森)  │  │  (CRM/ERP) │  │  (NAS等)   │            │
│  └────────────┘  └────────────┘  └────────────┘            │
└─────────────────────────────────────────────────────────────┘
```

---

## 前端技术栈

### 推荐技术选型

#### 方案A：React生态（推荐）
```javascript
// 核心技术栈
{
  "framework": "React 18+",
  "language": "TypeScript",
  "build": "Vite",
  "routing": "React Router v6",
  "state": "Redux Toolkit + RTK Query",
  "ui": "Ant Design 5.x",
  "charts": "ECharts / Recharts",
  "http": "Axios",
  "form": "React Hook Form",
  "styling": "Tailwind CSS + CSS Modules"
}
```

#### 方案B：Vue生态
```javascript
{
  "framework": "Vue 3",
  "language": "TypeScript",
  "build": "Vite",
  "routing": "Vue Router",
  "state": "Pinia",
  "ui": "Element Plus",
  "charts": "ECharts",
  "http": "Axios",
  "form": "VeeValidate",
  "styling": "Tailwind CSS"
}
```

---

## 后端技术栈

### 推荐技术选型

#### 方案A：Python FastAPI（推荐，AI/ML友好）
```python
# 核心技术栈
{
  "framework": "FastAPI",
  "language": "Python 3.11+",
  "orm": "SQLAlchemy 2.0",
  "migration": "Alembic",
  "validation": "Pydantic V2",
  "auth": "python-jose (JWT)",
  "celery": "Celery + Redis (异步任务)",
  "ai_ml": "scikit-learn / pandas / numpy",
  "testing": "pytest"
}
```

#### 方案B：Node.js Express
```javascript
{
  "framework": "Express.js",
  "language": "TypeScript",
  "orm": "Prisma / TypeORM",
  "validation": "Zod",
  "auth": "jsonwebtoken",
  "queue": "Bull (Redis)",
  "testing": "Jest"
}
```

#### 方案C：Java Spring Boot（企业级）
```java
{
  "framework": "Spring Boot 3.x",
  "language": "Java 17+",
  "orm": "Spring Data JPA",
  "security": "Spring Security + JWT",
  "queue": "Spring AMQP (RabbitMQ)",
  "cache": "Spring Cache + Redis",
  "testing": "JUnit 5"
}
```

---

## API通信方案

### 1. RESTful API规范

#### 基础URL结构
```
开发环境：http://localhost:8000/api/v1
测试环境：https://api-test.talent-insight.com/api/v1
生产环境：https://api.talent-insight.com/api/v1
```

#### 请求格式
```http
# 通用请求头
POST /api/v1/talent/recommend
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>

# 请求体
{
  "title": "高级产品经理",
  "scenario": "internal_recruitment",
  "required_skills": [
    {
      "name": "产品设计",
      "level": "高级",
      "priority": "required"
    }
  ],
  "weights": {
    "hard_skills": 0.3,
    "project_experience": 0.3,
    "growth_potential": 0.2,
    "culture_fit": 0.2
  }
}
```

#### 响应格式
```json
// 成功响应
{
  "code": 200,
  "message": "success",
  "data": {
    "id": "rec_123456",
    "job_requirement_id": "job_789",
    "created_at": "2024-01-16T10:00:00Z",
    "candidates": [
      {
        "employee_id": "emp_001",
        "employee_name": "张三",
        "match_score": 0.92,
        "match_reasons": [...]
      }
    ]
  },
  "timestamp": "2024-01-16T10:00:00Z"
}

// 错误响应
{
  "code": 400,
  "message": "Invalid request parameters",
  "errors": [
    {
      "field": "weights",
      "message": "权重总和必须等于1.0"
    }
  ],
  "timestamp": "2024-01-16T10:00:00Z"
}
```

### 2. WebSocket实时通信（可选）

```javascript
// 用于实时通知、进度推送
const ws = new WebSocket('wss://api.talent-insight.com/ws');

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);

  // 推荐任务进度更新
  if (data.type === 'recommendation_progress') {
    updateProgress(data.progress);
  }

  // 新风险预警推送
  if (data.type === 'new_risk_alert') {
    showNotification(data.alert);
  }
};
```

---

## 数据流示例

### 示例1：AI人才推荐流程

```
用户操作                前端处理              后端处理              数据库/AI服务
   │                      │                     │                      │
   │ 填写推荐需求          │                     │                      │
   │─────────────────────>│                     │                      │
   │                      │ 表单验证            │                      │
   │                      │ 构建请求体          │                      │
   │                      │                     │                      │
   │                      │ POST /recommend     │                      │
   │                      │────────────────────>│                      │
   │                      │                     │ 参数验证             │
   │                      │                     │ 创建任务记录         │
   │                      │                     │─────────────────────>│
   │                      │                     │ 返回任务ID           │
   │                      │<────────────────────│                      │
   │ 显示加载状态          │                     │                      │
   │<─────────────────────│                     │                      │
   │                      │                     │ 异步执行AI推荐算法    │
   │                      │                     │<─────────────────────│
   │                      │                     │ 获取员工数据          │
   │                      │                     │─────────────────────>│
   │                      │                     │ 计算匹配度           │
   │                      │                     │ 保存推荐结果         │
   │                      │                     │─────────────────────>│
   │                      │ WebSocket推送完成   │                      │
   │                      │<────────────────────│                      │
   │                      │ 跳转结果页          │                      │
   │                      │                     │                      │
   │                      │ GET /recommend/:id  │                      │
   │                      │────────────────────>│                      │
   │                      │                     │ 查询推荐结果         │
   │                      │                     │─────────────────────>│
   │ 展示推荐列表          │<────────────────────│                      │
   │<─────────────────────│                     │                      │
```

### 示例2：人才画像详情查询

```
用户操作                前端处理              后端处理              数据库
   │                      │                     │                      │
   │ 点击候选人卡片        │                     │                      │
   │─────────────────────>│                     │                      │
   │                      │ 跳转详情页          │                      │
   │                      │ GET /profile/:id    │                      │
   │                      │────────────────────>│                      │
   │                      │                     │ 检查缓存(Redis)      │
   │                      │                     │─────────────────────>│
   │                      │                     │ 缓存未命中           │
   │                      │                     │ 查询主数据库         │
   │                      │                     │─────────────────────>│
   │                      │                     │ 聚合多表数据          │
   │                      │                     │ (员工/绩效/项目/技能) │
   │                      │                     │ 写入缓存(30min)      │
   │                      │                     │─────────────────────>│
   │ 展示完整人才画像      │<────────────────────│                      │
   │<─────────────────────│                     │                      │
```

### 示例3：风险预警后台任务

```
定时任务调度器          后端服务              AI/ML服务            数据库             WebSocket服务
   │                      │                     │                      │                     │
   │ 每日凌晨2点触发       │                     │                      │                     │
   │─────────────────────>│                     │                      │                     │
   │                      │ 获取需监控的岗位     │                      │                     │
   │                      │─────────────────────────────────────────>│                     │
   │                      │                     │                      │                     │
   │                      │ 调用风险预测模型     │                      │                     │
   │                      │────────────────────>│                      │                     │
   │                      │                     │ 分析员工行为数据      │                     │
   │                      │                     │ 计算离职风险评分      │                     │
   │                      │                     │ 检测继任空白         │                     │
   │                      │<────────────────────│                      │                     │
   │                      │ 创建风险预警记录     │                      │                     │
   │                      │─────────────────────────────────────────>│                     │
   │                      │ 推送实时通知         │                      │                     │
   │                      │──────────────────────────────────────────────────────────────>│
   │                      │                     │                      │                     │
   │                      │                     │                      │                     │ 推送到在线用户
   │                      │                     │                      │                     │────────────────>
```

---

## 状态管理

### Redux Toolkit示例（React）

```typescript
// store/slices/talentSlice.ts
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';
import { talentApi } from '@/services/api';

// 异步Thunk
export const fetchTalentProfile = createAsyncThunk(
  'talent/fetchProfile',
  async (employeeId: string) => {
    const response = await talentApi.getProfile(employeeId);
    return response.data;
  }
);

export const createRecommendation = createAsyncThunk(
  'talent/createRecommendation',
  async (requirement: JobRequirement) => {
    const response = await talentApi.createRecommendation(requirement);
    return response.data;
  }
);

// Slice
const talentSlice = createSlice({
  name: 'talent',
  initialState: {
    currentProfile: null,
    recommendations: [],
    loading: false,
    error: null,
  },
  reducers: {
    clearProfile: (state) => {
      state.currentProfile = null;
    },
  },
  extraReducers: (builder) => {
    builder
      // 获取人才画像
      .addCase(fetchTalentProfile.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchTalentProfile.fulfilled, (state, action) => {
        state.loading = false;
        state.currentProfile = action.payload;
      })
      .addCase(fetchTalentProfile.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      })
      // 创建推荐
      .addCase(createRecommendation.fulfilled, (state, action) => {
        state.recommendations.push(action.payload);
      });
  },
});

export const { clearProfile } = talentSlice.actions;
export default talentSlice.reducer;
```

### RTK Query示例（推荐用于API调用）

```typescript
// store/api/talentApi.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const talentApi = createApi({
  reducerPath: 'talentApi',
  baseQuery: fetchBaseQuery({
    baseUrl: process.env.REACT_APP_API_URL,
    prepareHeaders: (headers, { getState }) => {
      const token = (getState() as RootState).auth.token;
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['TalentProfile', 'Recommendation', 'RiskAlert'],
  endpoints: (builder) => ({
    // 获取人才画像
    getTalentProfile: builder.query<TalentProfile, string>({
      query: (id) => `/talent/profile/${id}`,
      providesTags: (result, error, id) => [{ type: 'TalentProfile', id }],
    }),

    // 创建推荐
    createRecommendation: builder.mutation<Recommendation, JobRequirement>({
      query: (requirement) => ({
        url: '/talent/recommend',
        method: 'POST',
        body: requirement,
      }),
      invalidatesTags: [{ type: 'Recommendation', id: 'LIST' }],
    }),

    // 获取风险预警列表
    getRiskAlerts: builder.query<RiskAlert[], RiskAlertFilters>({
      query: (filters) => ({
        url: '/risk/alerts',
        params: filters,
      }),
      providesTags: ['RiskAlert'],
    }),
  }),
});

export const {
  useGetTalentProfileQuery,
  useCreateRecommendationMutation,
  useGetRiskAlertsQuery,
} = talentApi;
```

---

## 代码示例

### 前端：AI人才推荐页面

```typescript
// pages/TalentRecommend/index.tsx
import React, { useState } from 'react';
import { Form, Input, Button, Select, Slider, message } from 'antd';
import { useNavigate } from 'react-router-dom';
import { useCreateRecommendationMutation } from '@/store/api/talentApi';

const TalentRecommend: React.FC = () => {
  const navigate = useNavigate();
  const [form] = Form.useForm();
  const [createRecommendation, { isLoading }] = useCreateRecommendationMutation();

  const handleSubmit = async (values: any) => {
    try {
      // 调用API创建推荐
      const result = await createRecommendation({
        title: values.title,
        scenario: values.scenario,
        required_skills: values.skills.map((skill: any) => ({
          name: skill.name,
          level: skill.level,
          priority: 'required',
        })),
        weights: {
          hard_skills: values.weights.hardSkills / 100,
          project_experience: values.weights.projectExperience / 100,
          growth_potential: values.weights.growthPotential / 100,
          culture_fit: values.weights.cultureFit / 100,
        },
      }).unwrap();

      message.success('推荐任务已创建');

      // 跳转到结果页
      navigate(`/talent/recommend/results?id=${result.id}`);
    } catch (error) {
      message.error('创建推荐失败，请重试');
      console.error('Failed to create recommendation:', error);
    }
  };

  return (
    <div className="talent-recommend-page">
      <h1>AI人才推荐</h1>

      <Form
        form={form}
        layout="vertical"
        onFinish={handleSubmit}
        initialValues={{
          scenario: 'internal_recruitment',
          weights: {
            hardSkills: 30,
            projectExperience: 30,
            growthPotential: 20,
            cultureFit: 20,
          },
        }}
      >
        <Form.Item
          label="推荐场景"
          name="scenario"
          rules={[{ required: true }]}
        >
          <Select>
            <Select.Option value="internal_recruitment">内部招聘</Select.Option>
            <Select.Option value="succession_planning">继任规划</Select.Option>
            <Select.Option value="project_team">项目组队</Select.Option>
          </Select>
        </Form.Item>

        <Form.Item
          label="岗位名称"
          name="title"
          rules={[{ required: true, message: '请输入岗位名称' }]}
        >
          <Input placeholder="例如：高级产品经理" />
        </Form.Item>

        {/* 技能要求 */}
        <Form.List name="skills">
          {(fields, { add, remove }) => (
            <>
              {fields.map((field) => (
                <div key={field.key} className="skill-item">
                  <Form.Item
                    {...field}
                    name={[field.name, 'name']}
                    rules={[{ required: true }]}
                  >
                    <Input placeholder="技能名称" />
                  </Form.Item>
                  <Form.Item
                    {...field}
                    name={[field.name, 'level']}
                  >
                    <Select placeholder="熟练度">
                      <Select.Option value="初级">初级</Select.Option>
                      <Select.Option value="中级">中级</Select.Option>
                      <Select.Option value="高级">高级</Select.Option>
                    </Select>
                  </Form.Item>
                  <Button onClick={() => remove(field.name)}>删除</Button>
                </div>
              ))}
              <Button type="dashed" onClick={() => add()}>
                + 添加技能
              </Button>
            </>
          )}
        </Form.List>

        {/* 权重调整 */}
        <div className="weights-section">
          <h3>匹配权重调整</h3>

          <Form.Item label="硬技能匹配度" name={['weights', 'hardSkills']}>
            <Slider min={0} max={100} marks={{ 0: '0%', 100: '100%' }} />
          </Form.Item>

          <Form.Item label="项目经验相关性" name={['weights', 'projectExperience']}>
            <Slider min={0} max={100} marks={{ 0: '0%', 100: '100%' }} />
          </Form.Item>

          <Form.Item label="成长潜力" name={['weights', 'growthPotential']}>
            <Slider min={0} max={100} marks={{ 0: '0%', 100: '100%' }} />
          </Form.Item>

          <Form.Item label="文化契合度" name={['weights', 'cultureFit']}>
            <Slider min={0} max={100} marks={{ 0: '0%', 100: '100%' }} />
          </Form.Item>
        </div>

        <Form.Item>
          <Button type="primary" htmlType="submit" loading={isLoading} size="large">
            开始推荐
          </Button>
        </Form.Item>
      </Form>
    </div>
  );
};

export default TalentRecommend;
```

### 后端：FastAPI实现

```python
# app/main.py
from fastapi import FastAPI, Depends, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from sqlalchemy.orm import Session

from app.database import get_db
from app.routers import talent, risk, team
from app.auth import get_current_user

app = FastAPI(title="AI人才洞察平台 API", version="1.0.0")

# CORS配置
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "https://talent-insight.com"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# 注册路由
app.include_router(talent.router, prefix="/api/v1/talent", tags=["talent"])
app.include_router(risk.router, prefix="/api/v1/risk", tags=["risk"])
app.include_router(team.router, prefix="/api/v1/team", tags=["team"])

@app.get("/")
def read_root():
    return {"message": "AI人才洞察平台 API", "version": "1.0.0"}
```

```python
# app/routers/talent.py
from fastapi import APIRouter, Depends, HTTPException, BackgroundTasks
from sqlalchemy.orm import Session
from typing import List

from app.database import get_db
from app.schemas import JobRequirement, Recommendation, TalentProfile
from app.services.talent_service import TalentService
from app.auth import get_current_user
from app.models import User

router = APIRouter()

@router.post("/recommend", response_model=Recommendation)
async def create_recommendation(
    requirement: JobRequirement,
    background_tasks: BackgroundTasks,
    db: Session = Depends(get_db),
    current_user: User = Depends(get_current_user)
):
    """
    创建AI人才推荐任务
    """
    # 创建任务记录
    talent_service = TalentService(db)
    recommendation = talent_service.create_recommendation_task(
        requirement=requirement,
        user_id=current_user.id
    )

    # 将AI推荐任务加入后台队列
    background_tasks.add_task(
        talent_service.process_recommendation,
        recommendation.id
    )

    return recommendation

@router.get("/recommend/{recommendation_id}", response_model=Recommendation)
async def get_recommendation(
    recommendation_id: str,
    db: Session = Depends(get_db),
    current_user: User = Depends(get_current_user)
):
    """
    获取推荐结果
    """
    talent_service = TalentService(db)
    recommendation = talent_service.get_recommendation(recommendation_id)

    if not recommendation:
        raise HTTPException(status_code=404, detail="推荐记录不存在")

    # 检查权限
    if recommendation.user_id != current_user.id:
        raise HTTPException(status_code=403, detail="无权访问此推荐记录")

    return recommendation

@router.get("/profile/{employee_id}", response_model=TalentProfile)
async def get_talent_profile(
    employee_id: str,
    db: Session = Depends(get_db),
    current_user: User = Depends(get_current_user)
):
    """
    获取人才画像详情
    """
    talent_service = TalentService(db)
    profile = talent_service.get_talent_profile(employee_id)

    if not profile:
        raise HTTPException(status_code=404, detail="员工不存在")

    return profile
```

```python
# app/services/talent_service.py
from sqlalchemy.orm import Session
from typing import List, Optional
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity

from app.models import Employee, Recommendation, JobRequirement
from app.schemas import TalentProfile, CandidateMatch
from app.cache import cache

class TalentService:
    def __init__(self, db: Session):
        self.db = db

    def create_recommendation_task(
        self,
        requirement: JobRequirement,
        user_id: str
    ) -> Recommendation:
        """
        创建推荐任务记录
        """
        recommendation = Recommendation(
            job_requirement=requirement.dict(),
            user_id=user_id,
            status="pending"
        )
        self.db.add(recommendation)
        self.db.commit()
        self.db.refresh(recommendation)
        return recommendation

    async def process_recommendation(self, recommendation_id: str):
        """
        后台处理AI推荐任务
        """
        recommendation = self.db.query(Recommendation).filter(
            Recommendation.id == recommendation_id
        ).first()

        if not recommendation:
            return

        try:
            # 更新状态
            recommendation.status = "processing"
            self.db.commit()

            # 获取所有候选员工
            employees = self.db.query(Employee).all()

            # 执行AI推荐算法
            candidates = self._calculate_matches(
                employees,
                recommendation.job_requirement
            )

            # 保存结果
            recommendation.candidates = [c.dict() for c in candidates]
            recommendation.status = "completed"
            self.db.commit()

            # 推送WebSocket通知（可选）
            # await self.notify_user(recommendation.user_id, recommendation)

        except Exception as e:
            recommendation.status = "failed"
            recommendation.error = str(e)
            self.db.commit()

    def _calculate_matches(
        self,
        employees: List[Employee],
        requirement: dict
    ) -> List[CandidateMatch]:
        """
        AI推荐算法核心逻辑
        """
        weights = requirement.get('weights', {})
        required_skills = requirement.get('required_skills', [])

        matches = []

        for employee in employees:
            # 计算各维度得分
            hard_skills_score = self._calculate_skill_match(
                employee.skills,
                required_skills
            )

            project_score = self._calculate_project_relevance(
                employee.projects,
                requirement.get('description', '')
            )

            growth_score = self._calculate_growth_potential(employee)

            culture_score = self._calculate_culture_fit(employee)

            # 加权计算总分
            total_score = (
                hard_skills_score * weights.get('hard_skills', 0.3) +
                project_score * weights.get('project_experience', 0.3) +
                growth_score * weights.get('growth_potential', 0.2) +
                culture_score * weights.get('culture_fit', 0.2)
            )

            # 生成推荐理由
            reasons = self._generate_match_reasons(
                employee,
                hard_skills_score,
                project_score,
                growth_score,
                culture_score
            )

            matches.append(CandidateMatch(
                employee_id=employee.id,
                employee_name=employee.name,
                employee_position=employee.current_position,
                match_score=total_score,
                match_reasons=reasons,
                detailed_scores={
                    'hard_skills': hard_skills_score,
                    'project_experience': project_score,
                    'growth_potential': growth_score,
                    'culture_fit': culture_score
                }
            ))

        # 按匹配度排序
        matches.sort(key=lambda x: x.match_score, reverse=True)

        # 返回前20名
        return matches[:20]

    def _calculate_skill_match(
        self,
        employee_skills: List[dict],
        required_skills: List[dict]
    ) -> float:
        """
        计算技能匹配度
        """
        if not required_skills:
            return 0.5

        skill_levels = {'初级': 1, '中级': 2, '高级': 3, '专家': 4}

        total_match = 0
        for req_skill in required_skills:
            req_name = req_skill['name']
            req_level = skill_levels.get(req_skill.get('level', '中级'), 2)

            # 查找员工是否有该技能
            emp_skill = next(
                (s for s in employee_skills if s['name'] == req_name),
                None
            )

            if emp_skill:
                emp_level = skill_levels.get(emp_skill.get('level', '初级'), 1)
                # 技能等级匹配度
                match = min(emp_level / req_level, 1.0)
                total_match += match
            else:
                total_match += 0

        return total_match / len(required_skills)

    def _calculate_project_relevance(
        self,
        projects: List[dict],
        job_description: str
    ) -> float:
        """
        计算项目经验相关性（简化版本，实际应使用NLP）
        """
        if not projects or not job_description:
            return 0.5

        # 这里应该使用NLP技术计算语义相似度
        # 简化版本：基于关键词匹配
        keywords = set(job_description.lower().split())

        relevance_scores = []
        for project in projects:
            project_text = (
                project.get('description', '') + ' ' +
                ' '.join(project.get('achievements', []))
            ).lower()

            project_keywords = set(project_text.split())
            overlap = len(keywords & project_keywords)
            score = overlap / len(keywords) if keywords else 0
            relevance_scores.append(score)

        return np.mean(relevance_scores) if relevance_scores else 0.5

    def _calculate_growth_potential(self, employee: Employee) -> float:
        """
        计算成长潜力（基于绩效趋势、培训参与度等）
        """
        # 获取最近3年绩效
        recent_performance = sorted(
            employee.performance,
            key=lambda p: (p['year'], p.get('quarter', 0)),
            reverse=True
        )[:12]  # 最近12个季度

        if len(recent_performance) < 4:
            return 0.5

        # 计算绩效趋势
        scores = [p['score'] for p in recent_performance]
        trend = np.polyfit(range(len(scores)), scores, 1)[0]  # 线性拟合斜率

        # 培训参与度
        training_count = len(employee.trainings)
        training_score = min(training_count / 10, 1.0)  # 假设10次培训为满分

        # 综合评分
        potential_score = (
            0.6 * (0.5 + np.clip(trend, -0.5, 0.5)) +  # 绩效趋势
            0.4 * training_score  # 培训参与度
        )

        return np.clip(potential_score, 0, 1)

    def _calculate_culture_fit(self, employee: Employee) -> float:
        """
        计算文化契合度（简化版本）
        """
        # 实际应该基于价值观评估、团队反馈等
        # 这里简化为基于在职时长和团队评价

        tenure_months = (
            pd.Timestamp.now() - pd.Timestamp(employee.join_date)
        ).days / 30

        tenure_score = min(tenure_months / 36, 1.0)  # 3年为满分

        return tenure_score

    def _generate_match_reasons(
        self,
        employee: Employee,
        skill_score: float,
        project_score: float,
        growth_score: float,
        culture_score: float
    ) -> List[dict]:
        """
        生成匹配理由
        """
        reasons = []

        if skill_score >= 0.8:
            reasons.append({
                'category': '硬技能匹配',
                'score': skill_score,
                'details': f'{employee.name}在所需技能方面表现优秀，技能匹配度达到{skill_score*100:.0f}%'
            })

        if project_score >= 0.7:
            reasons.append({
                'category': '项目经验相关',
                'score': project_score,
                'details': f'参与过{len(employee.projects)}个相关项目，经验丰富'
            })

        if growth_score >= 0.75:
            reasons.append({
                'category': '成长潜力突出',
                'score': growth_score,
                'details': '近期绩效持续提升，展现出良好的学习能力和发展潜力'
            })

        return reasons

    @cache(expire=1800)  # 缓存30分钟
    def get_talent_profile(self, employee_id: str) -> Optional[TalentProfile]:
        """
        获取人才画像详情（带缓存）
        """
        employee = self.db.query(Employee).filter(
            Employee.id == employee_id
        ).first()

        if not employee:
            return None

        return TalentProfile(
            id=employee.id,
            name=employee.name,
            avatar=employee.avatar,
            current_position=employee.current_position,
            department=employee.department,
            join_date=employee.join_date,
            email=employee.email,
            phone=employee.phone,
            education=employee.education,
            performance=employee.performance,
            projects=employee.projects,
            skills=employee.skills,
            trainings=employee.trainings
        )
```

---

## 部署方案

### Docker Compose完整示例

```yaml
# docker-compose.yml
version: '3.8'

services:
  # 前端服务
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    environment:
      - REACT_APP_API_URL=http://backend:8000/api/v1
    depends_on:
      - backend
    networks:
      - talent-network

  # 后端服务
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://talent_user:talent_pass@postgres:5432/talent_insight
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=your-secret-key-change-in-production
      - CELERY_BROKER_URL=redis://redis:6379/0
    depends_on:
      - postgres
      - redis
    networks:
      - talent-network
    volumes:
      - ./backend/logs:/app/logs

  # Celery Worker（异步任务）
  celery-worker:
    build:
      context: ./backend
      dockerfile: Dockerfile
    command: celery -A app.celery_app worker --loglevel=info
    environment:
      - DATABASE_URL=postgresql://talent_user:talent_pass@postgres:5432/talent_insight
      - REDIS_URL=redis://redis:6379
      - CELERY_BROKER_URL=redis://redis:6379/0
    depends_on:
      - postgres
      - redis
    networks:
      - talent-network

  # PostgreSQL数据库
  postgres:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=talent_user
      - POSTGRES_PASSWORD=talent_pass
      - POSTGRES_DB=talent_insight
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - talent-network

  # Redis缓存
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - talent-network

  # Nginx反向代理（可选）
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    depends_on:
      - frontend
      - backend
    networks:
      - talent-network

volumes:
  postgres_data:
  redis_data:

networks:
  talent-network:
    driver: bridge
```

### 前端Dockerfile

```dockerfile
# frontend/Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM nginx:alpine

COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 后端Dockerfile

```dockerfile
# backend/Dockerfile
FROM python:3.11-slim

WORKDIR /app

# 安装系统依赖
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# 安装Python依赖
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# 复制应用代码
COPY . .

# 创建日志目录
RUN mkdir -p /app/logs

EXPOSE 8000

# 启动命令
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### Nginx配置

```nginx
# nginx/nginx.conf
upstream frontend {
    server frontend:80;
}

upstream backend {
    server backend:8000;
}

server {
    listen 80;
    server_name talent-insight.com;

    # 前端
    location / {
        proxy_pass http://frontend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # 后端API
    location /api/ {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        # CORS
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Authorization, Content-Type' always;
    }

    # WebSocket支持
    location /ws/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## 环境变量配置

```bash
# .env.example

# 前端环境变量
REACT_APP_API_URL=http://localhost:8000/api/v1
REACT_APP_WS_URL=ws://localhost:8000/ws
REACT_APP_ENV=development

# 后端环境变量
DATABASE_URL=postgresql://user:password@localhost:5432/talent_insight
REDIS_URL=redis://localhost:6379
JWT_SECRET=your-jwt-secret-key
JWT_ALGORITHM=HS256
JWT_EXPIRE_MINUTES=1440

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/1

# 外部系统集成
HR_SYSTEM_API_URL=https://hr-system.company.com/api
HR_SYSTEM_API_KEY=your-api-key

# AI/ML配置
MODEL_PATH=/app/models
ENABLE_AI_LOGGING=true
```

---

## 开发流程建议

### 1. 本地开发环境搭建

```bash
# 克隆项目
git clone <repository-url>
cd talent-insight-platform

# 前端设置
cd frontend
npm install
cp .env.example .env.local
npm run dev

# 后端设置
cd ../backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
uvicorn app.main:app --reload
```

### 2. 数据库迁移

```bash
# 创建迁移
alembic revision --autogenerate -m "Initial migration"

# 执行迁移
alembic upgrade head

# 回滚迁移
alembic downgrade -1
```

### 3. 测试

```bash
# 前端测试
npm run test

# 后端测试
pytest tests/ -v --cov=app
```

### 4. 构建与部署

```bash
# 使用Docker Compose
docker-compose up -d

# 查看日志
docker-compose logs -f backend

# 停止服务
docker-compose down
```

---

## 常见问题

### Q1: 如何处理跨域问题？
**A:** 在后端添加CORS中间件（如示例所示），或使用Nginx反向代理统一域名。

### Q2: 如何实现身份认证？
**A:** 使用JWT Token，前端在每个请求的Header中携带`Authorization: Bearer <token>`。

### Q3: 大数据量列表如何优化？
**A:** 使用分页、虚拟滚动、服务端缓存（Redis）。

### Q4: AI推荐任务耗时长怎么办？
**A:** 使用异步任务队列（Celery），前端轮询或WebSocket获取结果。

### Q5: 如何与现有HR系统集成？
**A:** 通过API对接、定时数据同步、消息队列等方式。

---

## 下一步

1. 参考README.md了解完整的技术架构
2. 查看HTML设计稿了解界面细节
3. 根据本文档实现前后端数据连接
4. 实施单元测试和集成测试
5. 部署到测试环境验证

如有问题，请参考其他文档或联系技术团队。
