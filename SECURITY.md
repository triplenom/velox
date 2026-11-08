# Running Velox with your data

## Tools

File, Python, shell, and terminal tools run with the permissions of the account running Velox. They are not an operating-system sandbox. Enable only the tool categories needed for a task and use a disposable workspace for unfamiliar code. Prompted write scopes and task ownership checks do not restrict general filesystem access.

Computer Use is off by default for every chat. When you turn it on for a chat, that chat and the agents it starts can see your screen and send real mouse and keyboard input to whatever window has focus. With it off, the model is only *instructed* not to touch your mouse, keyboard or window focus; System tools could still do so if a model ignored that instruction. Hold Ctrl+P+End at any time to abort in-flight Computer Use input and pause every running chat and agent.

## Model connections

Prompts, selected context, attachments, and tool results are sent to the configured model endpoint. A self-hosted endpoint can still be on another machine. Check the destination before using private data. Read-only account connectors can supply context to a model; read-only access does not keep that context on your computer.

## Local data and credentials

The working directory contains conversations, agent records, working files, UI state, and logs. Do not commit or publish that directory. Use normal account permissions and disk protection appropriate to its contents.

`config.json` (beside `velox.py` unless `--config` names another file) holds all application settings, endpoint API keys, and account credentials (Google OAuth clients and tokens, the Gmail app password) as plain JSON. It is not encrypted; treat it as readable by anyone who can read that file and protect it like a password store. The repository's `.gitignore` excludes it, but take care not to commit, sync, or share it. This alpha does not use an OS keychain.

Debug exports redact recognized credential patterns, but can still contain private conversation and tool content. Review them before sharing. Do not put credentials or personal transcripts in issues or pull requests.
