# AGENTS.md

## Project
This repository is menglei3002's personal collection of SuperPowers Skills. It is Markdown-based and optimized for reusable AI workflows including AI music generation, FFmpeg video composition, and capcut-mate integration.

## Commands
- Validate marketplace JSON: `python -m json.tool .claude-plugin/marketplace.json >/dev/null`
- List skills: `find plugins -name 'SKILL.md' | sort`

## Project Structure
- `plugins/myskills/skills/<skill-name>/SKILL.md` - required skill entry point with YAML frontmatter
- `.claude-plugin/marketplace.json` - plugin marketplace listing
- `README.md` - human-facing skill map

## Skill Conventions
- Keep `SKILL.md` frontmatter to `name` and `description` only
- Write descriptions as trigger rules: what the skill does, when to use it
- Keep `SKILL.md` as a dispatcher; move long guidance to skill body sections
- Do not add extra docs like changelogs or install guides inside a skill unless necessary

## Adding Or Updating Skills
- Create new skills under `plugins/myskills/skills/<lowercase-hyphen-name>/`
- Update `marketplace.json` plugins[0].skills array when adding new skills
- Update `README.md` to reflect new additions

## Safety
- Do not include secrets, tokens, private keys, or personal local paths in shared files
- Paths like `D:\AI\...` are machine-specific and should be clearly noted
