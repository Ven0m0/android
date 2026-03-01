# GitHub Copilot Dev Guardrails

**Purpose:** Code generation guardrails for GitHub Copilot
**Model:** copilot (GPT-4 based)
**Tone:** Blunt, precise. Result-first. Lists <=7

---

## Project Overview

Jekyll documentation site (just-the-docs theme) for Android development
tools, customization (ReVanced, Shizuku), ADB commands, and Chromium
browser builds. Deployed to GitHub Pages via tag-triggered GitHub Actions.

---

## Dev Commands

```bash
# Setup
bundle install

# Local preview (http://localhost:4000/android)
cd docs && bundle exec jekyll serve

# Build
cd docs && bundle exec jekyll build

# Lint Bash scripts
shellcheck -s bash -x -a -S warning <script.sh>
shfmt -ln bash -s -i 2 -bn --apply-ignore -w <script.sh>

# Deploy (CI handles automatically on tag push)
git tag v1.x.x && git push origin v1.x.x
```

---

## Conventions

### Markdown (docs/)

- Max 88 chars/line, LF endings, UTF-8
- File names: PascalCase-kebab (`Android-Apps.md`)
- Callouts: just-the-docs syntax (`{: .note }`, `{: .warning }`, `{: .tip }`)
- Code: fenced blocks with language tag (` ```bash `, ` ```yaml `)
- New pages: add YAML frontmatter with `title:`, `description:`, `nav_order:`

### YAML / JSON

- 2-space indent, 120-char max
- Formatter: Prettier (`.prettierrc.yml`)

### Bash

- `set -euo pipefail` — always, first line after shebang
- Quote all vars: `"${var}"` not `$var`
- Use `[[ ]]` for conditionals, `mapfile -t` for arrays, `printf` over `echo`
- 2-space indent, short flags (`-a` not `--all`)
- **Banned:** `eval`, parsing `ls`, backticks, unquoted variables
- **CI required:** shellcheck clean + shfmt formatted

---

## Core Principles

1. User cmds > Rules
2. Edit > Create (min diff)
3. Subtraction > Addition
4. Align w/ existing patterns

---

## Toolchain Preference

fd -> find | rg -> grep | bat -> cat | sd -> sed | aria2 -> curl | jaq -> jq | rust-parallel -> xargs

---

## Performance

- **Min forks:** Prefer builtins. Batch I/O. Avoid pipes where possible.
- **Regex:** Anchor patterns. Use `grep -F` for literals.
- **Async:** Background tasks for I/O. Wait at sync points.

---

## Code Style

- **Fmt:** 2-space indent. Strip invisibles (U+202F/200B/00AD).
- **Output:** Result-first. Lists <=7 items.
- **Abbr:** cfg, impl, deps, val, opt.

---

## Quality Gates

- **CI:** markdownlint + shellcheck + shfmt + jekyll build
- **Validation:** No syntax errors. All scripts executable.
- **Bash:** ShellCheck + ShFmt pass required before merge.

---

## Example

**Task:** Find all `.sh` files modified in last 7 days

```bash
# Preferred (fd over find)
fd -e sh -t f --changed-within 7d
```
