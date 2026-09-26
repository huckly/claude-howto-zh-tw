<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../resources/logos/claude-howto-logo-dark.svg">
  <img alt="Claude How To" src="../resources/logos/claude-howto-logo.svg">
</picture>

# 建置腳本

此目錄包含兩個產生器，可將教學 markdown 檔案轉換為可發佈的格式：

- [**EPUB 建置腳本**](#epub-建置腳本) — `build_epub.py`
- [**靜態網站產生器**](#靜態網站產生器) — `build_website.py`

兩者都將 `.md` 檔案視為唯一的事實來源——編輯 markdown 後，重新執行對應的腳本即可重新產生輸出。

---

# EPUB 建置腳本

從 Claude How-To markdown 檔案建置 EPUB 電子書。

## 功能

- 依資料夾結構組織章節（01-slash-commands、02-memory 等）
- 透過本機的 `mmdc` CLI 將 Mermaid 圖表算繪為 PNG 圖片（不需網路）
- 快取相同的圖表，每個不重複的圖表只算繪一次
- 從專案 logo 產生封面圖片
- 將內部 markdown 連結轉換為 EPUB 章節參考
- 嚴格錯誤模式 — 若任何圖表無法算繪則失敗

## 需求

- Python 3.10+
- [uv](https://github.com/astral-sh/uv)
- `PATH` 中需有 [`mmdc`](https://github.com/mermaid-js/mermaid-cli)，用於算繪 Mermaid 圖表（`npm install -g @mermaid-js/mermaid-cli`）

## 快速入門

```bash
# 最簡單的方式 - uv 處理一切
uv run scripts/build_epub.py
```

## 開發設定

```bash
# 建立虛擬環境
uv venv

# 啟動並安裝依賴項
source .venv/bin/activate
uv pip install -r requirements-dev.txt

# 執行測試
pytest scripts/tests/ -v

# 執行腳本
python scripts/build_epub.py
```

## 命令列選項

```text
usage: build_epub.py [-h] [--root ROOT] [--output OUTPUT] [--verbose]
                     [--mmdc-path MMDC_PATH] [--lang {en,vi,zh,ja}]
                     [--puppeteer-config PUPPETEER_CONFIG]

options:
  -h, --help            顯示此說明訊息並退出
  --root, -r ROOT       根目錄（預設：儲存庫根目錄）
  --output, -o OUTPUT   輸出路徑（預設：claude-howto-guide.epub）
  --verbose, -v         啟用詳細日誌
  --mmdc-path PATH      mmdc 執行檔路徑（預設：PATH 中的 mmdc）
  --lang {en,vi,zh,ja}  要建置的語言（預設：en）
  --puppeteer-config P  透過 -p 傳給 mmdc 的 Puppeteer 設定 JSON
```

## 範例

```bash
# 以詳細輸出建置
uv run scripts/build_epub.py --verbose

# 自訂輸出位置
uv run scripts/build_epub.py --output ~/Desktop/claude-guide.epub

# 建置翻譯版本
uv run scripts/build_epub.py --lang vi

# 指定不在 PATH 中的 mmdc
uv run scripts/build_epub.py --mmdc-path ./node_modules/.bin/mmdc
```

## 輸出

在儲存庫根目錄建立 `claude-howto-guide.epub`。

EPUB 包含：
- 含專案 logo 的封面圖片
- 含巢狀章節的目錄
- 所有 markdown 內容轉換為 EPUB 相容的 HTML
- Mermaid 圖表算繪為 PNG 圖片

## 執行測試

```bash
# 使用虛擬環境
source .venv/bin/activate
pytest scripts/tests/ -v

# 或直接使用 uv
uv run --with pytest --with pytest-asyncio \
    --with ebooklib --with markdown --with beautifulsoup4 \
    --with pillow \
    pytest scripts/tests/ -v
```

## 依賴項

透過 PEP 723 內嵌腳本中繼資料管理：

| 套件 | 用途 |
|---------|---------|
| `ebooklib` | EPUB 產生 |
| `markdown` | Markdown 轉 HTML |
| `beautifulsoup4` | HTML 解析 |
| `pillow` | 封面圖片產生 |

## 疑難排解

**建置失敗並顯示 `mmdc not found`**：安裝 Mermaid CLI（`npm install -g @mermaid-js/mermaid-cli`），若執行檔不在 `PATH` 中則傳入 `--mmdc-path`。內建的 Chromium 沒有可用的 arm64 版本，因此在 arm64 機器上請改在 CI 中建置 EPUB——`.github/workflows/test.yml` 中的 `build-epub` job 涵蓋所有語言。

**`mmdc` 在 CI 或容器中失敗**：Chromium 需要無沙箱（sandbox-free）的設定。將 `{"args":["--no-sandbox","--disable-setuid-sandbox"]}` 寫入檔案，並透過 `--puppeteer-config` 傳入。

**缺少 logo**：若找不到 `claude-howto-logo.png`，腳本會產生純文字封面。

---

# 靜態網站產生器

從 EPUB 建置所使用的同一批 markdown 檔案，產生優雅且適合行動裝置的靜態網站。網站只是算繪後的呈現；`.md` 檔案仍是唯一的事實來源。

## 功能

- 每個 markdown 來源對應一個 HTML 頁面——內部 `.md` 連結會改寫為網站上對應的頁面
- 指向非 markdown 儲存庫檔案（範本、腳本、JSON）的參考會轉為 GitHub blob URL，在 github.com 上開啟原始檔
- Mermaid 圖表透過 `mermaid.min.js` 在用戶端算繪，由建置後的網站提供（執行時不使用 CDN）
- Tailwind CSS 以獨立 CLI（Go 執行檔，不需 Node.js）編譯，並由建置後的網站提供——響應式版面，包含側邊欄導覽、頁內目錄、深色模式切換與上一頁／下一頁導覽
- Inter + JetBrains Mono 字型與 CSS 一同自行託管——頁面載入時不發出第三方請求
- 沿用 EPUB 的課程順序（`01-` … `10-` 加上頂層文件）
- 可作為純靜態檔案託管——設計用於部署到 GitHub Pages

## 快速入門

```bash
# 將英文網站建置到 ./site/
uv run scripts/build_website.py

# 在本機預覽
python -m http.server --directory site 8080
# 接著開啟 http://localhost:8080
```

## 命令列選項

```text
usage: build_website.py [-h] [--root ROOT] [--output OUTPUT]
                        [--lang {en,vi,zh,ja,uk}] [--repo-url REPO_URL]
                        [--branch BRANCH] [--verbose]

options:
  --root, -r ROOT       來源根目錄（預設：儲存庫根目錄）
  --output, -o OUTPUT   輸出目錄（預設：<repo>/site）
  --lang LANG           要建置的語言：en | vi | zh | ja | uk
  --repo-url URL        用於 blob 連結的 GitHub 儲存庫（預設：luongnv89/claude-howto）
  --branch BRANCH       用於 blob 連結的分支（預設：main）
  --verbose, -v         啟用詳細日誌
```

## GitHub Pages 部署

儲存庫附有 `.github/workflows/pages.yml` 工作流程，會在每次推送到 `main`（任何 `.md` 或產生器檔案變更時）時建置網站，並透過 `actions/deploy-pages` 發佈。在儲存庫設定中啟用 GitHub Pages，並選擇 **Source: GitHub Actions** 即可啟用。

## 架構

`build_website.py` 重用 `build_epub.py` 的章節排序邏輯，並在 `scripts/website_templates/` 下提供 HTML 範本：

- `page.html.j2` — 每頁的 Jinja2 範本，含側邊欄導覽、目錄、上一頁／下一頁
- `tailwind.config.js`、`tailwind.input.css` — Tailwind 獨立 CLI 的設定與入口 CSS；CLI 會掃描建置後的 HTML，產生只包含實際使用之 utility 的 `site/assets/tailwind.css`
- `site.css` — 少量網站專屬樣式，加上 Pygments 主題

Tailwind CLI 執行檔、Mermaid 套件與字型檔會在首次建置時下載到 `scripts/.vendor-cache/`（已列入 gitignore）——請參閱 `scripts/vendor_assets.py`。

標題錨點使用與 `check_cross_references.heading_to_anchor` 完全相同的演算法產生，因此經 pre-commit hook 驗證的 `#anchor` 連結在算繪後的網站上也能正確解析。
