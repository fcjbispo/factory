# Model Router Override — Customization per Agent

*Reference for the factory skill*

## Why Override?

The Model Router Base (`config/model-router-base.json`) defines default allocations
that work for 90% of cases. But a specific agent may need adjustments for:

- **Experimentation** — Testing new models
- **Specialization** — Deeper focus on a specific domain
- **Performance** — Faster responses for operational tasks
- **Availability** — Models that are not in the base but are available

## How to Create an Override

### 1. Create the File

```bash
mkdir -p $FACTORY_AGENT_OVERRIDES_DIR/<agent-name>/
touch $FACTORY_AGENT_OVERRIDES_DIR/<agent-name>/factory-overrides.json
```

### 2. Define the Override

```json
{
  "overrides": {
    "<role>": {
      "models": {
        "primary": "<model-name>",
        "fallback": ["<model-1>", "<model-2>"]
      }
    }
  }
}
```

### 3. Validate

The skill validates the override against the schema before applying.

## Priority Order

1. **Agent override** — Highest priority
2. **Base configuration** — Default
3. **Fallback** — When nothing else works

## Limitations

- Maximum 3 simultaneous models (provider restriction)
- Fallbacks must be available in runtime config (`$FACTORY_RUNTIME_CONFIG`)
- Overrides are optional — if invalid, the base is used

## Runtime-Specific Paths

| Runtime | Default `FACTORY_AGENT_OVERRIDES_DIR` |
|---------|--------------------------------------|
| OpenClaw | `~/.openclaw/workspace/agents/` |
| Claude Code | `~/.claude/agents/` |
| Custom | Set `FACTORY_AGENT_OVERRIDES_DIR` env var |

---

*Factory skill v1.3.1 reference*
