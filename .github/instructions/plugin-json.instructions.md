---
name: "Appoint Plugin JSON Instructions"
description: "Instructions for authoring and editing plugin.json manifest files in Appoint AI plugins."
applyTo: "**/.plugins/**/plugin.json"
---
# Plugin Manifest Instructions (`plugin.json`)

These instructions apply to all `plugin.json` files in the `.plugins/` directory.

## Required Fields

Every `plugin.json` MUST include:

```json
{
  "name": "appoint-<plugin-name>",
  "description": "One-sentence description of what the plugin does.",
  "version": "1.0.0",
  "author": {
    "name": "Appoint AI",
    "email": "dev@appoint.ai",
    "url": "https://github.com/Appoint-AI"
  },
  "skills": "skills/",
  "agents": "agents/",
  "hooks": "hooks/hooks.json",
  "mcpServers": ".mcp.json"
}
```

## Naming Rules

- `name` must be `appoint-<plugin-name>` format.
- Only lowercase letters, numbers, and hyphens allowed.
- Maximum 64 characters.
- No slashes, colons, dots, or namespace prefixes — these cause **silent load failures**.

## Version Bumping

- Follow semantic versioning (`MAJOR.MINOR.PATCH`).
- Bump `PATCH` for bug fixes, `MINOR` for new features, `MAJOR` for breaking changes.
- Always bump `version` when publishing changes to `marketplace.json`.

## Fields Reference

| Field        | Type     | Required | Description                                                    |
|--------------|----------|----------|----------------------------------------------------------------|
| `name`       | string   | ✅        | Kebab-case plugin identifier, must start with `appoint-`      |
| `description`| string   | ✅        | Brief plugin description (max 1024 chars)                      |
| `version`    | string   | ✅        | Semantic version (`1.0.0`)                                     |
| `author`     | object   | ✅        | `name` (required), `email`, `url`                              |
| `skills`     | string   | ✅        | Path to skills directory (default: `"skills/"`)                |
| `agents`     | string   | ✅        | Path to agents directory (default: `"agents/"`)                |
| `hooks`      | string   | ✅        | Path to hooks config (default: `"hooks/hooks.json"`)           |
| `mcpServers` | string   | optional | Path to MCP config file (default: `".mcp.json"`)               |
