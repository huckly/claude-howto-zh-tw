---
name: setup-ci-cd
description: 建置 pre-commit hooks 與 GitHub Actions 以確保品質
---

# 設定 CI/CD Pipeline

依專案類型實作完整的 DevOps 品質關卡：

1. **分析專案**：偵測程式語言、框架、建置系統與既有工具
2. **設定 pre-commit hooks**，使用各語言專屬工具：
   - 格式化：Prettier/Black/gofmt/rustfmt 等
   - Linting：ESLint/Ruff/golangci-lint/Clippy 等
   - 安全性：Bandit/gosec/cargo-audit/npm audit 等
   - 型別檢查：TypeScript/mypy/flow（若適用）
   - 測試：執行相關的測試套件
3. **建立 GitHub Actions workflows**（.github/workflows/）：
   - 在 push/PR 時執行與 pre-commit 相同的檢查
   - 多版本/多平台矩陣（若適用）
   - 建置與測試驗證
   - 部署步驟（若需要）
4. **驗證 pipeline**：在本機測試、建立測試 PR，確認所有檢查皆通過

使用免費/開源工具。尊重既有設定。保持執行快速。

---
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/commands
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
