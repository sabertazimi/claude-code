---
name: git-commit
description: Generates Conventional Commits messages. Use when the user says "commit", "git commit", or asks to commit changes, wants to create a commit, or when work is complete and ready to commit.
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git diff:*), Bash(git branch:*), Bash(git log:*), Bash(git commit:*)
license: MIT
---

# Git Commit

Generate Conventional Commits messages.

## When to Use This Skill

Activate this skill when:

- The user types "commit" or "git commit" (with or without slash command)
- The user says "commit this" or "let's commit"
- The user asks to create a commit message
- Work is complete and ready to commit
- The user mentions committing or pushing changes

## Critical Rules

**IMPORTANT: NEVER EVER ADD CO-AUTHOR TO THE GIT COMMIT MESSAGE**
**NEVER mention Claude Code in commit messages**

## Workflow

### 1. Gather Context

First, read `CLAUDE.md` in the project root to check if the user has defined Git commit preferences:

Look for sections like:

- `## Commit`
- `## Commit Guidelines`
- `## Git Commits`
- `## Git Commit Preferences`
- `## Commit Message Preferences`
- Or any section mentioning commit format/style

If found, parse the natural language preferences to understand:

- Whether `scope` is required/optional/omitted
- Whether `body` is required/optional/omitted
- Which `type` values are preferred
- Any other formatting preferences

If no preferences are defined, use the default Conventional Commits format.

Then, collect information about the current git state:

```bash
# Current git status
git status

# Current git diff (staged changes)
git diff --staged

# Recent commits for context
git log --oneline -10

# Current branch
git branch --show-current
```

### 2. Analyze Changes

Analyze the diff content to understand the nature and purpose of the changes

### 3. Create Enhanced Commit Message

Generate 3 commit message candidates based on the changes:

- Each candidate should be concise, clear, and capture the essence of the changes
- Follow Conventional Commits format, **adapting to user preferences discovered in step 1** (scope, body, type requirements)

**Format:**

```gitcommit
type(scope): concise subject line describing what changed

[Summary of the modifications]
```

### 4. Execute Commit

**IMPORTANT: Do not use `git add -A` or `git add .`**
Commit only the files that are already staged and understood.

Select the most appropriate commit message from the 3 candidates and explain the reasoning for your choice,
**commit with heredoc** (for multi-line messages):

```bash
git commit -m "$(cat <<'EOF'
type(scope): subject line

[modifications summary]
EOF
)"
```

## Important Notes

- **Use heredoc for multi-line commits** - Ensures proper formatting
- **Be specific** in summaries
- **Think about the reader** - someone explaining this code repository in 6 months
- **No co-authors** - Never add "Co-Authored-By" or mention Claude Code
