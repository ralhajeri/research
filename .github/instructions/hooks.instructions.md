---
name: "Appoint Hook Instructions"
description:
  "Instructions for authoring workspace and plugin hook configurations in
  Appoint AI repositories."
applyTo: "**/hooks/**/*.json"
---

<!-- @format -->

# Hook Configuration Instructions (`hooks.json`)

These instructions apply to workspace hook packs under
`.github/hooks/*.json` and plugin-local hook packs under
`.plugins/**/hooks/*.json`. Reference:
[VS Code Agent Hooks Documentation](https://code.visualstudio.com/docs/copilot/customization/hooks)

Operational guidance for this repository's active workspace hook pack lives
in [.github/hooks/README.md](../hooks/README.md).

## Authority

- Workspace hooks under `.github/hooks/*.json` are the authoritative
  runtime enforcement surface for workspace-wide behavior.
- Plugin-local hooks are supplementary only. Do not present them as the
  sole coverage path for built-in/default Copilot sessions.
- Do not use plugin-only path tokens in workspace hook files.

## Lifecycle Events

Appoint enforcement packs should define the full lifecycle set unless a
narrower pack is explicitly documented:

```json
{
  "hooks": {
    "SessionStart":       [...],
    "UserPromptSubmit":   [...],
    "PreToolUse":         [...],
    "PostToolUse":        [...],
    "PreCompact":         [...],
    "SubagentStart":      [...],
    "SubagentStop":       [...],
    "Stop":               [...]
  }
}
```

## Workspace Hook Command Format

Workspace hooks run from the repository root. Use literal repo-relative
commands and `cwd` relative to the repository root.

```json
{
  "type": "command",
  "command": "bash .github/hooks/scripts/pre-tool-use.sh",
  "windows": "powershell -NoProfile -ExecutionPolicy Bypass -File \".github/hooks/scripts/run-bash-hook.ps1\" \".github/hooks/scripts/pre-tool-use.sh\"",
  "cwd": ".",
  "timeout": 15
}
```

- Do not use `${PLUGIN_ROOT}` or other plugin-only interpolation tokens in
  workspace hook files.
- Prefer explicit `cwd` so relative command paths resolve from the
  repository root.
- On Windows, do not assume `bash` on `PATH` is a usable shell; it may
  resolve to the WSL launcher. Prefer a repo-local wrapper that resolves a
  verified Git Bash executable and fails fast when none is installed.
- If hooks write runtime artifacts, keep those files under repo-root
  `.runtime/**`, not under `.git/**`.

## Plugin-Local Hook Command Format

Plugin-local hooks remain secondary to workspace hooks for repo-wide
enforcement. Only keep a plugin-local hook pack when the active plugin
format explicitly documents its runtime root token and command semantics.

```json
{
  "type": "command",
  "command": "${DOCUMENTED_PLUGIN_ROOT}/scripts/<event-name>.sh",
  "windows": "powershell -NoProfile -ExecutionPolicy Bypass -File \"${DOCUMENTED_PLUGIN_ROOT}/scripts/<event-name>.ps1\"",
  "timeout": 30
}
```

- Treat `${DOCUMENTED_PLUGIN_ROOT}` above as a placeholder, not a universal
  token name. Replace it only with a runtime-specific token that the active
  plugin format documents.
- If the active plugin format does not document a plugin-root token, omit
  the plugin-local hook pack rather than inventing one.
- Do not copy plugin-local command shapes into `.github/hooks/*.json`.

## Hook Event Reference

| Event              | When                        | Script                  | Timeout |
| ------------------ | --------------------------- | ----------------------- | ------- |
| `SessionStart`     | First prompt of new session | `session-start.sh`      | 15s     |
| `UserPromptSubmit` | Every user prompt           | `user-prompt-submit.sh` | 10s     |
| `PreToolUse`       | Before any tool invocation  | `pre-tool-use.sh`       | 15s     |
| `PostToolUse`      | After tool completes        | `post-tool-use.sh`      | 30s     |
| `PreCompact`       | Before context compaction   | `pre-compact.sh`        | 10s     |
| `SubagentStart`    | Sub-agent spawned           | `subagent-start.sh`     | 10s     |
| `SubagentStop`     | Sub-agent completes         | `subagent-stop.sh`      | 10s     |
| `Stop`             | Session ends                | `stop.sh`               | 15s     |

## Hook Script Requirements

All hook shell scripts MUST:

1. Start with `#!/usr/bin/env bash`
2. Include `set -euo pipefail`
3. Read JSON from `stdin` (`INPUT="$(cat)"`)
4. Output valid JSON to `stdout`
5. Use only documented event output shapes for `continue`, `systemMessage`,
   `permissionDecision`, or `decision` / `reason`
6. Exit `0` for success, `2` for blocking error, non-zero for non-blocking
   warning

## Exit Code Behavior

| Exit Code | Behavior                                                 |
| --------- | -------------------------------------------------------- |
| `0`       | Success — VS Code parses stdout as JSON                  |
| `2`       | Blocking error — stops processing, shows stderr to model |
| Other     | Non-blocking warning — shows to user, continues          |

## PreToolUse Permission Decisions

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "allow",
    "permissionDecisionReason": "Operation is safe",
    "additionalContext": "Context for the AI model"
  }
}
```

| Value   | Behavior                           |
| ------- | ---------------------------------- |
| `allow` | Auto-approve (least restrictive)   |
| `ask`   | Require user confirmation          |
| `deny`  | Block operation (most restrictive) |

**Priority**: When multiple hooks run, the most restrictive wins: `deny` >
`ask` > `allow`.

## Stop-Time Blocking Shapes

- `PostToolUse` and `SubagentStop` block with top-level `decision` and
  `reason`.
- `Stop` blocks with `hookSpecificOutput.decision` and
  `hookSpecificOutput.reason`.
- `SessionStart` and `SubagentStart` inject `additionalContext` through
  `hookSpecificOutput`.
- `UserPromptSubmit` and `PreCompact` use the common `continue` /
  `stopReason` / `systemMessage` shape.

## OS-Specific Commands

```json
{
  "type": "command",
  "command": "${DOCUMENTED_PLUGIN_ROOT}/scripts/session-start.sh",
  "windows": "powershell -NoProfile -ExecutionPolicy Bypass -File \"${DOCUMENTED_PLUGIN_ROOT}/scripts/session-start.ps1\"",
  "linux": "${DOCUMENTED_PLUGIN_ROOT}/scripts/session-start.sh",
  "osx": "${DOCUMENTED_PLUGIN_ROOT}/scripts/session-start.sh",
  "timeout": 15
}
```
