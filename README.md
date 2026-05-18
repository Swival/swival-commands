# swival-commands

A small collection of user-contributed commands for [Swival](https://swival.dev).

These commands are meant to be installed into `~/.config/swival/commands/` and used with `! command_name` inside Swival.

Some commands in this repo are executable scripts. When you invoke them, Swival runs the script and uses its output. Others are plain text prompt files. When you invoke those, Swival injects the file contents into the prompt it sends to the model.

There are only a few commands here today, but the idea is simple: drop in useful commands as single files, keep the good ones, and grow your personal command toolbox over time.

## What this repo gives you

- Ready-to-install Swival commands
- A mix of tiny utility commands and heavier workflow commands
- Simple file-based customization: one command = one file
- A place to collect commands you may want to reuse across projects

## Requirements

You just need [Swival](https://swival.dev) installed on your machine.

If you want to verify that on your own machine, run:

```sh
swival --help
```

Swival supports `!` commands in interactive use, and in one-shot mode when you pass `--oneshot-commands`.

## Install

Create the commands directory if it does not already exist:

```sh
mkdir -p ~/.config/swival/commands
```

Copy all commands from this repo into your Swival commands directory:

```sh
cp commands/* ~/.config/swival/commands/
```

Make the script-based commands executable:

```sh
chmod +x ~/.config/swival/commands/hello
chmod +x ~/.config/swival/commands/pr-review
chmod +x ~/.config/swival/commands/commit
chmod +x ~/.config/swival/commands/explain
chmod +x ~/.config/swival/commands/test
chmod +x ~/.config/swival/commands/changes
```

That is it. Swival will pick them up by filename, so `commands/hello` becomes `! hello`, `commands/audit` becomes `! audit`, and so on.

## How to use commands

### In an interactive Swival session

Start Swival:

```sh
swival
```

Then call a command by typing `!` followed by the filename:

```text
! hello
```

You can also pass arguments to script-based commands:

```text
! pr-review 123
! pr-review https://github.com/owner/repo/pull/123
```

### In one-shot mode

If you want to use `!` commands without starting a full interactive session, enable one-shot command dispatch:

```sh
swival --oneshot-commands "! hello"
```

A more practical example:

```sh
swival --oneshot-commands "! pr-review https://github.com/owner/repo/pull/123"
```

## Included commands

### `hello`

A tiny smoke-test command.

Use it when you want to confirm that your command installation is working.

Example:

```text
! hello
```

Expected behavior: Swival replies with exactly:

```text
Hello from swival-commands.
```

### `pr-review`

A pull request review helper.

This command builds a detailed review prompt for Swival. You give it a pull request number or a full pull request URL, and it instructs Swival to inspect the PR, read the discussion, and produce a thoughtful review.

Examples:

```text
! pr-review 42
! pr-review https://github.com/owner/repo/pull/42
```

Good use cases:

- Reviewing your own PR before asking for human feedback
- Getting a second opinion on a teammate's PR
- Checking whether discussion threads were actually addressed

### `audit`

*** Superseded by the `/audit` built-in command since Swival 1.0.3 ***

A whole-repository logic and security audit prompt.

This is for situations where you want Swival to inspect an entire repository and only report findings it can actually prove from the code. If it finds real issues, it is instructed to produce one markdown report and one patch file per finding in `audit-findings/`.

Example:

```text
! audit
```

Good use cases:

- Sanity-checking a codebase before release
- Looking for real logic bugs, trust-boundary mistakes, and data integrity issues
- Running a strict audit instead of asking for broad suggestions

### `audit-c`

*** Superseded by the `/audit` built-in command since Swival 1.0.3 ***

A stricter audit prompt for C projects.

This command is like `audit`, but specialized for C code. It pushes Swival to look for memory-safety bugs, undefined behavior, ownership mistakes, integer issues, and other C-specific correctness failures.

Example:

```text
! audit-c
```

Good use cases:

- Auditing C or mixed C codebases
- Looking for out-of-bounds access, lifetime bugs, and UB with concrete impact
- Getting patch-oriented findings instead of generic code review notes

## Executable commands vs text commands

This repo currently contains both styles:

- Executable scripts: `hello`, `pr-review`, `commit`, `explain`, `test`, `changes`
- Text prompt files: `audit`, `audit-c`

That difference matters mostly when you install them:

- Scripts should be executable with `chmod +x`
- Text files can be copied as-is

From the user's point of view, they are invoked the same way with `! command_name`.

## Practical examples

A few realistic ways you might use this repo:

### 1. Check that command loading works

```sh
swival --oneshot-commands "! hello"
```

### 2. Review a pull request from the terminal

```sh
swival --oneshot-commands "! pr-review https://github.com/owner/repo/pull/87"
```

### 3. Open Swival and run an audit interactively

```sh
swival
```

Then:

```text
! audit
```

### 4. Audit a C project

```sh
cd ~/src/some-c-project
swival
```

Then:

```text
! audit-c
```

### 5. Generate a commit message

Stage your changes, then:

```sh
swival --oneshot-commands "! commit"
```

### 6. Explain a file

```sh
swival --oneshot-commands "! explain src/parser.ts"
```

### 7. Write tests for a module

```sh
swival --oneshot-commands "! test lib/auth.py"
```

### 8. Generate release notes

```sh
swival --oneshot-commands "! changes"
```

### `commit`

A commit message generator that analyzes your staged changes and writes a clear, well-structured commit message.

This command reads your staged diff, understands the intent of the change, and produces a commit message following conventional commit style. If your changes should be split into multiple commits, it will tell you.

Example:

```text
! commit
```

Good use cases:

- Writing clear commit messages instead of "fix stuff" or "wip"
- Making sure commit messages match the project's existing style
- Getting a second opinion before committing a large change
- Learning what a good commit message looks like for your changes

### `explain`

A code explanation assistant that reads a file or directory and produces a thorough explanation of what the code does.

This is for situations where you need to understand unfamiliar code quickly. It reads the target, traces the logic, identifies patterns, and explains the code in clear prose.

Example:

```text
! explain src/parser.ts
! explain lib/utils
```

Good use cases:

- Onboarding to a new codebase or unfamiliar module
- Understanding code you have not touched in a long time
- Preparing documentation or knowledge sharing for your team
- Reviewing a file before making changes to it

### `test`

A test-writing assistant that reads a source file and writes thorough, useful tests for it.

This command analyzes the code, identifies the public interface, chooses appropriate test strategies, and writes a complete test file. It follows the testing conventions already used in your project when possible.

Example:

```text
! test src/parser.ts
! test lib/auth.py
```

Good use cases:

- Adding tests to existing code that has none
- Getting a starting point for test coverage on a new module
- Identifying edge cases you might not have thought of
- Learning testing patterns for a language or framework you are less familiar with

### `changes`

A changelog and release notes generator that summarizes recent changes in your repository.

This command reads your git history, classifies every change, and produces an organized summary in changelog format. It respects the existing changelog style if your project has one.

Example:

```text
! changes
! changes v1.0.0..HEAD
```

Good use cases:

- Writing release notes before publishing a new version
- Summarizing what changed for a team update or standup
- Preparing a CHANGELOG entry after a sprint
- Auditing what went into a release branch

## Adding more commands later

This repo is intentionally simple. Each command lives in its own file under `commands/`, and the filename becomes the command name.

That means future additions stay easy to understand:

- browse `commands/`
- copy the files you want
- make scripts executable if needed
- run them with `! name`

You do not need a plugin manager or a special packaging step.

## Troubleshooting

If `! hello` does not work, check these first:

- the files are in `~/.config/swival/commands/`
- script commands are executable
- you are running `swival`, not a different wrapper
- if you are using one-shot mode, you passed `--oneshot-commands`

Helpful checks:

```sh
ls -l ~/.config/swival/commands
swival --help
```

## Why this repo exists

Swival gets much more useful when you can save workflows as reusable commands instead of rewriting the same prompt over and over.

This repo is a lightweight place to collect those workflows, share them, and make them easy to install.
