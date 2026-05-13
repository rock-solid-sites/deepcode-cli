# Translation Inventory

Generated: Session 1 — repository-wide scan of human-readable text files.
Purpose: Scope off-API translation work before Session 2.

## Summary

- **Total files inventoried**: 70 (10 documentation/text files + 60 TypeScript source files)
- **Files needing fresh translation**: 2 (`docs/configuration.md`, `docs/mcp.md` — Chinese only, no English counterpart exists)
- **Files needing English update**: 3 (`README.md` → `README_en.md` is stale, `README_cn.md` → `README_en.md` is stale)
- **Files in English already, no action needed**: 8 (all `docs/tools/*.md`, `.deepcode/AGENTS.md`, `docs/prompts/init_command.md.ejs`, `.husky/pre-commit`)
- **Files with mixed comments (CJK in src/)**: 5 source files with some Chinese in comments or test strings

### Key surprises

1. **Three READMEs, three different states**: `README.md` (Chinese) has a "New Features in this Fork" section about MCP that neither `README_cn.md` nor `README_en.md` contain. `README_cn.md` and `README_en.md` mirror each other structurally but `README_en.md` is missing the "How to configure MCP?" FAQ entry present in `README.md`.
2. **`docs/` and `docs/tools/` are different**: `docs/configuration.md` and `docs/mcp.md` are Chinese-only. The six `docs/tools/*.md` files are all English-only — they're tool descriptions injected directly into LLM system prompts.
3. **No English documentation exists** for the MCP feature or configuration details. The English README links to `docs/mcp.md` which is in Chinese only.
4. **src/ has very little Chinese**: Only 5 of 60 source files contain any CJK characters, and those are mostly in comments (SlashCommandMenu.tsx) or test fixture strings (session.test.ts). No code identifiers or string literals visible to users are in Chinese.

---

## 1. Root README Files

| File | Language | Sync status | Action needed | Size | Notes |
|------|----------|-------------|---------------|------|-------|
| `README.md` | Chinese | — | Create/update English counterpart | 132 lines, ~740 CJK chars | Default README shown on GitHub. Has "🚀 New Features (This Fork)" MCP section at top. Contains an "How to configure MCP?" FAQ entry. Links to `docs/mcp.md`. |
| `README_cn.md` | Chinese | Drifted from `README.md` — missing the "New Features (This Fork)" MCP section | Sync with `README.md` first, then translate to English | 115 lines, ~584 CJK chars | Pure upstream Chinese README (no fork features). Matches `README_en.md` in structure. Missing the MCP section. |
| `README_en.md` | English | Drifted from `README.md` — same content as `README_cn.md` (upstream scope only) | Update with fork features + missing MCP FAQ | 114 lines | Stale English README. Matches `README_cn.md` structure but missing: fork feature announcement, MCP FAQ entry. Links to `docs/mcp.md` which is Chinese-only. |

**Recommended next step for READMEs**: Consolidate into one true-source approach:
- Option A: Keep `README.md` as the Chinese original with fork features, sync `README_cn.md` to match, then regenerate `README_en.md` from the synced Chinese.
- Option B: Write a new `README.md` in English, deprecate `README_en.md`, keep `README_cn.md` as the Chinese translation.

---

## 2. Documentation Files (`docs/`)

| File | Language | Sync status | Action needed | Size | Notes |
|------|----------|-------------|---------------|------|-------|
| `docs/configuration.md` | Chinese (with embedded English terms) | N/A — no English version exists | **Fresh translation to English** | 173 lines, ~1169 CJK chars | Details on settings file, config hierarchy, all supported fields. Referenced from README. |
| `docs/mcp.md` | Chinese (with embedded English terms) | N/A — no English version exists | **Fresh translation to English** | 205 lines, ~639 CJK chars | MCP configuration guide for the fork's new feature. Referenced from `README.md` and (incorrectly) from `README_en.md`. |
| `docs/prompts/init_command.md.ejs` | English | N/A — single language | None | 44 lines | EJS template for generating `AGENTS.md`. Feeds into LLM prompts. Pure English. |

---

## 3. Tool Description Files (`docs/tools/`)

All six files are **English only**, injected directly into the LLM system prompt by `src/prompt.ts` (see `readToolDocs()` function). No translation needed.

| File | Language | Action needed | Size | Notes |
|------|----------|---------------|------|-------|
| `docs/tools/bash.md` | English | None | 71 lines | Tool definition + JSON schema for the bash tool |
| `docs/tools/read.md` | English | None | 48 lines | Tool definition + JSON schema for the read tool |
| `docs/tools/edit.md` | English | None | 53 lines | Tool definition + JSON schema for the edit tool |
| `docs/tools/write.md` | English | None | 35 lines | Tool definition + JSON schema for the write tool |
| `docs/tools/web-search.md` | English | None | 28 lines | Tool definition + JSON schema for the web-search tool |
| `docs/tools/ask-user-question.md` | English | None | 12 lines | Tool definition + JSON schema for the AskUserQuestion tool |

---

## 4. Other Text Files

| File | Language | Action needed | Size | Notes |
|------|----------|---------------|------|-------|
| `.deepcode/AGENTS.md` | English | None | 109 lines | Repository guidelines for the LLM agent. Heavy overlap with this very inventory document's instructions. |
| `.husky/pre-commit` | English | None | 1 line | Runs `npx lint-staged`. Only text is `npx lint-staged`. |

---

## 5. Source Files (`src/`) — Comment Language Status

All `.ts` and `.tsx` files in `src/`. For each file, the report column indicates whether code comments or string literals contain Chinese characters.

### Files with Chinese in comments or test strings (5 of 60)

| File | CJK Count | Context | Language | Notes |
|------|-----------|---------|----------|-------|
| `src/ui/SlashCommandMenu.tsx` | 66 | Comments | Mixed (Chinese comments, English code) | 3 Chinese comments about layout calculation logic. The code itself is English identifiers. |
| `src/ui/App.tsx` | 25 | Comments | Mixed (Chinese comments, English code) | 2 Chinese comments about `<Static>` index behavior. |
| `src/tests/session.test.ts` | 7 | Test fixture strings | Mixed (English code, CJK in test data) | Contains `"星期四"` (Thursday) in a mock output and `"思考"` (thinking) as test assertion values. |
| `src/tests/promptInputKeys.test.ts` | 2 | Test fixture strings | Mixed (English code, CJK in test data) | Contains `"你好"` (hello) as test input for cursor placement. |
| `src/ui/MessageView.tsx` | 1 | Code line | Mixed (English code, single CJK character) | Contains `"："` (Chinese colon) check — `result.endsWith("：")` — for CJK punctuation handling. |

### Files with no Chinese characters (55 of 60)

All remaining `src/` files are pure English in both code and comments. Full listing:

**Core modules (13 files):**
- `src/cli.tsx` (117 lines)
- `src/session.ts` (2125 lines)
- `src/settings.ts` (275 lines)
- `src/prompt.ts` (618 lines)
- `src/model-capabilities.ts` (5 lines)
- `src/AsciiArt.ts` (8 lines)
- `src/debug-logger.ts` (82 lines)
- `src/error-logger.ts` (129 lines)
- `src/notify.ts` (68 lines)
- `src/openai-thinking.ts` (25 lines)
- `src/updateCheck.ts` (296 lines)

**Common modules (4 files):**
- `src/common/file-utils.ts` (143 lines)
- `src/common/runtime.ts` (74 lines)
- `src/common/shell-utils.ts` (187 lines)
- `src/common/state.ts` (153 lines)

**MCP modules (2 files):**
- `src/mcp/mcp-client.ts` (239 lines)
- `src/mcp/mcp-manager.ts` (213 lines)

**Tool handlers (7 files):**
- `src/tools/executor.ts` (257 lines)
- `src/tools/bash-handler.ts` (262 lines)
- `src/tools/read-handler.ts` (650 lines)
- `src/tools/write-handler.ts` (168 lines)
- `src/tools/edit-handler.ts` (838 lines)
- `src/tools/web-search-handler.ts` (388 lines)
- `src/tools/ask-user-question-handler.ts` (148 lines)

**UI modules (14 files):**
- `src/ui/App.tsx` (653 lines) — *noted above for Chinese comments*
- `src/ui/AskUserQuestionPrompt.tsx` (274 lines)
- `src/ui/MessageView.tsx` (337 lines) — *noted above for Chinese colon check*
- `src/ui/PromptInput.tsx` (892 lines)
- `src/ui/SessionList.tsx` (187 lines)
- `src/ui/SlashCommandMenu.tsx` (76 lines) — *noted above for Chinese comments*
- `src/ui/WelcomeScreen.tsx` (133 lines)
- `src/ui/UpdatePrompt.tsx` (82 lines)
- `src/ui/ThemedGradient.tsx` (30 lines)
- `src/ui/askUserQuestion.ts` (133 lines)
- `src/ui/clipboard.ts` (158 lines)
- `src/ui/exitSummary.ts` (130 lines)
- `src/ui/index.ts` (83 lines)
- `src/ui/loadingText.ts` (70 lines)
- `src/ui/markdown.ts` (117 lines)
- `src/ui/promptBuffer.ts` (192 lines)
- `src/ui/slashCommands.ts` (95 lines)
- `src/ui/thinkingState.ts` (23 lines)
- `src/ui/prompt/cursor.ts` (262 lines)
- `src/ui/prompt/index.ts` (10 lines)
- `src/ui/prompt/useTerminalInput.ts` (139 lines)

**Test files (20 files):** All pure English.

---

## 6. Action Summary

| Action | Files | Notes |
|--------|-------|-------|
| **Fresh translation (Chinese → English)** | `docs/configuration.md`, `docs/mcp.md` | No English version exists at all. These are the highest priority — the English README links to them but they're Chinese-only. |
| **Update existing English** | `README_en.md` | Must be synced with the fork's new features (MCP section) and the MCP FAQ entry. |
| **Sync Chinese READMEs** | `README_cn.md` | Before or alongside translation — needs the fork feature section added to match `README.md`. |
| **Verify / no action** | All `docs/tools/*.md`, `.deepcode/AGENTS.md`, `docs/prompts/init_command.md.ejs`, `.husky/pre-commit` | Already in English. |
| **No action (source code)** | All 55 pure-English `src/` files | Code is code — comments are developer-facing, not user-facing. The 5 files with sparse CJK in comments/test strings are negligible and do not need translation. |

### Priority order for translation work

1. `README_cn.md` — sync with `README.md` (add fork features section)
2. `README_en.md` — update from synced `README_cn.md` or `README.md`
3. `docs/mcp.md` — fresh English translation (referenced from both READMEs)
4. `docs/configuration.md` — fresh English translation (referenced from both READMEs)

---

## Resolution — Session 2

All translations placed, file structure finalized.

| Inventory entry | Action taken | Commit |
|---|---|---|
| `docs/configuration.md` — fresh translation (Chinese → English) | Staging file `docs/en-configuration.md` copied over upstream Chinese original; staging file deleted. | `b0854ad` |
| `docs/mcp.md` — fresh translation (Chinese → English) | Staging file `docs/en-mcp.md` copied over upstream Chinese original; staging file deleted. | `b0854ad` |
| `README.md` (upstream Chinese) — create English counterpart | New English `README.md` authored by operator (staged as `docs/README-en.md`) moved to repo root, replacing upstream Chinese original. | `14b3c49` |
| `README_cn.md` (upstream Chinese, stale) — sync with fork features | Fork-aware Chinese translation (staged as `docs/cn-readme.md`) moved to repo root as `README_cn.md`, replacing upstream stale copy. | `14b3c49` |
| `README_en.md` — update or remove | Deleted. `README.md` is now the canonical English README, making the separate `README_en.md` convention redundant. | N/A — file was untracked, so deletion is not in git history. |
| All `docs/tools/*.md`, `.deepcode/AGENTS.md`, `docs/prompts/init_command.md.ejs`, `.husky/pre-commit` | No action needed (already English). | — |
| All 55 pure-English `src/` files | No action needed. | — |
