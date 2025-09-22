# Data Model: Task Management Platform

**Feature**: Task Management Platform 集中管理平台
**Date**: 2025-09-22
**Status**: Draft

## 領域實體 (Domain Entities)

### Project（專案）
代表組織的工作專案，作為任務的容器和組織單位。

**屬性**:
- `Id` (Guid): 專案唯一識別碼
- `Name` (string, required, max 200): 專案名稱
- `Description` (string, optional, max 1000): 專案描述
- `Status` (ProjectStatus enum): 專案狀態
- `StartDate` (DateTime): 專案開始日期
- `EndDate` (DateTime, optional): 專案結束日期
- `CreatedBy` (Guid): 建立者 ID
- `CreatedAt` (DateTime): 建立時間
- `UpdatedAt` (DateTime): 最後更新時間
- `IsDeleted` (bool): 軟刪除標記

**關聯**:
- `Tasks` (1:N): 專案包含的任務列表
- `ProjectMembers` (N:N): 專案成員關聯
- `Owner` (N:1): 專案擁有者

**狀態列舉**:
```csharp
public enum ProjectStatus
{
    Planning = 1,
    Active = 2,
    OnHold = 3,
    Completed = 4,
    Cancelled = 5
}
```

**業務規則**:
- 專案名稱在組織內必須唯一
- 結束日期不能早於開始日期
- 只有專案擁有者和管理員可以刪除專案
- 專案狀態變更需要記錄稽核日誌

### Task（任務）
代表具體的工作項目，是系統的核心實體。

**屬性**:
- `Id` (Guid): 任務唯一識別碼
- `Title` (string, required, max 200): 任務標題
- `Description` (string, optional, max 2000): 任務描述
- `Status` (TaskStatus enum): 任務狀態
- `Priority` (TaskPriority enum): 任務優先級
- `ProjectId` (Guid): 所屬專案 ID
- `AssignedTo` (Guid, optional): 指派對象 ID
- `CreatedBy` (Guid): 建立者 ID
- `DueDate` (DateTime, optional): 截止日期
- `EstimatedHours` (decimal, optional): 預估工時
- `ActualHours` (decimal, optional): 實際工時
- `CreatedAt` (DateTime): 建立時間
- `UpdatedAt` (DateTime): 最後更新時間
- `CompletedAt` (DateTime, optional): 完成時間
- `IsDeleted` (bool): 軟刪除標記

**關聯**:
- `Project` (N:1): 所屬專案
- `Assignee` (N:1): 指派對象
- `Creator` (N:1): 建立者
- `Comments` (1:N): 任務評論
- `TaskHistory` (1:N): 任務變更歷史
- `Attachments` (1:N): 任務附件

**狀態列舉**:
```csharp
public enum TaskStatus
{
    Created = 1,
    Assigned = 2,
    InProgress = 3,
    Review = 4,
    Completed = 5,
    OnHold = 6,
    Cancelled = 7
}
```

**優先級列舉**:
```csharp
public enum TaskPriority
{
    Low = 1,
    Medium = 2,
    High = 3,
    Critical = 4
}
```

**業務規則**:
- 任務必須屬於某個專案
- 指派對象必須是專案成員
- 狀態變更必須遵循工作流程規則
- 完成時間只有在狀態為 Completed 時才設定

### User（使用者）
代表平台的使用者，包含認證和授權資訊。

**屬性**:
- `Id` (Guid): 使用者唯一識別碼
- `Email` (string, required, unique): 電子郵件
- `FirstName` (string, required, max 50): 名字
- `LastName` (string, required, max 50): 姓氏
- `PasswordHash` (string, required): 密碼雜湊值
- `Role` (UserRole enum): 使用者角色
- `IsActive` (bool): 帳戶啟用狀態
- `LastLoginAt` (DateTime, optional): 最後登入時間
- `CreatedAt` (DateTime): 建立時間
- `UpdatedAt` (DateTime): 最後更新時間
- `ProfileImageUrl` (string, optional): 頭像圖片 URL

**關聯**:
- `CreatedProjects` (1:N): 建立的專案
- `AssignedTasks` (1:N): 指派的任務
- `CreatedTasks` (1:N): 建立的任務
- `ProjectMemberships` (N:N): 專案成員身份

**角色列舉**:
```csharp
public enum UserRole
{
    Member = 1,
    ProjectManager = 2,
    Administrator = 3
}
```

**業務規則**:
- 電子郵件必須唯一且格式正確
- 密碼必須符合安全性要求
- 只有啟用的使用者可以登入系統
- 管理員具有所有權限

### ProjectMember（專案成員）
代表使用者在專案中的成員身份和角色。

**屬性**:
- `Id` (Guid): 成員關聯唯一識別碼
- `ProjectId` (Guid): 專案 ID
- `UserId` (Guid): 使用者 ID
- `Role` (ProjectRole enum): 專案內角色
- `JoinedAt` (DateTime): 加入時間
- `IsActive` (bool): 成員狀態

**關聯**:
- `Project` (N:1): 所屬專案
- `User` (N:1): 成員使用者

**專案角色列舉**:
```csharp
public enum ProjectRole
{
    Viewer = 1,
    Contributor = 2,
    Manager = 3
}
```

### TaskComment（任務評論）
代表任務的評論和討論記錄。

**屬性**:
- `Id` (Guid): 評論唯一識別碼
- `TaskId` (Guid): 所屬任務 ID
- `UserId` (Guid): 評論者 ID
- `Content` (string, required, max 1000): 評論內容
- `CreatedAt` (DateTime): 建立時間
- `UpdatedAt` (DateTime, optional): 更新時間
- `IsDeleted` (bool): 軟刪除標記

**關聯**:
- `Task` (N:1): 所屬任務
- `User` (N:1): 評論者

### TaskHistory（任務歷史）
記錄任務的所有變更歷史，用於稽核追蹤。

**屬性**:
- `Id` (Guid): 歷史記錄唯一識別碼
- `TaskId` (Guid): 任務 ID
- `UserId` (Guid): 變更者 ID
- `ChangeType` (ChangeType enum): 變更類型
- `OldValue` (string, optional): 變更前值
- `NewValue` (string, optional): 變更後值
- `FieldName` (string): 變更欄位名稱
- `CreatedAt` (DateTime): 變更時間

**變更類型列舉**:
```csharp
public enum ChangeType
{
    Created = 1,
    Updated = 2,
    StatusChanged = 3,
    Assigned = 4,
    Deleted = 5
}
```

### Notification（通知）
代表系統通知和提醒功能。

**屬性**:
- `Id` (Guid): 通知唯一識別碼
- `UserId` (Guid): 接收者 ID
- `Type` (NotificationType enum): 通知類型
- `Title` (string, required, max 200): 通知標題
- `Content` (string, required, max 500): 通知內容
- `RelatedEntityId` (Guid, optional): 相關實體 ID
- `RelatedEntityType` (string, optional): 相關實體類型
- `IsRead` (bool): 已讀狀態
- `CreatedAt` (DateTime): 建立時間
- `ReadAt` (DateTime, optional): 閱讀時間

**通知類型列舉**:
```csharp
public enum NotificationType
{
    TaskAssigned = 1,
    TaskStatusChanged = 2,
    TaskDueSoon = 3,
    TaskOverdue = 4,
    ProjectStatusChanged = 5,
    CommentAdded = 6
}
```

## 值物件 (Value Objects)

### TaskWorkflow（任務工作流程）
定義任務狀態轉換的業務規則。

**屬性**:
- `FromStatus` (TaskStatus): 起始狀態
- `ToStatus` (TaskStatus): 目標狀態
- `RequiredRole` (ProjectRole): 執行轉換所需角色
- `IsAllowed` (bool): 是否允許轉換

### NotificationSettings（通知設定）
使用者的通知偏好設定。

**屬性**:
- `UserId` (Guid): 使用者 ID
- `EmailNotifications` (bool): 郵件通知
- `InAppNotifications` (bool): 應用程式內通知
- `TaskAssignmentNotification` (bool): 任務指派通知
- `DueDateReminderNotification` (bool): 截止日期提醒

## 資料庫設計考量

### 索引策略
- **Project**: `Name`, `Status`, `CreatedBy`
- **Task**: `ProjectId`, `AssignedTo`, `Status`, `Priority`, `DueDate`
- **User**: `Email` (unique), `Role`
- **TaskHistory**: `TaskId`, `CreatedAt`
- **Notification**: `UserId`, `IsRead`, `CreatedAt`

### 外鍵約束
- Task.ProjectId → Project.Id
- Task.AssignedTo → User.Id
- Task.CreatedBy → User.Id
- ProjectMember.ProjectId → Project.Id
- ProjectMember.UserId → User.Id
- TaskComment.TaskId → Task.Id
- TaskComment.UserId → User.Id

### 軟刪除策略
- 使用 `IsDeleted` 欄位實現軟刪除
- 查詢時預設過濾已刪除記錄
- 定期清理過期的軟刪除記錄

### 稽核欄位
所有主要實體包含稽核欄位：
- `CreatedAt`: 建立時間
- `UpdatedAt`: 最後更新時間
- `CreatedBy`: 建立者（適用時）

## 資料驗證規則

### 通用規則
- 所有 GUID 欄位不得為空
- 所有 required 字串欄位不得為空或空白
- 日期時間欄位使用 UTC 時區
- 軟刪除記錄不參與唯一性約束

### 特定規則
- Email 格式驗證
- 密碼強度驗證
- 日期邏輯驗證（結束日期 > 開始日期）
- 狀態轉換驗證

## 效能優化

### 分頁查詢
- 任務列表支援分頁和排序
- 使用 offset-based 分頁策略
- 預設每頁 20 筆記錄

### 快取策略
- 使用者資訊快取（5 分鐘）
- 專案成員關聯快取（10 分鐘）
- 通知計數快取（1 分鐘）

### 資料庫連線優化
- 使用連線池管理
- 非同步查詢操作
- 適當的查詢超時設定