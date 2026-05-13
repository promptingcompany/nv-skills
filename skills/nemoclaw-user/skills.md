# NemoClaw User Skill Index

This package consolidates the user-facing NemoClaw skills into one top-level
skill. Each referenced prompt is copied from the corresponding
`skills/NemoClaw/nemoclaw-user-*` source skill.

## User Workflows

| Area | Reference |
|---|---|
| Agent skills and coding assistant integration | [`references/nemoclaw-user-agent-skills/SKILL.md`](references/nemoclaw-user-agent-skills/SKILL.md) |
| Local inference configuration | [`references/nemoclaw-user-configure-inference/SKILL.md`](references/nemoclaw-user-configure-inference/SKILL.md) |
| Security posture and controls | [`references/nemoclaw-user-configure-security/SKILL.md`](references/nemoclaw-user-configure-security/SKILL.md) |
| Remote GPU deployment | [`references/nemoclaw-user-deploy-remote/SKILL.md`](references/nemoclaw-user-deploy-remote/SKILL.md) |
| Quickstart and first sandbox | [`references/nemoclaw-user-get-started/SKILL.md`](references/nemoclaw-user-get-started/SKILL.md) |
| Network policy management | [`references/nemoclaw-user-manage-policy/SKILL.md`](references/nemoclaw-user-manage-policy/SKILL.md) |
| Sandbox lifecycle management | [`references/nemoclaw-user-manage-sandboxes/SKILL.md`](references/nemoclaw-user-manage-sandboxes/SKILL.md) |
| Sandbox monitoring and debugging | [`references/nemoclaw-user-monitor-sandbox/SKILL.md`](references/nemoclaw-user-monitor-sandbox/SKILL.md) |
| Ecosystem overview | [`references/nemoclaw-user-overview/SKILL.md`](references/nemoclaw-user-overview/SKILL.md) |
| Architecture, CLI reference, and troubleshooting | [`references/nemoclaw-user-reference/SKILL.md`](references/nemoclaw-user-reference/SKILL.md) |
| Issue and PR triage instructions | [`references/nemoclaw-user-triage-instructions/SKILL.md`](references/nemoclaw-user-triage-instructions/SKILL.md) |

## Notes

Some referenced prompts include their own nested `references/` directory. Those
files are copied alongside the prompt so relative links inside each prompt still
resolve within this package.
