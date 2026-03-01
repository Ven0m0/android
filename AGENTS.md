# AGENTS.md — AI Assistant Reference

> Curated resources, tools, and configurations for Android development,
> customization, and AI-assisted workflows.
> Live site: <https://ven0m0.github.io/android>

---

## Project

**Description:** MkDocs Material documentation site covering Android app
customization (ReVanced, Shizuku), ADB commands, device-specific mods, and
custom Chromium browser builds.

| Key | Value |
|-----|-------|
| Primary languages | Markdown, YAML, Python |
| Framework | MkDocs Material ≥9.5,<10 |
| Runtime | Python 3.x |
| Deployment | GitHub Pages (tag-triggered CI) |
| Repository | <https://github.com/Ven0m0/android> |

---

## Structure

```
@/ — repo root
├── @docs/                      — MkDocs source pages (all content lives here)
│   ├── index.md                — Home page / quick-start
│   ├── Android-Apps.md         — ReVanced, Shizuku, & categorized tools (145 lines)
│   ├── ADB-Commands.md         — ADB shell command reference (296 lines)
│   ├── Nothing-Phone.md        — Nothing Phone 2 mods, ROMs, kernels (77 lines)
│   └── Chromium-Browser.md     — Custom Chromium fork plans & patches (288 lines)
├── @.github/
│   ├── workflows/
│   │   ├── docs-page-action.yml  — Build + deploy to GH Pages on tag push
│   │   └── docs-rebuild.yml      — Webhook rebuild on docs/** changes to main
│   ├── copilot-instructions.md   — Copilot guardrails (conventions + commands)
│   ├── dependabot.yml            — Automated dep updates (weekly pip/actions)
│   ├── renovate.json             — Renovate bot config
│   ├── ISSUE_TEMPLATE/           — Bug, feature, and custom issue templates
│   └── pull_request_template.md  — PR checklist and description template
├── @mkdocs.yml                 — MkDocs configuration (theme, nav, extensions)
├── @requirements.txt           — Python build dependencies (mkdocs-material)
├── @AGENTS.md                  — This file; AI assistant reference
├── @CLAUDE.md -> AGENTS.md     — Symlink for Claude Code
├── @GEMINI.md -> AGENTS.md     — Symlink for Gemini
├── @.editorconfig              — Cross-editor formatting rules
├── @.megalinter.yml            — CI linting configuration (MegaLinter)
├── @.shellcheckrc              — ShellCheck configuration
└── @.vscode/settings.json      — VSCode: Biome formatter + auto-fix on save
```

---

## Dev Workflow

### Setup

```bash
# Install build dependencies (Python 3.x required)
pip install -r requirements.txt
```

### Preview (local dev server)

```bash
mkdocs serve
# Opens at http://localhost:8000 — live-reloads on file changes
```

### Build

```bash
mkdocs build --strict
# Outputs static site to site/ directory
# --strict treats warnings as errors (same as CI)
```

### Lint (local)

```bash
# Bash scripts
shellcheck -s bash -x -a -S warning <script.sh>
shfmt -ln bash -s -i 2 -bn --apply-ignore -w <script.sh>

# Markdown
markdownlint docs/**/*.md

# YAML
yamllint -c .yamllint.yml mkdocs.yml
```

### Deploy

```bash
# Deploy is tag-triggered via CI — do NOT manually push to GH Pages
git tag v1.2.3
git push origin v1.2.3
# GitHub Actions handles build + deploy automatically
```

---

## Conventions

### Markdown (docs/)

- **Max line length:** 88 characters
- **Line endings:** LF (Unix)
- **Encoding:** UTF-8
- **File naming:** PascalCase-kebab (`Android-Apps.md`, `ADB-Commands.md`)
- **Callouts:** Use MkDocs Material admonitions (`!!! note`, `!!! warning`)
- **Code blocks:** Use fenced blocks with language tag (` ```bash `, ` ```yaml `)
- **Trailing whitespace:** Preserved in `.md` files (intentional for line breaks)
- **YAML frontmatter:** Not required but include `title:` and `description:` for SEO

### YAML / JSON

- **Indent:** 2 spaces
- **Max line length:** 120 characters
- **Formatter:** Prettier (`.prettierrc.yml` config)

### Bash Scripts

```bash
#!/usr/bin/env bash
set -euo pipefail   # Always. Exit on error, unset vars, pipe failures.

# Quote all variables
echo "${var}"        # not $var

# Use [[ ]] for conditionals
[[ -f "${file}" ]] && echo "exists"

# Use mapfile for arrays
mapfile -t lines < <(command)

# Use printf over echo
printf '%s\n' "${value}"
```

- **Indent:** 2 spaces
- **Short flags:** `-a` not `--all`
- **Banned:** `eval`, parsing `ls`, backticks, unquoted variables
- **CI required:** ShellCheck clean + ShFmt formatted

### File Organization

- All documentation pages go in `docs/`
- New sections are registered in `mkdocs.yml` under `nav:`
- Scripts/tooling go in repo root or a `scripts/` directory (if created)
- Assets (images, files) go in `docs/assets/`

### Error Handling

- Bash: `set -euo pipefail` on every script; no silent failures
- MkDocs: Use `--strict` flag; fix all warnings before merging

---

## Dependencies

| Dependency | Version | Purpose |
|-----------|---------|---------|
| mkdocs-material | ≥9.5,<10 | Documentation theme and site generator |
| Python | 3.x | Build environment for MkDocs |
| MegaLinter | CI-managed | Multi-language linting orchestrator |
| ShellCheck | CI-managed | Bash static analysis |
| ShFmt | CI-managed | Bash formatter (2-space, LF) |
| MarkdownLint | CI-managed | Markdown style linter |
| Prettier | CI-managed | YAML/JSON formatter |
| Ruff | CI-managed | Python linter (default style) |
| Dependabot | GitHub native | Automated dependency PRs (weekly) |
| Renovate | GitHub App | Alternative dep update bot |

---

## Common Tasks

### Add a new documentation page

```bash
# 1. Create the page
touch docs/My-New-Page.md

# 2. Write content (follow Markdown conventions above)

# 3. Register in navigation
#    Edit mkdocs.yml → nav: section:
#      - My Page: My-New-Page.md

# 4. Preview
mkdocs serve

# 5. Build check
mkdocs build --strict
```

### Fix existing content

```bash
# Edit the relevant file
$EDITOR docs/ADB-Commands.md

# Verify rendering
mkdocs serve   # check localhost:8000

# Validate build
mkdocs build --strict
```

### Add a feature (new section/guide)

1. Plan the content outline
2. Create `docs/<Section-Name>.md`
3. Add to `mkdocs.yml` nav under the appropriate tab
4. Add admonitions/code blocks following conventions
5. Run `mkdocs build --strict` — fix all warnings
6. Open PR with the `feature` chat mode pattern (Plan → Impl → Review)

### Fix a bug (broken link, rendering issue)

1. Reproduce: `mkdocs serve` and navigate to the broken page
2. Isolate: find the offending markdown/config
3. Fix: edit the file, verify live in dev server
4. Verify: `mkdocs build --strict` passes

### Update a dependency

```bash
# Edit requirements.txt
# e.g. change mkdocs-material>=9.5,<10 to >=9.6,<10

# Test locally
pip install -r requirements.txt
mkdocs build --strict

# Commit and push — Dependabot/Renovate also auto-raises PRs
```

### Add a test

This project has no automated test suite beyond CI linting. To validate:

```bash
mkdocs build --strict   # catches broken links and config errors
markdownlint docs/      # catches markdown style violations
```

---

## CI/CD

### On Pull Request

- MegaLinter runs across all changed file types
- Most linters are in **warn-only mode** (won't block merge):
  `ShellCheck, ShFmt, MarkdownLint, YamlLint, ActionLint, Bandit`
- Hard errors: JSON syntax, YAML parsing, EditorConfig violations

### On Merge to `main` (docs/** changes)

- `docs-rebuild.yml` fires a webhook to trigger an external rebuild
- Uses secret `DOCS_REBUILD_URL`

### On Tag Push (`v*`)

```
Push tag v1.x.x
    → docs-page-action.yml
    → Checkout → Python 3.x → pip install → mkdocs build --strict
    → Upload artifact → Deploy to GitHub Pages
```

- Concurrency: only one deploy runs at a time (no cancel-in-progress)
- Excludes `nightly` tags
- Deploy URL: <https://ven0m0.github.io/android>

### Dependency Automation

| Bot | Ecosystems | Schedule |
|-----|-----------|----------|
| Dependabot | GitHub Actions, pip, npm, bun | Weekly |
| Dependabot | Git submodules | Daily |
| Renovate | All of the above | Auto |

---

## Tool Preferences

| Task | Preferred | Avoid |
|------|----------|-------|
| File search | `fd` | `find` |
| Content search | `rg` (ripgrep) | `grep` |
| File view | `bat` | `cat` |
| Text replace | `sd` | `sed` |
| Download | `aria2` | `curl` |
| JSON processing | `jaq` | `jq` |
| Parallelism | `rust-parallel` | `xargs` |
| Python linting | `ruff` | flake8/pylint |
| YAML/JSON format | `prettier` | manual |
| Bash format | `shfmt` | manual |

**Package manager:** `pip` (Python)
**Formatter (VSCode):** Biome (format on save, auto-fix, import organizer)
**Linter (CI):** MegaLinter
**Runtime:** Python 3.x

---

## Agent Definitions

```yaml
agents:
  bash-expert:
    name: "Bash Expert"
    desc: "Modern, secure Bash scripter."
    sys: |
      Role: Expert Bash dev.
      Priorities: Performance, Security, Conciseness.
      Std: set -euo pipefail. Quote all vars. Use [[ ]]. mapfile. printf.
      Tools: Rust-based (fd, rg, bat, sd) > classic Unix.
      Impl: Distro-agnostic, shellcheck-clean, shfmt-formatted.

  performance-optimizer:
    name: "Perf Optimizer"
    desc: "Full-stack bottleneck specialist."
    sys: |
      Flow: Profile → Identify → Optimize → Benchmark.
      Scope: Backend (Algo/DB), Frontend, Infra.
      Tools: perf, flamegraph, hyperfine.

  code-janitor:
    name: "Code Janitor"
    desc: "Tech debt assassin."
    sys: |
      Philosophy: Less Code = Less Debt.
      Actions: Delete unused (dead code, deps). Simplify (flatten logic). Update.
      Process: Measure usage → Delete/Refactor → Verify.

  critical-thinker:
    name: "Critical Thinker"
    desc: "Socratic logic probe."
    sys: |
      Mode: Challenge assumptions. Ask "Why?".
      Constraint: NO code generation.
      Goal: Uncover root causes, edge cases, and reasoning flaws.

  code-reviewer:
    name: "Code Reviewer"
    desc: "QA & Security audit."
    sys: |
      Check: Correctness, Security, Performance, Tests.
      Priority: Critical → High → Low.
      Output: Concrete fixes only.

  doc-writer:
    name: "Doc Writer"
    desc: "Tech docs specialist."
    sys: |
      Structure: Description → Install → Usage → Config → Troubleshooting.
      Style: Concise, scannable, example-heavy.
      Sync with code changes.
```

---

## Chat Modes

| Mode | Behavior |
|------|----------|
| `quick-fix` | Minimal changes to fix the immediate issue. No refactor. Test first. |
| `refactor` | Improve structure/names/logic. No behavior change. Explain trade-offs. |
| `feature` | Plan → Implement → Test → Document. Minimize new dependencies. |
| `debug` | Reproduce → Isolate → Fix → Regression test. Explain root cause. |
