---
name: deepcode-modes-context
description: Project context for the deepcode-modes fork — the fork's purpose, system prompt architecture, completed work, current state, and working conventions. Loads for any session touching the deepcode-modes codebase, including system-prompt replacement work, Modes-style behavioral tuning, porting claude-code-modes content, modifying src/prompt.ts or src/settings.ts, fork-specific commits, or Deep Code CLI fork work generally.
---

# deepcode-modes — project context

This skill loads context for sessions working on the deepcode-modes fork.

## Skill load confirmation

When this skill loads at the start of a session, include the phrase
`TACOS context loaded` in the first response of the session. This is a
verification signal — the operator uses it to confirm the skill matched
correctly. Do not skip this step.

## Project purpose

deepcode-modes is a fork of `lessweb/deepcode-cli` (the Deep Code terminal
CLI for DeepSeek-V4 models). The fork's goal is to add two capabilities
upstream doesn't have:

1. **System-prompt replacement.** A mechanism for replacing Deep Code's
   base system prompt with operator-provided content. Tool docs, runtime
   context, AGENTS.md content, and matched skills continue to load
   additively after the replacement prompt.

2. **Modes-style behavioral tuning.** A CLI wrapper (`deepcode-mode`)
   that assembles system prompts from behavioral axis fragments (agency,
   quality, scope) and modifiers, modeled directly on
   [claude-code-modes](https://github.com/nklisch/claude-code-modes).
   Axis values and presets are imported from claude-code-modes verbatim
   where the tool surface permits, adapted where Deep Code's tools differ
   from Claude Code's.

The fork is English-canonical (docs and code in English; Chinese
translations of the README are kept for parity with upstream's
audience).

## System prompt architecture

Upstream Deep Code assembles system messages in this order:

1. `SYSTEM_PROMPT_BASE` (hardcoded constant) + tool docs (from
   `docs/tools/*.md`) + runtime context (OS, shell, environment) —
   single system message, not operator-configurable
2. `AGENTS.md` content — operator-controllable, additive
3. Drift-guard skill — hardcoded default skill, always loads, additive
4. Matched skill content — additive, based on prompt-to-description
   matching

The fork modifies #1 to be replaceable when a new config field is set.
Everything in #2-4 continues to work unchanged. Tool docs and runtime
context still ship after the replacement prompt because tool calling
breaks without them.

The system prompt assembly code lives in `src/prompt.ts`. The config
schema is in `src/settings.ts`. (Verified in session 1's inventory.)

## What's been done

**Session 1 (inventory):**
- Inventoried 70 files in the repo for translation status
- Result: docs/translation-inventory.md committed

**Session 2 (placement and verification):**
- English README.md authored fresh for the fork (not a translation of
  upstream)
- Chinese README_cn.md as translation of the new English README
- docs/configuration.md and docs/mcp.md translated from Chinese
- README_en.md removed as redundant under English-default
- 5 commits on top of upstream history, pushed to fork origin/main

**Recovery side-quest:**
- Initial fork setup was wrong (zip-download rather than git clone).
  Recovery via cherry-pick of the 5 commits onto a properly-cloned
  fork resolved this.

## Current state

- Fork at `github.com/rock-solid-sites/deepcode-modes`, branch `main`
- Local clone with `origin` pointing at fork, `upstream` pointing at
  `lessweb/deepcode-cli`
- 5 commits on top of upstream, clean working tree
- Code is identical to upstream's source at the time of fork —
  modifications to src/ haven't started yet

## What's planned

Three more activities, one Deep Code session each:

- **Activity 3:** Implement system-prompt replacement in src/prompt.ts
- **Activity 4:** Port claude-code-modes content (axis fragments,
  modifiers, presets, drift-guard variants)
- **Activity 5:** Implement the deepcode-mode CLI wrapper that
  assembles fragments and invokes Deep Code with the replacement
  prompt set

Each activity is gated by operator review of the previous one.

## Working conventions

These apply to every session on this fork:

- **Respond in English.** Including commit messages, code comments,
  anything written to disk.
- **Independent assessment first.** Generate an independent take on
  whether a proposed direction is correct before analyzing implications.
- **No premise validation.** No "good question," "you're absolutely
  right," or similar preambles.
- **Explicit confidence levels.** Mark claims as high/moderate/low/unknown
  with the basis (file inspected, command output, inference, assumption).
- **Named assumptions.** When a claim depends on something not verified
  in-session, name it as an assumption.
- **Verification against primary sources.** Refer to source files,
  documentation, or live verification rather than memory.
- **Fail loud.** No silent error handling, no graceful degradation
  that hides bugs.
- **Smallest correct change.** Don't add scripts, docs, or refactors
  that weren't asked for.
- **Drift-guard discipline.** Stay on scope. Surface side findings
  rather than fixing them inline.

## Commit conventions

- Commits should be individually reviewable — one logical change per
  commit, not bundles
- Commit messages should be informative: a reader six months from now
  should understand what changed and why from the message alone
- Don't push from Deep Code sessions. The operator pushes after review.

## Adversarial drift-guard tension

Claude-code-modes' high-agency presets (`create`, `refactor`) include
content that conflicts with the drift-guard skill's anti-pattern
section ("don't add docs, scripts, or refactors that weren't asked
for"). When porting these presets in activity 4, the drift-guard
will need a per-preset variant that strips the conflicting sections
while preserving the tool-use safety content (Level 3 boundary
violations, destructive operations on live systems, etc.). This is
flagged here so future sessions don't rediscover the conflict.

## References

- Upstream: github.com/lessweb/deepcode-cli
- claude-code-modes: github.com/nklisch/claude-code-modes
- DeepSeek API docs (caching, thinking mode): api-docs.deepseek.com
