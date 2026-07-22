---
name: Setup CI/CD Pipeline
description: Implement pre-commit hooks and GitHub Actions for quality assurance
tags: ci-cd, devops, automation
---

# Setup CI/CD Pipeline

針對專案類型實作全面的 DevOps 品質閘門：

1. **分析專案**：偵測程式語言、框架、建置系統及現有的工具鏈
2. **配置 pre-commit hooks** 並使用特定語言的工具：
   - 格式化 (Formatting)：Prettier/Black/gofmt/rustfmt/etc.
   - 語法檢查 (Linting)：ESLint/Ruff/golangci-lint/Clippy/etc.
   - 安全性 (Security)：Bandit/gosec/cargo-audit/npm audit/etc.
   - 型別檢查 (Type checking)：TypeScript/mypy/flow (若適用)
   - 測試 (Tests)：執行相關的測試套件
3. **建立 GitHub Actions workflows** (.github/workflows/)：
   - 在 push/PR 時鏡像執行 pre-commit 檢查
   - 多版本/平台矩陣 (Matrix) (若適用)
   - 建置與測試驗證
   - 部署步驟 (若需要)
4. **驗證 pipeline**：進行本地測試、建立測試 PR，並確認所有檢查皆通過

使用免費/開源工具。尊重現有的配置。保持執行速度快速。

---
**最後更新日期**：2026 年 4 月 9 日
