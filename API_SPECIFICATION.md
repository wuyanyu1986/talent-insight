# API 接口规范文档

## 目录
1. [接口设计原则](#接口设计原则)
2. [认证与鉴权](#认证与鉴权)
3. [通用响应格式](#通用响应格式)
4. [错误码规范](#错误码规范)
5. [人才推荐模块](#人才推荐模块)
6. [风险预警模块](#风险预警模块)
7. [团队评估模块](#团队评估模块)
8. [员工档案模块](#员工档案模块)
9. [业务分析模块](#业务分析模块)
10. [数据同步模块](#数据同步模块)
11. [WebSocket 实时通知](#websocket-实时通知)

---

## 接口设计原则

### RESTful 规范
- 使用标准 HTTP 方法：GET（查询）、POST（创建）、PUT/PATCH（更新）、DELETE（删除）
- URL 使用名词复数形式，避免动词
- 使用 HTTP 状态码表示请求结果
- 支持版本控制：`/api/v1/`

### 命名规范
- URL 使用小写字母和连字符（kebab-case）
- JSON 字段使用下划线命名（snake_case）
- 日期时间使用 ISO 8601 格式

### 基础 URL
```
开发环境：http://localhost:8000/api/v1
生产环境：https://api.talent-insight.com/api/v1
```

---

## 认证与鉴权

### JWT 认证

**登录接口**
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "username": "string",
  "password": "string"
}
```

**响应**
```json
{
  "code": 200,
  "message": "登录成功",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 7200,
    "user": {
      "id": "uuid",
      "username": "string",
      "email": "string",
      "role": "hrbp",
      "employee_id": "uuid",
      "employee_name": "string",
      "avatar_url": "string"
    }
  }
}
```

**刷新 Token**
```http
POST /api/v1/auth/refresh
Authorization: Bearer {refresh_token}
```

**所有受保护的接口都需要在请求头中携带 Token：**
```http
Authorization: Bearer {access_token}
```

### 权限级别
- `admin`：系统管理员，全部权限
- `hrbp`：人力资源业务伙伴，核心功能权限
- `manager`：部门经理，本部门数据权限
- `employee`：普通员工，个人数据权限

---

## 通用响应格式

### 成功响应
```json
{
  "code": 200,
  "message": "操作成功",
  "data": {
    // 业务数据
  },
  "meta": {
    "timestamp": "2024-01-15T10:30:00Z",
    "request_id": "uuid"
  }
}
```

### 分页响应
```json
{
  "code": 200,
  "message": "查询成功",
  "data": {
    "items": [
      // 数据列表
    ],
    "pagination": {
      "page": 1,
      "page_size": 20,
      "total": 150,
      "total_pages": 8
    }
  }
}
```

### 错误响应
```json
{
  "code": 400,
  "message": "请求参数错误",
  "errors": [
    {
      "field": "email",
      "message": "邮箱格式不正确"
    }
  ],
  "meta": {
    "timestamp": "2024-01-15T10:30:00Z",
    "request_id": "uuid"
  }
}
```

---

## 错误码规范

| 错误码 | 说明 | HTTP状态码 |
|--------|------|------------|
| 200 | 成功 | 200 |
| 201 | 创建成功 | 201 |
| 400 | 请求参数错误 | 400 |
| 401 | 未认证 | 401 |
| 403 | 无权限 | 403 |
| 404 | 资源不存在 | 404 |
| 409 | 资源冲突 | 409 |
| 422 | 业务逻辑错误 | 422 |
| 429 | 请求过于频繁 | 429 |
| 500 | 服务器内部错误 | 500 |
| 503 | 服务不可用 | 503 |

**业务错误码（自定义）**
- `40001`：用户名或密码错误
- `40002`：Token 已过期
- `40003`：Token 无效
- `42201`：员工不存在
- `42202`：推荐任务处理失败
- `42203`：评估数据不完整

---

## 人才推荐模块

### 1. 创建推荐请求

```http
POST /api/v1/talent/recommendations
Authorization: Bearer {token}
Content-Type: application/json

{
  "title": "高级产品经理",
  "department_id": "uuid",
  "scenario": "internal_recruitment",
  "description": "负责AI产品线的需求分析和产品设计",
  "required_skills": [
    {
      "skill_id": "uuid",
      "skill_name": "产品设计",
      "level": "advanced",
      "priority": "required"
    },
    {
      "skill_id": "uuid",
      "skill_name": "数据分析",
      "level": "intermediate",
      "priority": "preferred"
    }
  ],
  "preferred_conditions": "有AI产品经验优先",
  "min_years_experience": 3,
  "weights": {
    "hard_skills": 0.35,
    "project_experience": 0.30,
    "growth_potential": 0.20,
    "culture_fit": 0.15
  }
}
```

**响应**
```json
{
  "code": 201,
  "message": "推荐任务已创建",
  "data": {
    "recommendation_id": "uuid",
    "job_requirement_id": "uuid",
    "status": "processing",
    "estimated_completion_time": "2024-01-15T10:32:00Z"
  }
}
```

### 2. 获取推荐结果

```http
GET /api/v1/talent/recommendations/{recommendation_id}
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "id": "uuid",
    "job_requirement_id": "uuid",
    "status": "completed",
    "total_candidates": 15,
    "created_at": "2024-01-15T10:30:00Z",
    "completed_at": "2024-01-15T10:31:45Z",
    "processing_time_seconds": 105,
    "job_requirement": {
      "title": "高级产品经理",
      "department_name": "产品部",
      "scenario": "internal_recruitment"
    },
    "candidates": [
      {
        "rank": 1,
        "employee_id": "uuid",
        "employee_name": "张三",
        "employee_number": "E12345",
        "current_position": "产品经理",
        "department_name": "产品部",
        "avatar_url": "https://...",
        "match_score": 92.5,
        "detailed_scores": {
          "hard_skills": 95,
          "project_experience": 90,
          "growth_potential": 88,
          "culture_fit": 92
        },
        "match_reasons": [
          {
            "category": "hard_skills",
            "score": 95,
            "details": "精通产品设计、需求分析，拥有高级产品设计证书"
          },
          {
            "category": "project_experience",
            "score": 90,
            "details": "主导过3个AI产品从0到1的全流程，累计用户超100万"
          },
          {
            "category": "growth_potential",
            "score": 88,
            "details": "近2年绩效均为A，连续3个季度被评为高潜人才"
          }
        ],
        "skill_radar": {
          "产品设计": 95,
          "数据分析": 85,
          "项目管理": 88,
          "用户研究": 90,
          "沟通协作": 92
        }
      }
      // ... 更多候选人
    ]
  }
}
```

### 3. 更新推荐权重

```http
PUT /api/v1/talent/recommendations/{recommendation_id}/weights
Authorization: Bearer {token}
Content-Type: application/json

{
  "weights": {
    "hard_skills": 0.40,
    "project_experience": 0.35,
    "growth_potential": 0.15,
    "culture_fit": 0.10
  }
}
```

### 4. 反馈候选人

```http
POST /api/v1/talent/recommendations/{recommendation_id}/candidates/{candidate_id}/feedback
Authorization: Bearer {token}
Content-Type: application/json

{
  "feedback": "interested",  // interested, not_suitable, contacted, hired
  "notes": "已联系候选人，准备安排面试"
}
```

---

## 风险预警模块

### 1. 获取风险预警列表

```http
GET /api/v1/risks/alerts?page=1&page_size=20&type=turnover_risk&priority=high&status=active
Authorization: Bearer {token}
```

**查询参数**
- `page`：页码（默认1）
- `page_size`：每页数量（默认20，最大100）
- `type`：风险类型（可选）：succession_risk, turnover_risk, capability_gap, performance_decline
- `priority`：优先级（可选）：low, medium, high, critical
- `status`：状态（可选）：active, monitoring, resolved, dismissed
- `department_id`：部门ID（可选）
- `detected_after`：检测时间起始（可选）

**响应**
```json
{
  "code": 200,
  "message": "查询成功",
  "data": {
    "items": [
      {
        "id": "uuid",
        "title": "产品部关键岗位继任风险",
        "type": "succession_risk",
        "priority": "high",
        "detected_at": "2024-01-10T08:00:00Z",
        "expected_impact_date": "2024-03-01",
        "description": "产品总监计划离职，继任候选人未就绪",
        "affected_positions": ["产品总监", "高级产品经理"],
        "affected_departments": ["产品部"],
        "status": "active",
        "assigned_to": {
          "id": "uuid",
          "name": "林雨萱"
        }
      }
      // ... 更多预警
    ],
    "pagination": {
      "page": 1,
      "page_size": 20,
      "total": 45,
      "total_pages": 3
    },
    "statistics": {
      "total_active": 45,
      "by_priority": {
        "critical": 5,
        "high": 12,
        "medium": 20,
        "low": 8
      },
      "by_type": {
        "succession_risk": 10,
        "turnover_risk": 18,
        "capability_gap": 12,
        "performance_decline": 5
      }
    }
  }
}
```

### 2. 获取风险详情

```http
GET /api/v1/risks/alerts/{alert_id}
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "id": "uuid",
    "title": "产品部关键岗位继任风险",
    "type": "succession_risk",
    "priority": "high",
    "detected_at": "2024-01-10T08:00:00Z",
    "expected_impact_date": "2024-03-01",
    "description": "产品总监张明计划于3月离职，目前没有合适的继任者",
    "root_cause_analysis": "分析发现：1）现有高级产品经理经验不足；2）外部招聘周期长；3）知识传承不充分",
    "risk_signals": [
      {
        "signal_type": "resignation_intent",
        "severity": "high",
        "details": "员工已提交辞职申请",
        "detected_at": "2024-01-10T08:00:00Z"
      },
      {
        "signal_type": "succession_gap",
        "severity": "high",
        "details": "继任梯队中无满足条件的候选人",
        "detected_at": "2024-01-10T08:00:00Z"
      }
    ],
    "affected_departments": [
      {
        "id": "uuid",
        "name": "产品部"
      }
    ],
    "affected_positions": ["产品总监", "高级产品经理"],
    "affected_employees": [
      {
        "employee_id": "uuid",
        "employee_name": "张明",
        "position": "产品总监",
        "risk_level": "critical"
      }
    ],
    "impact_assessment": {
      "business_impact": "产品线决策和战略规划将受到严重影响，可能导致产品发布延迟",
      "affected_key_positions": ["产品总监"],
      "risk_spread_prediction": "如不及时处理，可能导致产品团队军心不稳，引发连锁离职"
    },
    "recommended_actions": [
      {
        "title": "立即启动内部继任计划",
        "steps": [
          "评估现有高级产品经理的能力差距",
          "制定为期2个月的加速培养计划",
          "安排与现任产品总监的知识传承"
        ],
        "expected_outcome": "2个月内培养出合格继任者",
        "difficulty": "medium",
        "estimated_days": 60
      },
      {
        "title": "同步启动外部招聘",
        "steps": [
          "发布职位信息",
          "联系猎头公司",
          "设置快速面试流程"
        ],
        "expected_outcome": "1.5个月内找到外部候选人",
        "difficulty": "high",
        "estimated_days": 45
      }
    ],
    "status": "active",
    "assigned_to": {
      "id": "uuid",
      "name": "林雨萱",
      "avatar_url": "https://..."
    },
    "created_at": "2024-01-10T08:00:00Z",
    "updated_at": "2024-01-10T08:00:00Z"
  }
}
```

### 3. 更新风险状态

```http
PATCH /api/v1/risks/alerts/{alert_id}/status
Authorization: Bearer {token}
Content-Type: application/json

{
  "status": "resolved",
  "resolution_notes": "已找到合适的继任者，并完成知识交接"
}
```

### 4. 获取风险统计

```http
GET /api/v1/risks/statistics?time_range=30d
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "time_range": "30d",
    "total_alerts": 45,
    "new_alerts_count": 12,
    "resolved_count": 8,
    "by_priority": {
      "critical": 5,
      "high": 12,
      "medium": 20,
      "low": 8
    },
    "by_type": {
      "succession_risk": 10,
      "turnover_risk": 18,
      "capability_gap": 12,
      "performance_decline": 5
    },
    "trend": [
      {
        "date": "2024-01-01",
        "new_alerts": 3,
        "resolved": 2
      }
      // ... 30天的趋势数据
    ]
  }
}
```

---

## 团队评估模块

### 1. 创建团队评估

```http
POST /api/v1/teams/assessments
Authorization: Bearer {token}
Content-Type: application/json

{
  "team_name": "产品研发团队",
  "department_id": "uuid",
  "team_lead_id": "uuid",
  "assessment_dimensions": [
    "technical_skills",
    "leadership",
    "collaboration",
    "innovation",
    "execution"
  ],
  "assessment_date": "2024-01-15"
}
```

**响应**
```json
{
  "code": 201,
  "message": "评估任务已创建",
  "data": {
    "assessment_id": "uuid",
    "status": "processing",
    "estimated_completion_time": "2024-01-15T10:35:00Z"
  }
}
```

### 2. 获取团队评估结果

```http
GET /api/v1/teams/assessments/{assessment_id}
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "id": "uuid",
    "team_name": "产品研发团队",
    "department": {
      "id": "uuid",
      "name": "产品部"
    },
    "team_lead": {
      "id": "uuid",
      "name": "王经理",
      "avatar_url": "https://..."
    },
    "assessment_date": "2024-01-15",
    "assessment_dimensions": [
      "technical_skills",
      "leadership",
      "collaboration",
      "innovation",
      "execution"
    ],
    "overview": {
      "total_members": 25,
      "average_tenure_months": 32.5,
      "key_positions_count": 5,
      "high_potential_count": 8
    },
    "capability_distribution": {
      "skills_heatmap": {
        "产品设计": {
          "beginner": 3,
          "intermediate": 12,
          "advanced": 8,
          "expert": 2
        },
        "数据分析": {
          "beginner": 5,
          "intermediate": 10,
          "advanced": 8,
          "expert": 2
        }
        // ... 更多技能
      },
      "experience_distribution": {
        "0-2年": 5,
        "2-5年": 12,
        "5-10年": 6,
        "10年以上": 2
      },
      "capability_radar": {
        "technical_skills": 82,
        "leadership": 75,
        "collaboration": 88,
        "innovation": 78,
        "execution": 85
      }
    },
    "members": [
      {
        "employee_id": "uuid",
        "name": "张三",
        "position": "高级产品经理",
        "avatar_url": "https://...",
        "overall_score": 88,
        "dimension_scores": {
          "technical_skills": 90,
          "leadership": 85,
          "collaboration": 92,
          "innovation": 87,
          "execution": 86
        },
        "strengths": [
          "产品设计能力突出",
          "跨部门协作能力强",
          "项目管理经验丰富"
        ],
        "development_areas": [
          "技术深度可以进一步提升",
          "可加强数据驱动决策能力"
        ],
        "potential_rating": "high"
      }
      // ... 更多成员
    ],
    "ai_insights": {
      "capability_gaps": [
        "团队整体在AI/ML领域的技术储备不足",
        "高级人才占比偏低，仅占8%",
        "数据分析能力需要系统性提升"
      ],
      "development_suggestions": [
        "建议组织AI产品专题培训，覆盖全员",
        "制定高潜人才培养计划，加速成长",
        "引入外部专家进行数据分析能力提升"
      ],
      "risk_warnings": [
        "3名核心成员近期绩效下滑，需关注",
        "团队平均司龄偏短，存在稳定性风险"
      ]
    },
    "status": "completed",
    "created_at": "2024-01-15T10:30:00Z",
    "completed_at": "2024-01-15T10:34:30Z"
  }
}
```

### 3. 导出评估报告

```http
POST /api/v1/teams/assessments/{assessment_id}/export
Authorization: Bearer {token}
Content-Type: application/json

{
  "format": "pdf",  // pdf, excel, word
  "include_sections": [
    "overview",
    "capability_distribution",
    "member_details",
    "ai_insights"
  ]
}
```

**响应**
```json
{
  "code": 200,
  "message": "报告生成中",
  "data": {
    "task_id": "uuid",
    "status": "processing",
    "download_url": null,
    "estimated_completion_time": "2024-01-15T10:36:00Z"
  }
}
```

### 4. 获取导出任务状态

```http
GET /api/v1/teams/assessments/export-tasks/{task_id}
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "task_id": "uuid",
    "status": "completed",
    "download_url": "https://cdn.example.com/reports/assessment_20240115.pdf",
    "expires_at": "2024-01-16T10:35:00Z"
  }
}
```

---

## 员工档案模块

### 1. 获取员工画像详情

```http
GET /api/v1/employees/{employee_id}/profile
Authorization: Bearer {token}
```

**响应**（数据结构较大，略，参考 DATABASE_SCHEMA.md 中的 v_employee_full_profile 视图）

### 2. 搜索员工

```http
GET /api/v1/employees/search?q=张三&department_id=uuid&skills=产品设计,数据分析&page=1&page_size=20
Authorization: Bearer {token}
```

### 3. 获取员工技能分布

```http
GET /api/v1/employees/{employee_id}/skills
Authorization: Bearer {token}
```

### 4. 获取员工绩效历史

```http
GET /api/v1/employees/{employee_id}/performance?years=2
Authorization: Bearer {token}
```

---

## 业务分析模块

### 1. 创建业务关联分析

```http
POST /api/v1/analysis/business
Authorization: Bearer {token}
Content-Type: application/json

{
  "analysis_name": "Q4业绩与人才配置关联分析",
  "department_ids": ["uuid1", "uuid2"],
  "business_kpis": [
    {
      "kpi_id": "uuid",
      "kpi_name": "销售额",
      "time_range": "2023-Q4"
    }
  ],
  "talent_dimensions": [
    "team_size",
    "average_tenure",
    "skill_level",
    "training_hours"
  ]
}
```

### 2. 获取分析结果

```http
GET /api/v1/analysis/business/{analysis_id}
Authorization: Bearer {token}
```

---

## 数据同步模块

### 1. 触发数据同步

```http
POST /api/v1/data/sync
Authorization: Bearer {token}
Content-Type: application/json

{
  "source_system": "sap_hr",  // sap_hr, beisen, file_import
  "sync_type": "incremental",  // full, incremental
  "data_types": ["employees", "performance", "training"]
}
```

**响应**
```json
{
  "code": 200,
  "message": "同步任务已创建",
  "data": {
    "sync_task_id": "uuid",
    "status": "running",
    "started_at": "2024-01-15T10:30:00Z"
  }
}
```

### 2. 获取同步状态

```http
GET /api/v1/data/sync/{sync_task_id}
Authorization: Bearer {token}
```

**响应**
```json
{
  "code": 200,
  "message": "获取成功",
  "data": {
    "sync_task_id": "uuid",
    "source_system": "sap_hr",
    "sync_type": "incremental",
    "status": "completed",
    "started_at": "2024-01-15T10:30:00Z",
    "completed_at": "2024-01-15T10:35:20Z",
    "duration_seconds": 320,
    "statistics": {
      "total_records": 1500,
      "created": 50,
      "updated": 1400,
      "failed": 50,
      "conflicts": 10
    },
    "error_summary": [
      {
        "error_type": "data_conflict",
        "count": 10,
        "sample_message": "员工E12345的部门信息在两个系统中不一致"
      }
    ]
  }
}
```

### 3. 获取数据冲突列表

```http
GET /api/v1/data/conflicts?status=unresolved&page=1&page_size=20
Authorization: Bearer {token}
```

### 4. 解决数据冲突

```http
PUT /api/v1/data/conflicts/{conflict_id}/resolve
Authorization: Bearer {token}
Content-Type: application/json

{
  "resolution": "use_source",  // use_source, use_target, manual
  "manual_value": null  // 如果选择 manual，则提供具体值
}
```

---

## WebSocket 实时通知

### 连接建立

```javascript
// 客户端连接示例
const ws = new WebSocket('wss://api.talent-insight.com/ws?token=<access_token>');

ws.onopen = () => {
  console.log('WebSocket 连接已建立');
};

ws.onmessage = (event) => {
  const notification = JSON.parse(event.data);
  handleNotification(notification);
};

ws.onerror = (error) => {
  console.error('WebSocket 错误:', error);
};

ws.onclose = () => {
  console.log('WebSocket 连接已关闭');
  // 实现重连逻辑
};
```

### 消息格式

**风险预警通知**
```json
{
  "type": "risk_alert",
  "priority": "high",
  "data": {
    "alert_id": "uuid",
    "title": "产品部关键岗位继任风险",
    "type": "succession_risk",
    "detected_at": "2024-01-15T10:30:00Z"
  },
  "timestamp": "2024-01-15T10:30:01Z"
}
```

**推荐任务完成通知**
```json
{
  "type": "recommendation_completed",
  "data": {
    "recommendation_id": "uuid",
    "job_title": "高级产品经理",
    "total_candidates": 15
  },
  "timestamp": "2024-01-15T10:31:45Z"
}
```

**评估报告生成完成通知**
```json
{
  "type": "report_ready",
  "data": {
    "report_type": "team_assessment",
    "report_id": "uuid",
    "download_url": "https://..."
  },
  "timestamp": "2024-01-15T10:35:00Z"
}
```

**数据同步完成通知**
```json
{
  "type": "sync_completed",
  "data": {
    "sync_task_id": "uuid",
    "status": "completed",
    "statistics": {
      "total_records": 1500,
      "created": 50,
      "updated": 1400,
      "conflicts": 10
    }
  },
  "timestamp": "2024-01-15T10:35:20Z"
}
```

---

## 接口限流

**限流规则**
- 普通接口：100 请求/分钟
- 搜索接口：30 请求/分钟
- 导出接口：5 请求/分钟
- WebSocket 连接：10 连接/IP

**限流响应头**
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1673782800
```

**超出限流响应**
```json
{
  "code": 429,
  "message": "请求过于频繁，请稍后再试",
  "data": {
    "retry_after": 60
  }
}
```

---

## 接口测试

### Postman Collection

项目提供完整的 Postman Collection，包含所有接口的示例请求：

```bash
# 导入 Postman Collection
文件路径：/docs/api/Talent_Insight_API.postman_collection.json
```

### cURL 示例

```bash
# 登录
curl -X POST https://api.talent-insight.com/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"demo","password":"demo123"}'

# 获取风险列表
curl -X GET "https://api.talent-insight.com/api/v1/risks/alerts?page=1&page_size=20" \
  -H "Authorization: Bearer <token>"

# 创建推荐任务
curl -X POST https://api.talent-insight.com/api/v1/talent/recommendations \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d @recommendation_request.json
```

---

## API 版本管理

- **当前版本**：v1
- **版本策略**：URL 路径版本控制 `/api/v1/`
- **弃用通知**：通过响应头 `X-API-Deprecation-Date` 通知
- **向后兼容**：次版本更新保持向后兼容

---

## 安全建议

1. **HTTPS Only**：生产环境强制使用 HTTPS
2. **Token 过期**：Access Token 2小时，Refresh Token 7天
3. **敏感数据**：薪酬等敏感数据需额外权限
4. **IP 白名单**：支持配置 IP 白名单
5. **审计日志**：所有操作记录审计日志

---

## 参考文档

- **数据库设计**：`DATABASE_SCHEMA.md`
- **后端集成**：`BACKEND_INTEGRATION.md`
- **开发文档**：`README.md`
