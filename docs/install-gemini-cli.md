# Install for Gemini CLI

Gemini CLI reads `SKILL.md`-format skills, loads their metadata at session start, and activates skill bodies on demand via its `activate_skill` tool.

## Discovery locations

Gemini CLI looks for skills in:

1. `~/.gemini/skills/` — user-wide
2. `.gemini/skills/` — project-local (relative to cwd)

## Install (symlink the contributing skill from this repo)

```bash
mkdir -p ~/.gemini/skills
ln -s /absolute/path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing \
      ~/.gemini/skills/tedi-react-contributing
ln -s /absolute/path/to/ai-skills/plugins/tedi-angular-contributing/skills/contributing \
      ~/.gemini/skills/tedi-angular-contributing
```

Or copy:

```bash
cp -R /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing \
      ~/.gemini/skills/tedi-react-contributing
```

## Verify

Start Gemini CLI inside the React or Angular TEDI repo and confirm the skill appears when listing available skills. Trigger phrases live in each skill's `description` frontmatter — Gemini CLI surfaces them as activation hints.

## Tool-name differences

`SKILL.md` references tools by their Claude Code names (`Read`, `Edit`, `Bash`, etc.). Gemini CLI auto-loads the equivalent mapping when activating — no manual edits to the skill body are needed. If you see "unknown tool" warnings, ensure your Gemini CLI is recent enough to include the bundled mapping for Claude Code tool names.

## What does **not** transfer

- The Claude Code `PostToolUse` hook (`hooks.json` + `post-edit-test.sh`) is not auto-installed. To replicate, point Gemini CLI's equivalent hook config at:

  ```
  plugins/tedi-react-contributing/hooks/post-edit-test.sh
  plugins/tedi-angular-contributing/hooks/post-edit-test.sh
  ```
