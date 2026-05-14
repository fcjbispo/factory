---
name: factory
description: |
  Initializes Factory projects in OpenClaw, loading context, documentation, and
  configuring the agent to operate according to the Factory framework.
  
  Use when starting work on any project documented in Factory.
  
  Features:
  - Agnostic runtime (OpenClaw, Claude Code, etc.)
  - Dynamic model mapping by role
  - Automatic fallback with concurrency control
  - Integration with Zikkaron for persistent memory
  
triggers:
  - "start project"
  - "factory"
  - "load factory context"
  - "init project"
  - "factory init"
  - "factory-init"

version: "1.3.1"
author: "Factory Team"
date: "2026-05-14"
---

# 🏭 Factory Init — Project Initialization Skill

This skill initializes the Factory context for projects documented in `$FACTORY_ROOT/`.

## 🎯 Objective

Provide the infrastructure for initializing Factory projects for OpenClaw agents,
ensuring:
- Correct loading of documentation per project
- Proper allocation of LLM models by role
- Intelligent fallback with a limit of 3 simultaneous models
- Integration with persistent memory (Zikkaron)

## 🏗️ Architecture

```
skill/
├── SKILL.md                          ← You are here
├── config/
│   ├── model-router-base.json        ← Base model allocation
│   └── model-router-schema.json      ← Validation schema
└── references/
    ├── init-sequence.md              ← Step-by-step initialization
    ├── projects-index.md             ← Factory projects index
    ├── model-router-base.md          ← Base router documentation
    ├── model-router-override.md      ← How to customize per agent
    └── model-router-examples.md      ← Override examples
```

## 🚀 How to Use

### Basic Initialization

```
User: "Start the ExampleProject project"
Agent: Load skill → Read configuration → Apply model by role → Init context
```

### Initialization Sequence

1. **Detect project** — Map name/path to Factory directory
2. **Load documentation** — INDEX.md, context/active.md, context/progress.md
3. **Determine role(s)** — Which role(s) will the agent perform?
4. **Allocate model** — Use Model Router (base or override)
5. **Load Zikkaron context** — `get_project_context(directory)`
6. **Confirm initialization** — Report allocated model and active roles

## 📋 Configuration per Agent

### Base Model (all agents)

The file `config/model-router-base.json` defines the default model allocation
by role, with hierarchical fallbacks.

### Agent Override (optional)

Each agent can customize their allocation by creating:

```
~/.openclaw/workspace/agents/<agent-name>/
└── factory-overrides.json
```

Or declaring in the workspace `AGENTS.md`:

```markdown
## Factory Init Overrides

When loading Factory project:
- Use model-router-base.json as base
- Apply agent-specific overrides
- Fallback: never exceed 3 simultaneous models
```

## 🔧 Integration with OpenClaw

### Skill Registration

To install globally after extracting the Factory distribution:

```bash
# Create symbolic link (recommended)
ln -s $FACTORY_ROOT/skill ~/.openclaw/skills/factory

# Or copy
# cp -r $FACTORY_ROOT/skill ~/.openclaw/skills/factory
```

### Model Registration

Models used by the router must be registered in `~/.openclaw/openclaw.json`:

```json
{
  "agents": {
    "defaults": {
      "models": {
        "ollama/kimi-k2.6:cloud": {},
        "ollama/deepseek-v4-pro:cloud": {},
        "ollama/devstral-2:123b-cloud": {},
        "ollama/qwen3-coder:480b-cloud": {},
        "ollama/deepseek-v4-flash:cloud": {},
        "ollama/glm-5.1:cloud": {}
      }
    }
  }
}
```

## 📚 Dependencies

- OpenClaw with skill support
- Access to `$FACTORY_ROOT/` (documentation)
- Zikkaron MCP configured (optional, recommended)
- LLM models available via Ollama

## 📝 Notes

- This skill is **agent-agnostic** — works for any OpenClaw agent
- The Model Router is **declarative** — no scripts, only JSON configuration
- Overrides are **optional** — the base should work for 90% of cases
- The 3-model restriction is **soft** — automatic fallback, not error

---

*"It is mapped. Now just don't ignore the map."* ᛉ
