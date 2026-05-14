# Model Router — Override Examples

*Reference for the factory skill*

## Example 1: Architect Agent — Override for cogito-2.1

**Situation:** Architect Agent wants to experiment with cogito-2.1 for architecture.

**File:** `~/.openclaw/workspace/agents/architect/factory-overrides.json`

```json
{
  "overrides": {
    "senior-architect": {
      "models": {
        "primary": "cogito-2.1",
        "fallback": ["kimi-k2.6", "deepseek-v4-pro"]
      }
    }
  }
}
```

**Result:** Architect Agent uses cogito-2.1 when available, falls back to kimi-k2.6.

## Example 2: DevOps Agent — Faster DevOps

**Situation:** DevOps Agent wants faster responses for infrastructure tasks.

**File:** `~/.openclaw/workspace/agents/devops/factory-overrides.json`

```json
{
  "overrides": {
    "devops-sre": {
      "models": {
        "primary": "deepseek-v4-flash",
        "fallback": ["glm-5.1", "deepseek-v4-pro"]
      }
    }
  }
}
```

## Example 3: DB Agent — Schema Focus

**Situation:** DB Agent needs deep reasoning for data modeling.

**File:** `~/.openclaw/workspace/agents/db/factory-overrides.json`

```json
{
  "overrides": {
    "db-architect": {
      "models": {
        "primary": "kimi-k2.6",
        "fallback": ["deepseek-v4-pro", "qwen3.5"]
      }
    }
  }
}
```

---

*Factory skill v1.3.1 reference*
