# Repository Guidelines

## Project Structure & Modules
- Entry points: `index.html`, `index-rp.html`, `index-panel.html`.
- Utilities: `utils/` (`split.mjs`, `join.mjs`, `parser.mjs`) for outlining/inlining `<script>`/`<style>` during development.
- Assets/docs: `koboldcpp_api.*`, `manifest.json`, `sw.js`, previews; GitHub config in `.github/`.

## Dev Commands
- `npm install`: installs `html-lexer` for the utils.
- `npm run split [index.html]`: writes parts to `split/<name>/...` for easier editing.
- `npm run join [index.html]`: inlines from `split/<name>/...` and overwrites the HTML.
- Run locally: open `index.html` (or serve statically, e.g., `python3 -m http.server`).

## Style & Conventions
- Canonical artifact: a single HTML file. Do not commit `split/` outputs.
- Tabs preferred; follow existing HTML/JS/CSS patterns and IDs.
- Keep UI dependency-free; avoid adding build steps or remote CDNs.

## Testing
- Manual smoke tests: editor boot, backend/model selection, TTS, image generation.
- PWA/offline checks when touching `sw.js` or `manifest.json`.
- Verify desktop and mobile layouts.

## Commits & PRs
- Commits: concise, imperative (e.g., “fix streaming error display”).
- PRs: clear scope/rationale, link issues, include UI screenshots for visual changes.
- Submit joined HTML and versioned assets only.

## Tavern Card (V1/V2/V3) Overview
- Purpose: portable character profiles for RP/chat.
- Identification: V2 uses `spec: "chara_card_v2"`, `spec_version: "2.0"`; V3 uses `spec: "chara_card_v3"`, `spec_version: "3.0"` with fields under `data`.
- Packaging:
  - V1: flat JSON; commonly embedded in PNG/APNG EXIF under “Chara” (base64).
  - V2: JSON with `data:{...}` (same PNG embedding lineage as V1 also common).
  - V3: preferred PNG/APNG tEXt chunk named `ccv3` (base64 JSON); also supports plain JSON and `*.charx` (zip with `card.json` and assets).
- Fields:
  - V1 (flat): `name`, `description`, `personality`, `scenario`, `first_mes`, `mes_example`.
  - V2 adds: `creator_notes`, `system_prompt`, `post_history_instructions`, `alternate_greetings`, `character_book`, `tags`, `creator`, `character_version`, `extensions`.
  - V3 adds: `assets[]`, `nickname`, `creator_notes_multilingual`, `source[]`, `group_only_greetings`, `creation_date`, `modification_date`.
- Import rules (UI): prefer V3 `ccv3` if present; otherwise detect V2 (`spec:"chara_card_v2"`), else V1. Preserve unknown keys and all `extensions` on round-trips.
- Quick checks: import a PNG (V1/V2/V3), JSON, and CHARX; validate greeting swipes, lorebook, and system/jailbreak replacements.
