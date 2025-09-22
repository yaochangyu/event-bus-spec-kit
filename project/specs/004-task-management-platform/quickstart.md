# Quick Start Guide: Task Management Platform

**Feature**: Task Management Platform 集中管理平台
**Date**: 2025-09-22
**Status**: Ready for Implementation

## 概述

本快速開始指南將引導您完成 Task Management Platform 的基本使用流程，從專案建立到任務完成的完整生命週期。

## 前置要求

- ✅ 已部署的 Task Management Platform API
- ✅ 有效的使用者帳戶
- ✅ API 存取權限
- 🔧 測試工具：Postman, curl, 或其他 HTTP 客戶端

## 基本流程測試

### 步驟 1: 使用者認證

**目標**: 取得 API 存取權杖

```bash
# 使用者登入
curl -X POST "https://api.taskmanagement.com/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepassword"
  }'
```

**預期回應**:
```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "def50200...",
  "expiresIn": 3600,
  "user": {
    "id": "user-uuid",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "role": "ProjectManager"
  }
}
```

### 步驟 2: 建立專案

**目標**: 建立新的工作專案

```bash
# 建立專案
curl -X POST "https://api.taskmanagement.com/v1/projects" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "網站重構專案",
    "description": "升級現有網站架構和使用者介面",
    "startDate": "2025-09-22T00:00:00Z"
  }'
```

**預期回應**:
```json
{
  "id": "project-uuid",
  "name": "網站重構專案",
  "description": "升級現有網站架構和使用者介面",
  "status": "Planning",
  "startDate": "2025-09-22T00:00:00Z",
  "createdBy": "user-uuid",
  "createdAt": "2025-09-22T10:00:00Z",
  "taskCount": 0,
  "completedTaskCount": 0
}
```

### 步驟 3: 建立任務

**目標**: 在專案中建立具體任務

```bash
# 建立任務
curl -X POST "https://api.taskmanagement.com/v1/projects/PROJECT_UUID/tasks" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "設計新的使用者介面",
    "description": "建立響應式設計模板和組件庫",
    "priority": "High",
    "dueDate": "2025-10-15T23:59:59Z",
    "estimatedHours": 40
  }'
```

**預期回應**:
```json
{
  "id": "task-uuid",
  "title": "設計新的使用者介面",
  "description": "建立響應式設計模板和組件庫",
  "status": "Created",
  "priority": "High",
  "projectId": "project-uuid",
  "projectName": "網站重構專案",
  "dueDate": "2025-10-15T23:59:59Z",
  "estimatedHours": 40,
  "createdAt": "2025-09-22T10:05:00Z"
}
```

### 步驟 4: 指派任務

**目標**: 將任務指派給團隊成員

```bash
# 指派任務
curl -X PATCH "https://api.taskmanagement.com/v1/tasks/TASK_UUID/assign" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "assignedTo": "developer-uuid"
  }'
```

**預期回應**:
```json
{
  "id": "task-uuid",
  "title": "設計新的使用者介面",
  "status": "Assigned",
  "assignedTo": "developer-uuid",
  "assigneeName": "Jane Smith",
  "updatedAt": "2025-09-22T10:10:00Z"
}
```

### 步驟 5: 更新任務狀態

**目標**: 追蹤任務進度

```bash
# 開始執行任務
curl -X PATCH "https://api.taskmanagement.com/v1/tasks/TASK_UUID/status" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "InProgress",
    "comment": "開始進行 UI 設計工作"
  }'
```

**預期回應**:
```json
{
  "id": "task-uuid",
  "title": "設計新的使用者介面",
  "status": "InProgress",
  "updatedAt": "2025-09-22T10:15:00Z"
}
```

### 步驟 6: 完成任務

**目標**: 標記任務為完成

```bash
# 完成任務
curl -X PATCH "https://api.taskmanagement.com/v1/tasks/TASK_UUID/status" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "Completed",
    "comment": "UI 設計已完成並通過審核"
  }'
```

```bash
# 更新實際工時
curl -X PUT "https://api.taskmanagement.com/v1/tasks/TASK_UUID" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "actualHours": 38
  }'
```

### 步驟 7: 查看專案進度

**目標**: 檢視專案整體狀況

```bash
# 取得專案詳細資訊
curl -X GET "https://api.taskmanagement.com/v1/projects/PROJECT_UUID" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

```bash
# 取得專案任務列表
curl -X GET "https://api.taskmanagement.com/v1/projects/PROJECT_UUID/tasks?status=Completed" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### 步驟 8: 查看通知

**目標**: 檢視系統通知

```bash
# 取得未讀通知
curl -X GET "https://api.taskmanagement.com/v1/notifications?isRead=false" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**預期回應**:
```json
{
  "data": [
    {
      "id": "notification-uuid",
      "type": "TaskStatusChanged",
      "title": "任務狀態更新",
      "content": "「設計新的使用者介面」已完成",
      "relatedEntityId": "task-uuid",
      "relatedEntityType": "Task",
      "isRead": false,
      "createdAt": "2025-09-22T10:20:00Z"
    }
  ],
  "unreadCount": 3
}
```

## 進階功能測試

### 任務篩選和搜尋

```bash
# 按優先級篩選任務
curl -X GET "https://api.taskmanagement.com/v1/projects/PROJECT_UUID/tasks?priority=High&page=1&pageSize=10" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"

# 按指派對象篩選
curl -X GET "https://api.taskmanagement.com/v1/projects/PROJECT_UUID/tasks?assignedTo=USER_UUID" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### 專案狀態管理

```bash
# 啟動專案
curl -X PUT "https://api.taskmanagement.com/v1/projects/PROJECT_UUID" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "Active"
  }'
```

### 權杖刷新

```bash
# 刷新存取權杖
curl -X POST "https://api.taskmanagement.com/v1/auth/refresh" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "refreshToken": "YOUR_REFRESH_TOKEN"
  }'
```

## BDD 測試場景

### 場景 1: 專案經理建立和管理專案

```gherkin
Scenario: 專案經理成功建立新專案
  Given 專案經理已登入系統
  When 專案經理建立名為 "網站重構專案" 的新專案
  Then 系統應該回傳專案 ID
  And 專案狀態應該是 "Planning"
  And 專案經理應該收到建立成功的通知
```

### 場景 2: 任務生命週期管理

```gherkin
Scenario: 任務從建立到完成的完整流程
  Given 存在一個啟動的專案
  When 專案經理建立高優先級任務
  And 將任務指派給開發者
  And 開發者接受任務並開始執行
  And 開發者完成任務
  Then 任務狀態應該是 "Completed"
  And 專案經理應該收到完成通知
  And 專案進度應該更新
```

### 場景 3: 權限驗證

```gherkin
Scenario: 非專案成員無法存取專案任務
  Given 使用者不是專案成員
  When 使用者嘗試存取專案任務列表
  Then 系統應該回傳 403 權限錯誤
  And 錯誤訊息應該說明權限不足
```

## 效能測試建議

### 負載測試重點

1. **併發登入測試**: 100 個並發使用者同時登入
2. **任務查詢效能**: 單專案 1000+ 任務的查詢回應時間
3. **通知推送效能**: 大量狀態變更通知的處理能力
4. **資料庫連線**: 連線池在高負載下的表現

### 效能指標

- 🎯 API 回應時間 < 200ms (95th percentile)
- 🎯 資料庫查詢時間 < 100ms
- 🎯 並發處理能力 > 1000 req/s
- 🎯 系統可用性 > 99.9%

## 錯誤處理測試

### 常見錯誤場景

```bash
# 無效的認證資訊
curl -X POST "https://api.taskmanagement.com/v1/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"email": "invalid", "password": "wrong"}'
# 預期: 401 Unauthorized

# 嘗試存取不存在的專案
curl -X GET "https://api.taskmanagement.com/v1/projects/non-existent-uuid" \
  -H "Authorization: Bearer YOUR_TOKEN"
# 預期: 404 Not Found

# 無效的任務狀態轉換
curl -X PATCH "https://api.taskmanagement.com/v1/tasks/TASK_UUID/status" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"status": "InvalidStatus"}'
# 預期: 400 Bad Request
```

## 驗證清單

完成快速開始測試後，確認以下功能正常運作：

- [ ] ✅ 使用者認證和權杖管理
- [ ] ✅ 專案 CRUD 操作
- [ ] ✅ 任務 CRUD 操作
- [ ] ✅ 任務狀態管理和工作流程
- [ ] ✅ 任務指派功能
- [ ] ✅ 通知系統
- [ ] ✅ 分頁和篩選功能
- [ ] ✅ 權限控制
- [ ] ✅ 錯誤處理
- [ ] ✅ API 回應時間符合要求

## 疑難排解

### 常見問題

**Q: 登入失敗，收到 401 錯誤**
A: 檢查電子郵件和密碼是否正確，確認帳戶已啟用

**Q: 無法建立任務，收到 403 錯誤**
A: 確認使用者是專案成員且具有適當權限

**Q: 任務狀態無法更新**
A: 檢查狀態轉換是否符合工作流程規則

**Q: 通知未送達**
A: 確認通知設定已啟用，檢查網路連線

### 支援資源

- 📖 API 文件: [OpenAPI 規格](contracts/openapi.yaml)
- 🔧 資料模型: [資料模型文件](data-model.md)
- 📋 問題回報: 聯繫開發團隊
- 💬 技術支援: support@taskmanagement.com

---

**注意**: 本指南使用範例 UUID 和權杖，實際使用時請替換為真實的值。確保在生產環境中使用 HTTPS 連線以保護敏感資料。