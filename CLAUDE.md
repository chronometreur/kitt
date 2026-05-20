# KITT

CLI tool for initializing AI-optimized project environments. Given a stack (language, framework, db, workflow) and a list of AI tools, it copies the right prompts, templates, skills, workflows, and conventions from a personal catalog into the target project.

## What this repo is

This repo has two roles:

1. **CLI source code** — `packages/` contains the tool itself
2. **Personal catalog** — `docs/` contains the collection of prompts, templates, skills, and workflows that `kitt` copies into other projects

## Tech stack

- TypeScript
- oclif (CLI framework)
- pnpm workspaces (monorepo)
- Node 20+

## Packages

- `packages/cli/` — Commands, flags, interactive prompts (inquirer), UX output
- `packages/core/` — Composition engine: resolves catalog items by flags, copies files, merges AI tool configs
- `packages/sdk/` — Types and interfaces for catalog items and plugins

## Commands

### `kitt init`

Initializes a fresh project. **Aborts if `.kitt` marker file already exists** in the target directory.

All flags are required. If not passed, they are asked interactively. If stdin is non-interactive (CI), errors with a list of missing flags.

```bash
kitt init \
  --language ruby \
  --framework rails \
  --type api \
  --db postgres \
  --workflow spec-driven \
  --testing rspec \
  --ai claude-code,cursor
```

Flags:
- `--language` — ruby, typescript, python, php, go, etc.
- `--framework` — rails, nextjs, laravel, fastapi, etc.
- `--type` — api, web, cli, worker, etc.
- `--db` — postgres, mysql, sqlite, none
- `--workflow` — spec-driven, tdd, etc.
- `--testing` — rspec, jest, pytest, etc.
- `--ai` — claude-code, cursor, copilot, codex (comma-separated)

What it generates in the target project:

| Flag value | Files/dirs created |
|---|---|
| always | `.ai/`, `docs/specs/`, `docs/architecture/`, `docs/decisions/`, `.kitt` |
| `--ai claude-code` | `CLAUDE.md` |
| `--ai cursor` | `.cursorrules`, `.cursor/` |
| `--ai copilot` | `.github/copilot-instructions.md` |
| `--ai codex` | `.codex/` |

### `kitt add`

Adds a specific catalog item to an already-initialized project. **Aborts if `.kitt` does not exist.**

```bash
kitt add --type prompt --name "pr-review-smell"
kitt add --type skill --name "deploy"
kitt add --type workflow --name "tdd"
kitt add --type template --name "ping-pong-use-case"
```

Types: `prompt`, `skill`, `workflow`, `template`, `convention`

### `kitt mcp` (planned)

Interactive wizard for configuring MCP servers. Separate command because it requires credentials/tokens specific to each service.

## Catalog structure (`docs/`)

Organized by the same taxonomy as `kitt init` flags. The core engine copies in order: `shared/` → `language/<x>/` → `framework/<x>/` → `workflow/<x>/`

```
docs/
  prompts/
    shared/
    language/
    framework/
    workflow/
  templates/       (same subdirs)
  skills/          (same subdirs)
  workflows/       (same subdirs)
  conventions/     (same subdirs)
  integrations/
    claude-code/
    cursor/
    copilot/
    codex/
```

All catalog items are Markdown files with kebab-case names.

## Development conventions

- No comments unless the why is non-obvious
- No backwards-compat shims
- Validate at system boundaries only (user input, fs operations)
- Interactive prompts use `@inquirer/prompts`
- File copies use `fs-extra`
- Path handling uses `globby`
