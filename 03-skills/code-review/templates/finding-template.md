# Code Review 發現事項範本

當你在記錄程式碼審查（code review）過程中發現的每個問題時，請使用此範本。

---

## 問題：[標題]

### 嚴重程度
- [ ] Critical (阻礙部署)
- [ ] High (應在合併前修復)
- [ ] Medium (應儘快修復)
- [ ] Low (建議修復)

### 類別
- [ ] Security
- [ ] Performance
- [ ] Code Quality
- [ ] Maintainability
- [ ] Testing
- [ ] Design Pattern
- [ ] Documentation

### 位置
**檔案：** `src/components/UserCard.tsx`

**行號：** 45-52

**函式/方法：** `renderUserDetails()`

### 問題描述

**內容：** 描述問題是什麼。

**重要性：** 解釋其影響以及為什麼需要修復。

**目前行為：** 顯示有問題的程式碼或行為。

**預期行為：** 描述應該發生的行為。

### 程式碼範例

#### 目前（有問題的）

```typescript
// Shows the N+1 query problem
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // Query per user!
  renderUserPosts(posts);
});
```

#### 建議修復方式

```typescript
// Optimized with JOIN query
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### 影響分析

| 項目 | 影響 | 嚴重程度 |
|--------|--------|----------|
| Performance | 20 個使用者會產生 100+ 次查詢 | High |
| User Experience | 頁面載入緩慢 | High |
| Scalability | 規模擴大時會失效 | Critical |
| Maintainability | 難以除錯 | Medium |

### 相關問題

- `AdminUserList.tsx` 第 120 行有類似問題
- 相關 PR：#456
- 相關 issue：#789

### 額外資源

- [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_problem)
- [Database Join Documentation](https://docs.example.com/joins)

### 審查者筆記

- 這是此程式碼庫中的常見模式
- 考慮將此加入程式碼風格指南
- 可能值得建立一個輔助函式

### 作者回覆（用於回饋）

*由程式碼作者填寫：*

- [ ] 已在 commit 中實作修復：`abc123`
- [ ] 修復狀態：已完成 / 進行中 / 需要討論
- [ ] 問題或疑慮：（描述）

---

## 發現統計（供審查者使用）

在審查多個發現時，請追蹤：

- **發現問題總數：** X
- **緊急 (Critical)：** X
- **高 (High)：** X
- **中 (Medium)：** X
- **低 (Low)：** X

**建議：** ✅ 核准 / ⚠️ 要求變更 / 🔄 需要討論

**整體程式碼品質：** 1-5 顆星
