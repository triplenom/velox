# Running Velox with your data

## Tools

File, Python, shell, and terminal tools run with the permissions of the account running Velox. They are not an operating-system sandbox. Enable only the tool categories needed for a task and use a disposable workspace for unfamiliar code. Prompted write scopes and task ownership checks do not restrict general filesystem access.

## Model connections

Prompts, selected context, attachments, and tool results are sent to the configured model endpoint. A self-hosted endpoint can still be on another machine. Check the destination before using private data. Read-only account connectors can supply context to a model; read-only access does not keep that context on your computer.

## Local data and credentials

The working directory contains application settings, conversations, agent records, working files, and logs. Do not commit or publish that directory. Use normal account permissions and disk protection appropriate to its contents.

Endpoint API keys are stored in application configuration. Account credential files use fixed-byte XOR obfuscation, not encryption. Treat both as readable by anyone who can access the data directory. This alpha does not use an OS keychain.

Debug exports redact recognized credential patterns, but can still contain private conversation and tool content. Review them before sharing. Do not put credentials or personal transcripts in issues or pull requests.
