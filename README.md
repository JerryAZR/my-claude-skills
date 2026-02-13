# My Claude Skills

A collection of custom Claude skills that extend Claude's capabilities with specialized knowledge, workflows, and tools.

## Available Skills

### skill-creator
Guide for creating effective skills. Use when you need to create a new skill (or update an existing skill) that extends Claude's capabilities.

## Adding a New Skill

1. Initialize the skill structure:
   ```bash
   python .claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path .claude/skills
   ```

2. Edit the generated SKILL.md with your skill's instructions

3. Package the skill:
   ```bash
   python .claude/skills/skill-creator/scripts/package_skill.py .claude/skills/<skill-name>
   ```
