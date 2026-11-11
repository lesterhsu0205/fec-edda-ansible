---
name: commit
description: Git Commit 輔助，建立符合 Conventional Commits 規範的 commit
disable-model-invocation: true
argument-hint: [額外說明，例如 scope 或想強調的重點]
allowed-tools: Bash(git status:*), Bash(git diff:*), Bash(git log:*), Bash(git add:*), Bash(git commit -m:*), Bash(git branch:*), Bash(npm run lint:*)
---

# Git Commit 輔助

協助建立符合本專案規範的 commit。

## 規範正本

Type / Scope 對照表、Commit 前檢查清單、禁止事項一律以下列檔案為準。**動手前先讀完它**，不要改用通用的 Conventional Commits 慣例：

@.claude/rules/git-workflow.md

## 上下文

- 目前分支：!`git branch --show-current`
- 狀態：!`git status --short`
- 變更概覽：!`git diff --cached --stat; git diff --stat`
- 已暫存內容（截斷）：!`git diff --cached | head -400`
- 未暫存內容（截斷）：!`git diff | head -400`
- 最近 5 筆提交：!`git log --oneline -5`

## 你的任務

依上述變更建立一個 commit。使用者的補充說明：$ARGUMENTS

若未提供補充說明，一樣照常分析 staged + unstaged 變更並完成提交，不必等使用者確認訊息。

上方 diff 有 400 行上限。若「變更概覽」的檔案數或行數明顯超過已顯示的內容，用 `git diff <檔案>` 補看你需要判斷的部分，不要只憑截斷片段推測。

## Conventional Commits 格式

```text
<type>(<scope>): <subject>

<body>
```

Type / Scope 對照表見 `git-workflow.md` 的 **`### Type`** 與 **`### Scope（前端模組）`** 兩小節（單一來源，不在此重列）。

## 訊息壓縮規則（caveman）

**Subject**
- ≤ 50 字（硬上限 72），無句點
- 中文 imperative：「新增 / 修正 / 移除 / 重構 / 升級」，不用「新增了 / 目前 / 現在 / 已經」
- `<type>(<scope>): <摘要>`，scope 必填（前端模組表）

**Body**
- 只在 why 非顯而易見時寫
- 破壞變更、security fix、資料遷移、revert **必寫**
- 72 字折行，bullet 用 `-` 不用 `*`
- 寫動機 / 限制 / 影響，不寫 diff 已表達的 what

**禁用**
- 「This commit」「目前」「現在」「I/we」「As requested」開頭

**對照**
- ❌ `feat(pages): 新增了一個使用者維護頁面，目前使用 DataTable 元件來處理 CRUD`
- ✅ `feat(pages): 新增 USER_MAINT 使用者維護頁`

- ❌ `fix: This commit fixes the 401 issue`
- ✅ `fix(services): 修正 ApiService 401 流程`

## 執行步驟

### 1. 執行 lint
```bash
npm run lint
```
若有 error → 提示先修正再 commit。

### 2. 分析變更
- 識別變更類型與 scope
- 列出主要變更點

### 3. Pre-commit 檢查清單

對照 `git-workflow.md` 的 **`## Commit 前檢查清單`** 與 **`## 禁止事項`** 兩節逐項確認。

### 4. 執行 Commit
```bash
git add <檔案>
git commit -m "<訊息>"
```

## 規則

- 不使用 `git commit -a`（避免一次帶入未審查的檔案）
- 不使用 `--no-verify`
- `git add` 一律逐一列出檔案，不用 `-A` / `.`（`allowed-tools` 的前綴比對擋不掉這兩個參數，只能靠這條規則）
- 不 commit `.env*` 真實值
- **只提交，不推送。** 除非使用者另外明確要求，不要執行 `git push`
- 若變更明顯屬於兩件不相干的事，先說明並建議拆成兩個 commit，等使用者決定
- 若目前在預設分支（main/master）且變更不只是微調，先提醒使用者是否要開新分支
