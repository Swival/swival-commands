## Workflow

- install: `mkdir -p ~/.config/swival/commands && cp commands/* ~/.config/swival/commands/ && chmod +x ~/.config/swival/commands/hello && chmod +x ~/.config/swival/commands/pr-review`
- test all: `swival --oneshot-commands "! hello"`
- test file: `swival --oneshot-commands "! <command_name>"`
- test case: `swival --oneshot-commands "! <command_name> <arg>"`
- lint: `:`
- format: `:`
- after every edit: `swival --oneshot-commands "! hello"`

## Conventions

- Each Swival command is one file under `commands/`, and the filename becomes the `!` command name.
- This repo mixes executable POSIX `sh` commands and plain text prompt/spec files; preserve that distinction when adding or installing commands.
- Larger prompt/spec commands use explicit sectioned instructions with named phases instead of short freeform prompts.

## Commit & Pull Request Guidelines

Use short, capitalized, imperative commit subjects with no scope prefix or ticket tag. Recent examples: `Add a README.md file`, `Import`, `Init`.

There is no checked-in PR template. Keep pull requests small and clear, and describe the command added or changed plus any example invocation needed to understand it.
