# 我的開發偏好

## 關於我
- **經驗等級**: 8 年全棧開發經驗
- **偏好的語言**: TypeScript, Python
- **溝通風格**: 直白，並提供範例
- **學習方式**: 視覺化圖表與程式碼

## 程式碼偏好

### 錯誤處理
我偏好明確的錯誤處理，使用 try-catch 區塊和有意義的錯誤訊息。
避免使用通用的錯誤。 務必記錄錯誤以利除錯。

### 註解
使用註解說明 WHY，而不是 WHAT。 程式碼應該是自我文件的。
註解應該解釋業務邏輯或非顯而見的決策。

### 測試
我偏好 TDD (測試驅動開發)。
首先撰寫測試，然後實作。
專注於行為，而不是實作細節。

### 架構
我偏好模組化、鬆散耦合的設計。
使用依賴注入以提高可測試性。
分離關注點 (Controllers, Services, Repositories)。

## 除錯偏好
- 使用 console.log 並加上前綴：`[DEBUG]`
- 包含上下文：函數名稱、相關變數
- 使用堆疊追蹤，如果有的話
- 務必在記錄中包含時間戳記

## 溝通
- 使用圖表解釋複雜的概念
- 在解釋理論之前，先展示具體的範例
- 包含程式碼片段的前後比較
- 在結尾總結重點

## 專案組織
我組織我的專案如下：
```
project/
  ├── src/
  │   ├── api/
  │   ├── services/
  │   ├── models/
  │   └── utils/
  ├── tests/
  ├── docs/
  └── docker/
```

## 工具
- **IDE**: VS Code with vim keybindings
- **Terminal**: Zsh with Oh-My-Zsh
- **格式化**: Prettier (100 char line length)
- **Linter**: ESLint with airbnb config
- **測試框架**: Jest with React Testing Library

---
**上次更新**: 2026 年 4 月 9 日
