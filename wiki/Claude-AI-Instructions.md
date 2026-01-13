# Claude AI Instructions

Configuration and best practices for using Claude AI in development workflows, specifically for dotfiles management and system automation.

## 🎯 Purpose

**Purpose:** Dotfiles management instructions for Claude AI
**Model:** claude-* (all Claude models)
**Tone:** Blunt, precise. `Result ∴ Cause`. Lists ≤7

---

## 🖥️ System Overview

### Dotfiles Management Stack

| Component | Scope | Purpose |
|-----------|-------|---------|
| **YADM** | `Home/` → `~/` | User-level dotfiles |
| **Tuckr** | `etc/`, `usr/` → `/` | System-level configs |

### Target Platforms

- **Arch Linux** (CachyOS)
- **Debian** / Ubuntu
- **Termux** (Android)

### Core Directives

1. **User>Rules** - User instructions override defaults
2. **Verify** - Always verify before actions
3. **Edit>Create** - Prefer editing over creating (min Δ)
4. **Debt-First** - Address technical debt first

---

## 📜 Standards

### Bash Scripting

#### Required Header
```bash
#!/usr/bin/env bash
set -euo pipefail
```

**Flags:**
- `-e` - Exit on error
- `-u` - Error on undefined variables
- `-o pipefail` - Fail on pipe errors

#### Preferred Idioms

| Use | Instead of | Reason |
|-----|------------|--------|
| `[[ regex ]]` | `[ test ]` | Better regex, safer |
| `mapfile -t` | `IFS= read -r` loops | Handles newlines correctly |
| `local -n` | Global variables | Proper scoping |
| `printf` | `echo` | More portable |
| `ret=$(fn)` | Backticks | More readable |

#### Banned Patterns

❌ **Never Use:**
- `eval` - Security risk
- Parsing `ls` output - Unreliable
- Backticks `` `cmd` `` - Use `$(cmd)` instead

#### Package Management

```bash
# Arch Linux
paru -S package      # Preferred AUR helper
yay -S package       # Fallback

# Check if installed first
pacman -Q package 2>/dev/null || paru -S package

# Debian/Ubuntu
apt install package
```

---

## 🛠️ Tool Preferences

Modern Rust-based alternatives to traditional tools:

| Traditional | Modern Alternative | Benefits |
|-------------|-------------------|----------|
| `find` | `fd` | Faster, better defaults |
| `grep` | `rg` (ripgrep) | Faster, respects .gitignore |
| `cat` | `bat` | Syntax highlighting |
| `sed` | `sd` | Simpler syntax |
| `curl` | `aria2` | Faster downloads, resume |
| `jq` | `jaq` | Faster JSON processing |
| `xargs` | `rust-parallel` | Better parallelization |

### Usage Examples

```bash
# Find files
fd '\.sh$' ~/.local/bin

# Search content
rg 'function' --type bash

# View with syntax
bat script.sh

# Replace text
sd 'old' 'new' file.txt

# Parallel execution
fd -x rust-parallel 'process {}'
```

---

## ⚡ Performance Guidelines

### I/O Optimization

```bash
# ❌ Bad: Multiple individual operations
for file in *.txt; do
    cat "$file" | process
done

# ✅ Good: Batch operations
fd -e txt -x process
```

### Async Operations

```bash
# Run tasks in parallel
fd -e sh -x bash {} &
wait

# Use rust-parallel for better control
fd -e sh | rust-parallel 'bash {}'
```

### Regex Optimization

```bash
# Use anchored regex
grep -F 'exact string' file.txt  # Fixed string

# Cache frequently accessed data
declare -A cache
get_data() {
    [[ -v cache[$1] ]] || cache[$1]=$(expensive_operation "$1")
    echo "${cache[$1]}"
}
```

---

## 🔒 Protected Files

**Never modify these files directly:**

- `pacman.conf` - Package manager config
- `.zshrc` - Shell configuration
- `.gitconfig` - Git configuration
- `sysctl.d/*` - Kernel parameters

**Rationale:** These are carefully tuned and have dependencies. Changes require review.

---

## 🔄 Workflow

### Development Cycle

**TDD Approach:** Red → Green → Refactor

1. **Red** - Write failing test
2. **Green** - Make it pass
3. **Refactor** - Improve code

### Sync Process

#### User Files (YADM)
```bash
# Manual sync script
~/.local/bin/yadm-sync.sh

# YADM operations
yadm add ~/.config/file
yadm commit -m "Update config"
yadm push
```

#### System Files (Tuckr)
```bash
# Deploy system configs
tuckr set etc usr

# Rollback if needed
tuckr unset etc usr
```

---

## 🧪 Quality Assurance

### CI Pipeline

**Script:** `~/.local/bin/lint-format`

**Tools:**
- **shfmt** - Shell script formatting
- **shellcheck** - Shell script linting
- **biome** - JavaScript/TypeScript formatting
- **ruff** - Python linting
- **actionlint** - GitHub Actions validation

### Pre-Commit Checks

```bash
# Run before commit
lint-format

# Individual checks
shellcheck script.sh
shfmt -d script.sh
```

---

## 📁 Key Assets

### Scripts (`~/.local/bin`)

#### System Management
- **pkgui** - TUI for package management
- **systool** - System maintenance utilities
- **dosudo** - Privilege elevation wrapper
- **autostart** - Session startup manager

#### Media Tools
- **media-opt** - Media optimization
- **ffwrap** - FFmpeg wrapper
- **wp** - Wallpaper manager

#### File & Network
- **fzgrep** - Fuzzy grep with fzf
- **fzgit** - Fuzzy git operations
- **netinfo** - Network information
- **websearch** - Terminal web search

#### Development
- **yadm-sync** - Dotfiles sync
- **lint-format** - Code quality checks

### Configuration Files

**Total:** 88 configuration files

**Categories:**
- **Shells** - Bash, Zsh, Fish
- **Terminals** - Alacritty, Kitty, etc.
- **Dev Tools** - VSCode, Git, Mise

### AI Configurations

- **Claude** - Agents & commands (this file)
- **Copilot** - GitHub Copilot settings
- **Gemini** - Gemini AI configs (`.gemini/`)

---

## 🚀 Deployment

### Initial Setup

```bash
# Clone dotfiles
yadm clone https://github.com/user/dotfiles.git

# Run bootstrap
yadm bootstrap
```

### Bootstrap Process

1. **Install Packages** - Install required system packages
2. **Deploy Home** - Set up user configurations with YADM
3. **Deploy System** - Set up system configs with Tuckr

---

## 📚 Examples

### Fix Shellcheck Error

**Task:** Fix script with shellcheck error
**Input:** `systool` has unquoted variable

#### Before
```bash
log_path=$HOME/.cache/systool.log
```

#### After
```bash
log_path="${HOME}/.cache/systool.log"
```

**Result:** Shellcheck passes ∴ Variables quoted per standards.

### Create New Script

**Task:** Create backup script
**Input:** Backup important configs

```bash
#!/usr/bin/env bash
set -euo pipefail

# Configuration
readonly BACKUP_DIR="${HOME}/.cache/backups"
readonly DATE=$(date +%Y%m%d_%H%M%S)

# Create backup
backup_configs() {
    local -a files=(
        "${HOME}/.zshrc"
        "${HOME}/.config/nvim"
    )

    mkdir -p "${BACKUP_DIR}"
    tar -czf "${BACKUP_DIR}/backup_${DATE}.tar.gz" "${files[@]}"
    printf 'Backup created: %s\n' "${BACKUP_DIR}/backup_${DATE}.tar.gz"
}

backup_configs
```

**Result:** Script follows standards ∴ Header, error handling, readonly vars.

---

## 🔍 Troubleshooting

### Common Issues

#### Shellcheck Warnings

```bash
# SC2086: Quote to prevent word splitting
var="$HOME/path"  # Good
var=$HOME/path    # Bad

# SC2155: Declare and assign separately
declare var
var=$(command)    # Good
declare var=$(command)  # Bad
```

#### YADM Conflicts

```bash
# Check status
yadm status

# Resolve conflicts
yadm mergetool

# Force pull (careful!)
yadm reset --hard origin/main
```

---

## 🔗 Related Resources

- [YADM Documentation](https://yadm.io/)
- [Tuckr GitHub](https://github.com/RaphGL/Tuckr)
- [Shellcheck Wiki](https://www.shellcheck.net/wiki/)
- [Bash Best Practices](https://google.github.io/styleguide/shellguide.html)

---

## 🔙 Navigation

- [← Back to Home](Home)
- [← Chromium Browser](Chromium-Browser)
- [Gemini AI Instructions →](Gemini-AI-Instructions)
