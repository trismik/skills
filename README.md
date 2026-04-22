# Trismik Skills

Claude Code skills for working with [Trismik](https://trismik.com).

## Available skills

| Skill | What it does |
|-------|--------------|
| [`quickcompare-format`](skills/quickcompare-format/) | Prepares a dataset (CSV or JSONL) for upload to Trismik QuickCompare. Advises on columns, names, and LLM-as-Judge annotation columns. |

## Installation

### Claude Code

This repo is a [Claude Code plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces). Add it once, then install any skill:

```
/plugin marketplace add trismik/skills
/plugin install quickcompare-format@trismik-skills
```

Invoke the skill with `/quickcompare-format`, or just describe your task and Claude will load it when the description matches.

### Codex

Use the built-in [`$skill-installer`](https://developers.openai.com/codex/skills) with a GitHub subdirectory URL:

```
$skill-installer install https://github.com/trismik/skills/tree/main/skills/quickcompare-format
```

Restart Codex after install.

### Manual (any tool)

Each skill is a drop-in folder following the [Agent Skills](https://agentskills.io) standard, so you can bypass the installers. Clone the repo and symlink into `~/.claude/skills/` (all projects) or your project's `.claude/skills/`:

```sh
git clone https://github.com/trismik/skills.git ~/trismik-skills
ln -s ~/trismik-skills/skills/quickcompare-format ~/.claude/skills/quickcompare-format
```

## Use with other tools

These skills follow the open [Agent Skills](https://agentskills.io) standard (`SKILL.md` + optional `references/`). Any assistant that natively loads skill folders will pick them up — just drop the folder in the location that tool expects:

I- [Cursor](https://cursor.com/docs/skills)
- [GitHub Copilot](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/add-skills) (and [Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/add-skills))
- [Gemini CLI](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/skills.md)
- See the full adopter list at [agentskills.io](https://agentskills.io).

For tools that don't support skill folders yet (Windsurf, Cline, Aider, Zed, …), flatten `SKILL.md` and its referenced files into that tool's rule format manually.

## Contributing

Issues and PRs welcome. Each skill lives under `skills/<name>/` and follows the [Agent Skills](https://agentskills.io) standard: a `SKILL.md` with YAML frontmatter (`name`, `description`) and optional `references/`.

## License

MIT — see [LICENSE](LICENSE).
