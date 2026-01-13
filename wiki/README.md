# Wiki Content for GitHub Wiki

This directory contains the structured wiki content for the Android Development & Tools repository.

## 📋 Wiki Structure

```
wiki/
├── Home.md                          # Main landing page
├── Android-Apps.md                  # Revanced & Shizuku apps
├── ADB-Commands.md                  # ADB configurations & commands
├── Chromium-Browser.md              # Browser development guide
├── Claude-AI-Instructions.md        # Claude AI configuration
├── Gemini-AI-Instructions.md        # Gemini AI configuration
├── _Sidebar.md                      # Navigation sidebar
├── _Footer.md                       # Footer for all pages
└── README.md                        # This file
```

## 🚀 Deploying to GitHub Wiki

### Option 1: Manual Upload (Recommended for first time)

1. Go to your repository's Wiki tab
2. Click "Create the first page" or "New Page"
3. Copy content from each `.md` file
4. Create pages with matching names (without `.md` extension)

### Option 2: Git Clone Method

GitHub wikis are separate git repositories. You can clone and push directly:

```bash
# Clone the wiki repository
git clone https://github.com/Ven0m0/android.wiki.git

# Copy wiki files
cp -r wiki/* android.wiki/

# Commit and push
cd android.wiki
git add .
git commit -m "Add comprehensive wiki structure"
git push origin master
```

### Option 3: Automated Script

```bash
#!/usr/bin/env bash
set -euo pipefail

REPO="Ven0m0/android"
WIKI_DIR="wiki"
TEMP_DIR=$(mktemp -d)

# Clone wiki
git clone "https://github.com/${REPO}.wiki.git" "${TEMP_DIR}"

# Copy files
cp "${WIKI_DIR}"/*.md "${TEMP_DIR}/"

# Push changes
cd "${TEMP_DIR}"
git add .
git commit -m "Update wiki content"
git push origin master

# Cleanup
rm -rf "${TEMP_DIR}"
```

## 📝 Page Naming Convention

GitHub wiki pages should follow these naming patterns:

| File Name | Wiki Page Name | URL |
|-----------|----------------|-----|
| `Home.md` | Home | `/wiki/Home` |
| `Android-Apps.md` | Android Apps | `/wiki/Android-Apps` |
| `ADB-Commands.md` | ADB Commands | `/wiki/ADB-Commands` |
| `_Sidebar.md` | _(Sidebar)_ | Auto-rendered |
| `_Footer.md` | _(Footer)_ | Auto-rendered |

## 🎨 Features

### Navigation
- **Sidebar** (`_Sidebar.md`) - Persistent navigation on all pages
- **Footer** (`_Footer.md`) - Consistent footer with links
- **Cross-links** - Internal links between pages

### Content Organization
- **Emoji Headers** - Visual hierarchy and quick scanning
- **Tables** - Structured information display
- **Code Blocks** - Syntax-highlighted examples
- **Collapsible Sections** - Better organization

### Responsive Design
- Mobile-friendly tables
- Clear visual hierarchy
- Accessible navigation

## 🔧 Maintenance

### Updating Content

1. Edit files in `wiki/` directory
2. Commit changes to main repository
3. Sync to GitHub wiki using one of the deployment methods

### Adding New Pages

1. Create new `.md` file in `wiki/`
2. Add entry to `_Sidebar.md`
3. Link from relevant pages
4. Update this README

### Link Format

Internal links should use wiki syntax:
```markdown
[Link Text](Page-Name)
```

NOT:
```markdown
[Link Text](Page-Name.md)
[Link Text](/wiki/Page-Name)
```

## 📊 Wiki Statistics

- **Total Pages**: 7 (5 content + 2 navigation)
- **Categories**:
  - Android Tools: 2 pages
  - Development: 1 page
  - AI Instructions: 2 pages
- **Internal Links**: 20+
- **External References**: 100+

## 🔗 External Links

All external links open in new tabs and include:
- Tool repositories
- Documentation
- Community resources
- Reference materials

## ⚠️ Important Notes

### GitHub Wiki Limitations

- No subdirectories in wiki repos
- Flat file structure only
- Limited to markdown
- No CI/CD integration

### Workarounds

- Use prefixes for organization (e.g., `Android-Apps`, `ADB-Commands`)
- Maintain source in main repo
- Manual or scripted sync to wiki

### Best Practices

1. Keep wiki files in sync with main repo
2. Use descriptive page names
3. Include navigation links on all pages
4. Regular content audits
5. Update external links periodically

---

## 🤝 Contributing

To contribute to the wiki:

1. Edit files in `wiki/` directory
2. Test locally with a markdown previewer
3. Submit pull request to main repository
4. Wiki will be updated after merge

## 📄 License

Same as repository license (see root LICENSE file).

---

*This wiki structure follows GitHub wiki best practices for maximum usability and maintainability.*
