# vscode-chat-export
Export VS Code chat sessions to structured, commit-ready Markdown.

Simply download and run the Python script in your workspace root:

``` sh
python export_chats.py
```

Your chats will exported as Markdown in the `chat` directory.

## About the Script

VS Code stores chat sessions as [JSONL files](https://jsonlines.org/) in workspace storage (see `Developer: Open Chat Storage Folder` in the [Command Palette][CP]).
While these logs contain the full history of each interaction, they are not designed to be human-readable or easily version-controlled.

[CP]: https://code.visualstudio.com/docs/getstarted/userinterface#_command-palette

This script converts those JSONL logs into clean, well-structured Markdown files that preserve:

* User prompts
* Assistant responses
* Tool interactions (e.g., file reads/writes)
* Inline reasoning ("thinking")
* Metadata such as timestamps, model identifiers, and elapsed time

The goal is to make AI-assisted development more transparent by capturing **intent and context alongside code changes**. The resulting Markdown files are suitable for committing to a repository, allowing both individuals and teams to understand *why* changes were made—not just *what* changed.

In addition to documentation, these Markdown files can also be reused as context in future chats, enabling a more continuous and informed workflow.

## Why Not Use Built-in Export?

VS Code already provides:

* **`Chat: Export Chat...`** — exports chats as JSON
    * Useful for re-importing sessions
    * Not human-readable or commit-friendly
* **"Copy All"** (right-click in chat panel) — copies Markdown
  * Human-readable
  * But lacks structure, metadata, and consistency

This project aims to combine the best of both by preserving the **completeness of the raw logs** while producing **readable, well-organized Markdown**.

The [export_chats.py](export_chats.py) script is a work in progress and probably has lots of bugs.
Feel free to open a pull request if you would like to contribute.

### Use Cases

* 📜 Maintain a history of AI-assisted decisions in your repo
* 👥 Help teammates understand how features were developed
* 🔍 Review prompts and responses during code review
* 🔁 Reuse prior conversations as context for future work
* 🧠 Reflect on and improve prompting strategies

## Related Projects

* [specstoryai/getspecstory](https://github.com/specstoryai/getspecstory) --
  VS Code extension that automatically stores chats as Markdown.
    - *In my experience, it was not reliable, and I wanted more detailed output.*
* [ruifigueira/chat-to-markdown](https://github.com/ruifigueira/chat-to-markdown) --
  Converts exported JSON chats to Markdown.
    - *Requires manual export and does not work directly from JSONL logs.*
* [vaneeic/chat-to-markdown](https://github.com/vaneeic/chat-to-markdown) --
  VS Code extension with similar goals.
    - *Limited adoption and not recently maintained.*
