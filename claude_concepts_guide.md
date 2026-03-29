<picture>
  <source media="(prefers-color-scheme: dark)" srcset="resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="resources/logos/claude-howto-logo.svg">
</picture>

# Claude 概念完整指南

涵蓋 Slash Commands、Subagents、Memory、MCP Protocol 和 Agent Skills 的全面參考指南，附表格、圖表和實際範例。

---

## 目錄

1. [Slash Commands](#slash-commands)
2. [Subagents](#subagents)
3. [Memory](#memory)
4. [MCP Protocol](#mcp-protocol)
5. [Agent Skills](#agent-skills)
6. [Plugins](#plugins)
7. [Hooks](#hooks)
8. [Checkpoints 與回溯](#checkpoints-與回溯)
9. [進階功能](#進階功能)
10. [比較與整合](#比較與整合)

---

## Slash Commands

### 概覽

Slash commands 是以 Markdown 檔案儲存的使用者觸發快捷方式，Claude Code 可以執行。它們讓團隊能夠標準化常用的提示和工作流程。

### 架構

```mermaid
graph TD
    A["使用者輸入：/command-name"] -->|觸發| B["搜尋 .claude/commands/"]
    B -->|找到| C["command-name.md"]
    C -->|載入| D["Markdown 內容"]
    D -->|執行| E["Claude 處理提示"]
    E -->|返回| F["上下文中的結果"]
```

### 檔案結構

```mermaid
graph LR
    A["專案根目錄"] -->|包含| B[".claude/commands/"]
    B -->|包含| C["optimize.md"]
    B -->|包含| D["test.md"]
    B -->|包含| E["docs/"]
    E -->|包含| F["generate-api-docs.md"]
    E -->|包含| G["generate-readme.md"]
```

### 指令組織表

| 位置 | 範圍 | 可用性 | 使用場景 | Git 追蹤 |
|----------|-------|--------------|----------|-------------|
| `.claude/commands/` | 專案特定 | 團隊成員 | 團隊工作流程、共享標準 | ✅ 是 |
| `~/.claude/commands/` | 個人 | 個別使用者 | 跨專案的個人快捷方式 | ❌ 否 |
| 子目錄 | 命名空間 | 基於父目錄 | 按類別組織 | ✅ 是 |

### 功能與能力

| 功能 | 範例 | 支援 |
|---------|---------|-----------|
| Shell 腳本執行 | `bash scripts/deploy.sh` | ✅ 是 |
| 檔案引用 | `@path/to/file.js` | ✅ 是 |
| Bash 整合 | `$(git log --oneline)` | ✅ 是 |
| 引數 | `/pr --verbose` | ✅ 是 |
| MCP 指令 | `/mcp__github__list_prs` | ✅ 是 |

### 實際範例

#### 範例 1：程式碼最佳化指令

**檔案：** `.claude/commands/optimize.md`

```markdown
---
name: Code Optimization
description: Analyze code for performance issues and suggest optimizations
tags: performance, analysis
---

# Code Optimization

Review the provided code for the following issues in order of priority:

1. **Performance bottlenecks** - identify O(n²) operations, inefficient loops
2. **Memory leaks** - find unreleased resources, circular references
3. **Algorithm improvements** - suggest better algorithms or data structures
4. **Caching opportunities** - identify repeated computations
5. **Concurrency issues** - find race conditions or threading problems

Format your response with:
- Issue severity (Critical/High/Medium/Low)
- Location in code
- Explanation
- Recommended fix with code example
```

**使用方式：**
```bash
# 使用者在 Claude Code 中輸入
/optimize

# Claude 載入提示並等待程式碼輸入
```

#### 範例 2：Pull Request 輔助指令

**檔案：** `.claude/commands/pr.md`

```markdown
---
name: Prepare Pull Request
description: Clean up code, stage changes, and prepare a pull request
tags: git, workflow
---

# Pull Request Preparation Checklist

Before creating a PR, execute these steps:

1. Run linting: `prettier --write .`
2. Run tests: `npm test`
3. Review git diff: `git diff HEAD`
4. Stage changes: `git add .`
5. Create commit message following conventional commits:
   - `fix:` for bug fixes
   - `feat:` for new features
   - `docs:` for documentation
   - `refactor:` for code restructuring
   - `test:` for test additions
   - `chore:` for maintenance

6. Generate PR summary including:
   - What changed
   - Why it changed
   - Testing performed
   - Potential impacts
```

**使用方式：**
```bash
/pr

# Claude 執行清單並準備 PR
```

#### 範例 3：階層式文件生成器

**檔案：** `.claude/commands/docs/generate-api-docs.md`

```markdown
---
name: Generate API Documentation
description: Create comprehensive API documentation from source code
tags: documentation, api
---

# API Documentation Generator

Generate API documentation by:

1. Scanning all files in `/src/api/`
2. Extracting function signatures and JSDoc comments
3. Organizing by endpoint/module
4. Creating markdown with examples
5. Including request/response schemas
6. Adding error documentation

Output format:
- Markdown file in `/docs/api.md`
- Include curl examples for all endpoints
- Add TypeScript types
```

### 指令生命週期圖

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant FS as 檔案系統
    participant CLI as Shell/Bash

    User->>Claude: 輸入 /optimize
    Claude->>FS: 搜尋 .claude/commands/
    FS-->>Claude: 返回 optimize.md
    Claude->>Claude: 載入 Markdown 內容
    Claude->>User: 顯示提示上下文
    User->>Claude: 提供要分析的程式碼
    Claude->>CLI: （可能執行腳本）
    CLI-->>Claude: 結果
    Claude->>User: 返回分析
```

### 最佳實踐

| ✅ 應該做 | ❌ 不應該做 |
|------|---------|
| 使用清晰、行動導向的名稱 | 為一次性任務建立指令 |
| 在描述中記錄觸發詞 | 在指令中建立複雜邏輯 |
| 讓指令專注於單一任務 | 建立冗餘指令 |
| 對專案指令進行版本控制 | 硬編碼敏感資訊 |
| 在子目錄中組織 | 建立過長的指令列表 |
| 使用簡單易讀的提示 | 使用縮寫或隱晦的措辭 |

---

## Subagents

### 概覽

Subagents 是具有獨立上下文視窗和自訂系統提示的專門化 AI 助理。它們在保持清晰關注點分離的同時，支援委派任務執行。

### 架構圖

```mermaid
graph TB
    User["👤 使用者"]
    Main["🎯 主 Agent<br/>（協調者）"]
    Reviewer["🔍 程式碼審查員<br/>Subagent"]
    Tester["✅ 測試工程師<br/>Subagent"]
    Docs["📝 文件<br/>Subagent"]

    User -->|詢問| Main
    Main -->|委派| Reviewer
    Main -->|委派| Tester
    Main -->|委派| Docs
    Reviewer -->|返回結果| Main
    Tester -->|返回結果| Main
    Docs -->|返回結果| Main
    Main -->|整合| User
```

### Subagent 生命週期

```mermaid
sequenceDiagram
    participant User
    participant MainAgent as 主 Agent
    participant CodeReviewer as 程式碼審查員<br/>Subagent
    participant Context as 獨立<br/>上下文視窗

    User->>MainAgent: "Build new auth feature"
    MainAgent->>MainAgent: 分析任務
    MainAgent->>CodeReviewer: "Review this code"
    CodeReviewer->>Context: 初始化乾淨上下文
    Context->>CodeReviewer: 載入審查員指令
    CodeReviewer->>CodeReviewer: 執行審查
    CodeReviewer-->>MainAgent: 返回發現
    MainAgent->>MainAgent: 整合結果
    MainAgent-->>User: 提供整合回應
```

### Subagent 配置表

| 配置 | 類型 | 目的 | 範例 |
|---------------|------|---------|---------|
| `name` | 字串 | Agent 識別符 | `code-reviewer` |
| `description` | 字串 | 目的與觸發詞 | `Comprehensive code quality analysis` |
| `tools` | 列表/字串 | 允許的能力 | `read, grep, diff, lint_runner` |
| `system_prompt` | Markdown | 行為指令 | 自訂指南 |

### 工具存取階層

```mermaid
graph TD
    A["Subagent 配置"] -->|選項 1| B["從主執行緒<br/>繼承所有工具"]
    A -->|選項 2| C["指定個別工具"]
    B -->|包含| B1["檔案操作"]
    B -->|包含| B2["Shell 指令"]
    B -->|包含| B3["MCP 工具"]
    C -->|明確列表| C1["read, grep, diff"]
    C -->|明確列表| C2["Bash(npm:*), Bash(test:*)"]
```

### 實際範例

#### 範例 1：完整 Subagent 設定

**檔案：** `.claude/agents/code-reviewer.md`

```yaml
---
name: code-reviewer
description: Comprehensive code quality and maintainability analysis
tools: read, grep, diff, lint_runner
---

# Code Reviewer Agent

You are an expert code reviewer specializing in:
- Performance optimization
- Security vulnerabilities
- Code maintainability
- Testing coverage
- Design patterns

## Review Priorities (in order)

1. **Security Issues** - Authentication, authorization, data exposure
2. **Performance Problems** - O(n²) operations, memory leaks, inefficient queries
3. **Code Quality** - Readability, naming, documentation
4. **Test Coverage** - Missing tests, edge cases
5. **Design Patterns** - SOLID principles, architecture

## Review Output Format

For each issue:
- **Severity**: Critical / High / Medium / Low
- **Category**: Security / Performance / Quality / Testing / Design
- **Location**: File path and line number
- **Issue Description**: What's wrong and why
- **Suggested Fix**: Code example
- **Impact**: How this affects the system

## Example Review

### Issue: N+1 Query Problem
- **Severity**: High
- **Category**: Performance
- **Location**: src/user-service.ts:45
- **Issue**: Loop executes database query in each iteration
- **Fix**: Use JOIN or batch query
```

**檔案：** `.claude/agents/test-engineer.md`

```yaml
---
name: test-engineer
description: Test strategy, coverage analysis, and automated testing
tools: read, write, bash, grep
---

# Test Engineer Agent

You are expert at:
- Writing comprehensive test suites
- Ensuring high code coverage (>80%)
- Testing edge cases and error scenarios
- Performance benchmarking
- Integration testing

## Testing Strategy

1. **Unit Tests** - Individual functions/methods
2. **Integration Tests** - Component interactions
3. **End-to-End Tests** - Complete workflows
4. **Edge Cases** - Boundary conditions
5. **Error Scenarios** - Failure handling

## Test Output Requirements

- Use Jest for JavaScript/TypeScript
- Include setup/teardown for each test
- Mock external dependencies
- Document test purpose
- Include performance assertions when relevant

## Coverage Requirements

- Minimum 80% code coverage
- 100% for critical paths
- Report missing coverage areas
```

**檔案：** `.claude/agents/documentation-writer.md`

```yaml
---
name: documentation-writer
description: Technical documentation, API docs, and user guides
tools: read, write, grep
---

# Documentation Writer Agent

You create:
- API documentation with examples
- User guides and tutorials
- Architecture documentation
- Changelog entries
- Code comment improvements

## Documentation Standards

1. **Clarity** - Use simple, clear language
2. **Examples** - Include practical code examples
3. **Completeness** - Cover all parameters and returns
4. **Structure** - Use consistent formatting
5. **Accuracy** - Verify against actual code

## Documentation Sections

### For APIs
- Description
- Parameters (with types)
- Returns (with types)
- Throws (possible errors)
- Examples (curl, JavaScript, Python)
- Related endpoints

### For Features
- Overview
- Prerequisites
- Step-by-step instructions
- Expected outcomes
- Troubleshooting
- Related topics
```

#### 範例 2：Subagent 委派實作

```markdown
# 情境：建立付款功能

## 使用者請求
"Build a secure payment processing feature that integrates with Stripe"

## 主 Agent 流程

1. **規劃階段**
   - 理解需求
   - 確定所需任務
   - 規劃架構

2. **委派給程式碼審查員 Subagent**
   - 任務：「Review the payment processing implementation for security」
   - 上下文：Auth、API 金鑰、token 處理
   - 審查：SQL 注入、金鑰洩露、HTTPS 強制

3. **委派給測試工程師 Subagent**
   - 任務：「Create comprehensive tests for payment flows」
   - 上下文：成功情境、失敗、邊界情況
   - 建立測試：有效付款、被拒絕的卡、網路失敗、webhooks

4. **委派給文件寫作員 Subagent**
   - 任務：「Document the payment API endpoints」
   - 上下文：請求/回應 schemas
   - 產出：附 curl 範例和錯誤碼的 API 文件

5. **整合**
   - 主 agent 收集所有輸出
   - 整合發現
   - 向使用者返回完整解決方案
```

#### 範例 3：工具權限範圍

**限制性設定——限制於特定指令**

```yaml
---
name: secure-reviewer
description: Security-focused code review with minimal permissions
tools: read, grep
---

# Secure Code Reviewer

Reviews code for security vulnerabilities only.

This agent:
- ✅ Reads files to analyze
- ✅ Searches for patterns
- ❌ Cannot execute code
- ❌ Cannot modify files
- ❌ Cannot run tests

This ensures the reviewer doesn't accidentally break anything.
```

**延伸設定——所有工具用於實作**

```yaml
---
name: implementation-agent
description: Full implementation capabilities for feature development
tools: read, write, bash, grep, edit, glob
---

# Implementation Agent

Builds features from specifications.

This agent:
- ✅ Reads specifications
- ✅ Writes new code files
- ✅ Runs build commands
- ✅ Searches codebase
- ✅ Edits existing files
- ✅ Finds files matching patterns

Full capabilities for independent feature development.
```

### Subagent 上下文管理

```mermaid
graph TB
    A["主 Agent 上下文<br/>50,000 tokens"]
    B["Subagent 1 上下文<br/>20,000 tokens"]
    C["Subagent 2 上下文<br/>20,000 tokens"]
    D["Subagent 3 上下文<br/>20,000 tokens"]

    A -->|乾淨起點| B
    A -->|乾淨起點| C
    A -->|乾淨起點| D

    B -->|僅結果| A
    C -->|僅結果| A
    D -->|僅結果| A

    style A fill:#e1f5ff
    style B fill:#fff9c4
    style C fill:#fff9c4
    style D fill:#fff9c4
```

### 何時使用 Subagents

| 情境 | 使用 Subagent | 原因 |
|----------|--------------|-----|
| 有許多步驟的複雜功能 | ✅ 是 | 分離關注點，防止上下文污染 |
| 快速程式碼審查 | ❌ 否 | 不必要的負擔 |
| 平行任務執行 | ✅ 是 | 每個 subagent 有自己的上下文 |
| 需要專門專業知識 | ✅ 是 | 自訂系統提示 |
| 長時間執行的分析 | ✅ 是 | 防止主上下文耗盡 |
| 單一任務 | ❌ 否 | 不必要地增加延遲 |

### Agent 團隊

Agent 團隊協調多個 agents 共同處理相關任務。與一次委派給一個 subagent 不同，Agent 團隊讓主 agent 可以編排一組 agents，它們協作、共享中間結果，並朝共同目標工作。這對於大規模任務（如全端功能開發，前端 agent、後端 agent 和測試 agent 平行工作）非常有用。

---

## Memory

### 概覽

Memory 讓 Claude 可以在會話和對話之間保留上下文。它存在兩種形式：claude.ai 中的自動合成，以及 Claude Code 中基於檔案系統的 CLAUDE.md。

### Memory 架構

```mermaid
graph TB
    A["Claude 會話"]
    B["使用者輸入"]
    C["Memory 系統"]
    D["Memory 儲存"]

    B -->|使用者提供資訊| C
    C -->|每 24 小時合成| D
    D -->|自動載入| A
    A -->|使用上下文| C
```

### Claude Code 中的 Memory 階層（7 個層級）

Claude Code 從 7 個層級載入 memory，按優先順序從高到低列出：

```mermaid
graph TD
    A["1. 受管政策<br/>企業管理員政策"] --> B["2. 專案 Memory<br/>./CLAUDE.md"]
    B --> C["3. 專案規則<br/>.claude/rules/*.md"]
    C --> D["4. 使用者 Memory<br/>~/.claude/CLAUDE.md"]
    D --> E["5. 使用者規則<br/>~/.claude/rules/*.md"]
    E --> F["6. 本地 Memory<br/>.claude/local/CLAUDE.md"]
    F --> G["7. 自動 Memory<br/>自動擷取的偏好"]

    style A fill:#fce4ec,stroke:#333,color:#333
    style B fill:#e1f5fe,stroke:#333,color:#333
    style C fill:#e1f5fe,stroke:#333,color:#333
    style D fill:#f3e5f5,stroke:#333,color:#333
    style E fill:#f3e5f5,stroke:#333,color:#333
    style F fill:#e8f5e9,stroke:#333,color:#333
    style G fill:#fff3e0,stroke:#333,color:#333
```

### Memory 位置表

| 層級 | 位置 | 範圍 | 優先順序 | 共享 | 最適用於 |
|------|----------|-------|----------|--------|----------|
| 1. 受管政策 | 企業管理員 | 組織 | 最高 | 所有組織使用者 | 合規、安全政策 |
| 2. 專案 | `./CLAUDE.md` | 專案 | 高 | 團隊（Git）| 團隊標準、架構 |
| 3. 專案規則 | `.claude/rules/*.md` | 專案 | 高 | 團隊（Git）| 模組化專案慣例 |
| 4. 使用者 | `~/.claude/CLAUDE.md` | 個人 | 中 | 個人 | 個人偏好 |
| 5. 使用者規則 | `~/.claude/rules/*.md` | 個人 | 中 | 個人 | 個人規則模組 |
| 6. 本地 | `.claude/local/CLAUDE.md` | 本地 | 低 | 不共享 | 機器特定設定 |
| 7. 自動 Memory | 自動 | 會話 | 最低 | 個人 | 學到的偏好、模式 |

### 自動 Memory

自動 Memory 在會話期間自動擷取使用者偏好和觀察到的模式。Claude 從你的互動中學習並記住：

- 程式碼風格偏好
- 你常做的修正
- 框架和工具選擇
- 溝通風格偏好

自動 Memory 在背景工作，不需要手動配置。

### Memory 更新生命週期

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Editor as 檔案系統
    participant Memory as CLAUDE.md

    User->>Claude: "Remember: use async/await"
    Claude->>User: "Which memory file?"
    User->>Claude: "Project memory"
    Claude->>Editor: 開啟 ~/.claude/settings.json
    Claude->>Memory: 寫入 ./CLAUDE.md
    Memory-->>Claude: 檔案已儲存
    Claude->>Claude: 載入更新後的 memory
    Claude-->>User: "Memory saved!"
```

### 實際範例

#### 範例 1：專案 Memory 結構

**檔案：** `./CLAUDE.md`

```markdown
# Project Configuration

## Project Overview
- **Name**: E-commerce Platform
- **Tech Stack**: Node.js, PostgreSQL, React 18, Docker
- **Team Size**: 5 developers
- **Deadline**: Q4 2025

## Architecture
@docs/architecture.md
@docs/api-standards.md
@docs/database-schema.md

## Development Standards

### Code Style
- Use Prettier for formatting
- Use ESLint with airbnb config
- Maximum line length: 100 characters
- Use 2-space indentation

### Naming Conventions
- **Files**: kebab-case (user-controller.js)
- **Classes**: PascalCase (UserService)
- **Functions/Variables**: camelCase (getUserById)
- **Constants**: UPPER_SNAKE_CASE (API_BASE_URL)
- **Database Tables**: snake_case (user_accounts)

### Git Workflow
- Branch names: `feature/description` or `fix/description`
- Commit messages: Follow conventional commits
- PR required before merge
- All CI/CD checks must pass
- Minimum 1 approval required

### Testing Requirements
- Minimum 80% code coverage
- All critical paths must have tests
- Use Jest for unit tests
- Use Cypress for E2E tests
- Test filenames: `*.test.ts` or `*.spec.ts`

### API Standards
- RESTful endpoints only
- JSON request/response
- Use HTTP status codes correctly
- Version API endpoints: `/api/v1/`
- Document all endpoints with examples

### Database
- Use migrations for schema changes
- Never hardcode credentials
- Use connection pooling
- Enable query logging in development
- Regular backups required

### Deployment
- Docker-based deployment
- Kubernetes orchestration
- Blue-green deployment strategy
- Automatic rollback on failure
- Database migrations run before deploy

## Common Commands

| Command | Purpose |
|---------|---------|
| `npm run dev` | Start development server |
| `npm test` | Run test suite |
| `npm run lint` | Check code style |
| `npm run build` | Build for production |
| `npm run migrate` | Run database migrations |

## Team Contacts
- Tech Lead: Sarah Chen (@sarah.chen)
- Product Manager: Mike Johnson (@mike.j)
- DevOps: Alex Kim (@alex.k)

## Known Issues & Workarounds
- PostgreSQL connection pooling limited to 20 during peak hours
- Workaround: Implement query queuing
- Safari 14 compatibility issues with async generators
- Workaround: Use Babel transpiler

## Related Projects
- Analytics Dashboard: `/projects/analytics`
- Mobile App: `/projects/mobile`
- Admin Panel: `/projects/admin`
```

#### 範例 2：目錄特定 Memory

**檔案：** `./src/api/CLAUDE.md`

~~~~markdown
# API Module Standards

This file overrides root CLAUDE.md for everything in /src/api/

## API-Specific Standards

### Request Validation
- Use Zod for schema validation
- Always validate input
- Return 400 with validation errors
- Include field-level error details

### Authentication
- All endpoints require JWT token
- Token in Authorization header
- Token expires after 24 hours
- Implement refresh token mechanism

### Response Format

All responses must follow this structure:

```json
{
  "success": true,
  "data": { /* actual data */ },
  "timestamp": "2025-11-06T10:30:00Z",
  "version": "1.0"
}
```

### Error responses:
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "User message",
    "details": { /* field errors */ }
  },
  "timestamp": "2025-11-06T10:30:00Z"
}
```

### Pagination
- Use cursor-based pagination (not offset)
- Include `hasMore` boolean
- Limit max page size to 100
- Default page size: 20

### Rate Limiting
- 1000 requests per hour for authenticated users
- 100 requests per hour for public endpoints
- Return 429 when exceeded
- Include retry-after header

### Caching
- Use Redis for session caching
- Cache duration: 5 minutes default
- Invalidate on write operations
- Tag cache keys with resource type
~~~~

#### 範例 3：個人 Memory

**檔案：** `~/.claude/CLAUDE.md`

~~~~markdown
# My Development Preferences

## About Me
- **Experience Level**: 8 years full-stack development
- **Preferred Languages**: TypeScript, Python
- **Communication Style**: Direct, with examples
- **Learning Style**: Visual diagrams with code

## Code Preferences

### Error Handling
I prefer explicit error handling with try-catch blocks and meaningful error messages.
Avoid generic errors. Always log errors for debugging.

### Comments
Use comments for WHY, not WHAT. Code should be self-documenting.
Comments should explain business logic or non-obvious decisions.

### Testing
I prefer TDD (test-driven development).
Write tests first, then implementation.
Focus on behavior, not implementation details.

### Architecture
I prefer modular, loosely-coupled design.
Use dependency injection for testability.
Separate concerns (Controllers, Services, Repositories).

## Debugging Preferences
- Use console.log with prefix: `[DEBUG]`
- Include context: function name, relevant variables
- Use stack traces when available
- Always include timestamps in logs

## Communication
- Explain complex concepts with diagrams
- Show concrete examples before explaining theory
- Include before/after code snippets
- Summarize key points at the end

## Project Organization
I organize my projects as:
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

## Tooling
- **IDE**: VS Code with vim keybindings
- **Terminal**: Zsh with Oh-My-Zsh
- **Format**: Prettier (100 char line length)
- **Linter**: ESLint with airbnb config
- **Test Framework**: Jest with React Testing Library
~~~~

#### 範例 4：會話期間的 Memory 更新

**會話互動：**

```markdown
User: Remember that I prefer using React hooks instead of class components
     for all new components.

Claude: I'm adding that to your memory. Which memory file should this go in?
        1. Project memory (./CLAUDE.md)
        2. Personal memory (~/.claude/CLAUDE.md)

User: Project memory

Claude: ✅ Memory saved!

Added to ./CLAUDE.md:
---

### Component Development
- Use functional components with React Hooks
- Prefer hooks over class components
- Custom hooks for reusable logic
- Use useCallback for event handlers
- Use useMemo for expensive computations
```

### Claude Web/Desktop 中的 Memory

#### Memory 合成時間軸

```mermaid
graph LR
    A["第 1 天：使用者<br/>對話"] -->|24 小時| B["第 2 天：Memory<br/>合成"]
    B -->|自動| C["Memory 已更新<br/>已摘要"]
    C -->|載入至| D["第 2-N 天：<br/>新對話"]
    D -->|新增至| E["Memory"]
    E -->|24 小時後| F["Memory 已更新"]
```

**Memory 摘要範例：**

```markdown
## Claude's Memory of User

### Professional Background
- Senior full-stack developer with 8 years experience
- Focus on TypeScript/Node.js backends and React frontends
- Active open source contributor
- Interested in AI and machine learning

### Project Context
- Currently building e-commerce platform
- Tech stack: Node.js, PostgreSQL, React 18, Docker
- Working with team of 5 developers
- Using CI/CD and blue-green deployments

### Communication Preferences
- Prefers direct, concise explanations
- Likes visual diagrams and examples
- Appreciates code snippets
- Explains business logic in comments

### Current Goals
- Improve API performance
- Increase test coverage to 90%
- Implement caching strategy
- Document architecture
```

### Memory 功能比較

| 功能 | Claude Web/Desktop | Claude Code（CLAUDE.md）|
|---------|-------------------|------------------------|
| 自動合成 | ✅ 每 24 小時 | ❌ 手動 |
| 跨專案 | ✅ 共享 | ❌ 專案特定 |
| 團隊存取 | ✅ 共享專案 | ✅ Git 追蹤 |
| 可搜尋 | ✅ 內建 | ✅ 透過 `/memory` |
| 可編輯 | ✅ 對話中 | ✅ 直接編輯檔案 |
| 匯入/匯出 | ✅ 是 | ✅ 複製/貼上 |
| 持久性 | ✅ 24 小時以上 | ✅ 無限期 |

---

## MCP Protocol

### 概覽

MCP（Model Context Protocol）是 Claude 存取外部工具、API 和即時資料來源的標準化方式。與 Memory 不同，MCP 提供對變化資料的即時存取。

### MCP 架構

```mermaid
graph TB
    A["Claude"]
    B["MCP Server"]
    C["外部服務"]

    A -->|請求：list_issues| B
    B -->|查詢| C
    C -->|資料| B
    B -->|回應| A

    A -->|請求：create_issue| B
    B -->|動作| C
    C -->|結果| B
    B -->|回應| A
```

### MCP 生態系統

```mermaid
graph TB
    A["Claude"] -->|MCP| B["Filesystem<br/>MCP Server"]
    A -->|MCP| C["GitHub<br/>MCP Server"]
    A -->|MCP| D["Database<br/>MCP Server"]
    A -->|MCP| E["Slack<br/>MCP Server"]
    A -->|MCP| F["Google Docs<br/>MCP Server"]

    B -->|檔案 I/O| G["本地檔案"]
    C -->|API| H["GitHub Repos"]
    D -->|查詢| I["PostgreSQL/MySQL"]
    E -->|訊息| J["Slack Workspace"]
    F -->|文件| K["Google Drive"]
```

### MCP 設定流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Config as 配置檔案
    participant Service as 外部服務

    User->>Claude: 輸入 /mcp
    Claude->>Claude: 列出可用的 MCP servers
    Claude->>User: 顯示選項
    User->>Claude: 選擇 GitHub MCP
    Claude->>Config: 更新配置
    Config->>Claude: 啟用連線
    Claude->>Service: 測試連線
    Service-->>Claude: 驗證成功
    Claude->>User: ✅ MCP 已連線！
```

### 可用的 MCP Servers 表

| MCP Server | 目的 | 常見工具 | 驗證 | 即時 |
|------------|---------|--------------|------|-----------|
| **Filesystem** | 檔案操作 | read, write, delete | OS 權限 | ✅ 是 |
| **GitHub** | 儲存庫管理 | list_prs, create_issue, push | OAuth | ✅ 是 |
| **Slack** | 團隊溝通 | send_message, list_channels | Token | ✅ 是 |
| **Database** | SQL 查詢 | query, insert, update | 憑證 | ✅ 是 |
| **Google Docs** | 文件存取 | read, write, share | OAuth | ✅ 是 |
| **Asana** | 專案管理 | create_task, update_status | API Key | ✅ 是 |
| **Stripe** | 付款資料 | list_charges, create_invoice | API Key | ✅ 是 |
| **Memory** | 持久化 memory | store, retrieve, delete | 本地 | ❌ 否 |

### 實際範例

#### 範例 1：GitHub MCP 配置

**檔案：** `.mcp.json`（專案範圍）或 `~/.claude.json`（使用者範圍）

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**可用的 GitHub MCP 工具：**

~~~~markdown
# GitHub MCP Tools

## Pull Request Management
- `list_prs` - List all PRs in repository
- `get_pr` - Get PR details including diff
- `create_pr` - Create new PR
- `update_pr` - Update PR description/title
- `merge_pr` - Merge PR to main branch
- `review_pr` - Add review comments

Example request:
```
/mcp__github__get_pr 456

# Returns:
Title: Add dark mode support
Author: @alice
Description: Implements dark theme using CSS variables
Status: OPEN
Reviewers: @bob, @charlie
```

## Issue Management
- `list_issues` - List all issues
- `get_issue` - Get issue details
- `create_issue` - Create new issue
- `close_issue` - Close issue
- `add_comment` - Add comment to issue

## Repository Information
- `get_repo_info` - Repository details
- `list_files` - File tree structure
- `get_file_content` - Read file contents
- `search_code` - Search across codebase

## Commit Operations
- `list_commits` - Commit history
- `get_commit` - Specific commit details
- `create_commit` - Create new commit
~~~~

#### 範例 2：Database MCP 設定

**配置：**

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-database"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    }
  }
}
```

**使用範例：**

```markdown
User: Fetch all users with more than 10 orders

Claude: I'll query your database to find that information.

# Using MCP database tool:
SELECT u.*, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id
HAVING COUNT(o.id) > 10
ORDER BY order_count DESC;

# Results:
- Alice: 15 orders
- Bob: 12 orders
- Charlie: 11 orders
```

#### 範例 3：多 MCP 工作流程

**情境：每日報告生成**

```markdown
# 使用多個 MCPs 的每日報告工作流程

## 設定
1. GitHub MCP - 取得 PR 指標
2. Database MCP - 查詢銷售資料
3. Slack MCP - 發布報告
4. Filesystem MCP - 儲存報告

## 工作流程

### 步驟 1：取得 GitHub 資料
/mcp__github__list_prs completed:true last:7days

輸出：
- 總 PR 數：42
- 平均合併時間：2.3 小時
- 審查周轉時間：1.1 小時

### 步驟 2：查詢資料庫
SELECT COUNT(*) as sales, SUM(amount) as revenue
FROM orders
WHERE created_at > NOW() - INTERVAL '1 day'

輸出：
- 銷售數：247
- 收入：$12,450

### 步驟 3：生成報告
將資料組合成 HTML 報告

### 步驟 4：儲存到 Filesystem
將 report.html 寫入 /reports/

### 步驟 5：發布到 Slack
將摘要發送到 #daily-reports 頻道

最終輸出：
✅ 報告已生成並發布
📊 本週合併了 47 個 PR
💰 每日銷售 $12,450
```

#### 範例 4：Filesystem MCP 操作

**配置：**

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    }
  }
}
```

**可用操作：**

| 操作 | 指令 | 目的 |
|-----------|---------|---------|
| 列出檔案 | `ls ~/projects` | 顯示目錄內容 |
| 讀取檔案 | `cat src/main.ts` | 讀取檔案內容 |
| 寫入檔案 | `create docs/api.md` | 建立新檔案 |
| 編輯檔案 | `edit src/app.ts` | 修改檔案 |
| 搜尋 | `grep "async function"` | 在檔案中搜尋 |
| 刪除 | `rm old-file.js` | 刪除檔案 |

### MCP 與 Memory 的決策矩陣

```mermaid
graph TD
    A["需要外部資料？"]
    A -->|否| B["使用 Memory"]
    A -->|是| C["資料是否頻繁變更？"]
    C -->|否/很少| B
    C -->|是/經常| D["使用 MCP"]

    B -->|儲存| E["偏好<br/>上下文<br/>歷史"]
    D -->|存取| F["即時 API<br/>資料庫<br/>服務"]

    style B fill:#e1f5ff
    style D fill:#fff9c4
```

### 請求/回應模式

```mermaid
sequenceDiagram
    participant App as Claude
    participant MCP as MCP Server
    participant DB as 資料庫

    App->>MCP: 請求："SELECT * FROM users WHERE id=1"
    MCP->>DB: 執行查詢
    DB-->>MCP: 結果集
    MCP-->>App: 返回解析後的資料
    App->>App: 處理結果
    App->>App: 繼續任務

    Note over MCP,DB: 即時存取<br/>無快取
```

---

## Agent Skills

### 概覽

Agent Skills 是以資料夾形式封裝的可重複使用、模型觸發的能力，包含指令、腳本和資源。Claude 自動偵測並使用相關的 skills。

### Skill 架構

```mermaid
graph TB
    A["Skill 目錄"]
    B["SKILL.md"]
    C["YAML Metadata"]
    D["指令"]
    E["腳本"]
    F["範本"]

    A --> B
    B --> C
    B --> D
    E --> A
    F --> A
```

### Skill 載入流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude
    participant System as 系統
    participant Skill as Skill

    User->>Claude: "Create Excel report"
    Claude->>System: 掃描可用的 skills
    System->>System: 載入 skill metadata
    Claude->>Claude: 將使用者請求與 skills 匹配
    Claude->>Skill: 載入 xlsx skill SKILL.md
    Skill-->>Claude: 返回指令 + 工具
    Claude->>Claude: 執行 skill
    Claude->>User: 生成 Excel 檔案
```

### Skill 類型與位置表

| 類型 | 位置 | 範圍 | 共享 | 同步 | 最適用於 |
|------|----------|-------|--------|------|----------|
| 預建 | 內建 | 全域 | 所有使用者 | 自動 | 文件建立 |
| 個人 | `~/.claude/skills/` | 個人 | 否 | 手動 | 個人自動化 |
| 專案 | `.claude/skills/` | 團隊 | 是 | Git | 團隊標準 |
| Plugin | 透過 plugin 安裝 | 各異 | 取決於 | 自動 | 整合功能 |

### 預建 Skills

```mermaid
graph TB
    A["預建 Skills"]
    B["PowerPoint (pptx)"]
    C["Excel (xlsx)"]
    D["Word (docx)"]
    E["PDF"]

    A --> B
    A --> C
    A --> D
    A --> E

    B --> B1["建立簡報"]
    B --> B2["編輯投影片"]
    C --> C1["建立試算表"]
    C --> C2["分析資料"]
    D --> D1["建立文件"]
    D --> D2["格式化文字"]
    E --> E1["生成 PDF"]
    E --> E2["填寫表單"]
```

### 套件 Skills

Claude Code 現在包含 5 個開箱即用的套件 skills：

| Skill | 指令 | 目的 |
|-------|---------|---------|
| **Simplify** | `/simplify` | 簡化複雜程式碼或說明 |
| **Batch** | `/batch` | 在多個檔案或項目上執行操作 |
| **Debug** | `/debug` | 附根本原因分析的系統性除錯 |
| **Loop** | `/loop` | 按計時器排程重複任務 |
| **Claude API** | `/claude-api` | 直接與 Anthropic API 互動 |

這些套件 skills 始終可用，不需要安裝或配置。

### 實際範例

#### 範例 1：自訂程式碼審查 Skill

**目錄結構：**

```
~/.claude/skills/code-review/
├── SKILL.md
├── templates/
│   ├── review-checklist.md
│   └── finding-template.md
└── scripts/
    ├── analyze-metrics.py
    └── compare-complexity.py
```

**檔案：** `~/.claude/skills/code-review/SKILL.md`

```yaml
---
name: Code Review Specialist
description: Comprehensive code review with security, performance, and quality analysis
version: "1.0.0"
tags:
  - code-review
  - quality
  - security
when_to_use: When users ask to review code, analyze code quality, or evaluate pull requests
effort: high
shell: bash
---

# Code Review Skill

This skill provides comprehensive code review capabilities focusing on:

1. **Security Analysis**
   - Authentication/authorization issues
   - Data exposure risks
   - Injection vulnerabilities
   - Cryptographic weaknesses
   - Sensitive data logging

2. **Performance Review**
   - Algorithm efficiency (Big O analysis)
   - Memory optimization
   - Database query optimization
   - Caching opportunities
   - Concurrency issues

3. **Code Quality**
   - SOLID principles
   - Design patterns
   - Naming conventions
   - Documentation
   - Test coverage

4. **Maintainability**
   - Code readability
   - Function size (should be < 50 lines)
   - Cyclomatic complexity
   - Dependency management
   - Type safety

## Review Template

For each piece of code reviewed, provide:

### Summary
- Overall quality assessment (1-5)
- Key findings count
- Recommended priority areas

### Critical Issues (if any)
- **Issue**: Clear description
- **Location**: File and line number
- **Impact**: Why this matters
- **Severity**: Critical/High/Medium
- **Fix**: Code example

### Findings by Category

#### Security (if issues found)
List security vulnerabilities with examples

#### Performance (if issues found)
List performance problems with complexity analysis

#### Quality (if issues found)
List code quality issues with refactoring suggestions

#### Maintainability (if issues found)
List maintainability problems with improvements
```
## Python Script: analyze-metrics.py

```python
#!/usr/bin/env python3
import re
import sys

def analyze_code_metrics(code):
    """Analyze code for common metrics."""

    # Count functions
    functions = len(re.findall(r'^def\s+\w+', code, re.MULTILINE))

    # Count classes
    classes = len(re.findall(r'^class\s+\w+', code, re.MULTILINE))

    # Average line length
    lines = code.split('\n')
    avg_length = sum(len(l) for l in lines) / len(lines) if lines else 0

    # Estimate complexity
    complexity = len(re.findall(r'\b(if|elif|else|for|while|and|or)\b', code))

    return {
        'functions': functions,
        'classes': classes,
        'avg_line_length': avg_length,
        'complexity_score': complexity
    }

if __name__ == '__main__':
    with open(sys.argv[1], 'r') as f:
        code = f.read()
    metrics = analyze_code_metrics(code)
    for key, value in metrics.items():
        print(f"{key}: {value:.2f}")
```

## Python Script: compare-complexity.py

```python
#!/usr/bin/env python3
"""
Compare cyclomatic complexity of code before and after changes.
Helps identify if refactoring actually simplifies code structure.
"""

import re
import sys
from typing import Dict, Tuple

class ComplexityAnalyzer:
    """Analyze code complexity metrics."""

    def __init__(self, code: str):
        self.code = code
        self.lines = code.split('\n')

    def calculate_cyclomatic_complexity(self) -> int:
        """
        Calculate cyclomatic complexity using McCabe's method.
        Count decision points: if, elif, else, for, while, except, and, or
        """
        complexity = 1  # Base complexity

        # Count decision points
        decision_patterns = [
            r'\bif\b',
            r'\belif\b',
            r'\bfor\b',
            r'\bwhile\b',
            r'\bexcept\b',
            r'\band\b(?!$)',
            r'\bor\b(?!$)'
        ]

        for pattern in decision_patterns:
            matches = re.findall(pattern, self.code)
            complexity += len(matches)

        return complexity

    def calculate_cognitive_complexity(self) -> int:
        """
        Calculate cognitive complexity - how hard is it to understand?
        Based on nesting depth and control flow.
        """
        cognitive = 0
        nesting_depth = 0

        for line in self.lines:
            # Track nesting depth
            if re.search(r'^\s*(if|for|while|def|class|try)\b', line):
                nesting_depth += 1
                cognitive += nesting_depth
            elif re.search(r'^\s*(elif|else|except|finally)\b', line):
                cognitive += nesting_depth

            # Reduce nesting when unindenting
            if line and not line[0].isspace():
                nesting_depth = 0

        return cognitive

    def calculate_maintainability_index(self) -> float:
        """
        Maintainability Index ranges from 0-100.
        > 85: Excellent
        > 65: Good
        > 50: Fair
        < 50: Poor
        """
        lines = len(self.lines)
        cyclomatic = self.calculate_cyclomatic_complexity()
        cognitive = self.calculate_cognitive_complexity()

        # Simplified MI calculation
        mi = 171 - 5.2 * (cyclomatic / lines) - 0.23 * (cognitive) - 16.2 * (lines / 1000)

        return max(0, min(100, mi))

    def get_complexity_report(self) -> Dict:
        """Generate comprehensive complexity report."""
        return {
            'cyclomatic_complexity': self.calculate_cyclomatic_complexity(),
            'cognitive_complexity': self.calculate_cognitive_complexity(),
            'maintainability_index': round(self.calculate_maintainability_index(), 2),
            'lines_of_code': len(self.lines),
            'avg_line_length': round(sum(len(l) for l in self.lines) / len(self.lines), 2) if self.lines else 0
        }


def compare_files(before_file: str, after_file: str) -> None:
    """Compare complexity metrics between two code versions."""

    with open(before_file, 'r') as f:
        before_code = f.read()

    with open(after_file, 'r') as f:
        after_code = f.read()

    before_analyzer = ComplexityAnalyzer(before_code)
    after_analyzer = ComplexityAnalyzer(after_code)

    before_metrics = before_analyzer.get_complexity_report()
    after_metrics = after_analyzer.get_complexity_report()

    print("=" * 60)
    print("CODE COMPLEXITY COMPARISON")
    print("=" * 60)

    print("\nBEFORE:")
    print(f"  Cyclomatic Complexity:    {before_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {before_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {before_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {before_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {before_metrics['avg_line_length']}")

    print("\nAFTER:")
    print(f"  Cyclomatic Complexity:    {after_metrics['cyclomatic_complexity']}")
    print(f"  Cognitive Complexity:     {after_metrics['cognitive_complexity']}")
    print(f"  Maintainability Index:    {after_metrics['maintainability_index']}")
    print(f"  Lines of Code:            {after_metrics['lines_of_code']}")
    print(f"  Avg Line Length:          {after_metrics['avg_line_length']}")

    print("\nCHANGES:")
    cyclomatic_change = after_metrics['cyclomatic_complexity'] - before_metrics['cyclomatic_complexity']
    cognitive_change = after_metrics['cognitive_complexity'] - before_metrics['cognitive_complexity']
    mi_change = after_metrics['maintainability_index'] - before_metrics['maintainability_index']
    loc_change = after_metrics['lines_of_code'] - before_metrics['lines_of_code']

    print(f"  Cyclomatic Complexity:    {cyclomatic_change:+d}")
    print(f"  Cognitive Complexity:     {cognitive_change:+d}")
    print(f"  Maintainability Index:    {mi_change:+.2f}")
    print(f"  Lines of Code:            {loc_change:+d}")

    print("\nASSESSMENT:")
    if mi_change > 0:
        print("  ✅ Code is MORE maintainable")
    elif mi_change < 0:
        print("  ⚠️  Code is LESS maintainable")
    else:
        print("  ➡️  Maintainability unchanged")

    if cyclomatic_change < 0:
        print("  ✅ Complexity DECREASED")
    elif cyclomatic_change > 0:
        print("  ⚠️  Complexity INCREASED")
    else:
        print("  ➡️  Complexity unchanged")

    print("=" * 60)


if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python compare-complexity.py <before_file> <after_file>")
        sys.exit(1)

    compare_files(sys.argv[1], sys.argv[2])
```

## Template: review-checklist.md

```markdown
# Code Review Checklist

## Security Checklist
- [ ] No hardcoded credentials or secrets
- [ ] Input validation on all user inputs
- [ ] SQL injection prevention (parameterized queries)
- [ ] CSRF protection on state-changing operations
- [ ] XSS prevention with proper escaping
- [ ] Authentication checks on protected endpoints
- [ ] Authorization checks on resources
- [ ] Secure password hashing (bcrypt, argon2)
- [ ] No sensitive data in logs
- [ ] HTTPS enforced

## Performance Checklist
- [ ] No N+1 queries
- [ ] Appropriate use of indexes
- [ ] Caching implemented where beneficial
- [ ] No blocking operations on main thread
- [ ] Async/await used correctly
- [ ] Large datasets paginated
- [ ] Database connections pooled
- [ ] Regular expressions optimized
- [ ] No unnecessary object creation
- [ ] Memory leaks prevented

## Quality Checklist
- [ ] Functions < 50 lines
- [ ] Clear variable naming
- [ ] No duplicate code
- [ ] Proper error handling
- [ ] Comments explain WHY, not WHAT
- [ ] No console.logs in production
- [ ] Type checking (TypeScript/JSDoc)
- [ ] SOLID principles followed
- [ ] Design patterns applied correctly
- [ ] Self-documenting code

## Testing Checklist
- [ ] Unit tests written
- [ ] Edge cases covered
- [ ] Error scenarios tested
- [ ] Integration tests present
- [ ] Coverage > 80%
- [ ] No flaky tests
- [ ] Mock external dependencies
- [ ] Clear test names
```

## Template: finding-template.md

~~~~markdown
# Code Review Finding Template

Use this template when documenting each issue found during code review.

---

## Issue: [TITLE]

### Severity
- [ ] Critical (blocks deployment)
- [ ] High (should fix before merge)
- [ ] Medium (should fix soon)
- [ ] Low (nice to have)

### Category
- [ ] Security
- [ ] Performance
- [ ] Code Quality
- [ ] Maintainability
- [ ] Testing
- [ ] Design Pattern
- [ ] Documentation

### Location
**File:** `src/components/UserCard.tsx`

**Lines:** 45-52

**Function/Method:** `renderUserDetails()`

### Issue Description

**What:** Describe what the issue is.

**Why it matters:** Explain the impact and why this needs to be fixed.

**Current behavior:** Show the problematic code or behavior.

**Expected behavior:** Describe what should happen instead.

### Code Example

#### Current (Problematic)

```typescript
// Shows the N+1 query problem
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // Query per user!
  renderUserPosts(posts);
});
```

#### Suggested Fix

```typescript
// Optimized with JOIN query
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### Impact Analysis

| Aspect | Impact | Severity |
|--------|--------|----------|
| Performance | 100+ queries for 20 users | High |
| User Experience | Slow page load | High |
| Scalability | Breaks at scale | Critical |
| Maintainability | Hard to debug | Medium |

### Related Issues

- Similar issue in `AdminUserList.tsx` line 120
- Related PR: #456
- Related issue: #789

### Additional Resources

- [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_problem)
- [Database Join Documentation](https://docs.example.com/joins)
- [Performance Optimization Guide](./docs/performance.md)

### Reviewer Notes

- This is a common pattern in this codebase
- Consider adding this to the code style guide
- Might be worth creating a helper function

### Author Response (for feedback)

*To be filled by the code author:*

- [ ] Fix implemented in commit: `abc123`
- [ ] Fix status: Complete / In Progress / Needs Discussion
- [ ] Questions or concerns: (describe)

---

## Finding Statistics (for Reviewer)

When reviewing multiple findings, track:

- **Total Issues Found:** X
- **Critical:** X
- **High:** X
- **Medium:** X
- **Low:** X

**Recommendation:** ✅ Approve / ⚠️ Request Changes / 🔄 Needs Discussion

**Overall Code Quality:** 1-5 stars
~~~~

## 使用範例

~~~~markdown
User: Review this React component code

Claude: I'll conduct a comprehensive code review using my Code Review Skill.

[Loads SKILL.md]
[Analyzes code against checklist]
[Runs Python metrics script]
[Applies templates]

## Code Review Results

### Summary
Quality Score: 3.5/5
Key Findings: 7 items
Priority Areas: Performance optimization, error handling

### Critical Issues: 1

**Issue**: N+1 Query Problem
**Location**: UserComponent.tsx, lines 45-52
**Impact**: Renders 100+ database queries for list of 20 users
**Severity**: High
**Fix**:
```javascript
// Before: N+1 queries
const users = fetchUsers();
users.forEach(user => fetchUserPosts(user.id)); // 20+ queries

// After: Single query with JOIN
const users = fetchUsersWithPosts(); // 1 query
```

### Performance Findings
- Missing pagination on large lists
- Recommendation: Use React.memo() for items
- Database query: Could be optimized with index

### Quality Findings
- Function in line 20 is 127 lines (max: 50)
- Missing error boundary
- Props should have TypeScript types
~~~~

#### 範例 2：品牌聲音 Skill

**目錄結構：**

```
.claude/skills/brand-voice/
├── SKILL.md
├── brand-guidelines.md
├── tone-examples.md
└── templates/
    ├── email-template.txt
    ├── social-post-template.txt
    └── blog-post-template.md
```

**檔案：** `.claude/skills/brand-voice/SKILL.md`

```yaml
---
name: Brand Voice Consistency
description: Ensure all communication matches brand voice and tone guidelines
tags:
  - brand
  - writing
  - consistency
when_to_use: When creating marketing copy, customer communications, or public-facing content
---

# Brand Voice Skill

## Overview
This skill ensures all communications maintain consistent brand voice, tone, and messaging.

## Brand Identity

### Mission
Help teams automate their development workflows with AI

### Values
- **Simplicity**: Make complex things simple
- **Reliability**: Rock-solid execution
- **Empowerment**: Enable human creativity

### Tone of Voice
- **Friendly but professional** - approachable without being casual
- **Clear and concise** - avoid jargon, explain technical concepts simply
- **Confident** - we know what we're doing
- **Empathetic** - understand user needs and pain points

## Writing Guidelines

### Do's ✅
- Use "you" when addressing readers
- Use active voice: "Claude generates reports" not "Reports are generated by Claude"
- Start with value proposition
- Use concrete examples
- Keep sentences under 20 words
- Use lists for clarity
- Include calls-to-action

### Don'ts ❌
- Don't use corporate jargon
- Don't patronize or oversimplify
- Don't use "we believe" or "we think"
- Don't use ALL CAPS except for emphasis
- Don't create walls of text
- Don't assume technical knowledge

## Vocabulary

### ✅ Preferred Terms
- Claude (not "the Claude AI")
- Code generation (not "auto-coding")
- Agent (not "bot")
- Streamline (not "revolutionize")
- Integrate (not "synergize")

### ❌ Avoid Terms
- "Cutting-edge" (overused)
- "Game-changer" (vague)
- "Leverage" (corporate-speak)
- "Utilize" (use "use")
- "Paradigm shift" (unclear)
```
## Examples

### ✅ Good Example
"Claude automates your code review process. Instead of manually checking each PR, Claude reviews security, performance, and quality—saving your team hours every week."

Why it works: Clear value, specific benefits, action-oriented

### ❌ Bad Example
"Claude leverages cutting-edge AI to provide comprehensive software development solutions."

Why it doesn't work: Vague, corporate jargon, no specific value

## Template: Email

```
Subject: [Clear, benefit-driven subject]

Hi [Name],

[Opening: What's the value for them]

[Body: How it works / What they'll get]

[Specific example or benefit]

[Call to action: Clear next step]

Best regards,
[Name]
```

## Template: Social Media

```
[Hook: Grab attention in first line]
[2-3 lines: Value or interesting fact]
[Call to action: Link, question, or engagement]
[Emoji: 1-2 max for visual interest]
```

## File: tone-examples.md
```
Exciting announcement:
"Save 8 hours per week on code reviews. Claude reviews your PRs automatically."

Empathetic support:
"We know deployments can be stressful. Claude handles testing so you don't have to worry."

Confident product feature:
"Claude doesn't just suggest code. It understands your architecture and maintains consistency."

Educational blog post:
"Let's explore how agents improve code review workflows. Here's what we learned..."
```

#### 範例 3：文件生成器 Skill

**檔案：** `.claude/skills/doc-generator/SKILL.md`

~~~~yaml
---
name: API Documentation Generator
description: Generate comprehensive, accurate API documentation from source code
version: "1.0.0"
tags:
  - documentation
  - api
  - automation
when_to_use: When creating or updating API documentation
---

# API Documentation Generator Skill

## Generates

- OpenAPI/Swagger specifications
- API endpoint documentation
- SDK usage examples
- Integration guides
- Error code references
- Authentication guides

## Documentation Structure

### For Each Endpoint

```markdown
## GET /api/v1/users/:id

### Description
Brief explanation of what this endpoint does

### Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| id | string | Yes | User ID |

### Response

**200 Success**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 Not Found**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "User does not exist"
}
```

### Examples

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```

## Python Script: generate-docs.py

```python
#!/usr/bin/env python3
import ast
import json
from typing import Dict, List

class APIDocExtractor(ast.NodeVisitor):
    """Extract API documentation from Python source code."""

    def __init__(self):
        self.endpoints = []

    def visit_FunctionDef(self, node):
        """Extract function documentation."""
        if node.name.startswith('get_') or node.name.startswith('post_'):
            doc = ast.get_docstring(node)
            endpoint = {
                'name': node.name,
                'docstring': doc,
                'params': [arg.arg for arg in node.args.args],
                'returns': self._extract_return_type(node)
            }
            self.endpoints.append(endpoint)
        self.generic_visit(node)

    def _extract_return_type(self, node):
        """Extract return type from function annotation."""
        if node.returns:
            return ast.unparse(node.returns)
        return "Any"

def generate_markdown_docs(endpoints: List[Dict]) -> str:
    """Generate markdown documentation from endpoints."""
    docs = "# API Documentation\n\n"

    for endpoint in endpoints:
        docs += f"## {endpoint['name']}\n\n"
        docs += f"{endpoint['docstring']}\n\n"
        docs += f"**Parameters**: {', '.join(endpoint['params'])}\n\n"
        docs += f"**Returns**: {endpoint['returns']}\n\n"
        docs += "---\n\n"

    return docs

if __name__ == '__main__':
    import sys
    with open(sys.argv[1], 'r') as f:
        tree = ast.parse(f.read())

    extractor = APIDocExtractor()
    extractor.visit(tree)

    markdown = generate_markdown_docs(extractor.endpoints)
    print(markdown)
~~~~

### Skill 探索與觸發

```mermaid
graph TD
    A["使用者請求"] --> B["Claude 分析"]
    B -->|掃描| C["可用的 Skills"]
    C -->|Metadata 檢查| D["Skill 描述匹配？"]
    D -->|是| E["載入 SKILL.md"]
    D -->|否| F["嘗試下一個 skill"]
    F -->|更多 skills？| D
    F -->|沒有更多| G["使用一般知識"]
    E --> H["提取指令"]
    H --> I["執行 Skill"]
    I --> J["返回結果"]
```

### Skill 與其他功能的比較

```mermaid
graph TB
    A["擴展 Claude"]
    B["Slash Commands"]
    C["Subagents"]
    D["Memory"]
    E["MCP"]
    F["Skills"]

    A --> B
    A --> C
    A --> D
    A --> E
    A --> F

    B -->|使用者觸發| G["快速捷徑"]
    C -->|自動委派| H["獨立上下文"]
    D -->|持久| I["跨會話上下文"]
    E -->|即時| J["外部資料存取"]
    F -->|自動觸發| K["自主執行"]
```

---

## Claude Code Plugins

### 概覽

Claude Code Plugins 是自訂（slash commands、subagents、MCP servers 和 hooks）的套件集合，以單一指令安裝。它們代表最高層級的擴充機制——將多個功能組合成有凝聚力、可共享的套件。

### 架構

```mermaid
graph TB
    A["Plugin"]
    B["Slash Commands"]
    C["Subagents"]
    D["MCP Servers"]
    E["Hooks"]
    F["配置"]

    A -->|套件| B
    A -->|套件| C
    A -->|套件| D
    A -->|套件| E
    A -->|套件| F
```

### Plugin 載入流程

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Plugin as Plugin Marketplace
    participant Install as 安裝
    participant SlashCmds as Slash Commands
    participant Subagents
    participant MCPServers as MCP Servers
    participant Hooks
    participant Tools as 已配置工具

    User->>Claude: /plugin install pr-review
    Claude->>Plugin: 下載 plugin manifest
    Plugin-->>Claude: 返回 plugin 定義
    Claude->>Install: 提取元件
    Install->>SlashCmds: 配置
    Install->>Subagents: 配置
    Install->>MCPServers: 配置
    Install->>Hooks: 配置
    SlashCmds-->>Tools: 準備使用
    Subagents-->>Tools: 準備使用
    MCPServers-->>Tools: 準備使用
    Hooks-->>Tools: 準備使用
    Tools-->>Claude: Plugin 安裝完成 ✅
```

### Plugin 類型與分發

| 類型 | 範圍 | 共享 | 權威 | 範例 |
|------|-------|--------|-----------|----------|
| 官方 | 全域 | 所有使用者 | Anthropic | PR Review、Security Guidance |
| 社群 | 公開 | 所有使用者 | 社群 | DevOps、Data Science |
| 組織 | 內部 | 團隊成員 | 公司 | 內部標準、工具 |
| 個人 | 個別 | 單一使用者 | 開發者 | 自訂工作流程 |

### Plugin 定義結構

```yaml
---
name: plugin-name
version: "1.0.0"
description: "What this plugin does"
author: "Your Name"
license: MIT

# Plugin metadata
tags:
  - category
  - use-case

# Requirements
requires:
  - claude-code: ">=1.0.0"

# Components bundled
components:
  - type: commands
    path: commands/
  - type: agents
    path: agents/
  - type: mcp
    path: mcp/
  - type: hooks
    path: hooks/

# Configuration
config:
  auto_load: true
  enabled_by_default: true
---
```

### Plugin 結構

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   ├── task-1.md
│   ├── task-2.md
│   └── workflows/
├── agents/
│   ├── specialist-1.md
│   ├── specialist-2.md
│   └── configs/
├── skills/
│   ├── skill-1.md
│   └── skill-2.md
├── hooks/
│   └── hooks.json
├── .mcp.json
├── .lsp.json
├── settings.json
├── templates/
│   └── issue-template.md
├── scripts/
│   ├── helper-1.sh
│   └── helper-2.py
├── docs/
│   ├── README.md
│   └── USAGE.md
└── tests/
    └── plugin.test.js
```

### 實際範例

#### 範例 1：PR Review Plugin

**檔案：** `.claude-plugin/plugin.json`

```json
{
  "name": "pr-review",
  "version": "1.0.0",
  "description": "Complete PR review workflow with security, testing, and docs",
  "author": {
    "name": "Anthropic"
  },
  "license": "MIT"
}
```

**檔案：** `commands/review-pr.md`

```markdown
---
name: Review PR
description: Start comprehensive PR review with security and testing checks
---

# PR Review

This command initiates a complete pull request review including:

1. Security analysis
2. Test coverage verification
3. Documentation updates
4. Code quality checks
5. Performance impact assessment
```

**檔案：** `agents/security-reviewer.md`

```yaml
---
name: security-reviewer
description: Security-focused code review
tools: read, grep, diff
---

# Security Reviewer

Specializes in finding security vulnerabilities:
- Authentication/authorization issues
- Data exposure
- Injection attacks
- Secure configuration
```

**安裝：**

```bash
/plugin install pr-review

# 結果：
# ✅ 3 個 slash commands 已安裝
# ✅ 3 個 subagents 已配置
# ✅ 2 個 MCP servers 已連線
# ✅ 4 個 hooks 已註冊
# ✅ 準備使用！
```

#### 範例 2：DevOps Plugin

**元件：**

```
devops-automation/
├── commands/
│   ├── deploy.md
│   ├── rollback.md
│   ├── status.md
│   └── incident.md
├── agents/
│   ├── deployment-specialist.md
│   ├── incident-commander.md
│   └── alert-analyzer.md
├── mcp/
│   ├── github-config.json
│   ├── kubernetes-config.json
│   └── prometheus-config.json
├── hooks/
│   ├── pre-deploy.js
│   ├── post-deploy.js
│   └── on-error.js
└── scripts/
    ├── deploy.sh
    ├── rollback.sh
    └── health-check.sh
```

#### 範例 3：文件 Plugin

**套件元件：**

```
documentation/
├── commands/
│   ├── generate-api-docs.md
│   ├── generate-readme.md
│   ├── sync-docs.md
│   └── validate-docs.md
├── agents/
│   ├── api-documenter.md
│   ├── code-commentator.md
│   └── example-generator.md
├── mcp/
│   ├── github-docs-config.json
│   └── slack-announce-config.json
└── templates/
    ├── api-endpoint.md
    ├── function-docs.md
    └── adr-template.md
```

### Plugin Marketplace

```mermaid
graph TB
    A["Plugin Marketplace"]
    B["官方<br/>Anthropic"]
    C["社群<br/>Marketplace"]
    D["企業<br/>Registry"]

    A --> B
    A --> C
    A --> D

    B -->|類別| B1["開發"]
    B -->|類別| B2["DevOps"]
    B -->|類別| B3["文件"]

    C -->|搜尋| C1["DevOps Automation"]
    C -->|搜尋| C2["Mobile Dev"]
    C -->|搜尋| C3["Data Science"]

    D -->|內部| D1["公司標準"]
    D -->|內部| D2["舊有系統"]
    D -->|內部| D3["合規"]
```

### Plugin 安裝與生命週期

```mermaid
graph LR
    A["探索"] -->|瀏覽| B["Marketplace"]
    B -->|選擇| C["Plugin 頁面"]
    C -->|查看| D["元件"]
    D -->|安裝| E["/plugin install"]
    E -->|提取| F["配置"]
    F -->|啟用| G["使用"]
    G -->|檢查| H["更新"]
    H -->|可用| G
    G -->|完成| I["停用"]
    I -->|稍後| J["啟用"]
    J -->|返回| G
```

### Plugin 功能比較

| 功能 | Slash Command | Skill | Subagent | Plugin |
|---------|---------------|-------|----------|--------|
| **安裝** | 手動複製 | 手動複製 | 手動配置 | 單一指令 |
| **設定時間** | 5 分鐘 | 10 分鐘 | 15 分鐘 | 2 分鐘 |
| **套件** | 單一檔案 | 單一檔案 | 單一檔案 | 多個 |
| **版本控制** | 手動 | 手動 | 手動 | 自動 |
| **團隊共享** | 複製檔案 | 複製檔案 | 複製檔案 | 安裝 ID |
| **更新** | 手動 | 手動 | 手動 | 自動可用 |
| **依賴** | 無 | 無 | 無 | 可能包含 |
| **Marketplace** | 否 | 否 | 否 | 是 |
| **分發** | 儲存庫 | 儲存庫 | 儲存庫 | Marketplace |

### Plugin 使用場景

| 使用場景 | 建議 | 原因 |
|----------|-----------------|-----|
| **團隊新人引導** | ✅ 使用 Plugin | 即時設定，所有配置 |
| **框架設定** | ✅ 使用 Plugin | 套件框架特定指令 |
| **企業標準** | ✅ 使用 Plugin | 集中分發、版本控制 |
| **快速任務自動化** | ❌ 使用 Command | 過於複雜 |
| **單一領域專業** | ❌ 使用 Skill | 太重，改用 skill |
| **專業化分析** | ❌ 使用 Subagent | 手動建立或使用 skill |
| **即時資料存取** | ❌ 使用 MCP | 獨立使用，不要套件 |

### 何時建立 Plugin

```mermaid
graph TD
    A["我應該建立 Plugin 嗎？"]
    A -->|需要多個元件| B{"需要多個 commands<br/>或 subagents<br/>或 MCPs？"}
    B -->|是| C["✅ 建立 Plugin"]
    B -->|否| D["使用個別功能"]
    A -->|團隊工作流程| E{"與<br/>團隊共享？"}
    E -->|是| C
    E -->|否| F["保留為本地設定"]
    A -->|複雜設定| G{"需要自動<br/>配置？"}
    G -->|是| C
    G -->|否| D
```

### 發布 Plugin

**發布步驟：**

1. 建立包含所有元件的 plugin 結構
2. 撰寫 `.claude-plugin/plugin.json` manifest
3. 建立附文件的 `README.md`
4. 使用 `/plugin install ./my-plugin` 在本地測試
5. 提交到 plugin marketplace
6. 通過審查和批准
7. 在 marketplace 上發布
8. 使用者可以用單一指令安裝

**提交範例：**

~~~~markdown
# PR Review Plugin

## Description
Complete PR review workflow with security, testing, and documentation checks.

## What's Included
- 3 slash commands for different review types
- 3 specialized subagents
- GitHub and CodeQL MCP integration
- Automated security scanning hooks

## Installation
```bash
/plugin install pr-review
```

## Features
✅ Security analysis
✅ Test coverage checking
✅ Documentation verification
✅ Code quality assessment
✅ Performance impact analysis

## Usage
```bash
/review-pr
/check-security
/check-tests
```

## Requirements
- Claude Code 1.0+
- GitHub access
- CodeQL (optional)
~~~~

### Plugin 與手動配置的比較

**手動設定（2+ 小時）：**
- 逐一安裝 slash commands
- 個別建立 subagents
- 分別配置 MCPs
- 手動設定 hooks
- 記錄所有東西
- 與團隊共享（希望他們能正確配置）

**使用 Plugin（2 分鐘）：**
```bash
/plugin install pr-review
# ✅ 所有東西已安裝和配置
# ✅ 立即可以使用
# ✅ 團隊可以重現完全相同的設定
```

---

## 比較與整合

### 功能比較矩陣

| 功能 | 觸發方式 | 持久性 | 範圍 | 使用場景 |
|---------|-----------|------------|-------|----------|
| **Slash Commands** | 手動（`/cmd`）| 僅限會話 | 單一指令 | 快速捷徑 |
| **Subagents** | 自動委派 | 獨立上下文 | 專門任務 | 任務分工 |
| **Memory** | 自動載入 | 跨會話 | 使用者/團隊上下文 | 長期學習 |
| **MCP Protocol** | 自動查詢 | 即時外部 | 即時資料存取 | 動態資訊 |
| **Skills** | 自動觸發 | 基於檔案系統 | 可重複使用的專業知識 | 自動化工作流程 |

### 互動時間軸

```mermaid
graph LR
    A["會話開始"] -->|載入| B["Memory（CLAUDE.md）"]
    B -->|探索| C["可用的 Skills"]
    C -->|註冊| D["Slash Commands"]
    D -->|連線| E["MCP Servers"]
    E -->|準備好| F["使用者互動"]

    F -->|輸入 /cmd| G["Slash Command"]
    F -->|請求| H["Skill 自動觸發"]
    F -->|查詢| I["MCP 資料"]
    F -->|複雜任務| J["委派給 Subagent"]

    G -->|使用| B
    H -->|使用| B
    I -->|使用| B
    J -->|使用| B
```

### 實際整合範例：客戶支援自動化

#### 架構

```mermaid
graph TB
    User["客戶電子郵件"] -->|接收| Router["支援路由器"]

    Router -->|分析| Memory["Memory<br/>客戶歷史"]
    Router -->|查詢| MCP1["MCP：客戶 DB<br/>先前的 tickets"]
    Router -->|確認| MCP2["MCP：Slack<br/>團隊狀態"]

    Router -->|路由複雜| Sub1["Subagent：技術支援<br/>上下文：技術問題"]
    Router -->|路由簡單| Sub2["Subagent：計費<br/>上下文：付款問題"]
    Router -->|路由緊急| Sub3["Subagent：升級<br/>上下文：優先處理"]

    Sub1 -->|格式化| Skill1["Skill：回應生成器<br/>保持品牌聲音"]
    Sub2 -->|格式化| Skill2["Skill：回應生成器"]
    Sub3 -->|格式化| Skill3["Skill：回應生成器"]

    Skill1 -->|生成| Output["格式化回應"]
    Skill2 -->|生成| Output
    Skill3 -->|生成| Output

    Output -->|發布| MCP3["MCP：Slack<br/>通知團隊"]
    Output -->|傳送| Reply["客戶回覆"]
```

#### 請求流程

```markdown
## 客戶支援請求流程

### 1. 傳入電子郵件
"I'm getting error 500 when trying to upload files. This is blocking my workflow!"

### 2. Memory 查詢
- 載入附支援標準的 CLAUDE.md
- 檢查客戶歷史：VIP 客戶，本月第 3 個事件

### 3. MCP 查詢
- GitHub MCP：列出開放 issues（找到相關錯誤報告）
- Database MCP：確認系統狀態（沒有報告的服務中斷）
- Slack MCP：確認工程團隊是否知情

### 4. Skill 偵測與載入
- 請求匹配「技術支援」skill
- 從 Skill 載入支援回應範本

### 5. Subagent 委派
- 路由到技術支援 Subagent
- 提供上下文：客戶歷史、錯誤細節、已知問題
- Subagent 可完整存取：read、bash、grep 工具

### 6. Subagent 處理
技術支援 Subagent：
- 在程式碼庫中搜尋檔案上傳的 500 錯誤
- 在 commit 8f4a2c 中找到最近的變更
- 建立變通方案文件

### 7. Skill 執行
回應生成器 Skill：
- 使用品牌聲音指南
- 以同理心格式化回應
- 包含變通方案步驟
- 連結到相關文件

### 8. MCP 輸出
- 發布更新到 #support Slack 頻道
- 標記工程團隊
- 在 Jira MCP 中更新 ticket

### 9. 回應
客戶收到：
- 同理心認可
- 原因說明
- 即時變通方案
- 永久修復的時間表
- 相關 issues 的連結
```

### 完整功能編排

```mermaid
sequenceDiagram
    participant User
    participant Claude as Claude Code
    participant Memory as Memory<br/>CLAUDE.md
    participant MCP as MCP Servers
    participant Skills as Skills
    participant SubAgent as Subagents

    User->>Claude: 請求："Build auth system"
    Claude->>Memory: 載入專案標準
    Memory-->>Claude: Auth 標準、團隊實踐
    Claude->>MCP: 查詢 GitHub 尋找類似實作
    MCP-->>Claude: 程式碼範例、最佳實踐
    Claude->>Skills: 偵測匹配的 Skills
    Skills-->>Claude: Security Review Skill + Testing Skill
    Claude->>SubAgent: 委派實作
    SubAgent->>SubAgent: 建立功能
    Claude->>Skills: 套用 Security Review Skill
    Skills-->>Claude: 安全清單結果
    Claude->>SubAgent: 委派測試
    SubAgent-->>Claude: 測試結果
    Claude->>User: 交付完整系統
```

### 何時使用各個功能

```mermaid
graph TD
    A["新任務"] --> B{任務類型？}

    B -->|重複的工作流程| C["Slash Command"]
    B -->|需要即時資料| D["MCP Protocol"]
    B -->|記住以備下次| E["Memory"]
    B -->|專門化子任務| F["Subagent"]
    B -->|特定領域工作| G["Skill"]

    C --> C1["✅ 團隊捷徑"]
    D --> D1["✅ 即時 API 存取"]
    E --> E1["✅ 持久上下文"]
    F --> F1["✅ 平行執行"]
    G --> G1["✅ 自動觸發的專業知識"]
```

### 選擇決策樹

```mermaid
graph TD
    Start["需要擴展 Claude？"]

    Start -->|快速重複任務| A{"手動或自動？"}
    A -->|手動| B["Slash Command"]
    A -->|自動| C["Skill"]

    Start -->|需要外部資料| D{"即時？"}
    D -->|是| E["MCP Protocol"]
    D -->|否/跨會話| F["Memory"]

    Start -->|複雜專案| G{"多個角色？"}
    G -->|是| H["Subagents"]
    G -->|否| I["Skills + Memory"]

    Start -->|長期上下文| J["Memory"]
    Start -->|團隊工作流程| K["Slash Command +<br/>Memory"]
    Start -->|完全自動化| L["Skills +<br/>Subagents +<br/>MCP"]
```

---

## 摘要表

| 面向 | Slash Commands | Subagents | Memory | MCP | Skills | Plugins |
|--------|---|---|---|---|---|---|
| **設定難度** | 簡單 | 中等 | 簡單 | 中等 | 中等 | 簡單 |
| **學習曲線** | 低 | 中等 | 低 | 中等 | 中等 | 低 |
| **團隊效益** | 高 | 高 | 中等 | 高 | 高 | 非常高 |
| **自動化程度** | 低 | 高 | 中等 | 高 | 高 | 非常高 |
| **上下文管理** | 單一會話 | 獨立 | 持久 | 即時 | 持久 | 所有功能 |
| **維護負擔** | 低 | 中等 | 低 | 中等 | 中等 | 低 |
| **可擴展性** | 良好 | 優秀 | 良好 | 優秀 | 優秀 | 優秀 |
| **可共享性** | 一般 | 一般 | 良好 | 良好 | 良好 | 優秀 |
| **版本控制** | 手動 | 手動 | 手動 | 手動 | 手動 | 自動 |
| **安裝** | 手動複製 | 手動配置 | 不適用 | 手動配置 | 手動複製 | 單一指令 |

---

## 快速開始指南

### 第 1 週：從簡單開始
- 為常見任務建立 2-3 個 slash commands
- 在設定中啟用 Memory
- 在 CLAUDE.md 中記錄團隊標準

### 第 2 週：新增即時存取
- 設定 1 個 MCP（GitHub 或 Database）
- 使用 `/mcp` 配置
- 在工作流程中查詢即時資料

### 第 3 週：分配工作
- 為特定角色建立第一個 Subagent
- 使用 `/agents` 指令
- 以簡單任務測試委派

### 第 4 週：自動化一切
- 為重複自動化建立第一個 Skill
- 使用 Skill marketplace 或自行建立
- 組合所有功能實現完整工作流程

### 持續進行
- 每月審查和更新 Memory
- 隨著模式出現新增 Skills
- 最佳化 MCP 查詢
- 精煉 Subagent 提示

---

## Hooks

### 概覽

Hooks 是事件驅動的 shell 指令，自動回應 Claude Code 事件執行。它們支援在不需要手動干預的情況下進行自動化、驗證和自訂工作流程。

### Hook 事件

Claude Code 支援跨四種 hook 類型（command、http、prompt、agent）的 **25 個 hook 事件**：

| Hook 事件 | 觸發 | 使用場景 |
|------------|---------|-----------|
| **SessionStart** | 會話開始/恢復/清除/壓縮 | 環境設定、初始化 |
| **InstructionsLoaded** | CLAUDE.md 或規則檔案載入 | 驗證、轉換、擴充 |
| **UserPromptSubmit** | 使用者提交提示 | 輸入驗證、提示過濾 |
| **PreToolUse** | 任何工具執行前 | 驗證、批准門控、記錄 |
| **PermissionRequest** | 顯示權限對話方塊 | 自動批准/拒絕流程 |
| **PostToolUse** | 工具成功後 | 自動格式化、通知、清理 |
| **PostToolUseFailure** | 工具執行失敗 | 錯誤處理、記錄 |
| **Notification** | 傳送通知 | 警報、外部整合 |
| **SubagentStart** | Subagent 生成 | 上下文注入、初始化 |
| **SubagentStop** | Subagent 完成 | 結果驗證、記錄 |
| **Stop** | Claude 完成回應 | 摘要生成、清理任務 |
| **StopFailure** | API 錯誤結束輪次 | 錯誤恢復、記錄 |
| **TeammateIdle** | Agent 團隊隊友閒置 | 工作分配、協調 |
| **TaskCompleted** | 任務標記為完成 | 任務後處理 |
| **TaskCreated** | 透過 TaskCreate 建立任務 | 任務追蹤、記錄 |
| **ConfigChange** | 配置檔案變更 | 驗證、傳播 |
| **CwdChanged** | 工作目錄變更 | 目錄特定設定 |
| **FileChanged** | 監看的檔案變更 | 檔案監控、重建觸發 |
| **PreCompact** | 上下文壓縮前 | 狀態保留 |
| **PostCompact** | 壓縮完成後 | 壓縮後動作 |
| **WorktreeCreate** | Worktree 正在建立 | 環境設定、依賴安裝 |
| **WorktreeRemove** | Worktree 正在移除 | 清理、資源釋放 |
| **Elicitation** | MCP server 請求使用者輸入 | 輸入驗證 |
| **ElicitationResult** | 使用者回應 elicitation | 回應處理 |
| **SessionEnd** | 會話終止 | 清理、最終記錄 |

### 常見 Hooks

Hooks 在 `~/.claude/settings.json`（使用者層級）或 `.claude/settings.json`（專案層級）中配置：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_FILE_PATH"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint $CLAUDE_FILE_PATH"
          }
        ]
      }
    ]
  }
}
```

### Hook 環境變數

- `$CLAUDE_FILE_PATH` - 正在編輯/寫入的檔案路徑
- `$CLAUDE_TOOL_NAME` - 正在使用的工具名稱
- `$CLAUDE_SESSION_ID` - 目前會話識別符
- `$CLAUDE_PROJECT_DIR` - 專案目錄路徑

### 最佳實踐

✅ **應該做：**
- 讓 hooks 快速（< 1 秒）
- 使用 hooks 進行驗證和自動化
- 優雅地處理錯誤
- 使用絕對路徑

❌ **不應該做：**
- 讓 hooks 具有互動性
- 使用 hooks 執行長時間任務
- 硬編碼憑證

**請參閱**：[06-hooks/](06-hooks/) 了解詳細範例

---

## Checkpoints 與回溯

### 概覽

Checkpoints 讓你可以儲存對話狀態並回溯到之前的時間點，支援安全實驗和探索多種方法。

### 關鍵概念

| 概念 | 描述 |
|---------|-------------|
| **Checkpoint** | 對話狀態的快照，包含訊息、檔案和上下文 |
| **Rewind** | 返回之前的 checkpoint，丟棄後續的變更 |
| **Branch Point** | 探索多種方法的 checkpoint |

### 存取 Checkpoints

每次使用者提示時自動建立 Checkpoints。若要回溯：

```bash
# 按兩次 Esc 開啟 checkpoint 瀏覽器
Esc + Esc

# 或使用 /rewind 指令
/rewind
```

選擇 checkpoint 時，你可以從五個選項中選擇：
1. **還原程式碼和對話** - 將兩者都還原到那個時間點
2. **還原對話** - 回溯訊息，保留目前程式碼
3. **還原程式碼** - 還原檔案，保留對話
4. **從此處摘要** - 將對話壓縮成摘要
5. **取消** - 取消

### 使用場景

| 情境 | 工作流程 |
|----------|----------|
| **探索方法** | 儲存 → 嘗試 A → 儲存 → 回溯 → 嘗試 B → 比較 |
| **安全重構** | 儲存 → 重構 → 測試 → 若失敗：回溯 |
| **A/B 測試** | 儲存 → 設計 A → 儲存 → 回溯 → 設計 B → 比較 |
| **錯誤恢復** | 發現問題 → 回溯到上次良好狀態 |

### 配置

```json
{
  "autoCheckpoint": true
}
```

**請參閱**：[08-checkpoints/](08-checkpoints/) 了解詳細範例

---

## 進階功能

### 規劃模式

在撰寫程式碼前建立詳細的實作計畫。

**啟用：**
```bash
/plan Implement user authentication system
```

**優點：**
- 附時間估算的清晰路線圖
- 風險評估
- 系統性任務分解
- 審查和修改的機會

### 延伸思考

針對複雜問題的深度推理。

**啟用：**
- 在會話期間用 `Alt+T`（macOS 上用 `Option+T`）切換
- 設定 `MAX_THINKING_TOKENS` 環境變數進行程式化控制

```bash
# 透過環境變數啟用延伸思考
export MAX_THINKING_TOKENS=50000
claude -p "Should we use microservices or monolith?"
```

**優點：**
- 徹底分析權衡取捨
- 更好的架構決策
- 考慮邊界情況
- 系統性評估

### 背景任務

在不阻塞對話的情況下執行長時間操作。

**用法：**
```bash
User: Run tests in background

Claude: Started task bg-1234

/task list           # 顯示所有任務
/task status bg-1234 # 查看進度
/task show bg-1234   # 查看輸出
/task cancel bg-1234 # 取消任務
```

### 權限模式

控制 Claude 可以執行的操作。

| 模式 | 描述 | 使用場景 |
|------|-------------|----------|
| **default** | 標準權限，對敏感動作進行提示 | 一般開發 |
| **acceptEdits** | 自動接受檔案編輯而不需確認 | 受信任的編輯工作流程 |
| **plan** | 僅分析和規劃，不修改檔案 | 程式碼審查、架構規劃 |
| **auto** | 自動批准安全動作，只對有風險的提示 | 附安全性的平衡自主 |
| **dontAsk** | 執行所有動作而不需確認提示 | 有經驗的使用者、自動化 |
| **bypassPermissions** | 完整不受限存取，無安全檢查 | CI/CD 流水線、受信任腳本 |

**用法：**
```bash
claude --permission-mode plan          # 唯讀分析
claude --permission-mode acceptEdits   # 自動接受編輯
claude --permission-mode auto          # 自動批准安全動作
claude --permission-mode dontAsk       # 不需確認提示
```

### Headless 模式（Print 模式）

使用 `-p`（print）旗標在沒有互動輸入的情況下執行 Claude Code，用於自動化和 CI/CD。

**用法：**
```bash
# 執行特定任務
claude -p "Run all tests"

# 管道輸入進行分析
cat error.log | claude -p "explain this error"

# CI/CD 整合（GitHub Actions）
- name: AI Code Review
  run: claude -p "Review PR changes and report issues"

# JSON 輸出用於腳本
claude -p --output-format json "list all functions in src/"
```

### 排程任務

使用 `/loop` 指令按重複排程執行任務。

**用法：**
```bash
/loop every 30m "Run tests and report failures"
/loop every 2h "Check for dependency updates"
/loop every 1d "Generate daily summary of code changes"
```

排程任務在背景執行並在完成時回報結果。它們對於持續監控、定期檢查和自動化維護工作流程非常有用。

### Chrome 整合

Claude Code 可以與 Chrome 瀏覽器整合，用於 web 自動化任務。這讓你可以在開發工作流程中直接導航網頁、填寫表單、截圖和從網站提取資料。

### 會話管理

管理多個工作會話。

**指令：**
```bash
/resume                # 繼續之前的對話
/rename "Feature"      # 為目前會話命名
/fork                  # Fork 成新會話
claude -c              # 繼續最近的對話
claude -r "Feature"    # 以名稱/ID 繼續會話
```

### 互動功能

**鍵盤快捷鍵：**
- `Ctrl + R` - 搜尋指令歷史
- `Tab` - 自動補全
- `↑ / ↓` - 指令歷史
- `Ctrl + L` - 清除畫面

**多行輸入：**
```bash
User: \
> Long complex prompt
> spanning multiple lines
> \end
```

### 配置

完整配置範例：

```json
{
  "planning": {
    "autoEnter": true,
    "requireApproval": true
  },
  "extendedThinking": {
    "enabled": true,
    "showThinkingProcess": true
  },
  "backgroundTasks": {
    "enabled": true,
    "maxConcurrentTasks": 5
  },
  "permissions": {
    "mode": "default"
  }
}
```

**請參閱**：[09-advanced-features/](09-advanced-features/) 了解全面指南

---

## 資源

- [Claude Code 文件](https://code.claude.com/docs/en/overview)
- [Anthropic 文件](https://docs.anthropic.com)
- [MCP GitHub Servers](https://github.com/modelcontextprotocol/servers)
- [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

---

*最後更新：2026 年 3 月*
*適用於 Claude Haiku 4.5、Sonnet 4.6 和 Opus 4.6*
