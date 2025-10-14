# Repository Structure

This document explains the organization of the Hassalarm Wake-Up Light Integration repository.

## 📁 Directory Layout

```
hassalarm-wakeup-light/
├── README.md                      # Main documentation (start here!)
├── QUICK_START.md                 # Fast setup guide (15 minutes)
├── TROUBLESHOOTING.md             # Common issues and solutions
├── CHANGELOG.md                   # Version history and updates
├── LICENSE                        # MIT License
├── REPOSITORY_STRUCTURE.md        # This file
│
├── examples/                      # All configuration examples
│   ├── configuration.yaml         # Full config.yaml example
│   ├── scripts.yaml              # All script variations
│   ├── automations.yaml          # All automation examples
│   └── dashboard.yaml            # Dashboard card examples
│
├── docs/                         # Additional documentation
│   ├── images/                   # Screenshots and diagrams
│   │   ├── dashboard-card.png
│   │   ├── hassalarm-app.png
│   │   ├── setup-flow.png
│   │   └── status-indicators.png
│   ├── CUSTOMIZATION.md          # Advanced customization guide
│   ├── API.md                    # Webhook and API details
│   └── CONTRIBUTING.md           # Contribution guidelines
│
├── .github/                      # GitHub-specific files
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   ├── feature_request.md
│   │   └── help_wanted.md
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/
│       └── yaml-lint.yml         # Automatic YAML validation
│
└── assets/                       # Media files for documentation
    ├── logo.png
    ├── banner.png
    └── demo.gif
```

---

## 📄 File Descriptions

### Root Files

| File | Purpose | Audience |
|------|---------|----------|
| `README.md` | Complete documentation with setup, features, and usage | All users |
| `QUICK_START.md` | Streamlined 15-minute setup guide | New users |
| `TROUBLESHOOTING.md` | Common issues and detailed solutions | Users having problems |
| `CHANGELOG.md` | Version history and feature additions | All users |
| `LICENSE` | MIT License and attribution | Contributors |
| `REPOSITORY_STRUCTURE.md` | This file - repo organization | Contributors |

### Examples Directory

Contains complete, copy-paste ready configuration files:

- **`configuration.yaml`**: All entries for main config file
  - Input datetime, input boolean, sensors
  - Optional template sensors
  - History and recorder settings

- **`scripts.yaml`**: Multiple script variations
  - Basic flash (default)
  - Multiple lights
  - Brightness ramp
  - Color cycle
  - Fast strobe
  - Gentle wake

- **`automations.yaml`**: All automation examples
  - Main wake-up automation
  - Pre-alarm warning
  - Notifications
  - Auto enable/disable
  - Snooze functionality

- **`dashboard.yaml`**: Dashboard card options
  - Main control card (button-card)
  - Simple entities card
  - Compact card
  - Full dashboard view
  - Markdown info card

### Docs Directory

Extended documentation for advanced users:

- **`images/`**: Screenshots and visual guides
- **`CUSTOMIZATION.md`**: Advanced customization options
- **`API.md`**: Webhook setup and API integration
- **`CONTRIBUTING.md`**: How to contribute to the project

### .github Directory

GitHub-specific files for community management:

- **Issue templates**: Structured bug reports and feature requests
- **Pull request template**: Guidelines for code contributions
- **Workflows**: Automated testing and validation

---

## 🚀 How to Use This Repository

### For End Users

1. **Start with README.md**
   - Understand what the integration does
   - Review requirements and features

2. **Follow QUICK_START.md**
   - Set up in 15 minutes
   - Test each component

3. **Copy from examples/**
   - Use example files as templates
   - Customize for your setup

4. **Reference TROUBLESHOOTING.md**
   - Solve common issues
   - Check before asking for help

### For Contributors

1. **Read CONTRIBUTING.md** (in docs/)
   - Understand coding standards
   - Learn git workflow

2. **Use issue templates**
   - Report bugs properly
   - Request features clearly

3. **Test changes**
   - Validate YAML syntax
   - Test on real Home Assistant instance

4. **Update CHANGELOG.md**
   - Document all changes
   - Follow versioning scheme

---

## 📝 Documentation Standards

### README.md Guidelines

- **Table of contents** for easy navigation
- **Code blocks** with syntax highlighting
- **Clear headings** for sections
- **Screenshots** where helpful
- **Links** to external resources
- **Bold** for important notes
- **Emojis** for visual organization

### Example File Guidelines

- **Comments** explaining each section
- **Placeholder values** clearly marked with `# CHANGE THIS`
- **Complete** and working examples
- **Variations** for different use cases
- **Proper indentation** (2 spaces for YAML)

### Issue Template Guidelines

- **Clear title** format
- **Required fields** for reproduction
- **Checkbox lists** for testing steps
- **Version information** fields

---

## 🔄 Workflow for Updates

### Making Changes

1. **Update relevant files**
   - Code in `examples/`
   - Documentation in root or `docs/`

2. **Update CHANGELOG.md**
   - Add to [Unreleased] section
   - Follow format: Added/Changed/Fixed

3. **Test locally**
   - Validate YAML syntax
   - Test in Home Assistant

4. **Update version** (for releases)
   - Tag with version number
   - Move [Unreleased] to new version in CHANGELOG

### Creating Releases

1. **Version bump** in CHANGELOG.md
2. **Git tag** with version (e.g., `v1.0.0`)
3. **GitHub release** with notes from CHANGELOG
4. **Announce** in Home Assistant community

---

## 🎨 Assets Guidelines

### Images

- **Format**: PNG for screenshots, GIF for animations
- **Size**: Max 1920px width
- **Compression**: Optimize before committing
- **Naming**: Descriptive kebab-case (e.g., `dashboard-card-green.png`)

### Screenshots Should Show

- Clean Home Assistant interface
- Relevant configuration highlighted
- Clear and readable text
- Consistent theme (light mode preferred)

---

## 📦 Release Checklist

Before creating a new release:

- [ ] All code tested in real Home Assistant
- [ ] CHANGELOG.md updated with version
- [ ] README.md reflects all features
- [ ] Example files are current and working
- [ ] Screenshots are up to date
- [ ] No placeholder values in docs
- [ ] All links work
- [ ] YAML files validated
- [ ] Version number incremented
- [ ] Git tag created
- [ ] GitHub release created

---

## 🤝 Contributing Files

When contributing, ensure:

1. **New features** get:
   - Example in `examples/`
   - Documentation in README.md
   - Entry in CHANGELOG.md

2. **Bug fixes** get:
   - Fix in example files
   - Entry in CHANGELOG.md
   - Update TROUBLESHOOTING.md if relevant

3. **Documentation** updates:
   - Clear and concise writing
   - Code examples when helpful
   - Screenshots if visual

---

## 📚 Documentation Hierarchy

```
README.md (overview + complete guide)
├── QUICK_START.md (fast setup)
├── examples/ (copy-paste configurations)
├── TROUBLESHOOTING.md (problem solving)
└── docs/ (advanced topics)
    ├── CUSTOMIZATION.md
    ├── API.md
    └── CONTRIBUTING.md
```

---

## 🔗 External Links

Maintain these links across documentation:

- [Hassalarm GitHub](https://github.com/Johboh/hassalarm)
- [Hassalarm Play Store](https://play.google.com/store/apps/details?id=com.fjun.hassalarm)
- [Home Assistant Companion](https://companion.home-assistant.io/)
- [button-card GitHub](https://github.com/custom-cards/button-card)
- [HACS](https://hacs.xyz/)
- [Home Assistant Community](https://community.home-assistant.io/)

---

## ✅ Repository Checklist

A complete repository should have:

- [x] Clear README with setup instructions
- [x] Quick start guide
- [x] Troubleshooting documentation
- [x] Complete examples
- [x] License file
- [x] Changelog
- [x] Issue templates
- [x] Contributing guidelines
- [x] Screenshots and diagrams
- [x] Proper repository structure

---

This structure ensures:
- Easy navigation for users
- Clear contribution process
- Professional presentation
- Maintainable codebase
- Helpful documentation