# Contributing to Hassalarm Wake-Up Light Integration

Thank you for your interest in contributing! This project thrives on community input and improvements.

## 🎯 How You Can Help

There are many ways to contribute:

- 🐛 **Report bugs** - Found something that doesn't work?
- 💡 **Suggest features** - Have ideas for improvements?
- 📝 **Improve documentation** - Make guides clearer
- 🔧 **Submit code** - Fix bugs or add features
- 🎨 **Create examples** - Share your customizations
- 💬 **Help others** - Answer questions in issues
- ⭐ **Star the repo** - Show your support!

---

## 🐛 Reporting Bugs

Before reporting a bug:

1. **Check existing issues** - Your bug might already be reported
2. **Try troubleshooting** - Read [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
3. **Test on latest version** - Update Home Assistant and try again

When reporting a bug, include:

### Required Information

```markdown
**Home Assistant Version:** 2024.10.0
**Hassalarm App Version:** 1.2.3
**Integration Version:** 1.0.0

**Describe the bug:**
A clear description of what's wrong.

**Expected behavior:**
What should happen instead.

**Steps to reproduce:**
1. Set alarm to 7:00 AM
2. Wait for alarm to trigger
3. Light doesn't flash

**Configuration:**
```yaml
# Paste relevant configuration here
```

**Logs:**
```
# Paste relevant Home Assistant logs here
```

**Screenshots:**
If applicable, add screenshots showing the issue.
```

### Use the Bug Report Template

We have an issue template that includes all necessary fields. Use it!

---

## 💡 Suggesting Features

We love new ideas! Before suggesting:

1. **Check existing issues** - Might already be planned
2. **Check FAQ** - Might already be possible
3. **Consider scope** - Should fit project goals

When suggesting a feature:

### Feature Request Template

```markdown
**Feature Description:**
Clear description of what you want.

**Use Case:**
Why is this useful? Who benefits?

**Proposed Solution:**
How might this work?

**Alternatives Considered:**
Other ways to achieve the same goal?

**Additional Context:**
Any other information, mockups, examples?
```

---

## 📝 Improving Documentation

Documentation improvements are always welcome!

### Types of Documentation

- **README.md** - Main documentation
- **QUICK_START.md** - Setup guide
- **TROUBLESHOOTING.md** - Problem solving
- **Examples** - Configuration samples
- **Comments** - Code explanations

### Documentation Standards

- Use **clear, simple language**
- Include **code examples**
- Add **screenshots** where helpful
- Test all **instructions** yourself
- Use **proper markdown** formatting
- Follow existing **style and tone**

### Small Documentation Fixes

For typos, grammar, or small clarifications:

1. Click "Edit this file" on GitHub
2. Make your changes
3. Submit a pull request
4. We'll review and merge quickly!

---

## 🔧 Contributing Code

### Development Setup

1. **Fork the repository**
2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/hassalarm-wakeup-light.git
   cd hassalarm-wakeup-light
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

### Code Guidelines

#### YAML Configuration

```yaml
# Use 2 spaces for indentation
automation:
  - alias: "Clear, Descriptive Names"
    description: "Explain what this does"
    trigger:
      - trigger: template
        value_template: "{{ ... }}"
    # Add comments for complex logic
    condition:
      - condition: state
        entity_id: input_boolean.example
        state: "on"
    action:
      - action: light.turn_on
        target:
          entity_id: light.bedroom  # Use descriptive entity names
```

#### Best Practices

- **Use descriptive names** for entities and aliases
- **Add comments** explaining complex logic
- **Test thoroughly** in a real Home Assistant setup
- **Validate YAML** before committing
- **Keep it simple** - prefer clarity over cleverness
- **Make it configurable** - use variables where possible

### Testing Your Changes

Before submitting:

1. **Test in Home Assistant**
   - Does it work as expected?
   - No errors in logs?
   - Automations trigger correctly?

2. **Validate YAML**
   - Use Home Assistant's YAML validator
   - Check syntax online: https://www.yamllint.com/

3. **Test edge cases**
   - What if device is offline?
   - What if alarm is cancelled?
   - What if Home Assistant restarts?

4. **Update documentation**
   - Add to README.md if needed
   - Update examples/
   - Add to CHANGELOG.md

---

## 📤 Submitting Changes

### Pull Request Process

1. **Update your fork**
   ```bash
   git fetch upstream
   git merge upstream/main
   ```

2. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: Add brightness ramp option to light script"
   ```

3. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

4. **Create Pull Request**
   - Go to GitHub
   - Click "New Pull Request"
   - Fill in the template
   - Wait for review!

### Commit Message Format

Use conventional commits:

- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation changes
- `style:` Code style (formatting, no logic change)
- `refactor:` Code refactoring
- `test:` Adding tests
- `chore:` Maintenance tasks

Examples:
```
feat: Add color cycling light pattern
fix: Correct time zone comparison in automation
docs: Update troubleshooting guide with new solution
```

### Pull Request Template

When creating a PR, include:

```markdown
**Description:**
What does this PR do?

**Type of Change:**
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Breaking change

**Checklist:**
- [ ] Tested in Home Assistant
- [ ] Updated documentation
- [ ] Added to CHANGELOG.md
- [ ] YAML validated
- [ ] No errors in HA logs

**Screenshots:**
If applicable, show before/after.

**Related Issues:**
Closes #123
```

---

## 🎨 Contributing Examples

Have a cool customization? Share it!

### Example Contributions

1. **New light patterns** - Unique flash sequences
2. **Alternative automations** - Different trigger conditions
3. **Dashboard designs** - Creative card layouts
4. **Advanced integrations** - Combined with other services

### Submitting Examples

1. Create file in `examples/` or `examples/custom/`
2. Add clear comments explaining each section
3. Include description in pull request
4. Add screenshot if visual

---

## 💬 Community Guidelines

### Be Respectful

- Welcome newcomers
- Be patient with questions
- Provide constructive feedback
- Respect different skill levels
- No harassment or discrimination

### Communication

- **Issues** - Bug reports, feature requests
- **Pull Requests** - Code contributions
- **Discussions** - Questions, ideas, general chat
- **Discord/Forum** - Real-time help

### Response Times

We're volunteers! Please be patient:

- Issues reviewed within 1 week
- Pull requests reviewed within 2 weeks
- Complex changes may take longer

---

## 📋 Checklist for Major Contributions

Before submitting major changes:

- [ ] Discussed in an issue first
- [ ] Tested extensively
- [ ] Updated all relevant docs
- [ ] Added examples if needed
- [ ] Updated CHANGELOG.md
- [ ] Validated all YAML
- [ ] Checked Home Assistant logs
- [ ] Screenshots if visual changes
- [ ] Follows code style
- [ ] No breaking changes (or documented)

---

## 🏆 Recognition

Contributors are recognized in:

- **CHANGELOG.md** - Your contribution logged
- **README.md** - Credits section
- **GitHub** - Contributor badge
- **Release notes** - Highlighted changes

---

## ❓ Questions?

- **General questions** - Open a Discussion
- **Bug reports** - Open an Issue
- **Feature ideas** - Open an Issue
- **Code questions** - Comment on PR
- **Urgent matters** - Contact maintainer

---

## 📚 Resources

### Learn Home Assistant

- [Home Assistant Docs](https://www.home-assistant.io/docs/)
- [Automation Guide](https://www.home-assistant.io/docs/automation/)
- [YAML Syntax](https://www.home-assistant.io/docs/configuration/yaml/)
- [Templating Guide](https://www.home-assistant.io/docs/configuration/templating/)

### Learn Git & GitHub

- [GitHub Docs](https://docs.github.com/en)
- [Git Tutorial](https://git-scm.com/docs/gittutorial)
- [Markdown Guide](https://www.markdownguide.org/)

### Related Projects

- [Hassalarm](https://github.com/Johboh/hassalarm)
- [button-card](https://github.com/custom-cards/button-card)
- [HACS](https://hacs.xyz/)

---

## 🎉 Thank You!

Every contribution helps make this project better. Whether you:

- Report a bug
- Fix a typo
- Add a feature
- Help someone in issues

**You're making a difference!**

We appreciate you taking the time to contribute. 🙏

---

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

See [LICENSE](LICENSE) for details.