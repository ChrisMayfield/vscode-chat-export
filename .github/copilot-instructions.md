# Project Guidelines

## Scope and Intent
- This repository is a single-script Python utility that exports VS Code Copilot Chat JSONL sessions to Markdown.
- Keep changes focused on chat export behavior and output quality; avoid adding unrelated infrastructure.

## Architecture
- Main entrypoint: `export_chats.py`.
- The script is intentionally self-contained (stdlib only) and organized by sections:
  - Workspace resolution
  - Utility helpers
  - JSONL parsing
  - Markdown rendering
  - Main execution
- Output directory is currently `chat/` (from `DST_DIR` in `export_chats.py`).

## Build and Run
- Run from workspace root:
  - `python export_chats.py`
- There is no build system, test suite, or CI pipeline in this repo.

## Conventions
- Python 3.10+ features are used (for example `match/case`), so preserve compatibility with modern Python.
- Prefer `pathlib.Path` for filesystem logic.
- Keep type hints and concise docstrings on functions.
- Preserve the existing parsing flow and rendering structure unless a task explicitly requires refactoring.
- For output format changes, keep Markdown human-readable and commit-friendly.

## Agent-Critical Pitfalls
- The script relies on VS Code internal workspace storage layout and JSONL schema; updates to VS Code internals can break parsing.
- Workspace detection depends on matching the current working directory to `workspace.json` metadata.
- Keep path handling cross-platform (`os.name` and `sys.platform` branches are intentional).

## Reference Docs
- Project overview and usage context: [README.md](../README.md)
- Historical development context: [init/README.md](../init/README.md)
- Example exported chats and output format references: [chat/](../chat/)
