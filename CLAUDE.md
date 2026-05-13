# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Script

```sh
python export_chats.py
```

Run from the workspace root. There is no build system, test suite, or CI pipeline.

## Architecture

`export_chats.py` is a single-file, stdlib-only Python utility organized into four sections:

1. **Workspace resolution** — locates VS Code's `workspaceStorage` directory by OS, then matches the current working directory against `workspace.json` metadata files to find the workspace-specific hash directory.
2. **Utility helpers** — timestamp conversion, slugification, overlap detection for deduplicating partial responses, and path relativization (converts `file://` URIs to workspace-relative paths with emoji prefixes).
3. **JSONL parsing** — `parse_session()` reads a `.jsonl` file entry-by-entry; each entry has a `kind` (int) and optional `k`/`v` fields. Kind 0 = session metadata, kind 1 = mutations (title, result timings), kind 2 = request/response arrays. `parse_response()` handles the nested item types within a response: `toolInvocationSerialized`, `progressTaskSerialized`, `questionCarousel`, `inlineReference`, and plain text/thinking items.
4. **Markdown rendering** — `render_session()` produces a file named `{timestamp}_{slug}.md` in `chat/`, with an HTML metadata table, a numbered TOC, and per-prompt sections including timing info and escaped HTML.

Key globals: `WS_ROOT` (workspace root = `cwd()`), `DST_DIR = "chat"`, `MAX_LEN = 50`.

## Conventions

- Python 3.10+ (`match/case`, union type hints `X | Y`). Do not regress compatibility.
- Use `pathlib.Path` for all filesystem logic.
- Keep type hints and single-line docstrings on public functions.
- The script is intentionally self-contained — do not add third-party dependencies.
- Path handling has explicit `os.name == "nt"` / `sys.platform == "darwin"` branches; keep these intact for cross-platform support.

## Pitfalls

- The JSONL schema is VS Code-internal and undocumented — it can change across VS Code releases.
- Workspace detection matches `cwd()` against the `folder` field in `workspace.json`; the Windows branch strips the drive letter (`C:\` → `/`) before comparing.
- `parse_response()` deduplicates tool calls via `call_ids` and merges partial text responses using `overlap_length()` to avoid double-rendering streamed chunks.
- Output format should remain human-readable and commit-friendly Markdown; avoid changes that break diff readability.
