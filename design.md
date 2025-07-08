# Agent开发平台 - 模块功能与交互设计

## 🏗️ 整体架构

```
用户 → 项目管理 → 目标配置 → 能力配置 → 测试调试 → 部署运行
         ↓           ↓           ↓           ↓           ↓
    项目创建      目标设定      能力集成      功能验证      上线运行
                    ↓
              决策模块配置
                    ↓
              感知模块配置
                    ↓
              行动模块配置
                    ↓
            MCP工具市场选择
                    ↓
            公司FaaS函数选择
```

## 1. 🏠 项目管理页面

### 核心功能
- 项目CRUD操作
- 模板选择和配置
- 基础信息管理

### 交互形式
```
[创建项目] 按钮 → 弹出模板选择框 → 填写项目信息 → 创建完成
[项目列表] 卡片展示 → 点击进入 → 项目详情页
[项目设置] 表单编辑 → 保存配置
```

### 数据存储
```json
{
  "project_id": "agent_001",
  "name": "代码审查助手",
  "template": "code_review",
  "created_at": "2024-01-01",
  "status": "active"
}
```

---

## 2. 🎯 目标配置页面

### 核心功能
- 目标描述和定义
- 成功条件设置
- 约束条件配置

### 交互形式
```
[目标描述] 文本框 → 输入主要目标
[成功条件] 列表编辑 → 添加/删除条件
[约束设置] 开关选项 → 启用/禁用约束
```

### 数据存储
```json
{
  "main_goal": "为开发团队提供智能代码审查服务",
  "success_conditions": [
    "代码审查准确率>95%",
    "响应时间<5秒",
    "漏洞检出率>90%",
    "误报率<5%"
  ],
  "constraints": [
    "仅审查代码质量，不涉及业务逻辑",
    "不直接修改代码，只提供建议",
    "遵循公司编码规范",
    "保护代码隐私"
  ]
}
```

---

## 3. 🔧 能力配置页面

### 3.1 决策模块

#### 核心功能
- 大模型选择和配置
- 系统提示词设置
- 决策规则定义

#### 交互形式
```
[模型选择] 下拉菜单 → 选择GPT-4/Claude等
[参数调节] 滑块控件 → 调整温度/长度
[提示词编辑] 大文本框 → 输入系统指令
[规则配置] 表格编辑 → 添加条件-动作规则
```

#### 数据存储
```json
{
  "model": "gpt-4",
  "temperature": 0.3,
  "max_tokens": 2000,
  "system_prompt": "你是一个专业的代码审查助手，负责分析代码质量、安全性和性能问题",
  "rules": [
    {
      "condition": "发现安全漏洞",
      "action": "标记为高危并通知"
    },
    {
      "condition": "性能问题",
      "action": "提供优化建议"
    },
    {
      "condition": "代码复杂度高",
      "action": "建议重构"
    }
  ]
}
```

### 3.2 感知模块

#### 核心功能
- 文档上传和管理
- 数据库连接配置
- API数据源设置

#### 交互形式
```
[文档上传] 拖拽区域 → 选择文件 → 自动处理
[数据库配置] 表单填写 → 测试连接 → 保存配置
[API配置] 模板填写 → 参数设置 → 测试调用
```

#### 数据存储
```json
{
  "documents": [
    {"name": "编码规范.md", "type": "markdown", "status": "processed"},
    {"name": "安全指南.pdf", "type": "pdf", "status": "processed"},
    {"name": "最佳实践.docx", "type": "docx", "status": "processing"}
  ],
  "databases": [
    {"type": "postgresql", "host": "code-db.company.com", "database": "code_reviews"}
  ],
  "apis": [
    {"name": "GitLab API", "url": "https://gitlab.company.com/api/v4"},
    {"name": "SonarQube API", "url": "https://sonar.company.com/api"},
    {"name": "Jira API", "url": "https://jira.company.com/rest/api/2"}
  ]
}
```

### 3.3 行动模块

#### 核心功能
- MCP工具市场集成
- 公司FaaS平台函数选择
- 执行参数设置

#### 交互形式
```
[MCP市场] 搜索筛选 → 分类标签 → 多选工具 → 配置管理
[FaaS平台] 函数搜索 → 多选函数 → 运行时配置
[执行设置] 参数调节 → 超时/重试配置
```

#### 数据存储
```json
{
  "mcp_marketplace": {
    "selected_tools": [
      {
        "name": "Git MCP",
        "version": "v1.2.0",
        "category": "development",
        "description": "Git版本控制操作",
        "config": {
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-git"],
          "env": {
            "GIT_REPO_PATH": "/path/to/repo"
          }
        }
      },
      {
        "name": "Database MCP",
        "version": "v2.1.0",
        "category": "database",
        "description": "数据库连接和操作",
        "config": {
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-database"],
          "env": {
            "DATABASE_URL": "postgresql://localhost:5432/code_reviews"
          }
        }
      },
      {
        "name": "Security Scanner",
        "version": "v1.5.0",
        "category": "security",
        "description": "代码安全漏洞扫描",
        "config": {
          "command": "npx",
          "args": ["-y", "@modelcontextprotocol/server-security"],
          "env": {
            "SCAN_LEVEL": "high"
          }
        }
      }
    ],
    "available_categories": [
      "development",
      "database", 
      "security",
      "monitoring",
      "communication"
    ],
    "search_keywords": ["git", "database", "security", "monitor", "notification"]
  },
  "faas_platform": {
    "selected_functions": [
      {
        "name": "send-slack-notification",
        "description": "发送Slack消息通知",
        "runtime": "Node.js 18",
        "category": "notification",
        "endpoint": "https://faas.company.com/functions/send-slack-notification",
        "auth": "service_account"
      },
      {
        "name": "send-email",
        "description": "发送邮件通知",
        "runtime": "Python 3.9",
        "category": "notification",
        "endpoint": "https://faas.company.com/functions/send-email",
        "auth": "service_account"
      },
      {
        "name": "query-database",
        "description": "执行数据库查询",
        "runtime": "Python 3.9",
        "category": "database",
        "endpoint": "https://faas.company.com/functions/query-database",
        "auth": "service_account"
      },
      {
        "name": "generate-report",
        "description": "生成分析报告",
        "runtime": "Python 3.9",
        "category": "analysis",
        "endpoint": "https://faas.company.com/functions/generate-report",
        "auth": "service_account"
      }
    ],
    "available_functions": [
      {
        "name": "upload-file",
        "description": "文件上传到云存储",
        "runtime": "Node.js 18",
        "category": "storage"
      },
      {
        "name": "trigger-webhook",
        "description": "触发外部webhook",
        "runtime": "Node.js 18",
        "category": "integration"
      }
    ]
  },
  "execution_config": {
    "timeout": 60,
    "retry_count": 3,
    "max_file_size": "10MB",
    "supported_languages": ["python", "javascript", "java", "go", "rust"],
    "mcp_config_file": "mcp-servers.json",
    "faas_timeout": 30,
    "faas_retry_count": 2
  }
}
```

### 3.4 MCP工具市场

#### 核心功能
- MCP工具搜索和筛选
- 分类标签管理
- 多选工具配置
- 配置文件管理

#### 交互形式
```
[搜索框] 输入关键字 → 实时筛选结果
[分类标签] 点击标签 → 按类别筛选
[工具卡片] 显示信息 → 复选框选择 → 配置编辑
[配置管理] 浏览文件 → 手动编辑 → 保存配置
```

#### 工具分类
```json
{
  "categories": {
    "development": {
      "name": "开发工具",
      "tools": ["Git MCP", "Code Analyzer", "File System", "Search Tool"]
    },
    "database": {
      "name": "数据库",
      "tools": ["Database MCP", "SQL Query", "Data Migration"]
    },
    "security": {
      "name": "安全",
      "tools": ["Security Scanner", "Vulnerability Check", "Code Audit"]
    },
    "monitoring": {
      "name": "监控",
      "tools": ["Performance Monitor", "Log Analyzer", "Metrics Collector"]
    },
    "communication": {
      "name": "通信",
      "tools": ["Slack Notifier", "Email Sender", "Webhook Trigger"]
    }
  }
}
```

### 3.5 公司FaaS平台

#### 核心功能
- FaaS函数搜索
- 多选函数配置
- 运行时环境管理
- 权限和认证配置

#### 交互形式
```
[搜索框] 输入函数名 → 实时筛选结果
[函数卡片] 显示信息 → 复选框选择 → 配置参数
[运行时] 显示环境 → 版本信息 → 依赖管理
```

#### 函数分类
```json
{
  "function_categories": {
    "notification": {
      "name": "通知服务",
      "functions": ["send-slack-notification", "send-email", "send-sms"]
    },
    "database": {
      "name": "数据库操作",
      "functions": ["query-database", "update-record", "batch-process"]
    },
    "storage": {
      "name": "文件存储",
      "functions": ["upload-file", "download-file", "delete-file"]
    },
    "analysis": {
      "name": "数据分析",
      "functions": ["generate-report", "data-visualization", "statistics-calc"]
    },
    "integration": {
      "name": "系统集成",
      "functions": ["trigger-webhook", "api-gateway", "message-queue"]
    }
  }
}
```

---

## 4. 🧪 测试与调试页面

### 核心功能
- 对话测试界面
- 执行日志查看
- 错误诊断工具

### 交互形式
```
[测试对话] 聊天界面 → 输入测试问题 → 查看回答
[日志查看] 表格展示 → 筛选/搜索日志
[错误诊断] 错误列表 → 点击查看详情
```

### 数据展示
```json
{
  "test_session": {
    "input": "帮我检查这段代码有什么问题",
    "output": "发现3个安全问题，2个性能问题，1个代码质量问题...",
    "response_time": "4.2s",
    "mcp_tools_used": [
      {
        "name": "Git MCP",
        "action": "获取代码变更历史",
        "duration": "0.8s"
      },
      {
        "name": "Security Scanner",
        "action": "扫描安全漏洞",
        "duration": "1.2s"
      },
      {
        "name": "Code Analyzer",
        "action": "分析代码质量",
        "duration": "0.9s"
      }
    ],
    "faas_functions_called": [
      {
        "name": "send-slack-notification",
        "action": "发送审查结果通知",
        "status": "success",
        "duration": "0.3s"
      },
      {
        "name": "generate-report",
        "action": "生成详细报告",
        "status": "success",
        "duration": "0.5s"
      }
    ],
    "issues_found": {
      "security": 3,
      "performance": 2,
      "quality": 1
    },
    "total_execution_time": "4.2s",
    "mcp_execution_time": "2.9s",
    "faas_execution_time": "0.8s",
    "model_inference_time": "0.5s"
  }
}
```

---

## 5. 🚀 部署与运行页面

### 核心功能
- 一键部署操作
- 运行状态监控
- 基础性能统计

### 交互形式
```
[部署选项] 单选按钮 → 选择本地/云端部署
[启动按钮] 点击启动 → 显示运行状态
[监控面板] 实时图表 → 显示性能指标
```

### 状态显示
```json
{
  "deployment_status": "running",
  "uptime": "2h 15m",
  "request_count": 2847,
  "avg_response_time": "3.2s",
  "error_rate": "0.8%",
  "code_reviews_completed": 156,
  "issues_detected": 89,
  "security_vulnerabilities": 12
}
```

---

## 6. 📊 使用统计页面

### 核心功能
- 使用量统计
- 性能指标展示
- 简单的图表分析

### 交互形式
```
[时间选择] 日期范围选择器 → 选择统计时间
[指标卡片] 数值展示 → 显示关键指标
[图表展示] 折线图/柱状图 → 趋势分析
```

### 数据统计
```json
{
  "daily_stats": {
    "total_requests": 2847,
    "successful_requests": 2812,
    "avg_response_time": 3.2,
    "code_reviews_completed": 156,
    "issues_detected": 89,
    "security_vulnerabilities": 12,
    "performance_issues": 23,
    "code_quality_issues": 54,
    "top_languages": ["javascript", "python", "java"],
    "top_repositories": ["frontend-app", "backend-api", "mobile-app"]
  }
}
```

这个设计保持了简洁性，同时确保了各模块之间的清晰交互和数据流转。