---
description: 分析程式碼效能問題並提供最佳化建議
---

# 程式碼最佳化

檢視提供的程式碼，依照優先順序，針對以下問題進行分析：

1. **效能瓶頸** - 找出 O(n²) 運算、效率不佳的迴圈
2. **記憶體洩漏** - 找出未釋放的資源、循環參考
3. **演算法改進** - 建議更好的演算法或資料結構
4. **快取機會** - 找出重複的運算
5. **並發問題** - 找出競爭狀況或多執行緒問題

以以下格式呈現您的回覆：
- 問題嚴重性 (Critical/High/Medium/Low)
- 程式碼中位置
- 解釋
- 建議的修正方案，包含程式碼範例

---
**上次更新**: 2026 年 8 月 4 日
**Claude Code 版本**: 2.1.220
**來源**:
- https://code.claude.com/docs/en/commands
**相容模型**: Claude Fable 5, Claude Opus 5, Claude Sonnet 5, Claude Sonnet 4.6, Claude Opus 4.8, Claude Haiku 4.5
