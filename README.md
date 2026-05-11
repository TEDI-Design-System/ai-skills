# TEDI AI skills

AI assistant skills for contributing to the [TEDI Design System](https://github.com/TEDI-Design-System). This repo is a **Claude Code plugin marketplace** so contributors can install the skills with two commands; the same skill files also work with other AI tooling (instructions below).

## What's in the box

Two plugins, one per framework:

| Plugin | Skill | What it does |
| --- | --- | --- |
| `tedi-react-contributing` | `contributing` | Guides creating, refactoring, testing, and a11y-auditing components in [`TEDI-Design-System/react`](https://github.com/TEDI-Design-System/react). Bundles a post-edit hook that auto-runs the matching Jest spec. |
| `tedi-angular-contributing` | `contributing` | Same workflow for [`TEDI-Design-System/angular`](https://github.com/TEDI-Design-System/angular), with an Angular-aware post-edit hook. |

Out of scope: the consumer-facing *integration* skills (`tedi-react`, `tedi-angular`) that live inside the implementation repos for downstream apps.

## Prerequisite: sibling working repos

The skills themselves don't need to live anywhere specific — the marketplace install handles that, and manual installs can live anywhere on disk. But the *working* TEDI repos (the ones you're contributing to) reference each other with relative paths: the React contributor skill peeks at `../angular/` and `../core/` for behavioral parity and design tokens, and the Angular skill mirrors that. For those references to resolve, the TEDI implementation repos must sit side-by-side:

```
your-workspace/
├── react/      # TEDI-Design-System/react
├── angular/    # TEDI-Design-System/angular
└── core/       # TEDI-Design-System/core   (design tokens)
```

```bash
mkdir tedi && cd tedi
git clone https://github.com/TEDI-Design-System/react.git
git clone https://github.com/TEDI-Design-System/angular.git
git clone https://github.com/TEDI-Design-System/core.git
```

If you only ever work on one framework you can skip the others, but cross-references in the skill will surface as "not found" notes.

## Install for Claude Code (primary path)

```
/plugin marketplace add TEDI-Design-System/ai-skills
/plugin install tedi-react-contributing@tedi
/plugin install tedi-angular-contributing@tedi
```

Then invoke from inside the relevant repo:

```
/tedi-react-contributing:contributing add a new Button variant per the Figma frame
```

The post-edit hook activates automatically — when you edit a file under `src/tedi/` (React) or `tedi/` (Angular), the matching spec runs.

See [`docs/install-claude-code.md`](docs/install-claude-code.md) for details, local-dev install, and uninstall.

## Install for other tools

- [Copilot CLI](docs/install-copilot-cli.md)
- [Gemini CLI](docs/install-gemini-cli.md)
- [Cursor / VS Code (manual)](docs/install-cursor-vscode.md)

The `SKILL.md` format (Markdown body + YAML frontmatter) is portable across these tools; only the discovery mechanism differs.

## Repo layout

```
ai-skills/
├── .claude-plugin/marketplace.json     # marketplace manifest
├── plugins/
│   ├── tedi-react-contributing/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/contributing/        # SKILL.md + references/
│   │   └── hooks/                      # hooks.json + post-edit-test.sh
│   └── tedi-angular-contributing/      # (mirror)
├── docs/                               # per-tool install guides
├── LICENSE
└── README.md
```

## Contributing to the skills

Skills are plain Markdown files with YAML frontmatter. Edit `plugins/<plugin>/skills/contributing/SKILL.md` or any file under `references/`. Validate with:

```bash
claude plugin validate .
```

Then update the marketplace cache locally to pick up your changes:

```bash
claude plugin marketplace update tedi
```

## License

MIT — see [LICENSE](LICENSE).
