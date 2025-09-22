# event-bus-spec-kit

## 1. Install Specify

在 claude code，使用 /specify 指令來安裝 Specify 工具。

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init "task-management"
```

## 3. Create the spec

範例：

```text
uvx --from git+https://github.com/github/spec-kit.git specify init 建立第一版的 Task Management Platform 集中管的理平台，需要以下 WebAPI 功能
1. 調用端呼叫建立任務 API，API 在 Queue 建立任務資訊
2. 調用端呼叫取出任務 API，API 從 Queue 取出任務資訊，並在資料庫新增任務資訊
3. 調用端呼叫執行任務 API，API 從資料庫取出任務資訊，欄位資訊包含了 Callback API 的位置
4. 調用端使用 HttpClient 呼叫 Callback API

注意：
- 編碼原則要參考 https://github.com/yaochangyu/api.template CLAUDE.md
- 實作的時要從 https://github.com/yaochangyu/api.template 複製出來改，改成符合需求的命名空間
- 文件需要流程圖、有限狀態機、循序圖，使用 mermaid 編寫。
```



產生 spec.md 需求文件，檢核需求文件，沒有問題就輸入 "繼續"
