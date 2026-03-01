# AGENTS.md — AI Assistant Reference

> Curated resources, tools, and configurations for Android development,
> customization, and AI-assisted workflows.
> Live site: <https://ven0m0.github.io/android>

---

## Project

**Description:** Jekyll documentation site (just-the-docs theme) covering
Android app customization (ReVanced, Shizuku), ADB commands, device-specific
mods, and custom Chromium browser builds.

| Key | Value |
|-----|-------|
| Primary languages | Markdown, YAML |
| Framework | Jekyll + just-the-docs v0.10.1 |
| Runtime | Ruby 3.x |
| Deployment | GitHub Pages (tag-triggered CI) |
| Repository | <https://github.com/Ven0m0/android> |

---

## Structure

```
@/ — repo root
├── @docs/ — Jekyll source pages (all content lives here)
│   ├── _config.yml — Jekyll configuration (theme, nav, plugins)
│   ├── index.md — Home page / quick-start
│   ├── Android-Apps.md — ReVanced, Shizuku, & categorized tools
│   ├── ADB-Commands.md — ADB shell command reference
│   ├── Nothing-Phone.md — Nothing Phone 2 mods, ROMs, kernels
│   └── Chromium-Browser.md — Custom Chromium fork plans & patches
├── @.github/
│   ├── workflows/
│   │   ├── docs-page-action.yml — Build + deploy to GH Pages on tag push
│   │   └── docs-rebuild.yml — Webhook rebuild on docs/** changes to main
│   ├── copilot-instructions.md — Copilot guardrails (conventions + commands)
│   ├── dependabot.yml — Automated dep updates (weekly actions)
│   ├── renovate.json — Renovate bot config
│   ├── ISSUE_TEMPLATE/ — Bug, feature, and custom issue templates
│   └── pull_request_template.md — PR checklist and description template
├── @Gemfile — Ruby build dependencies (Jekyll plugins)
├── @AGENTS.md — This file; AI assistant reference
├── @CLAUDE.md -> AGENTS.md — Symlink for Claude Code
├── @GEMINI.md -> AGENTS.md — Symlink for Gemini
├── @.editorconfig — Cross-editor formatting rules
├── @.megalinter.yml — CI linting configuration (MegaLinter)
├── @.shellcheckrc — ShellCheck configuration
└── @.vscode/settings.json — VSCode: Biome formatter + auto-fix on save
```

---

## Dev Workflow

### Setup

```bash
# Install build dependencies (Ruby 3.x required)
bundle install
```

### Preview (local dev server)

```bash
cd docs
bundle exec jekyll serve
# Opens at http://localhost:4000/android — live-reloads on file changes
```

### Build

```bash
cd docs
bundle exec jekyll build
# Outputs static site to _site/ directory
```

### Lint (local)

```bash
# Bash scripts
shellcheck -s bash -x -a -S warning <script.sh>
shfmt -ln bash -s -i 2 -bn --apply-ignore -w <script.sh>

# Markdown
markdownlint docs/**/*.md

# YAML
yamllint docs/_config.yml
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
- **Callouts:** Use just-the-docs callouts (`{: .note }`, `{: .warning }`)
- **Code blocks:** Use fenced blocks with language tag (` ```bash `, ` ```yaml `)
- **Trailing whitespace:** Preserved in `.md` files (intentional for line breaks)
- **YAML frontmatter:** Include `title:`, `description:`, and `nav_order:` fields

### YAML / JSON

- **Indent:** 2 spaces
- **Max line length:** 120 characters
- **Formatter:** Prettier (`.prettierrc.yml` config)

### Bash Scripts

```bash
#!/usr/bin/env bash
set -euo pipefail # Always. Exit on error, unset vars, pipe failures.

# Quote all variables
echo "${var}" # not $var

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
- New sections need `nav_order` in YAML frontmatter for ordering
- Scripts/tooling go in repo root or a `scripts/` directory (if created)
- Assets (images, files) go in `docs/assets/`

### Error Handling

- Bash: `set -euo pipefail` on every script; no silent failures

---

## Dependencies

| Dependency | Version | Purpose |
|-----------|---------|---------|
| just-the-docs | v0.10.1 | Documentation theme (remote_theme) |
| jekyll-remote-theme | latest | Load theme from GitHub |
| jekyll-seo-tag | latest | SEO meta tags |
| jekyll-sitemap | latest | Auto-generate sitemap.xml |
| Ruby | 3.x | Build environment for Jekyll |
| MegaLinter | CI-managed | Multi-language linting orchestrator |
| ShellCheck | CI-managed | Bash static analysis |
| ShFmt | CI-managed | Bash formatter (2-space, LF) |
| MarkdownLint | CI-managed | Markdown style linter |
| Prettier | CI-managed | YAML/JSON formatter |
| Dependabot | GitHub native | Automated dependency PRs (weekly) |
| Renovate | GitHub App | Alternative dep update bot |

---

## Common Tasks

### Add a new documentation page

```bash
# 1. Create the page with YAML frontmatter
cat > docs/My-New-Page.md << 'EOF'
---
title: My New Page
description: Brief description for SEO
nav_order: 6
---

# My New Page

Content here.
EOF

# 2. Preview
cd docs && bundle exec jekyll serve

# 3. Build check
bundle exec jekyll build
```

### Fix existing content

```bash
# Edit the relevant file
$EDITOR docs/ADB-Commands.md

# Verify rendering
cd docs && bundle exec jekyll serve  # check localhost:4000/android

# Validate build
bundle exec jekyll build
```

### Add a feature (new section/guide)

1. Plan the content outline
2. Create `docs/<Section-Name>.md` with frontmatter
3. Set `nav_order` for position in navigation
4. Add callouts/code blocks following conventions
5. Run `bundle exec jekyll build` — fix all warnings
6. Open PR with the `feature` chat mode pattern (Plan -> Impl -> Review)

### Fix a bug (broken link, rendering issue)

1. Reproduce: `cd docs && bundle exec jekyll serve` and navigate to the broken page
2. Isolate: find the offending markdown/config
3. Fix: edit the file, verify live in dev server
4. Verify: `bundle exec jekyll build` passes

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
  -> docs-page-action.yml
  -> Checkout -> Ruby 3.3 -> bundle install -> jekyll build
  -> Upload artifact -> Deploy to GitHub Pages
```

- Concurrency: only one deploy runs at a time (no cancel-in-progress)
- Excludes `nightly` tags
- Deploy URL: <https://ven0m0.github.io/android>

### Dependency Automation

| Bot | Ecosystems | Schedule |
|-----|-----------|----------|
| Dependabot | GitHub Actions | Weekly |
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
| YAML/JSON format | `prettier` | manual |
| Bash format | `shfmt` | manual |

**Package manager:** `gem` / `bundler` (Ruby)
**Formatter (VSCode):** Biome (format on save, auto-fix, import organizer)
**Linter (CI):** MegaLinter
**Runtime:** Ruby 3.x

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
      Flow: Profile -> Identify -> Optimize -> Benchmark.
      Scope: Backend (Algo/DB), Frontend, Infra.
      Tools: perf, flamegraph, hyperfine.

  code-janitor:
    name: "Code Janitor"
    desc: "Tech debt assassin."
    sys: |
      Philosophy: Less Code = Less Debt.
      Actions: Delete unused (dead code, deps). Simplify (flatten logic). Update.
      Process: Measure usage -> Delete/Refactor -> Verify.

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
      Priority: Critical -> High -> Low.
      Output: Concrete fixes only.

  doc-writer:
    name: "Doc Writer"
    desc: "Tech docs specialist."
    sys: |
      Structure: Description -> Install -> Usage -> Config -> Troubleshooting.
      Style: Concise, scannable, example-heavy.
      Sync with code changes.
```

---

## Chat Modes

| Mode | Behavior |
|------|----------|
| `quick-fix` | Minimal changes to fix the immediate issue. No refactor. Test first. |
| `refactor` | Improve structure/names/logic. No behavior change. Explain trade-offs. |
| `feature` | Plan -> Implement -> Test -> Document. Minimize new dependencies. |
| `debug` | Reproduce -> Isolate -> Fix -> Regression test. Explain root cause. |
