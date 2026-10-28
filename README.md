# Accessibility Audit

An AI skill that audits a web page or component against WCAG 2.2 level AA. It walks through keyboard access, focus visibility, contrast, text alternatives, form labels and errors, ARIA misuse, motion, target size and reflow, and adds checks specific to Arabic and right-to-left interfaces. The result is a findings table with the success criterion, severity, element and a code-level fix, plus a list of what could not be tested.

It reports findings; it does not certify compliance. Confirm results with assistive-technology users.

## Install

Claude Code:

```
git clone https://github.com/AIX-store/skill-accessibility-audit ~/.claude/skills/accessibility-audit
```

Other agents (Codex, Cursor, Claude Desktop projects): copy the contents of `SKILL.md` into the agent's instructions or rules file.

## Example prompts

- "Audit this signup form component for accessibility issues."
- "Check our Arabic RTL checkout page for keyboard and screen reader problems."
- "Review these colour tokens for contrast in light and dark mode."

## Licence

MIT, © 2026 CodeMeld LLC.

Listed on AiX: https://aixstore.ai/store
