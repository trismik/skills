# Trismik Skills

Claude Code skills for working with [Trismik](https://trismik.com).

## Available skills

| Skill | What it does |
|-------|--------------|
| [`quickcompare-format`](skills/quickcompare-format/) | Prepares a dataset (CSV or JSONL) for upload to Trismik QuickCompare. Advises on columns, names, and LLM-as-Judge annotation columns. |

## Install

Skills are drop-in folders. Copy the one you want into your project's `.claude/skills/` (project-scoped) or `~/.claude/skills/` (available everywhere).

**Project scope:**

```sh
mkdir -p .claude/skills
cp -r path/to/trismik-skills/skills/quickcompare-format .claude/skills/
```

**User scope (all projects):**

```sh
mkdir -p ~/.claude/skills
cp -r path/to/trismik-skills/skills/quickcompare-format ~/.claude/skills/
```

Or clone this repo and symlink:

```sh
git clone https://github.com/trismik/skills.git ~/trismik-skills
ln -s ~/trismik-skills/skills/quickcompare-format ~/.claude/skills/quickcompare-format
```

Claude Code picks up the skill on next start. Invoke it with `/quickcompare-format` or just describe your task and Claude will load the skill when its description matches.

## Use with other tools

These skills are written in the [Anthropic Agent Skills format](https://www.anthropic.com/news/skills) (`SKILL.md` + optional `references/`). To use the same guidance in Cursor, GitHub Copilot, or another assistant, flatten `SKILL.md` and the referenced files into a single file that matches that tool's rule format:

- **Cursor:** `.cursor/rules/<name>.mdc` with MDC frontmatter
- **Copilot:** `.github/prompts/<name>.prompt.md`

We may ship adapters later; for now, a manual copy works.

## Contributing

Issues and PRs welcome. Each skill lives under `skills/<name>/` and follows the Agent Skills format: a `SKILL.md` with YAML frontmatter (`name`, `description`) and optional `references/`.

## License

MIT — see [LICENSE](LICENSE).
