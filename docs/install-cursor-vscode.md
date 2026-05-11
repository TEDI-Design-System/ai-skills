# Install for Cursor / VS Code (manual)

Cursor and VS Code (with GitHub Copilot or generic AI extensions) don't natively load the Anthropic Agent Skill format. The workaround is to flatten the SKILL.md + reference files into the per-editor rules file that already feeds your assistant's system prompt.

You lose the user-invocable trigger (`/tedi-react-contributing:contributing`) and the auto-fire post-edit hook, but the *content* of the skill still steers the assistant.

## Cursor

Cursor reads project-level rules from `.cursor/rules/*.md` (newer Cursor) or `.cursorrules` (legacy single-file).

### Newer Cursor (recommended)

```bash
mkdir -p .cursor/rules
cat /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing/SKILL.md \
    /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing/references/*.md \
    > .cursor/rules/tedi-react-contributing.md
```

Add a frontmatter block at the top of the resulting file to scope it:

```markdown
---
description: TEDI Design System React contributor guide
globs:
  - "src/tedi/**"
alwaysApply: false
---
```

### Legacy single-file `.cursorrules`

Append the SKILL body (and the references you want as ambient context) to `.cursorrules`. Note this file has no scoping, so the rules always apply — keep the content focused.

## VS Code + GitHub Copilot

Copilot for VS Code reads `.github/copilot-instructions.md` from the repo root.

```bash
mkdir -p .github
cat /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing/SKILL.md \
    /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing/references/best-practices.md \
    > .github/copilot-instructions.md
```

Only include the references you want loaded into every Copilot turn — appending all six bloats the system prompt.

## Caveats

- **No user-invocable command.** The skill becomes ambient context, not something you trigger by name. Phrase your prompts explicitly: "follow the TEDI contributor guide", "do a TEDI a11y review", etc.
- **No post-edit hook.** Run the linked `post-edit-test.sh` manually, or wire it into your editor's "save" event using your task-runner of choice.
- **Stale copies.** Because this is a snapshot, re-run the `cat` after pulling updates from `ai-skills`. A small shell script that re-flattens on demand keeps drift low.
