# ACP Protocol Specification v0.1.0

**Draft Specification**

---

## 1. Introduction

ACP (Agent Communication Protocol) is a standardized protocol for communication between AI Agents.

### 1.1 Purpose

This specification defines the message format, transport mechanisms, and security requirements for ACP.

### 1.2 Scope

- Message serialization format
- Transport protocols (WebSocket, HTTP, gRPC)
- Authentication and authorization
- Error handling

---

## 2. Message Format

### 2.1 Structure

All ACP messages use JSON format:

```json
{
  "version": "1.0",
  "message_id": "550e8400-e29b-41d4-a716-446655440000",
  "timestamp": "2026-05-09T10:30:00Z",
  "sender": {
    "agent_id": "agent-001",
    "agent_type": "assistant",
    "capabilities": ["code.generate", "code.review"]
  },
  "recipient": {
    "agent_id": "agent-002",
    "agent_type": "worker"
  },
  "message_type": "request",
  "payload": {
    "action": "code.generate",
    "parameters": {
      "language": "python",
      "description": "Create a REST API"
    },
    "context": {
      "project_id": "proj-123",
      "session_id": "sess-456"
    }
  },
  "metadata": {
    "priority": "normal",
    "ttl_seconds": 300,
    "trace_id": "trace-789"
  }
}
```

### 2.2 Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `version` | string | ✅ | Protocol version (e.g., "1.0") |
| `message_id` | UUID | ✅ | Unique message identifier |
| `timestamp` | ISO-8601 | ✅ | Message creation time (UTC) |
| `sender` | object | ✅ | Sender agent information |
| `recipient` | object | ✅ | Recipient agent information |
| `message_type` | enum | ✅ | Message type (see 2.3) |
| `payload` | object | ✅ | Message payload (see 2.4) |
| `metadata` | object | ❌ | Optional metadata |

### 2.3 Message Types

| Type | Description | Use Case |
|------|-------------|----------|
| `request` | Task request | Request an agent to perform an action |
| `response` | Task response | Return result of a request |
| `event` | Asynchronous event | Notify about state changes |
| `stream` | Stream data | Real-time data streaming |

### 2.4 Payload Structure

```json
{
  "action": "string",
  "parameters": {},
  "context": {}
}
```

---

## 3. Transport Protocols

### 3.1 WebSocket (Recommended)

```
ws://api.ocp-protocol.ai/v1/messages
wss://api.ocp-protocol.ai/v1/messages (secure)
```

### 3.2 HTTP/REST

```
POST https://api.ocp-protocol.ai/v1/messages
GET https://api.ocp-protocol.ai/v1/messages/{message_id}
```

### 3.3 gRPC (High Performance)

```protobuf
service ACPService {
  rpc SendMessage(Message) returns (MessageResponse);
  rpc StreamMessages(StreamRequest) returns (stream Message);
}
```

---

## 4. Security

### 4.1 Authentication

All requests must include an API key:

```
Authorization: Bearer <api-key>
```

### 4.2 Encryption

- Transport: TLS 1.3 required
- Payload: Optional end-to-end encryption

### 4.3 Rate Limiting

| Tier | Requests/minute | Requests/day |
|------|-----------------|--------------|
| Free | 60 | 1,000 |
| Pro | 600 | 50,000 |
| Enterprise | Custom | Custom |

---

## 5. Error Handling

### 5.1 Error Response Format

```json
{
  "error": {
    "code": "INVALID_MESSAGE",
    "message": "Message validation failed",
    "details": {
      "field": "message_id",
      "reason": "Invalid UUID format"
    }
  }
}
```

### 5.2 Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_MESSAGE` | 400 | Message validation failed |
| `UNAUTHORIZED` | 401 | Invalid or missing API key |
| `FORBIDDEN` | 403 | Insufficient permissions |
| `NOT_FOUND` | 404 | Resource not found |
| `RATE_LIMITED` | 429 | Rate limit exceeded |
| `INTERNAL_ERROR` | 500 | Internal server error |

---

## 6. Versioning

### 6.1 Protocol Versioning

ACP uses semantic versioning: `MAJOR.MINOR.PATCH`

- `MAJOR`: Breaking changes
- `MINOR`: Backward-compatible features
- `PATCH`: Backward-compatible bug fixes

### 6.2 Current Version

**v0.1.0** — Draft specification

---

## 7. Implementation Guidelines

### 7.1 For Adapter Developers

1. Implement message serialization
2. Handle authentication
3. Implement retry logic
4. Log all messages for debugging

### 7.2 For Agent Developers

1. Register agent capabilities
2. Handle incoming messages
3. Return structured responses
4. Handle errors gracefully

---

## 8. Examples

### 8.1 Code Generation Request

```json
{
  "version": "1.0",
  "message_id": "msg-001",
  "timestamp": "2026-05-09T10:30:00Z",
  "sender": {
    "agent_id": "coordinator-001",
    "agent_type": "coordinator"
  },
  "recipient": {
    "agent_id": "coder-001",
    "agent_type": "worker"
  },
  "message_type": "request",
  "payload": {
    "action": "code.generate",
    "parameters": {
      "language": "python",
      "description": "Create a Flask REST API with user authentication"
    }
  }
}
```

### 8.2 Response

```json
{
  "version": "1.0",
  "message_id": "msg-002",
  "timestamp": "2026-05-09T10:31:00Z",
  "sender": {
    "agent_id": "coder-001",
    "agent_type": "worker"
  },
  "recipient": {
    "agent_id": "coordinator-001",
    "agent_type": "coordinator"
  },
  "message_type": "response",
  "payload": {
    "action": "code.generate",
    "result": {
      "status": "success",
      "code": "from flask import Flask...\n",
      "files": ["app.py", "requirements.txt"]
    }
  },
  "metadata": {
    "execution_time_ms": 5432
  }
}
```

---

## 9. Changelog

### v0.1.0 (2026-05-09)
- Initial draft specification
- Message format definition
- Transport protocol specs
- Security requirements

---

**Status**: Draft  
**Contact**: spec@ocp-protocol.ai
