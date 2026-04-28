# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Script

```sh
python export_chats.py
```

Run from the workspace root. There is no build system, test suite, or CI pipeline.

## Architecture

`export_chats.py` is a self-contained stdlib-only Python script organized into five sections:

1. **Workspace resolution** — locates VS Code's `workspaceStorage` directory for the current OS, finds the hash directory for the current workspace by matching `workspace.json` metadata, then returns all `chatSessions/*.jsonl` files.
2. **Utility helpers** — timestamp conversion, slugification, path relativization (`make_relative`/`make_paths_relative`), and Markdown HTML escaping.
3. **JSONL parsing** (`parse_session`, `parse_response`) — reads each JSONL line as a keyed entry (`kind`, `k`, `v`) and reconstructs prompt-response groups. `parse_response` handles multiple item kinds: `toolInvocationSerialized`, `progressTaskSerialized`, `questionCarousel`, `inlineReference`, and plain text/thinking responses.
4. **Markdown rendering** (`render_session`) — converts parsed groups into a Markdown file with a metadata table, table of contents, and per-prompt sections including timing.
5. **Main** — orchestrates the pipeline and writes output to `chat/`.

## Key Constraints

- **Python 3.10+** — `match/case` is used; preserve modern Python compatibility.
- **stdlib only** — no external dependencies; keep it that way.
- **Cross-platform path handling** — the `os.name`/`sys.platform` branches and Windows `C:\` URL-encoding in `find_workspace_hash_dir` are intentional.
- **VS Code internals** — parsing relies on the JSONL schema of VS Code's internal workspace storage. Schema changes in VS Code can silently break parsing.
- Use `pathlib.Path` for all filesystem operations.
