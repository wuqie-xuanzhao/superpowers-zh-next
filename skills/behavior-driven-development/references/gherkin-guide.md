# Gherkin 语法参考

Gherkin 是 BDD 的场景描述语言，设计为非技术人员可读，描述软件行为而不涉及实现。

## 核心原则

1. **声明式而非命令式：** 描述用户做了*什么*，而非*怎么*做的。
   - 差：`Given 我点击用户字段，输入 'Alice'，点击密码字段...`
   - 好：`Given 我以 'Alice' 身份登录`

2. **业务语言：** 使用领域通用语言，而非 `JSON`、`Database`、`CSS selector` 等技术术语。

3. **单一职责：** 每个场景只验证一个规则或行为。

## 关键字详解

### Feature（功能）

功能的顶层描述，包含角色、目标和理由。

```gherkin
Feature: 用户登录
  作为一名注册用户
  我想要登录系统
  以便访问我的个人仪表盘
```

### Scenario / Example（场景）

一个具体的业务规则示例。

```gherkin
Scenario: 登录成功
  Given 我在登录页面
  When 我使用有效凭证登录
  Then 我应该被导航到仪表盘
```

### Given（前置条件）

系统的初始状态或上下文。

- 用 `Background` 提取多个场景共享的 Given

### When（动作）

用户或系统触发的事件。

- 通常一个场景只有一个 When 步骤，明确触发点

### Then（结果）

期望的结果。

- 断言在这里

### And / But

连接同类型的多个步骤，提升可读性。

### Scenario Outline & Examples（场景大纲）

用不同数据运行同一场景：

```gherkin
Scenario Outline: 根据会员等级计算折扣
  Given 用户是 "<level>" 会员
  When 用户购买金额为 <amount> 元的商品
  Then 折扣应该是 <discount> 元

  Examples:
    | level | amount | discount |
    | 普通  | 100    | 0        |
    | 白银  | 100    | 5        |
    | 黄金  | 100    | 10       |
    | 钻石  | 100    | 20       |
```

### Background（背景）

Feature 下所有场景共享的前置条件：

```gherkin
Feature: 商品搜索

  Background:
    Given 商店有以下商品
      | 名称         | 分类   | 价格 |
      | iPhone 15    | 手机   | 5999 |
      | MacBook Pro  | 电脑   | 14999|
      | AirPods Pro  | 配件   | 1899 |

  Scenario: 按关键词搜索
    When 我搜索 "iPhone"
    Then 我应该看到 "iPhone 15"

  Scenario: 按分类筛选
    When 我选择分类 "电脑"
    Then 我应该看到 "MacBook Pro"
    And 我不应该看到 "iPhone 15"
```

## 文件组织结构

```text
tests/
├── features/                  # BDD 场景文件（业务可读）
│   ├── user-login.feature     # Given/When/Then 规格
│   ├── user-registration.feature
│   └── order-processing.feature
├── step-definitions/          # 场景到代码的映射
│   ├── login-steps.ts
│   └── order-steps.ts
└── unit/                      # 纯单元测试
```

### 为什么用 .feature 文件而非代码注释？

- 注释不能被 BDD 框架执行
- 非技术人员（产品经理、测试）无法读取代码注释
- 违反关注点分离：行为（WHAT）vs 实现（HOW）
- 丧失了 BDD "活文档" 的核心价值

## 最佳实践清单

- [ ] **黄金法则：** 非技术干系人能读懂这个场景吗？
- [ ] **独立性：** 场景之间不能有依赖（无共享状态）
- [ ] **简洁性：** 保持场景在 3-5 个步骤
- [ ] **数据表：** 用数据表避免重复的 Given 步骤
- [ ] **Background：** 用 Background 消除重复的 setup，但不要隐藏关键上下文
- [ ] **分离：** 场景存储在 .feature 文件中，而非代码注释