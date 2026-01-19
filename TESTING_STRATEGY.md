# 测试策略文档

本文档定义AI人才洞察平台的完整测试策略。

---

## 测试金字塔

```
        /\
       /E2E\          10%  - 端到端测试
      /------\
     / 集成测试 \       30%  - 集成测试
    /----------\
   /  单元测试   \      60%  - 单元测试
  /--------------\
```

---

## 1. 单元测试

### 前端单元测试（Vitest + React Testing Library）

**安装依赖**
```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

**示例：组件测试**
```typescript
import { render, screen } from '@testing-library/react';
import { StatCard } from './StatCard';

describe('StatCard', () => {
  it('should render correctly', () => {
    render(<StatCard label="风险数" value={12} />);
    expect(screen.getByText('风险数')).toBeInTheDocument();
  });
});
```

### 后端单元测试（pytest）

**安装依赖**
```bash
pip install pytest pytest-asyncio pytest-cov
```

**示例：算法测试**
```python
def test_calculate_skill_match(talent_service):
    score = talent_service._calculate_skill_match(
        employee_skills, required_skills
    )
    assert score >= 80
```

---

## 2. 集成测试

### 前端集成测试
- RTK Query API 集成测试
- Redux Store 集成测试

### 后端集成测试
- API 端点测试
- 数据库操作测试

---

## 3. 端到端测试（Playwright）

**安装**
```bash
npm install -D @playwright/test
```

**示例：推荐流程测试**
```typescript
test('AI人才推荐流程', async ({ page }) => {
  await page.goto('/talent/recommend');
  await page.fill('input[name="title"]', '高级产品经理');
  await page.click('button:has-text("开始推荐")');
  await expect(page.locator('.candidate-card')).toHaveCount.greaterThan(0);
});
```

---

## 4. 性能测试

### 前端：Lighthouse CI
### 后端：Locust

```python
# locustfile.py
from locust import HttpUser, task

class User(HttpUser):
    @task
    def get_risks(self):
        self.client.get("/api/v1/risks/alerts")
```

---

## 5. 安全测试

- 依赖漏洞扫描：`npm audit` / `safety check`
- SQL注入防护测试
- XSS防护测试

---

## 6. 测试覆盖率目标

| 模块 | 覆盖率目标 |
|------|----------|
| 核心业务逻辑 | ≥ 90% |
| API 路由 | ≥ 80% |
| 工具函数 | ≥ 95% |
| UI 组件 | ≥ 70% |
| 整体项目 | ≥ 80% |

---

## 7. CI/CD 集成

```yaml
# .github/workflows/test.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run test:coverage
      - run: npx playwright test
```

---

## 8. 运行测试

```bash
# 前端
npm run test              # 运行测试
npm run test:coverage     # 生成覆盖率报告
npx playwright test       # E2E测试

# 后端
pytest                    # 运行测试
pytest --cov=app          # 生成覆盖率报告
locust -f locustfile.py   # 性能测试
```

---

## 总结

完整测试策略确保：
- ✅ 功能正确性（单元测试）
- ✅ 集成稳定性（集成测试）
- ✅ 用户体验（E2E测试）
- ✅ 系统性能（性能测试）
- ✅ 安全防护（安全测试）

建议开发团队优先编写核心业务逻辑的单元测试，再逐步完善其他测试。
