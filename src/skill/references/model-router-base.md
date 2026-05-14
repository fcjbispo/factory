# Model Router Base — Documentation

*Reference for the factory skill*

## Concept

The Model Router is a **declarative** component (JSON) that defines:
1. Which LLM models are available
2. How to allocate them by agent role
3. Which fallback to use when the ideal model is unavailable

## How It Works

### Discovery Phase

When an agent initializes, the skill:
1. Lists available models from `$FACTORY_RUNTIME_CONFIG` or provider API
2. Maps model capabilities (coding, reasoning, analysis)
3. Matches capabilities to role requirements

### Allocation Phase

For the requested role, find models that:
- Satisfy minimum requirements (context window, reasoning level)
- Are currently available (loaded in provider)
- Do not exceed the 3-model concurrency limit

### Fallback Phase

If the primary model is unavailable:
1. Try the first fallback in the same family
2. If all fail, use the default model
3. Report to the user which model was allocated and why

## File Structure

```json
{
  "schema": "model-router-v1",
  "roles": {
    "senior-architect": {
      "requirements": {
        "minContextWindow": 128000,
        "reasoningLevel": "high",
        "codingCapability": "advanced"
      },
      "models": {
        "primary": "kimi-k2.6",
        "fallback": ["deepseek-v4-pro", "qwen3.5"]
      }
    }
  }
}
```

## Roles Defined

| Role | Requirements | Primary | Fallback |
|------|-------------|---------|----------|
| Senior Architect | High reasoning, large context | kimi-k2.6 | deepseek-v4-pro, qwen3.5 |
| Full-Stack Developer | Coding, versatility | deepseek-v4-pro | qwen3-coder, kimi-k2.6 |
| Code Reviewer | Analysis, detail | devstral-2 | qwen3-coder, deepseek-v4-pro |
| DB Architect | Schema, SQL | kimi-k2.6 | deepseek-v4-pro, qwen3.5 |
| QA Tester | Testing, automation | qwen3-coder | deepseek-v4-pro, kimi-k2.6 |
| DevOps/SRE | Speed, ops | deepseek-v4-flash | glm-5.1, deepseek-v4-pro |
| Security Analyst | Depth, risk | deepseek-v4-pro | kimi-k2.6, qwen3.5 |

---

*Factory skill v1.3.1 reference*
