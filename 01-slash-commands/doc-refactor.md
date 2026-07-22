---
name: Documentation Refactor
description: 重構專案文件以提升清晰度與易讀性
tags: documentation, refactoring, organization
---

# Documentation Refactor

根據專案類型重構專案文件結構：

1. **分析專案**：識別類型（library/API/web app/CLI/microservices）、架構以及使用者角色（user personas）
2. **集中化文件**：將技術文件移至 `docs/` 並建立適當的交叉引用
3. **根目錄 README.md**：將其精簡為進入點，包含總覽、快速入門、模組/組件摘要、授權與聯絡資訊
4. **組件文件**：新增模組/套件/服務層級的 README 檔案，並包含安裝與測試說明
5. **依相關類別整理 `docs/`**：
   - 架構 (Architecture)、API Reference、資料庫 (Database)、設計 (Design)、疑難排解 (Troubleshooting)、部署 (Deployment)、貢獻指南 (Contributing)（請根據專案需求調整）
6. **建立指南**（選擇適用項）：
   - 使用者指南 (User Guide)：針對應用程式的終端使用者文件
   - API 文件 (API Documentation)：針對 API 的端點 (Endpoints)、身分驗證與範例
   - 開發指南 (Development Guide)：環境設定、測試與貢獻工作流程
   - 部署指南 (Deployment Guide)：針對服務/應用程式的正式環境部署
7. **使用 Mermaid** 繪製所有圖表（架構、流程、Schema）

保持文件簡潔、易於掃描，並符合專案類型的上下文。

---
**最後更新日期**：2026 年 4 月 9 日
