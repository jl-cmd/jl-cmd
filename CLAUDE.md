# CLAUDE.md

## Code Rules

@~/.claude/docs/CODE_RULES.md

## Hook System Architecture

### Hook Directory Structure

```
~/.claude/hooks/
|-- notification/
|   |-- attention-needed-notify.py
|   |-- claude-notification-handler.py
|   +-- notification_utils.py
|-- advisory/
|   |-- refactor-guard.py
|   +-- migration-safety-advisor.py
|-- validation/
|   |-- hook-format-validator.py
|   +-- mypy_validator.py
|-- lifecycle/
|   |-- config-change-guard.py
|   +-- session-end-cleanup.py
|-- blocking/
|   |-- block-main-commit.py
|   |-- code-rules-enforcer.py
|   |-- destructive-command-blocker.py
|   |-- hedging-language-blocker.py
|   |-- pr-description-enforcer.py
|   |-- sensitive-file-protector.py
|   |-- tdd-enforcer.py
|   +-- write-existing-file-blocker.py
|-- workflow/
|   |-- auto-formatter.py
|   +-- investigation-tracker-reset.py
+-- validators/
    |-- (validation check modules)
    +-- test_files/
```

### Event Types

| Event | When | Can Block? |
|-------|------|------------|
| SessionStart | Session begins | No |
| UserPromptSubmit | User sends message | No (advisory) |
| PreToolUse | Before tool execution | Yes (exit 2) |
| PostToolUse | After tool execution | No |
| Stop | Session ends | Yes (exit 2) |

### Adding a New Hook

1. Create the hook file in the appropriate subfolder
2. Register in `settings.json` under the correct event type
3. Set explicit timeouts (10-30 seconds)
4. PreToolUse hooks use `matcher` to scope which tools they fire on
5. Print output = context injected into Claude's conversation
6. Advisory hooks exit 0; blocking hooks exit 2

## Bulk Edits

When performing bulk updates (update all, replace all, change all, fix all, rename all): use a Python script with `--preview`/`--apply` flags instead of making line-by-line edits.

## Code Standards Reminder

When writing or modifying code, follow these mandatory rules:

1. No comments -- self-documenting names only
2. No magic values -- everything named and in config
3. No abbreviations -- full words always
4. Complete type hints -- all parameters and returns typed
5. Centralized config -- constants in config/, imported everywhere
6. Search before create -- check existing constants before defining new ones
7. All imports shown -- every code block includes its imports
8. Self-contained components -- components own their modals/toasts/state

These rules apply to code you write or modify. Do not fix violations in untouched code unless explicitly instructed.
