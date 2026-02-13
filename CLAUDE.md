# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a meta-skill project for creating Claude skills. It contains the `skill-creator` skill, which provides guidance and tooling for building new Claude skills.

## Commands

### Initialize a new skill
```bash
python .claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path <output-directory>
```

### Package a skill into a .skill file
```bash
python .claude/skills/skill-creator/scripts/package_skill.py <path/to/skill-folder> [output-directory]
```

### Quick validation
```bash
python .claude/skills/skill-creator/scripts/quick_validate.py
```

## Architecture

This project follows the standard skill structure:

- **Skills** are stored in `.claude/skills/`
- Each skill requires a `SKILL.md` file with YAML frontmatter containing `name` and `description` fields
- Optional bundled resources:
  - `scripts/` - Executable code (Python/Bash)
  - `references/` - Documentation loaded as needed
  - `assets/` - Files used in output (templates, images)

The `package_skill.py` script creates distributable `.skill` files (which are zip files) after validating the skill structure.

## Skill Creation Workflow

1. Use the `init_skill.py` script to scaffold a new skill
2. Edit the generated SKILL.md with the skill's purpose and instructions
3. Add scripts, references, and/or assets as needed
4. Run `package_skill.py` to validate and package the skill
