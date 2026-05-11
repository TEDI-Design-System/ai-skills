# Install for Claude Code

Claude Code is the primary target — the repo *is* a Claude Code plugin marketplace.

## From GitHub (once the repo is published)

```
/plugin marketplace add TEDI-Design-System/ai-skills
/plugin install tedi-react-contributing@tedi
/plugin install tedi-angular-contributing@tedi
```

Pin to a branch or tag by appending `@ref`:

```
/plugin marketplace add TEDI-Design-System/ai-skills@v1.0
```

## From a local checkout (recommended while iterating)

```bash
git clone https://github.com/TEDI-Design-System/ai-skills.git
```

Inside Claude Code:

```
/plugin marketplace add /absolute/path/to/ai-skills
/plugin install tedi-react-contributing@tedi
```

## Using the skills

Plugin-installed skills are namespaced by plugin name. From inside the React or Angular TEDI repo:

```
/tedi-react-contributing:contributing add a new Button variant per the Figma frame
/tedi-angular-contributing:contributing audit Dropdown for WCAG 2.2 AA
```

The skill router (`SKILL.md`) inspects your request and pulls in the relevant `references/*.md` file (`new-component`, `a11y-review`, `stories`, etc.).

## Post-edit hook

Each plugin bundles a hook registered as `PostToolUse` on `Edit|Write`. When you edit a TEDI component file:

- **React**: a file under `src/tedi/` triggers `npm test -- --testPathPattern=<matching .spec.tsx>`.
- **Angular**: a file under `tedi/` triggers `npx jest <matching .spec.ts> --no-coverage`.

The hook is path-filtered, so editing files in non-TEDI projects is a silent no-op.

## Updating

```
/plugin marketplace update tedi
```

This re-fetches the marketplace and rebuilds plugin caches. Since the plugin manifests omit the `version` field, every commit on the marketplace's default branch is treated as a new version.

## Uninstalling

```
/plugin uninstall tedi-react-contributing@tedi
/plugin uninstall tedi-angular-contributing@tedi
/plugin marketplace remove tedi
```

## Validating changes (when editing the marketplace)

```bash
claude plugin validate /path/to/ai-skills
```

Or from inside Claude Code, after adding the local marketplace:

```
/plugin validate /path/to/ai-skills
```
