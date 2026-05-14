# Factory Initialization Sequence

*Reference for the factory skill*

## Step-by-Step to Start a Factory Project

### 1. Receive PO Request

```
PO: "Start the ExampleProject project"
```

### 2. Load Factory Skill

The agent must:
- Identify that a Factory project initialization was requested
- Load the `factory` skill (available globally at `$FACTORY_SKILLS_DIR/`)
- Read the Model Router base configuration

### 3. Detect Project

Map the name/path to the corresponding Factory directory:

```
$FACTORY_ROOT/
├── ExampleProject/          ← Requested project
│   ├── docs/
│   │   ├── INDEX.md           ← Load first
│   │   ├── GUIDE.md           ← Load second
│   │   ├── context/
│   │   │   ├── active.md      ← Load third
│   │   │   └── progress.md    ← Load fourth
│   │   ├── architecture/
│   │   │   └── overview.md    ← Load if exists
│   │   └── domain/
│   │       └── context-map.md  ← Load if exists
│   └── .factory               ← Configuration file
```

### 4. Determine Active Role(s)

Based on the agent's identity and the project phase:

| Agent | Default Role(s) | When to Invoke |
|-------|-----------------|----------------|
| **Architect Agent** | Senior Architect | Start of project, structural decisions |
| **Developer Agent** | Full-Stack Developer | Feature implementation |
| **Reviewer Agent** | Code Reviewer | PR review, quality validation |
| **DB Agent** | DB Architect | Data modeling, migrations |
| **DevOps Agent** | DevOps/SRE + Security | Infrastructure, CI/CD, security |

### 5. Allocate Model

Use the Model Router to choose the best available model:

```json
{
  "role": "senior-architect",
  "preferred": "kimi-k2.6",
  "fallback": ["deepseek-v4-pro", "qwen3.5"]
}
```

### 6. Load Zikkaron Context

```bash
# Via MCP Zikkaron
get_project_context("$FACTORY_ROOT/ExampleProject")
```

### 7. Confirm Initialization

Report to the user:
- ✅ Project detected: `ExampleProject`
- ✅ Role: `Senior Architect`
- ✅ Model: `kimi-k2.6:cloud`
- ✅ Documentation loaded: INDEX.md, GUIDE.md, active.md
- ✅ Zikkaron context: [N] memories recovered

### 8. Start Work

Proceed with the first task defined in `context/active.md`.

---

## Special Cases

### Project Not Found

```
Error: Project "X" not found in $FACTORY_ROOT/
```
- Check if the project was registered with `factory-init.sh new <name>`
- Verify if the `.factory` file exists in the project directory

### Missing Documentation

```
Warning: docs/context/active.md not found
```
- Create from template: `cp docs/templates/context/active.md <project>/docs/context/`
- Update frontmatter with current date and owner

### Model Not Available

```
Warning: Preferred model "X" not available
```
- Use fallback from `config/model-router-base.json`
- If all fail, use the default model configured in the runtime

### Runtime Not OpenClaw

If using Claude Code or another runtime:
- Set `FACTORY_SKILLS_DIR` to the appropriate path (e.g., `~/.claude/skills`)
- Set `FACTORY_RUNTIME_CONFIG` to the runtime's config file
- Set `FACTORY_AGENT_OVERRIDES_DIR` to the agents directory
