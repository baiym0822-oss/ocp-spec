# ACP (Agent Communication Protocol)

**标准化 AI Agent 间通信协议**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version: 0.1.0](https://img.shields.io/badge/Version-0.1.0-blue.svg)]()

---

## 概述

ACP (Agent Communication Protocol) 是一个开放的、标准化的 AI Agent 间通信协议。

**使命**: 让不同框架开发的 AI Agent 能够互相通信和协作。

**愿景**: 成为 AI Agent 之间的 TCP/IP。

---

## 为什么需要 ACP?

当前 AI Agent 生态的问题:

- ❌ LangChain Agent 无法与 CrewAI Agent 通信
- ❌ 每个框架使用私有协议
- ❌ 企业被锁定在单一生态
- ❌ 无法构建跨 Agent 的工作流

ACP 的解决方案:

- ✅ 开放标准，任何框架都可以实现
- ✅ 跨框架互操作性
- ✅ 支持多 Agent 编排
- ✅ 内置经济激励机制

---

## 快速开始

### 安装适配器

```bash
# Claude Code 适配器
npm install @ocp/adapter-claude

# CrewAI 适配器
pip install ocp-crewai

# LangChain 适配器
pip install ocp-langchain
```

### 发送第一条消息

```typescript
import { ACPClient } from '@ocp/sdk';

const client = new ACPClient({
  endpoint: 'https://api.ocp-protocol.ai',
  apiKey: 'your-api-key'
});

// 发送任务给 Agent
const result = await client.send({
  agentId: 'assistant-001',
  action: 'code.generate',
  payload: {
    language: 'python',
    description: 'Create a REST API endpoint'
  }
});

console.log(result);
```

---

## 核心概念

### 消息格式

```json
{
  "version": "1.0",
  "message_id": "uuid-v4",
  "timestamp": "ISO-8601",
  "sender": {
    "agent_id": "string",
    "agent_type": "string"
  },
  "recipient": {
    "agent_id": "string",
    "agent_type": "string"
  },
  "message_type": "request|response|event|stream",
  "payload": {
    "action": "string",
    "parameters": {},
    "context": {}
  }
}
```

### 消息类型

| 类型 | 用途 | 示例 |
|------|------|------|
| `request` | 任务请求 | 代码生成、数据分析 |
| `response` | 任务响应 | 结果返回、状态更新 |
| `event` | 异步事件 | 状态变更、通知 |
| `stream` | 流式传输 | 实时日志、进度更新 |

---

## 架构

```
┌─────────────────────────────────────────────────────────────┐
│                    Application Layer                        │
│                    (Agent Business Logic)                   │
├─────────────────────────────────────────────────────────────┤
│                    Message Layer                            │
│                    (Serialization/Deserialization)          │
├─────────────────────────────────────────────────────────────┤
│                    Transport Layer                          │
│                    (WebSocket/HTTP/gRPC)                    │
├─────────────────────────────────────────────────────────────┤
│                    Security Layer                           │
│                    (Auth/Encryption/Signing)                │
└─────────────────────────────────────────────────────────────┘
```

---

## 适配器

| 适配器 | 状态 | 文档 |
|--------|------|------|
| Claude Code | 🚧 开发中 | [文档]() |
| OpenCode | 🚧 开发中 | [文档]() |
| CrewAI | 📋 计划中 | [文档]() |
| LangChain | 📋 计划中 | [文档]() |
| AutoGen | 📋 计划中 | [文档]() |

---

## 开发

### 本地开发环境

```bash
# 克隆仓库
git clone https://github.com/agent-communication-protocol/ocp-spec.git
cd ocp-spec

# 安装依赖
npm install

# 运行测试
npm test

# 构建文档
npm run docs
```

---

## 路线图

### v0.1.0 (当前)
- [ ] ACP 协议规范草案
- [ ] Claude Code 适配器原型
- [ ] 基础文档

### v0.2.0
- [ ] 协议规范 v1.0
- [ ] LangChain 适配器
- [ ] CrewAI 适配器
- [ ] 示例项目

### v1.0.0
- [ ] 稳定版协议
- [ ] 生产级 SDK
- [ ] 公开 API 服务

---

## 参与贡献

欢迎贡献！请查看 [贡献指南](CONTRIBUTING.md)。

### 贡献方式
- 📝 改进文档
- 🐛 报告 Bug
- 💡 提出新功能
- 🔧 提交 PR

---

## 社区

- 💬 [Discord](https://discord.gg/ocp-protocol)
- 🐦 [Twitter](https://twitter.com/ocp_protocol)
- 📧 [邮件列表](mailto:hello@ocp-protocol.ai)

---

## 许可证

MIT License — 详见 [LICENSE](LICENSE)

---

## 相关链接

- [完整文档](https://docs.ocp-protocol.ai)
- [协议规范](./spec/)
- [示例项目](./examples/)
- [SDK](./sdk/)
