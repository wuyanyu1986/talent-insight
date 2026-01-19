# 数据库 Schema 设计

## 技术选型

**推荐数据库：** PostgreSQL 15+

**理由：**
- 成熟的关系型数据库，支持复杂查询
- 原生支持 JSONB，灵活存储半结构化数据
- 强大的全文搜索能力
- 支持地理位置、时间序列扩展
- 开源免费，社区活跃

---

## 核心表设计

### 1. 员工表（employees）

```sql
CREATE TABLE employees (
    -- 基础信息
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_number VARCHAR(50) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    name_en VARCHAR(100),
    avatar_url TEXT,
    gender VARCHAR(10),
    birth_date DATE,

    -- 联系方式
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20),
    emergency_contact JSONB,  -- {name, phone, relationship}

    -- 职位信息
    current_position VARCHAR(100) NOT NULL,
    position_level VARCHAR(50),  -- P5, P6, M1, M2, etc.
    department_id UUID REFERENCES departments(id),
    business_unit VARCHAR(100),
    direct_manager_id UUID REFERENCES employees(id),

    -- 雇佣信息
    employment_type VARCHAR(50) NOT NULL,  -- 'full_time', 'contract', 'intern'
    join_date DATE NOT NULL,
    probation_end_date DATE,
    contract_end_date DATE,
    resignation_date DATE,
    status VARCHAR(20) NOT NULL DEFAULT 'active',  -- 'active', 'resigned', 'on_leave'

    -- 薪酬信息（加密存储）
    salary_grade VARCHAR(20),
    last_promotion_date DATE,

    -- 教育背景
    education JSONB,  -- [{degree, major, school, graduation_year}]

    -- 元数据
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    created_by UUID REFERENCES users(id),
    updated_by UUID REFERENCES users(id),

    -- 索引
    CONSTRAINT valid_status CHECK (status IN ('active', 'resigned', 'on_leave')),
    CONSTRAINT valid_employment_type CHECK (employment_type IN ('full_time', 'contract', 'intern'))
);

-- 索引
CREATE INDEX idx_employees_department ON employees(department_id);
CREATE INDEX idx_employees_manager ON employees(direct_manager_id);
CREATE INDEX idx_employees_status ON employees(status);
CREATE INDEX idx_employees_join_date ON employees(join_date);
CREATE INDEX idx_employees_name ON employees USING gin(to_tsvector('english', name));
```

### 2. 部门表（departments）

```sql
CREATE TABLE departments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) NOT NULL,
    code VARCHAR(50) UNIQUE NOT NULL,
    parent_id UUID REFERENCES departments(id),
    level INTEGER NOT NULL,
    head_id UUID REFERENCES employees(id),
    description TEXT,
    business_unit VARCHAR(100),
    cost_center VARCHAR(50),
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_departments_parent ON departments(parent_id);
CREATE INDEX idx_departments_head ON departments(head_id);
```

### 3. 绩效记录表（performance_records）

```sql
CREATE TABLE performance_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_id UUID NOT NULL REFERENCES employees(id) ON DELETE CASCADE,

    -- 考核周期
    year INTEGER NOT NULL,
    quarter INTEGER CHECK (quarter BETWEEN 1 AND 4),
    period_type VARCHAR(20) NOT NULL,  -- 'quarterly', 'annual', 'mid_year'

    -- 评分
    score DECIMAL(5,2) CHECK (score >= 0 AND score <= 100),
    rating VARCHAR(10) NOT NULL,  -- 'S', 'A', 'B', 'C', 'D'
    ranking_percentile DECIMAL(5,2),  -- 在部门的排名百分位

    -- 详细内容
    objectives JSONB,  -- [{objective, weight, score, comments}]
    competencies JSONB,  -- [{competency, level, comments}]
    achievements TEXT,
    improvements TEXT,
    comments TEXT,

    -- 审核信息
    reviewer_id UUID REFERENCES employees(id),
    review_date DATE,
    status VARCHAR(20) DEFAULT 'draft',  -- 'draft', 'submitted', 'reviewed', 'approved'

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT valid_rating CHECK (rating IN ('S', 'A', 'B', 'C', 'D')),
    CONSTRAINT valid_status CHECK (status IN ('draft', 'submitted', 'reviewed', 'approved'))
);

CREATE INDEX idx_performance_employee ON performance_records(employee_id);
CREATE INDEX idx_performance_period ON performance_records(year, quarter);
CREATE INDEX idx_performance_rating ON performance_records(rating);
```

### 4. 项目经历表（project_experiences）

```sql
CREATE TABLE project_experiences (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_id UUID NOT NULL REFERENCES employees(id) ON DELETE CASCADE,

    -- 项目信息
    project_name VARCHAR(200) NOT NULL,
    project_code VARCHAR(50),
    role VARCHAR(100) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE,
    duration_months INTEGER,

    -- 项目详情
    description TEXT,
    responsibilities TEXT[],
    achievements TEXT[],
    technologies TEXT[],
    team_size INTEGER,

    -- 项目成果
    business_impact TEXT,
    quantifiable_results JSONB,  -- {metric, value, unit}

    -- 评价
    project_rating VARCHAR(10),
    manager_feedback TEXT,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_projects_employee ON project_experiences(employee_id);
CREATE INDEX idx_projects_dates ON project_experiences(start_date, end_date);
```

### 5. 技能表（skills）

```sql
CREATE TABLE skills (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(100) UNIQUE NOT NULL,
    category VARCHAR(50) NOT NULL,  -- 'technical', 'soft', 'domain', 'language'
    subcategory VARCHAR(50),
    description TEXT,
    is_active BOOLEAN DEFAULT true,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_skills_category ON skills(category);
```

### 6. 员工技能关联表（employee_skills）

```sql
CREATE TABLE employee_skills (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_id UUID NOT NULL REFERENCES employees(id) ON DELETE CASCADE,
    skill_id UUID NOT NULL REFERENCES skills(id) ON DELETE CASCADE,

    -- 技能水平
    level VARCHAR(20) NOT NULL,  -- 'beginner', 'intermediate', 'advanced', 'expert'
    proficiency_score INTEGER CHECK (proficiency_score BETWEEN 0 AND 100),

    -- 认证信息
    is_certified BOOLEAN DEFAULT false,
    certification_name VARCHAR(200),
    certificate_url TEXT,
    certification_date DATE,
    expiry_date DATE,

    -- 来源与验证
    source VARCHAR(50),  -- 'self_reported', 'manager_verified', 'assessment', 'project'
    verified_by UUID REFERENCES employees(id),
    verified_at TIMESTAMP WITH TIME ZONE,

    -- 使用情况
    last_used_date DATE,
    years_of_experience DECIMAL(4,1),

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    UNIQUE(employee_id, skill_id),
    CONSTRAINT valid_level CHECK (level IN ('beginner', 'intermediate', 'advanced', 'expert'))
);

CREATE INDEX idx_employee_skills_employee ON employee_skills(employee_id);
CREATE INDEX idx_employee_skills_skill ON employee_skills(skill_id);
CREATE INDEX idx_employee_skills_level ON employee_skills(level);
```

### 7. 培训记录表（training_records）

```sql
CREATE TABLE training_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_id UUID NOT NULL REFERENCES employees(id) ON DELETE CASCADE,

    -- 培训信息
    training_name VARCHAR(200) NOT NULL,
    training_type VARCHAR(50) NOT NULL,  -- 'internal', 'external', 'online', 'certification'
    provider VARCHAR(200),
    category VARCHAR(50),

    -- 时间与时长
    start_date DATE NOT NULL,
    end_date DATE,
    duration_hours DECIMAL(6,2),

    -- 完成情况
    completion_status VARCHAR(20) NOT NULL DEFAULT 'registered',
    -- 'registered', 'in_progress', 'completed', 'cancelled'
    completion_date DATE,
    score DECIMAL(5,2),
    passed BOOLEAN,

    -- 费用
    cost DECIMAL(10,2),
    currency VARCHAR(10) DEFAULT 'CNY',

    -- 证书
    certificate_url TEXT,
    certificate_number VARCHAR(100),

    -- 效果评估
    feedback TEXT,
    application_plan TEXT,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_training_employee ON training_records(employee_id);
CREATE INDEX idx_training_dates ON training_records(start_date, end_date);
CREATE INDEX idx_training_status ON training_records(completion_status);
```

### 8. 岗位需求表（job_requirements）

```sql
CREATE TABLE job_requirements (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    -- 基础信息
    title VARCHAR(200) NOT NULL,
    department_id UUID REFERENCES departments(id),
    position_level VARCHAR(50),

    -- 场景
    scenario VARCHAR(50) NOT NULL,
    -- 'internal_recruitment', 'succession_planning', 'project_team', 'capability_gap'

    -- 岗位描述
    description TEXT NOT NULL,
    responsibilities TEXT[],

    -- 要求
    required_skills JSONB NOT NULL,  -- [{skill_id, skill_name, level, priority}]
    preferred_conditions TEXT,
    min_years_experience INTEGER,
    education_requirement VARCHAR(50),

    -- 匹配权重
    weights JSONB NOT NULL DEFAULT '{
        "hard_skills": 0.3,
        "project_experience": 0.3,
        "growth_potential": 0.2,
        "culture_fit": 0.2
    }'::jsonb,

    -- 请求人信息
    requester_id UUID REFERENCES users(id),
    urgency VARCHAR(20) DEFAULT 'medium',  -- 'low', 'medium', 'high', 'critical'
    expected_start_date DATE,

    -- 状态
    status VARCHAR(20) DEFAULT 'draft',  -- 'draft', 'active', 'filled', 'cancelled'

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT valid_scenario CHECK (scenario IN ('internal_recruitment', 'succession_planning', 'project_team', 'capability_gap'))
);

CREATE INDEX idx_job_requirements_department ON job_requirements(department_id);
CREATE INDEX idx_job_requirements_scenario ON job_requirements(scenario);
CREATE INDEX idx_job_requirements_status ON job_requirements(status);
```

### 9. 人才推荐表（talent_recommendations）

```sql
CREATE TABLE talent_recommendations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    job_requirement_id UUID NOT NULL REFERENCES job_requirements(id) ON DELETE CASCADE,

    -- 推荐状态
    status VARCHAR(20) DEFAULT 'processing',
    -- 'processing', 'completed', 'failed'

    -- 推荐结果
    total_candidates INTEGER,
    recommendation_data JSONB,  -- 完整的推荐结果JSON

    -- 算法版本
    algorithm_version VARCHAR(20),

    -- 执行信息
    started_at TIMESTAMP WITH TIME ZONE,
    completed_at TIMESTAMP WITH TIME ZONE,
    processing_time_seconds INTEGER,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_recommendations_job ON talent_recommendations(job_requirement_id);
CREATE INDEX idx_recommendations_status ON talent_recommendations(status);
```

### 10. 推荐候选人详情表（recommendation_candidates）

```sql
CREATE TABLE recommendation_candidates (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    recommendation_id UUID NOT NULL REFERENCES talent_recommendations(id) ON DELETE CASCADE,
    employee_id UUID NOT NULL REFERENCES employees(id),

    -- 匹配分数
    match_score DECIMAL(5,2) NOT NULL CHECK (match_score >= 0 AND match_score <= 100),
    rank INTEGER NOT NULL,

    -- 详细分数
    detailed_scores JSONB NOT NULL,
    -- {hard_skills, project_experience, growth_potential, culture_fit}

    -- 匹配原因
    match_reasons JSONB,  -- [{category, score, details}]

    -- 用户反馈
    user_feedback VARCHAR(20),  -- 'interested', 'not_suitable', 'contacted', 'hired'
    feedback_notes TEXT,
    feedback_date TIMESTAMP WITH TIME ZONE,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    UNIQUE(recommendation_id, employee_id)
);

CREATE INDEX idx_recommendation_candidates_recommendation ON recommendation_candidates(recommendation_id);
CREATE INDEX idx_recommendation_candidates_employee ON recommendation_candidates(employee_id);
CREATE INDEX idx_recommendation_candidates_score ON recommendation_candidates(match_score DESC);
```

### 11. 风险预警表（risk_alerts）

```sql
CREATE TABLE risk_alerts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    -- 基础信息
    title VARCHAR(200) NOT NULL,
    type VARCHAR(50) NOT NULL,
    -- 'succession_risk', 'turnover_risk', 'capability_gap', 'performance_decline'
    priority VARCHAR(20) NOT NULL,  -- 'low', 'medium', 'high', 'critical'

    -- 时间信息
    detected_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    expected_impact_date DATE,

    -- 风险描述
    description TEXT NOT NULL,
    root_cause_analysis TEXT,

    -- 影响范围
    affected_departments UUID[],
    affected_positions TEXT[],
    affected_employee_ids UUID[],

    -- 风险信号
    risk_signals JSONB,  -- [{signal_type, severity, details}]

    -- 影响评估
    impact_assessment JSONB,
    -- {business_impact, affected_key_positions, risk_spread_prediction}

    -- 应对建议
    recommended_actions JSONB,
    -- [{title, steps, expected_outcome, difficulty}]

    -- 状态管理
    status VARCHAR(20) DEFAULT 'active',  -- 'active', 'monitoring', 'resolved', 'dismissed'
    assigned_to UUID REFERENCES users(id),
    resolution_notes TEXT,
    resolved_at TIMESTAMP WITH TIME ZONE,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT valid_type CHECK (type IN ('succession_risk', 'turnover_risk', 'capability_gap', 'performance_decline')),
    CONSTRAINT valid_priority CHECK (priority IN ('low', 'medium', 'high', 'critical'))
);

CREATE INDEX idx_risk_alerts_type ON risk_alerts(type);
CREATE INDEX idx_risk_alerts_priority ON risk_alerts(priority);
CREATE INDEX idx_risk_alerts_status ON risk_alerts(status);
CREATE INDEX idx_risk_alerts_detected ON risk_alerts(detected_at);
```

### 12. 团队评估表（team_assessments）

```sql
CREATE TABLE team_assessments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    -- 团队信息
    team_name VARCHAR(200) NOT NULL,
    department_id UUID REFERENCES departments(id),
    team_lead_id UUID REFERENCES employees(id),

    -- 评估配置
    assessment_dimensions TEXT[],  -- ['technical_skills', 'leadership', 'collaboration', ...]
    assessment_date DATE NOT NULL,

    -- 统计概览
    total_members INTEGER NOT NULL,
    average_tenure_months DECIMAL(6,2),
    key_positions_count INTEGER,
    high_potential_count INTEGER,

    -- 能力分布
    capability_distribution JSONB,
    -- {skills_heatmap, experience_distribution, capability_radar}

    -- AI洞察
    ai_insights JSONB,
    -- {capability_gaps, development_suggestions, risk_warnings}

    -- 评估状态
    status VARCHAR(20) DEFAULT 'draft',  -- 'draft', 'completed', 'archived'

    -- 评估人
    assessor_id UUID REFERENCES users(id),

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_team_assessments_department ON team_assessments(department_id);
CREATE INDEX idx_team_assessments_date ON team_assessments(assessment_date);
```

### 13. 团队成员评估详情表（team_member_assessments）

```sql
CREATE TABLE team_member_assessments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    team_assessment_id UUID NOT NULL REFERENCES team_assessments(id) ON DELETE CASCADE,
    employee_id UUID NOT NULL REFERENCES employees(id),

    -- 评分
    overall_score DECIMAL(5,2) CHECK (overall_score >= 0 AND overall_score <= 100),
    dimension_scores JSONB,  -- {dimension: score}

    -- 分析
    strengths TEXT[],
    development_areas TEXT[],
    potential_rating VARCHAR(20),  -- 'low', 'medium', 'high', 'exceptional'

    -- 建议
    development_recommendations TEXT[],

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    UNIQUE(team_assessment_id, employee_id)
);

CREATE INDEX idx_team_member_assessments_team ON team_member_assessments(team_assessment_id);
CREATE INDEX idx_team_member_assessments_employee ON team_member_assessments(employee_id);
```

### 14. 用户表（users）

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    employee_id UUID UNIQUE REFERENCES employees(id),

    -- 认证信息
    username VARCHAR(100) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,

    -- 角色权限
    role VARCHAR(50) NOT NULL DEFAULT 'hrbp',
    -- 'admin', 'hrbp', 'manager', 'employee'
    permissions TEXT[],

    -- 状态
    is_active BOOLEAN DEFAULT true,
    is_verified BOOLEAN DEFAULT false,

    -- 登录信息
    last_login_at TIMESTAMP WITH TIME ZONE,
    login_count INTEGER DEFAULT 0,

    -- 个人设置
    preferences JSONB DEFAULT '{}'::jsonb,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT valid_role CHECK (role IN ('admin', 'hrbp', 'manager', 'employee'))
);

CREATE INDEX idx_users_employee ON users(employee_id);
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

### 15. 审计日志表（audit_logs）

```sql
CREATE TABLE audit_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    -- 操作信息
    user_id UUID REFERENCES users(id),
    action VARCHAR(100) NOT NULL,  -- 'create', 'update', 'delete', 'view', 'export'
    resource_type VARCHAR(50) NOT NULL,  -- 'employee', 'risk_alert', 'recommendation', etc.
    resource_id UUID,

    -- 详细信息
    changes JSONB,  -- {field: {old_value, new_value}}
    ip_address INET,
    user_agent TEXT,

    -- 时间
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE INDEX idx_audit_logs_user ON audit_logs(user_id);
CREATE INDEX idx_audit_logs_resource ON audit_logs(resource_type, resource_id);
CREATE INDEX idx_audit_logs_created ON audit_logs(created_at);
```

---

## 视图设计

### 1. 员工完整信息视图

```sql
CREATE VIEW v_employee_full_profile AS
SELECT
    e.*,
    d.name as department_name,
    d.business_unit,
    m.name as manager_name,

    -- 最新绩效
    (SELECT json_agg(pr.* ORDER BY pr.year DESC, pr.quarter DESC)
     FROM performance_records pr
     WHERE pr.employee_id = e.id
     LIMIT 3) as recent_performance,

    -- 技能
    (SELECT json_agg(json_build_object(
        'skill_name', s.name,
        'skill_category', s.category,
        'level', es.level,
        'proficiency_score', es.proficiency_score,
        'is_certified', es.is_certified
     ))
     FROM employee_skills es
     JOIN skills s ON es.skill_id = s.id
     WHERE es.employee_id = e.id) as skills,

    -- 项目经历
    (SELECT json_agg(pe.* ORDER BY pe.start_date DESC)
     FROM project_experiences pe
     WHERE pe.employee_id = e.id
     LIMIT 5) as projects,

    -- 培训记录
    (SELECT json_agg(tr.* ORDER BY tr.start_date DESC)
     FROM training_records tr
     WHERE tr.employee_id = e.id
     LIMIT 5) as trainings

FROM employees e
LEFT JOIN departments d ON e.department_id = d.id
LEFT JOIN employees m ON e.direct_manager_id = m.id;
```

### 2. 风险统计视图

```sql
CREATE VIEW v_risk_statistics AS
SELECT
    type,
    priority,
    status,
    COUNT(*) as count,
    DATE_TRUNC('month', detected_at) as month
FROM risk_alerts
GROUP BY type, priority, status, DATE_TRUNC('month', detected_at);
```

---

## 初始化脚本

```sql
-- 启用UUID扩展
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pgcrypto";

-- 创建更新时间触发器函数
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

-- 为所有需要的表添加触发器
CREATE TRIGGER update_employees_updated_at BEFORE UPDATE ON employees
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_departments_updated_at BEFORE UPDATE ON departments
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- ... 为其他表添加类似触发器

-- 插入初始数据
INSERT INTO departments (name, code, level, is_active) VALUES
    ('技术部', 'TECH', 1, true),
    ('产品部', 'PROD', 1, true),
    ('人力资源部', 'HR', 1, true);

INSERT INTO skills (name, category, subcategory) VALUES
    ('Python', 'technical', 'programming'),
    ('JavaScript', 'technical', 'programming'),
    ('项目管理', 'soft', 'management'),
    ('数据分析', 'technical', 'analytics');
```

---

## 数据迁移建议

### 从现有HR系统迁移

```sql
-- 示例：从临时表导入员工数据
INSERT INTO employees (
    employee_number, name, email, current_position,
    department_id, join_date, employment_type, status
)
SELECT
    emp_no,
    emp_name,
    email,
    position,
    (SELECT id FROM departments WHERE code = temp.dept_code),
    hire_date,
    emp_type,
    CASE WHEN active = 1 THEN 'active' ELSE 'resigned' END
FROM temp_employee_import temp
ON CONFLICT (employee_number) DO UPDATE
SET
    name = EXCLUDED.name,
    email = EXCLUDED.email,
    updated_at = NOW();
```

---

## 性能优化建议

1. **分区表**：对于大表（如 audit_logs），按时间分区
```sql
CREATE TABLE audit_logs (
    -- ... columns
) PARTITION BY RANGE (created_at);

CREATE TABLE audit_logs_2024_q1 PARTITION OF audit_logs
    FOR VALUES FROM ('2024-01-01') TO ('2024-04-01');
```

2. **物化视图**：对于复杂的统计查询
```sql
CREATE MATERIALIZED VIEW mv_employee_statistics AS
SELECT
    department_id,
    COUNT(*) as total_employees,
    AVG(EXTRACT(YEAR FROM AGE(NOW(), join_date))) as avg_tenure_years
FROM employees
WHERE status = 'active'
GROUP BY department_id;

-- 定期刷新
REFRESH MATERIALIZED VIEW CONCURRENTLY mv_employee_statistics;
```

3. **适当的索引策略**：已在各表定义中包含

---

## 备份与恢复

```bash
# 备份
pg_dump -h localhost -U postgres -d talent_insight \
    -F c -b -v -f "backup_$(date +%Y%m%d_%H%M%S).dump"

# 恢复
pg_restore -h localhost -U postgres -d talent_insight_new \
    -v backup_20240101_120000.dump

# 仅备份 schema
pg_dump -h localhost -U postgres -d talent_insight \
    --schema-only -f schema.sql
```

---

## 环境变量配置

```bash
# .env 文件示例
DATABASE_URL=postgresql://talent_user:secure_password@localhost:5432/talent_insight
DATABASE_POOL_SIZE=20
DATABASE_MAX_OVERFLOW=10
DATABASE_POOL_TIMEOUT=30
DATABASE_POOL_RECYCLE=3600
```

---

## 安全建议

1. **敏感数据加密**：薪酬等敏感字段使用 pgcrypto 加密
2. **行级安全**：使用 PostgreSQL Row-Level Security (RLS)
3. **审计日志**：记录所有敏感操作
4. **定期备份**：每日增量备份 + 每周全量备份
5. **权限控制**：最小权限原则，应用使用专用数据库用户

```sql
-- 示例：创建只读用户
CREATE ROLE readonly_user WITH LOGIN PASSWORD 'password';
GRANT CONNECT ON DATABASE talent_insight TO readonly_user;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly_user;
```
