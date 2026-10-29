# 🎯 Claude Code: Complete Beginner Learning Program

*A self-paced course to master Claude Code (Anthropic's AI coding CLI)*

---

## 📋 Course Overview

| Module | Topic | Est. Time | Prerequisites |
|--------|-------|-----------|---------------|
| 0 | **Setup & First Run** | 15 min | None |
| 1 | **Core Concepts** | 20 min | Module 0 |
| 2 | **Essential Commands** | 30 min | Module 1 |
| 3 | **Slash Commands Deep Dive** | 25 min | Module 2 |
| 4 | **File Operations & Editing** | 30 min | Module 2 |
| 5 | **Git Integration** | 20 min | Module 2 |
| 6 | **Hooks & Automation** | 30 min | Module 3 |
| 7 | **MCP Servers** | 20 min | Module 3 |
| 8 | **Advanced Workflows** | 35 min | Modules 4–6 |
| 9 | **Best Practices & Tips** | 20 min | All prior |
| **Total** | | **~4 hours** | |

---

## 📦 Module 0: Setup & First Run

### Prerequisites
- **Node.js 18+** — `node --version` to check
- **Git** — `git --version`
- **Terminal** — Windows: Git Bash / WSL / PowerShell; Mac/Linux: any terminal

### Installation

```bash
# Install globally via npm
npm install -g @anthropic-ai/claude-code

# Verify installation
claude --version
# Should show: claude X.Y.Z
```

### Authentication

```bash
# Start Claude Code — opens browser for OAuth
claude

# Or use API key (for CI/headless)
export ANTHROPIC_API_KEY="sk-ant-..."
claude
```

### First Interaction

```
> claude
✦ Welcome to Claude Code!

> help
# Shows available commands

> "Create a hello world Python file"
# Try a natural language request
```

**✅ Checkpoint**: You see the Claude prompt (`>`), can run `help`, and Claude creates a file.

---

## 🧠 Module 1: Core Concepts

### What Is Claude Code?

| Aspect | Description |
|--------|-------------|
| **Type** | AI-powered coding agent in your terminal |
| **Model** | Claude 3.5 Sonnet / Opus / Haiku (configurable) |
| **Context** | Your entire codebase + conversation history |
| **Tools** | Read, Write, Edit, Bash, Grep, Glob, Task, WebFetch, etc. |
| **Cost** | Pay-per-token via Anthropic API (or subscription) |

### Key Terminology

| Term | Meaning |
|------|---------|
| **Session** | One `claude` invocation — maintains context |
| **Tool** | An action Claude can take (Read, Write, Bash, etc.) |
| **Slash Command** | Built-in commands starting with `/` |
| **Hook** | Auto-run scripts on events (pre-write, post-edit, etc.) |
| **MCP Server** | External tool server (databases, APIs, browsers) |
| **Context Window** | Max tokens Claude can "see" at once (~200k) |

### How It Works (Mental Model)

```
You type → Claude plans → Executes tools → Shows results → You respond → Repeat
```

Claude **reads your codebase** to understand context before acting.

---

## ⌨️ Module 2: Essential Commands

### Navigation & Exploration

| Command | Purpose | Example |
|---------|---------|---------|
| `ls`, `cd`, `pwd` | Standard shell (work in any dir) | `cd src/components` |
| `glob "**/*.ts"` | Find files by pattern | `glob "**/*.test.ts"` |
| `grep "TODO"` | Search codebase | `grep -n "FIXME" src/` |
| `cat file.ts` | Read file | `cat package.json` |

### File Operations

```bash
# Read a file
> Read package.json

# Write new file
> Write hello.py with: print("Hello!")

# Edit existing file (exact string replacement)
> Edit src/app.ts: replace "old" with "new"

# Multi-edit
> Edit src/app.ts: replace_all "console.log" with "logger.info"
```

### Running Commands

```bash
# Any bash command
> Bash npm test
> Bash git status
> Bash "find . -name '*.log' -delete"

# Background processes
> Bash npm run dev --background
```

### Search & Analysis

```bash
# Search across codebase
> Grep "useState" --glob "*.tsx"

# Count occurrences
> Grep "TODO" --output_mode count

# Find files
> Glob "**/components/**/*.tsx"
```

**🎯 Exercise**: In a sample repo, find all `console.log` statements, replace with a logger, run tests.

---

## 🔧 Module 3: Slash Commands Deep Dive

### Built-in Slash Commands

| Command | Description | Use Case |
|---------|-------------|----------|
| `/help` | List all commands | Quick reference |
| `/config` | View/edit settings | Model, permissions, hooks |
| `/init` | Create CLAUDE.md for project | Project documentation |
| `/review` | Code review current changes | Pre-commit quality check |
| `/bug` | Report a bug to Anthropic | Feedback |
| `/cost` | Show token usage/cost | Budget tracking |
| `/clear` | Clear conversation history | Fresh context |
| `/compact` | Summarize conversation | Save tokens |
| `/permissions` | Manage tool permissions | Allow/deny tools |
| `/mcp` | Manage MCP servers | Add/remove servers |

### Configuring via `/config`

```bash
# Open config in editor
/config

# Or edit directly
# ~/.claude/settings.json (user)
# .claude/settings.json (project)
```

**Key settings**:
```json
{
  "model": "claude-3-5-sonnet-20241022",
  "permissions": {
    "allow": ["Bash", "Read", "Write", "Edit"],
    "deny": ["Bash:rm -rf"]
  },
  "hooks": {
    "PreToolUse": [{"matcher": "Write", "hooks": [{"type": "command", "command": "prettier --write"}]}]
  }
}
```

**🎯 Exercise**: Run `/init` in a project, customize `/config` to add a hook that runs `eslint --fix` on Write.

---

## 📝 Module 4: File Operations & Editing

### Reading Files

```bash
# Full file
> Read src/main.ts

# Specific lines
> Read src/main.ts:10-50

# Large files (summary)
> Read huge.log --limit 100
```

### Writing Files

```bash
# New file
> Write README.md with: "# My Project\n\nDescription..."

# From template
> Write .github/workflows/ci.yml with: |
>   name: CI
>   on: [push]
>   jobs:
>     test:
>       runs-on: ubuntu-latest
```

### Editing Patterns

```bash
# Single replacement
> Edit src/config.ts: replace "localhost" with "0.0.0.0"

# Replace all
> Edit src/config.ts: replace_all "http://" with "https://"

# Regex (use carefully)
> Edit src/types.ts: replace "/interface\\s+(\\w+)/" with "type $1 ="
```

### Multi-File Operations

```bash
# Edit multiple files
> Edit "src/**/*.ts": replace "oldImport" with "newImport"

# Create multiple files
> Task: "Create React components for Header, Footer, Sidebar"
```

**🎯 Exercise**: Refactor a codebase: rename a function across 10+ files, update imports, run tests.

---

## 🌿 Module 5: Git Integration

### Common Git Workflows

```bash
# Status & diff
> Bash git status
> Bash git diff
> Bash git diff --staged

# Commit with message
> Bash git commit -am "feat: add user authentication"

# Branch operations
> Bash git checkout -b feature/new-ui
> Bash git push -u origin feature/new-ui

# History
> Bash git log --oneline -10
> Bash git show HEAD~2
```

### Claude-Powered Git

```bash
# "Write a commit message for my staged changes"
> Bash git diff --staged | head -200
> "Write a conventional commit message"

# "Review my PR changes"
> Bash git diff main...HEAD
> "Review these changes for bugs and style"

# "Resolve merge conflicts"
> Bash git status
> "Help me resolve these conflicts"
```

### Git Hooks via Claude Hooks

```json
// .claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash:git commit",
        "hooks": [{"type": "command", "command": "npm run lint"}]
      }
    ]
  }
}
```

**🎯 Exercise**: Create a feature branch, make changes, have Claude write commit message, push, create PR.

---

## 🪝 Module 6: Hooks & Automation

### Hook Types

| Hook | When It Runs | Common Uses |
|------|--------------|-------------|
| `PreToolUse` | Before any tool | Validation, formatting, blocking |
| `PostToolUse` | After tool succeeds | Logging, notifications, cleanup |
| `Stop` | Session ends | Summary, metrics |
| `SubagentStop` | Subagent completes | Aggregation |

### Hook Configuration

```json
// .claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {"type": "command", "command": "prettier --write ${FILE}"},
          {"type": "command", "command": "eslint --fix ${FILE}"}
        ]
      },
      {
        "matcher": "Bash:npm test",
        "hooks": [{"type": "command", "command": "echo 'Running tests...'"}]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [{"type": "command", "command": "git add ${FILE}"}]
      }
    ]
  }
}
```

### Useful Hook Examples

**Auto-format on save**:
```json
{"matcher": "Write|Edit", "hooks": [{"type": "command", "command": "prettier --write ${FILE}"}]}
```

**Run tests after changes**:
```json
{"matcher": "Write|Edit", "hooks": [{"type": "command", "command": "npm test -- ${FILE}"}]}
```

**Block dangerous commands**:
```json
{"matcher": "Bash:rm -rf|Bash:sudo", "hooks": [{"type": "command", "command": "echo 'BLOCKED' && exit 1"}]}
```

**🎯 Exercise**: Set up hooks to auto-format (Prettier), lint (ESLint), and run tests on every file write.

---

## 🔌 Module 7: MCP Servers

### What Are MCP Servers?

**Model Context Protocol** — standardized way to give Claude access to external tools:
- Databases (PostgreSQL, SQLite)
- APIs (GitHub, Jira, Slack)
- Browsers (Playwright, Puppeteer)
- File systems, search engines, etc.

### Installing MCP Servers

```bash
# Via npm (many official servers)
npm install -g @modelcontextprotocol/server-github
npm install -g @modelcontextprotocol/server-postgres
npm install -g @modelcontextprotocol/server-filesystem

# Configure in settings
/mcp
# Or edit settings.json:
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {"GITHUB_TOKEN": "ghp_..."}
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed"]
    }
  }
}
```

### Using MCP Tools

Once configured, new tools appear automatically:

```bash
# GitHub MCP
> "List my open PRs"
> "Create issue: 'Add dark mode'"

# Filesystem MCP
> "Search for config files in /etc"
> "Read /etc/nginx/nginx.conf"

# Database MCP
> "Query: SELECT * FROM users WHERE active=1"
```

**🎯 Exercise**: Add GitHub MCP, create an issue from Claude, read a file via filesystem MCP.

---

## 🚀 Module 8: Advanced Workflows

### Multi-File Refactoring

```bash
# "Rename UserService to AuthService across the codebase"
> Task: "Find all references to UserService, rename to AuthService, update imports"
```

### Debugging Assistance

```bash
# "My test fails - help me debug"
> Bash npm test 2>&1 | head -50
> "Analyze this failure and suggest fixes"

# "Add debug logging"
> Edit src/api.ts: add console.log before each return
```

### Code Generation

```bash
# "Create REST API for User CRUD"
> Task: "Generate Express routes, Typescript types, Prisma schema, and tests for User CRUD"

# "Add React form with validation"
> Task: "Create UserForm.tsx with Zod validation, React Hook Form, and TypeScript"
```

### Architecture & Planning

```bash
# "Design a caching layer for this API"
> Read src/api/*.ts
> "Propose Redis caching strategy with code examples"

# "Plan migration from REST to GraphQL"
> Glob "**/*.ts"
> "Create migration plan with steps and code samples"
```

### Parallel Work with Subagents

```bash
# "Review these 3 files in parallel for bugs"
> Task: "Review auth.ts for security issues"
> Task: "Review payments.ts for logic bugs"
> Task: "Review ui.tsx for accessibility"
```

**🎯 Exercise**: Pick a feature in your project. Have Claude: plan → generate code → write tests → run tests → fix failures.

---

## 💡 Module 9: Best Practices & Pro Tips

### Prompting Effectively

| ❌ Avoid | ✅ Do |
|----------|-------|
| "Fix it" | "Fix the TypeError in UserService.getUser() - it returns null but code expects User" |
| "Make it better" | "Add input validation, error handling, and JSDoc to this function" |
| "Refactor" | "Extract the pricing logic into a separate PricingCalculator class" |

### Context Management

```bash
# Check token usage
/cost

# Compact history (summarize, keep context)
/compact

# Clear for fresh start
/clear

# Read only what you need
> Read src/large-file.ts:1-100  # Not the whole 5000 lines
```

### Cost Control

| Tip | Impact |
|-----|--------|
| Use `/compact` regularly | 50-80% token reduction |
| Read specific line ranges | Avoids 10k+ token reads |
| Use `grep`/`glob` before `Read` | Finds relevant files first |
| Batch related tasks | Fewer context reloads |
| Use Haiku for simple tasks | 10x cheaper than Opus |

### Common Pitfalls

| Pitfall | Solution |
|---------|----------|
| Claude edits wrong file | Use full paths, verify with `Read` first |
| Infinite loops | Set `max_turns` in complex tasks |
| Permission denied | Check `/permissions`, allow needed tools |
| Context overflow | `/compact` or `/clear` periodically |
| Hooks not firing | Check matcher syntax, use exact tool names |

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Ctrl+C` | Interrupt current operation |
| `Ctrl+L` | Clear screen |
| `↑/↓` | History navigation |
| `Tab` | Auto-complete paths/commands |

---

## 📚 Resources & Further Learning

### Official
- **Docs**: https://docs.anthropic.com/claude-code
- **GitHub**: https://github.com/anthropics/claude-code
- **Changelog**: https://github.com/anthropics/claude-code/releases

### Community
- **Reddit**: r/ClaudeCode
- **Discord**: Anthropic Community Server
- **Twitter**: @AnthropicAI

### Practice Projects
1. **Todo CLI** — Build a task manager with file storage
2. **Blog Engine** — Static site generator with Markdown
3. **API Wrapper** — TypeScript client for a public API
4. **Code Analyzer** — Tool that scans repo for patterns

### Next Steps After This Course
- **Advanced**: Custom slash commands, plugin development
- **Team**: Shared hooks, project templates, CI integration
- **Scale**: MCP server development, multi-repo workflows

---

## ✅ Completion Checklist

- [ ] Module 0: Installed, authenticated, ran first command
- [ ] Module 1: Can explain core concepts to someone else
- [ ] Module 2: Comfortable with Read, Write, Edit, Bash, Grep, Glob
- [ ] Module 3: Uses `/config`, `/init`, `/review`, `/cost` regularly
- [ ] Module 4: Can do multi-file edits confidently
- [ ] Module 5: Uses git integration for commits, reviews, conflicts
- [ ] Module 6: Has working hooks for format/lint/test
- [ ] Module 7: Connected at least one MCP server
- [ ] Module 8: Completed a multi-step feature using Task tool
- [ ] Module 9: Applies best practices, monitors costs

---

## 🎓 Graduation Project

**Build a complete feature** using everything learned:

1. `/init` to document the project
2. Create feature branch
3. Use `Task` to plan implementation
4. Generate code with `Write`/`Edit`
5. Auto-format/lint via hooks
6. Write tests, run via hook
7. `/review` the changes
8. Commit with Claude-written message
9. Push and create PR via GitHub MCP

**Time**: 1-2 hours | **Goal**: End-to-end workflow mastery

---

*Happy coding with Claude Code! 🚀*

**Version**: 1.0 | **Updated**: 2026 | **License**: MIT