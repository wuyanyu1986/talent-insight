# AI人才洞察平台 - 开发文档

## 项目概述

这是一款为HRBP设计的AI驱动的人才管理与决策分析平台（Web端），帮助HRBP从"信息收集者"转变为"数据驱动的业务伙伴"。

**核心价值：**
- 统一人才数据视图，30分钟内获得完整人才画像
- AI驱动的人才推荐与匹配，科学化决策支持
- 智能风险预警，提前3-6个月识别人才风险
- 业务关联分析，量化人才工作的业务价值
- 团队能力盘点，快速生成能力地图和洞察报告

## 项目结构

```
workspace/paraflow/
├── Global Context/              # 战略基础文档
│   ├── product_charter.md       # 产品定位文档
│   └── user_persona.md          # 用户画像
├── Feature Plan/                # 功能规划文档
│   ├── prd.md                   # 产品需求文档
│   └── user_flow.md             # 用户流程图
├── Style Guide/                 # 设计系统
│   ├── intelligent_tech.style-guide.md     # 智能科技感（主用）
│   ├── modern_business_blue.style-guide.md # 现代商务蓝（备选）
│   └── premium_grayscale.style-guide.md    # 高级灰度（备选）
└── Screen & Prototype/          # 界面设计
    ├── dashboard-intelligent-tech.html      # 工作台首页
    ├── talent-recommend.html                # AI人才推荐
    ├── recommend-results.html               # 推荐结果列表
    ├── talent-profile.html                  # 人才画像详情
    ├── risk-alerts.html                     # 风险预警列表
    ├── risk-detail.html                     # 风险详情
    └── team-assessment.html                 # 团队能力盘点
```

## 核心用户

**林雨萱** - 业务伙伴型HRBP
- 32岁，服务于中大型科技公司
- 支持3个业务部门（约200名员工）
- 核心痛点：数据割裂、分析工具缺失、决策缺乏科学依据
- 核心目标：用数据和洞察赢得业务部门信任，成为战略业务伙伴

## 核心功能模块

### 1. 统一人才数据视图
**功能描述：** 整合多源人才数据，提供完整的人才画像
**关键界面：** `talent-profile.html`
**技术要点：**
- 多系统数据集成（HR系统、业务系统、文件）
- 实时数据同步与冲突检测
- 多维度数据展示（基本信息、绩效、项目经历、技能、培训）

### 2. AI人才推荐与匹配
**功能描述：** 根据岗位要求智能推荐候选人，提供匹配度分析
**关键界面：** `talent-recommend.html`, `recommend-results.html`
**技术要点：**
- 岗位需求解析（NLP）
- 多维度匹配算法（硬技能、项目经验、成长潜力）
- 可解释性AI（展示推荐理由）
- 权重调整与二次筛选

### 3. 智能人才风险预警
**功能描述：** 持续监控人才指标，主动预警风险
**关键界面：** `risk-alerts.html`, `risk-detail.html`
**技术要点：**
- 风险信号检测（离职倾向、继任空白、能力缺口）
- 预警规则引擎（可自定义）
- 优先级评估
- 应对建议生成

### 4. 团队能力盘点与可视化
**功能描述：** 自动生成团队能力地图和洞察报告
**关键界面：** `team-assessment.html`
**技术要点：**
- 团队数据聚合分析
- 可视化图表（技能热力图、经验分布、能力雷达图）
- AI洞察生成（能力缺口、培养建议）
- 报告导出

### 5. 业务关联人才分析
**功能描述：** 关联业务KPI与人才指标，展示业务价值
**技术要点：**
- 业务数据集成
- 相关性分析
- 因果链路可视化
- ROI计算

## 技术架构建议

### 前端技术栈
```
- 框架：React/Vue.js
- UI组件库：Ant Design / Element Plus
- 状态管理：Redux / Pinia
- 图表库：ECharts / D3.js
- 样式：Tailwind CSS / CSS-in-JS
```

### 后端技术栈
```
- 语言：Python / Node.js / Java
- 框架：FastAPI / Express / Spring Boot
- 数据库：PostgreSQL / MySQL
- 缓存：Redis
- 消息队列：RabbitMQ / Kafka
```

### AI/ML模块
```
- NLP：用于岗位需求解析、简历分析
- 推荐算法：协同过滤 + 基于内容的混合推荐
- 风险预测：时间序列分析、分类模型
- 相关性分析：统计分析、因果推断
```

## 页面路由设计

```javascript
// 主路由
/dashboard                          # 工作台首页

// AI人才推荐流程
/talent/recommend                   # AI人才推荐（输入需求）
/talent/recommend/results           # 推荐结果列表
/talent/profile/:id                 # 人才画像详情

// 风险管理流程
/risk/alerts                        # 风险预警列表
/risk/detail/:id                    # 风险详情

// 团队能力盘点
/team/assessment                    # 团队能力盘点

// 业务关联分析（待开发）
/analysis/business                  # 业务关联分析
/analysis/business/result/:id       # 分析结果

// 人才搜索（待开发）
/talent/search                      # 人才搜索
/talent/search/results              # 搜索结果
```

## 数据模型设计

### 员工（Employee）
```json
{
  "id": "string",
  "name": "string",
  "avatar": "string",
  "current_position": "string",
  "department": "string",
  "join_date": "date",
  "email": "string",
  "phone": "string",
  "education": [
    {
      "degree": "string",
      "major": "string",
      "school": "string",
      "graduation_year": "number"
    }
  ],
  "performance": [
    {
      "year": "number",
      "quarter": "number",
      "score": "number",
      "rating": "string",
      "comments": "string"
    }
  ],
  "projects": [
    {
      "name": "string",
      "role": "string",
      "duration": "string",
      "description": "string",
      "achievements": "string[]"
    }
  ],
  "skills": [
    {
      "name": "string",
      "level": "string",
      "certified": "boolean",
      "certificate_url": "string"
    }
  ],
  "trainings": [
    {
      "name": "string",
      "date": "date",
      "duration": "number",
      "provider": "string",
      "completion_status": "string"
    }
  ]
}
```

### 岗位需求（JobRequirement）
```json
{
  "id": "string",
  "title": "string",
  "department": "string",
  "scenario": "string", // "internal_recruitment", "succession_planning", "project_team"
  "description": "string",
  "required_skills": [
    {
      "name": "string",
      "level": "string",
      "priority": "string" // "required", "preferred"
    }
  ],
  "preferred_conditions": "string",
  "weights": {
    "hard_skills": "number",
    "project_experience": "number",
    "growth_potential": "number",
    "culture_fit": "number"
  }
}
```

### 人才推荐（TalentRecommendation）
```json
{
  "id": "string",
  "job_requirement_id": "string",
  "created_at": "datetime",
  "candidates": [
    {
      "employee_id": "string",
      "employee_name": "string",
      "employee_position": "string",
      "match_score": "number",
      "match_reasons": [
        {
          "category": "string", // "hard_skills", "project_experience", etc.
          "score": "number",
          "details": "string"
        }
      ],
      "detailed_scores": {
        "hard_skills": "number",
        "project_experience": "number",
        "growth_potential": "number",
        "culture_fit": "number"
      }
    }
  ]
}
```

### 风险预警（RiskAlert）
```json
{
  "id": "string",
  "title": "string",
  "type": "string", // "succession_risk", "turnover_risk", "capability_gap"
  "priority": "string", // "high", "medium", "low"
  "detected_at": "datetime",
  "expected_impact_date": "date",
  "description": "string",
  "root_cause_analysis": "string",
  "affected_positions": "string[]",
  "affected_employees": [
    {
      "employee_id": "string",
      "employee_name": "string",
      "risk_signals": "string[]"
    }
  ],
  "impact_assessment": {
    "business_impact": "string",
    "affected_key_positions": "string[]",
    "risk_spread_prediction": "string"
  },
  "recommended_actions": [
    {
      "title": "string",
      "steps": "string[]",
      "expected_outcome": "string",
      "difficulty": "string" // "low", "medium", "high"
    }
  ],
  "status": "string" // "active", "resolved", "monitoring"
}
```

### 团队评估（TeamAssessment）
```json
{
  "id": "string",
  "team_id": "string",
  "team_name": "string",
  "created_at": "datetime",
  "assessment_dimensions": "string[]",
  "overview": {
    "total_members": "number",
    "average_tenure": "number",
    "key_positions_count": "number",
    "high_potential_count": "number"
  },
  "capability_distribution": {
    "skills_heatmap": "object",
    "experience_distribution": "object",
    "capability_radar": "object"
  },
  "members": [
    {
      "employee_id": "string",
      "name": "string",
      "position": "string",
      "overall_score": "number",
      "strengths": "string[]",
      "development_areas": "string[]"
    }
  ],
  "ai_insights": {
    "capability_gaps": "string[]",
    "development_suggestions": "string[]",
    "risk_warnings": "string[]"
  }
}
```

## API接口设计

### 人才推荐相关
```
POST   /api/talent/recommend           # 创建推荐请求
GET    /api/talent/recommend/:id       # 获取推荐结果
PUT    /api/talent/recommend/:id       # 更新推荐权重
GET    /api/talent/profile/:id         # 获取人才画像详情
```

### 风险预警相关
```
GET    /api/risk/alerts                # 获取风险预警列表
GET    /api/risk/alerts/:id            # 获取风险详情
PUT    /api/risk/alerts/:id/status     # 更新风险状态
GET    /api/risk/statistics            # 获取风险统计数据
```

### 团队评估相关
```
POST   /api/team/assessment            # 创建团队评估
GET    /api/team/assessment/:id        # 获取评估结果
GET    /api/team/assessment/history    # 获取历史评估记录
POST   /api/team/assessment/:id/export # 导出评估报告
```

### 业务分析相关
```
POST   /api/analysis/business          # 创建业务关联分析
GET    /api/analysis/business/:id      # 获取分析结果
GET    /api/analysis/kpi/list          # 获取可用KPI列表
```

### 数据同步相关
```
POST   /api/data/sync                  # 触发数据同步
GET    /api/data/sync/status           # 获取同步状态
GET    /api/data/conflicts             # 获取数据冲突列表
PUT    /api/data/conflicts/:id/resolve # 解决数据冲突
```

## 视觉设计系统

**采用方案：智能科技感**

### 颜色系统
```css
/* 主色 */
--primary-color: #2DD4BF;           /* 科技青色 */
--primary-hover: #22C1AE;
--primary-active: #1BA897;

/* 辅助色 */
--accent-1: #4ADE80;                /* 青绿 */
--accent-2: #64748B;                /* 柔和蓝灰 */
--accent-3: #C7D2FE;                /* 淡银色 */

/* 背景 */
--background: linear-gradient(135deg, #F8FAFC 0%, #E8F4F4 100%);
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
```

### 圆角
```css
--radius-sm: 6px;
--radius-md: 8px;
--radius-lg: 12px;
--radius-xl: 16px;
```

### 阴影
```css
--shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
--shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.08);
--shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
--shadow-xl: 0 20px 25px -5px rgba(0, 0, 0, 0.1);
```

### 字体
```css
--font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
--font-size-xs: 12px;
--font-size-sm: 14px;
--font-size-base: 16px;
--font-size-lg: 18px;
--font-size-xl: 20px;
--font-size-2xl: 24px;
--font-size-3xl: 30px;
```

## 关键交互说明

### 1. AI人才推荐流程
1. 用户在推荐页面输入岗位信息和要求
2. 用户调整匹配权重（可选）
3. 点击"开始推荐"，显示加载状态
4. 跳转到推荐结果页，展示排序的候选人列表
5. 用户可查看候选人详细匹配分析（雷达图）
6. 点击"查看详情"跳转到人才画像详情页

### 2. 风险预警流程
1. 系统后台持续监控人才指标
2. 检测到风险时自动创建预警记录
3. 用户登录后在工作台看到预警摘要
4. 用户进入风险预警列表页查看所有预警
5. 可按类型、优先级筛选
6. 点击查看风险详情，了解根因分析和应对建议
7. 用户可标记为已处理、生成报告、设置提醒

### 3. 团队能力盘点流程
1. 用户选择要盘点的团队/部门
2. 配置评估维度
3. 系统自动分析团队数据
4. 展示能力分布可视化（热力图、雷达图）
5. 展示成员列表和评分
6. AI生成洞察建议
7. 用户可导出报告

## 性能优化建议

1. **数据分页与懒加载**
   - 人才列表、风险列表使用虚拟滚动
   - 图表数据按需加载

2. **缓存策略**
   - 人才画像数据缓存30分钟
   - 风险预警列表缓存5分钟
   - 团队评估结果缓存1小时

3. **异步处理**
   - AI推荐使用异步任务队列
   - 团队评估数据生成使用后台任务
   - 报告导出异步生成，完成后通知

4. **前端优化**
   - 组件懒加载
   - 图片压缩与CDN加速
   - 路由级别代码分割

## 安全考虑

1. **数据权限**
   - 基于角色的访问控制（RBAC）
   - 敏感数据脱敏展示
   - 操作日志记录

2. **API安全**
   - JWT认证
   - 请求频率限制
   - 输入验证与防注入

3. **数据传输**
   - HTTPS加密传输
   - 敏感数据加密存储

## 开发优先级建议

### Phase 1 - 核心功能（MVP）
- [ ] 工作台首页
- [ ] 人才画像详情页
- [ ] AI人才推荐功能
- [ ] 风险预警列表
- [ ] 基础数据同步

### Phase 2 - 扩展功能
- [ ] 团队能力盘点
- [ ] 风险详情与应对建议
- [ ] 数据可视化增强
- [ ] 报告导出功能

### Phase 3 - 高级功能
- [ ] 业务关联分析
- [ ] 人才搜索
- [ ] 自定义预警规则
- [ ] 移动端适配

## 测试建议

### 单元测试
- AI推荐算法测试
- 风险检测逻辑测试
- 数据同步逻辑测试

### 集成测试
- API接口测试
- 数据库集成测试
- 第三方系统集成测试

### E2E测试
- 关键用户流程测试（推荐、预警、盘点）
- 跨页面导航测试

## 部署建议

### 环境配置
```
开发环境：本地开发与调试
测试环境：功能测试与集成测试
预生产环境：性能测试与用户验收
生产环境：正式上线
```

### 容器化部署
```yaml
# Docker Compose示例
services:
  frontend:
    image: talent-insight-frontend:latest
    ports:
      - "3000:3000"

  backend:
    image: talent-insight-backend:latest
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/talent_insight
      - REDIS_URL=redis://redis:6379

  db:
    image: postgres:15
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
```

## 参考文档

- **产品定位**：`Global Context/product_charter.md`
- **用户研究**：`Global Context/user_persona.md`
- **功能需求**：`Feature Plan/prd.md`
- **用户流程**：`Feature Plan/user_flow.md`
- **设计规范**：`Style Guide/intelligent_tech.style-guide.md`
- **界面参考**：`Screen & Prototype/` 目录下所有HTML文件

## 联系与支持

项目开发过程中如有问题，请参考以上文档或查看具体界面实现。所有设计文件均可直接作为开发参考。
