# 我的開發偏好

## 關於我
- **經驗等級**: 8 年全棧開發經驗
- **偏好的語言**: TypeScript, Python
- **溝通風格**: 直白，並提供範例
- **學習方式**: 視覺化圖表與程式碼

## 程式碼偏好

### 錯誤處理
我偏好明確的錯誤處理，使用 try-catch 區塊和有意義的錯誤訊息。
避免使用通用的錯誤。務必記錄錯誤以利除錯。

### 註解
使用註解說明 WHY，而不是 WHAT。程式碼應該是自我文件的。
註解應該解釋業務邏輯或非顯而易見的決策。

### 測試
我偏好 TDD（測試驅動開發）。
首先撰寫測試，然後再實作。
專注於行為，而不是實作細節。

### 架構
我偏好模組化、鬆散耦合的設計。
使用依賴注入以提高可測試性。
分離關注點（Controllers、Services、Repositories）。

## 除錯偏好
- 使用 console.log 並加上前綴：`[DEBUG]`
- 包含上下文：函式名稱、相關變數
- 有堆疊追蹤時務必使用
- 務必在日誌中包含時間戳記

## 溝通
- 使用圖表解釋複雜的概念
- 在解釋理論之前，先展示具體的範例
- 包含程式碼片段的前後對比
- 在結尾總結重點

## 專案組織
我組織專案的方式如下：
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
- **IDE**: VS Code 搭配 vim 快捷鍵
- **Terminal**: Zsh 搭配 Oh-My-Zsh
- **格式化**: Prettier（每行 100 字元）
- **Linter**: ESLint 搭配 airbnb 設定
- **測試框架**: Jest 搭配 React Testing Library

---
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/memory
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
