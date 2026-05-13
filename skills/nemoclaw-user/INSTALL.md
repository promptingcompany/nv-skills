# Install nemoclaw-user

Copy this skill directory into the skill directory used by your agent runtime.

## Claude Code

```bash
cp -r skills/nemoclaw-user ~/.claude/skills/
```

## Codex

```bash
mkdir -p .codex/skills
cp -r skills/nemoclaw-user .codex/skills/
```

## Cursor

```bash
mkdir -p .cursor/skills
cp -r skills/nemoclaw-user .cursor/skills/
```

After copying, restart or reload the agent runtime so it can discover the skill
metadata.
