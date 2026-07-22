# AI 程式碼生成之 Clean Code 規則

這些規則旨在引導程式碼生成過程，以產出具備可維護性且專業品質的程式碼。

## 有意義的命名
- 使用能揭示意圖的名稱，解釋某個事物存在的目的
- 避免誤導性資訊與無意義的區分（例如：`data`、`info`、`manager`）
- 使用易於發音且易於搜尋的名稱
- 類別名稱：使用名詞（例如：`UserAccount`、`PaymentProcessor`）
- 方法名稱：使用動詞（例如：`calculateTotal`、`sendEmail`）
- 避免心智映射與編碼（如：匈牙利命名法、前綴）

## 函式
- 保持函式精簡（理想情況下 < 20 行）
- 只做一件事 —— 單一職責原則 (Single Responsibility Principle)
- 每個函式僅包含一個抽象層級
- 限制參數數量：理想為 0-2 個，最多 3 個，避免使用旗標參數 (flag arguments)
- 不產生副作用 —— 函式應該執行其名稱所描述的操作
- 將指令（改變狀態）與查詢（回傳資訊）分開
- 優先使用異常 (exceptions) 而非錯誤碼 (error codes)

## 註解
- 程式碼應具備自我解釋性 —— 盡可能避免使用註解
- 良好的註解：法律資訊、警告、TODO、公開 API 文件
- 錯誤的註解：冗餘、誤導或用來解釋糟糕的程式碼
- 切勿將程式碼註解掉 —— 直接刪除它（版本控制會保留歷史紀錄）
- 如果你需要寫註解，請考慮進行程式碼重構 (refactoring)

## 格式化
- 保持檔案精簡且專注於單一目標
- 垂直格式化：將相關概念靠在一起，使用空行分隔不同概念
- 水平格式化：限制行長（80-120 字元）
- 使用一致的縮排與團隊風格
- 將相關函式分組在一起

## 物件與資料結構
- 物件：將資料隱藏在抽象層後，透過方法揭露行為
- 資料結構：揭露資料，具有極少的行為
- Demeter 定律：只與直接的朋友交談，避免使用 `a.getB().getC().doSomething()`
- 不要盲目地透過 getter/setter 揭露內部結構

## 錯誤處理
- 使用異常（exceptions），而非回傳碼或錯誤旗標
- 當程式碼可能失敗時，優先撰寫 `try-catch-finally`
- 在異常訊息中提供上下文（context）
- 不要回傳 `null` — 改為回傳空集合或使用 Optional/Maybe
- 不要將 `null` 作為參數傳遞

## 類別
- 小型類別：以職責而非行數來衡量
- 單一職責原則（Single Responsibility Principle）：一個類別只有一個改變的原因
- 高內聚（High cohesion）：類別變數被多個方法使用
- 低耦合（Low coupling）：類別之間的依賴性最小化
- 開閉原則（Open/Closed Principle）：對擴充開放，對修改封閉

## 單元測試
- 快速、獨立、可重複、自我驗證、及時（F.I.R.S.T.）
- 每個測試僅包含一個斷言（assert）或一個概念
- 測試程式碼的品質應等同於正式環境程式碼的品質
- 使用可讀且能描述測試內容的測試名稱
- 使用 Arrange-Act-Assert 模式

## 程式碼品質原則
- **DRY (Don't Repeat Yourself)**：不要重複自己
- **YAGNI (You Aren't Gonna Need It)**：不要為了假設性的未來而開發
- **KISS (Keep It Simple)**：保持簡單，避免不必要的複雜性
- **童子軍守則 (Boy Scout Rule)**：離開時讓程式碼比你發現時更乾淨

## 應避免的程式碼壞味道 (Code Smells)
- 過長的函式或類別
- 重複的程式碼
- 死碼（Dead code，如未使用的變數、函式、參數）
- 特徵羨慕 (Feature envy，方法對其他類別更感興趣)
- 不當親密 (Inappropriate intimacy，類別之間過度了解彼此)
- 過長的參數列表
- 基本型別執著 (Primitive obsession，過度使用基本型別而非小型物件)
- Switch/case 語句（考慮使用多型 polymorphism）
- 暫時性欄位 (Temporary fields，僅在某些情況下使用的類別變數)

## Concurrency
- 將並行程式碼與其他程式碼分開
- 限制同步/鎖定資料的範圍
- 使用執行緒安全（thread-safe）的集合
- 保持同步區塊精簡
- 了解你的執行模型與原語（primitives）

## System Design
- 將建構與使用分離（相依性注入）
- 對於複雜的物件建立，使用工廠（factories）或建構器（builders）
- 針對介面程式設計，而非針對實作
- 偏好組合（composition）而非繼承（inheritance）
- 當設計模式能簡化流程時才使用，而非為了炫技

## Refactoring
- 持續進行重構，而非大規模批次處理
- 在重構前後務必確保測試通過
- 小步進行：一次只做一個變動
- 常見的重構：Extract Method、Rename、Move、Inline

## Documentation
- 自我文件化程式碼 > 註解 > 外部文件
- 公開 API 需要清晰的文件
- 在文件中包含範例
- 將文件與程式碼保持接近（理想情況下直接寫在程式碼中）

---

**核心哲學**：程式碼被閱讀的次數比被撰寫的次數多 10 倍。針對可讀性與可維護性進行優化，而非針對聰明程度。

---
**最後更新日期**：2026 年 4 月 9 日
