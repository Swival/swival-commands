# swival-commands

A small collection of user-contributed commands for [Swival](https://swival.dev).

These commands are meant to be installed into `~/.config/swival/commands/` and used with `!command_name` inside Swival.

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
chmod +x ~/.config/swival/commands/pr-review2
chmod +x ~/.config/swival/commands/issue-respond
chmod +x ~/.config/swival/commands/commit
chmod +x ~/.config/swival/commands/explain
chmod +x ~/.config/swival/commands/test
chmod +x ~/.config/swival/commands/changes
chmod +x ~/.config/swival/commands/debug
chmod +x ~/.config/swival/commands/refactor
chmod +x ~/.config/swival/commands/docs
chmod +x ~/.config/swival/commands/ci
```

The `audit-light` command is a plain text prompt file and does not need to be made executable.

That is it. Swival will pick them up by filename, so `commands/hello` becomes `!hello`, `commands/debug` becomes `!debug`, and so on.

## How to use commands

### In an interactive Swival session

Start Swival:

```sh
swival
```

Then call a command by typing `!` followed by the filename:

```text
!hello
```

You can also pass arguments to script-based commands:

```text
!pr-review 123
!pr-review https://github.com/owner/repo/pull/123
```

### In one-shot mode

If you want to use `!` commands without starting a full interactive session, enable one-shot command dispatch:

```sh
swival --oneshot-commands "!hello"
```

A more practical example:

```sh
swival --oneshot-commands "!pr-review https://github.com/owner/repo/pull/123"
```

## Included commands

### `hello`

A tiny smoke-test command.

Use it when you want to confirm that your command installation is working.

Example:

```text
!hello
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
!pr-review 42
!pr-review https://github.com/owner/repo/pull/42
```

Good use cases:

- Reviewing your own PR before asking for human feedback
- Getting a second opinion on a teammate's PR
- Checking whether discussion threads were actually addressed

### `pr-review2`

A stricter pull request review variant intended for PRs opened by bots or contributors who are unfamiliar with the project.

This command builds the same kind of review prompt as `pr-review`, but starts from the assumption that the change is likely wrong and pushes Swival to verify every claim carefully against the repository. If the PR is not worth merging, it will say so and close it.

Examples:

```text
!pr-review2 42
!pr-review2 https://github.com/owner/repo/pull/42
```

Good use cases:

- Triaging AI-generated pull requests that touch unfamiliar areas of the project
- Filtering drive-by contributions that do not match local conventions
- Catching plausible-looking changes that do not actually fit the project
- Closing low-value PRs with a clear, kind explanation

### `issue-respond`

An autonomous GitHub issue response assistant.

Give it an issue number or a full issue URL, and it instructs Swival to read the report, check the repository, search prior issues and pull requests, consult the documentation, and post a thoughtful reply on the issue using `gh`. If the issue turns out to be a duplicate, already fixed, out of scope, or clearly not actionable, the command will also close it.

Examples:

```text
!issue-respond 42
!issue-respond https://github.com/owner/repo/issues/42
```

Good use cases:

- Triaging incoming issues on a project that gets more reports than maintainers can handle
- Pointing reporters at existing documentation or prior threads that already cover their question
- Asking for missing reproduction details in a precise, friendly way
- Confirming real bugs with concrete file and line references before maintainers look at them

### `commit`

A commit message generator that analyzes your staged changes and writes a clear, well-structured commit message.

This command reads your staged diff, understands the intent of the change, and produces a commit message that matches the project's existing style when history is available. If your changes should be split into multiple commits, it will tell you.

Example:

```text
!commit
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
!explain src/parser.ts
!explain lib/utils
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
!test src/parser.ts
!test lib/auth.py
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
!changes
!changes v1.0.0..HEAD
```

Good use cases:

- Writing release notes before publishing a new version
- Summarizing what changed for a team update or standup
- Preparing a CHANGELOG entry after a sprint
- Auditing what went into a release branch

### `debug`

A root-cause debugging assistant for bugs, failing tests, and broken commands.

This command pushes Swival to reproduce the failure, localize the cause, make the smallest correct fix, add or update tests when useful, and verify the result.

Example:

```text
!debug npm test fails in auth.spec.ts
!debug login redirects loop after token refresh
```

Good use cases:

- Turning a failing test or command into a concrete fix
- Investigating regressions without jumping to a workaround
- Explaining the actual root cause after a bug is fixed
- Keeping verification tied to the original failure

### `refactor`

A scoped refactoring assistant for behavior-preserving cleanup.

This command asks Swival to define the refactor boundary, read call sites and tests, preserve public contracts, avoid unrelated cleanup, and report how unchanged behavior was verified.

Example:

```text
!refactor simplify src/cache.ts without changing behavior
!refactor split the payment parser into smaller functions
```

Good use cases:

- Making complex code easier to read before adding a feature
- Removing duplication while keeping behavior stable
- Improving testability without changing public APIs
- Keeping refactor diffs focused and reviewable

### `docs`

A documentation assistant that writes or updates docs from repository evidence.

This command makes Swival read the source of truth first, choose the right documentation shape, avoid invented claims, and verify commands, paths, options, and examples.

Example:

```text
!docs document the plugin configuration flow
!docs update README setup steps for Docker
```

Good use cases:

- Adding practical setup or usage documentation
- Updating stale docs after a feature change
- Creating reference material from implementation details
- Writing examples that match real project commands

### `audit-light`

A lightweight, static-prompt counterpart to Swival's built-in `/audit` command.

Where `/audit` is a fully orchestrated, multi-stage security audit, `audit-light` is a single prompt that asks the model to run the same staged, evidence-based audit in one shot: profile the repository, triage files by attack surface, deep-review only the ones with credible risk, treat every candidate finding as a hypothesis that must be verified against the actual code, and produce reviewer-ready reports with minimal patches for the bugs it can reproduce. It is cheaper and faster than `/audit`, and is meant for quick passes rather than full audits. It is strict about scope and will reject defense-in-depth, style, missing-test, and correctness-only issues.

Example:

```text
!audit-light
!audit-light focus on src/auth and src/parser
```

Good use cases:

- A quick security pass on a module before shipping it, without the cost of a full `/audit` run
- Getting a fast second opinion on a security-sensitive change
- Triaging a repository you are not yet familiar with for obvious vulnerabilities
- Producing minimal, evidence-backed fixes instead of broad speculative hardening

If you need a deeper, more rigorous review, use Swival's built-in `/audit` command instead.

### `ci`

A CI and validation triage assistant.

This command focuses Swival on the failing job, script, log, or validation command. It classifies whether the cause is code, tests, tooling, environment, dependency drift, configuration, or flakiness before making a minimal fix.

Example:

```text
!ci pytest fails on Python 3.12 in GitHub Actions
!ci npm run lint fails only in CI
```

Good use cases:

- Diagnosing broken GitHub Actions, GitLab CI, or local validation scripts
- Separating product regressions from infrastructure failures
- Fixing toolchain and dependency mismatches safely
- Avoiding lazy fixes that skip or weaken useful checks

## Executable commands vs text commands

This repo can contain both styles:

- Executable scripts print a prompt dynamically and can validate arguments before Swival sees it.
- Text prompt files are copied as-is and injected directly into the Swival prompt.

Most checked-in commands are executable scripts. `audit-light` is a plain text prompt file. If you add more text prompt files later, copy them as-is and only run `chmod +x` for scripts.

From the user's point of view, both styles are invoked the same way with `!command_name`.

## Practical examples

A few realistic ways you might use this repo:

### 1. Check that command loading works

```sh
swival --oneshot-commands "!hello"
```

### 2. Review a pull request from the terminal

```sh
swival --oneshot-commands "!pr-review https://github.com/owner/repo/pull/87"
```

### 3. Generate a commit message

Stage your changes, then:

```sh
swival --oneshot-commands "!commit"
```

### 4. Explain a file

```sh
swival --oneshot-commands "!explain src/parser.ts"
```

### 5. Write tests for a module

```sh
swival --oneshot-commands "!test lib/auth.py"
```

### 6. Generate release notes

```sh
swival --oneshot-commands "!changes"
```

### 7. Debug a failing command

```sh
swival --oneshot-commands "!debug npm test fails in auth.spec.ts"
```

### 8. Refactor a module safely

```sh
swival --oneshot-commands "!refactor simplify src/cache.ts without changing behavior"
```

### 9. Update documentation from code

```sh
swival --oneshot-commands "!docs document the plugin configuration flow"
```

### 10. Triage a CI failure

```sh
swival --oneshot-commands "!ci pytest fails on Python 3.12 in GitHub Actions"
```

### 11. Review a suspicious bot-authored PR

```sh
swival --oneshot-commands "!pr-review2 https://github.com/owner/repo/pull/87"
```

### 12. Respond to a GitHub issue

```sh
swival --oneshot-commands "!issue-respond https://github.com/owner/repo/issues/42"
```

### 13. Run a focused security audit

```sh
swival --oneshot-commands "!audit-light focus on src/auth"
```

## Adding more commands later

This repo is intentionally simple. Each command lives in its own file under `commands/`, and the filename becomes the command name.

That means future additions stay easy to understand:

- browse `commands/`
- copy the files you want
- make scripts executable if needed
- run them with `!name`

You do not need a plugin manager or a special packaging step.

## Troubleshooting

If `!hello` does not work, check these first:

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
