# Install for GitHub Copilot CLI

Copilot CLI supports Anthropic's open Agent Skills format (`SKILL.md` with YAML frontmatter) and exposes installed skills through its `skill` tool. The TEDI skills work without modification.

## Option 1: copy the skill folder into Copilot's user skills directory

Copilot CLI auto-discovers skills from:

```
~/.copilot/skills/
```

Symlink (so updates flow when you `git pull` in `ai-skills`):

```bash
mkdir -p ~/.copilot/skills
ln -s /absolute/path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing \
      ~/.copilot/skills/tedi-react-contributing
ln -s /absolute/path/to/ai-skills/plugins/tedi-angular-contributing/skills/contributing \
      ~/.copilot/skills/tedi-angular-contributing
```

Or copy if you prefer to pin a snapshot:

```bash
cp -R /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing \
      ~/.copilot/skills/tedi-react-contributing
cp -R /path/to/ai-skills/plugins/tedi-angular-contributing/skills/contributing \
      ~/.copilot/skills/tedi-angular-contributing
```

## Option 2: project-local install

Place skills under `.copilot/skills/` in the repo where you'll use them:

```bash
mkdir -p .copilot/skills
cp -R /path/to/ai-skills/plugins/tedi-react-contributing/skills/contributing .copilot/skills/tedi-react-contributing
```

## Using the skills

Copilot CLI invokes skills by name through its `skill` tool. The skill description (from the `SKILL.md` frontmatter) drives matching, so prompts like:

> "add a new Card variant per the Figma frame, follow TEDI contributing"

will auto-activate `tedi-react-contributing` when you're inside the React repo.

## What does **not** transfer

- The `PostToolUse` hook that auto-runs Jest specs is Claude-Code-specific. Copilot CLI uses its own hook system; if you want the same behavior, configure it through Copilot's tool-event hooks pointing at the same `post-edit-test.sh` script (path: `plugins/tedi-react-contributing/hooks/post-edit-test.sh`).
- Skill namespacing differs — there is no `/<plugin>:<skill>` syntax in Copilot CLI.
