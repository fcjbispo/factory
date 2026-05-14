# Model Router — Override Examples

*Reference for the factory skill*

## Example 1: Zwen — Override for cogito-2.1

**Situation:** Zwen wants to experiment with cogito-2.1 for architecture.

**File:** `~/.openclaw/workspace/agents/zwen/factory-overrides.json`

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

**Result:** Zwen uses cogito-2.1 when available, falls back to kimi-k2.6.

## Example 2: Hel — Faster DevOps

**Situation:** Hel wants faster responses for infrastructure tasks.

**File:** `~/.openclaw/workspace/agents/hel/factory-overrides.json`

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

## Example 3: Balder — Schema Focus

**Situation:** Balder needs deep reasoning for data modeling.

**File:** `~/.openclaw/workspace/agents/balder/factory-overrides.json`

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
