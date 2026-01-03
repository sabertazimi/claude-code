---
name: Git Commit
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

First, collect information about the current state:

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
- Prefer Conventional Commits format

**Format:**

```gitcommit
type(scope): concise subject line describing what changed

[Summary of the modifications]
```

**Conventional Commits Types:**

- **feat**: new features
- **fix**: bug fixes
- **docs**: documentation changes
- **style**: formatting, missing semicolons, etc.
- **refactor**: code restructuring without changing functionality
- **test**: adding or updating tests
- **chore**: maintenance tasks, dependencies, build process
- **perf**: performance improvements
- **ci**: continuous integration changes

### 4. Execute Commit

**IMPORTANT: Do not use `git add -A` or `git add .`**
Commit only the files that are already staged and understood.

Select the most appropriate commit message from the 3 candidates and explain the reasoning for your choice,
**Commit with heredoc** (for multi-line messages):

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
