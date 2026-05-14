# Factory Projects Index

*Reference for the factory skill — maintained manually or generated automatically*

## Example Projects

| # | Project | Status | Code | Factory Docs |
|---|---------|--------|------|--------------|
| 1 | **ExampleApp** | Production | `$FACTORY_PROJECTS_DIR/ExampleApp/` | ✅ Complete |
| 2 | **AnotherApp** | Production | `$FACTORY_PROJECTS_DIR/AnotherApp/` | ✅ Complete |
| 3 | **ExampleProject** | In development | `$FACTORY_PROJECTS_DIR/ExampleProject/` | ✅ Complete |
| 4 | **SampleProject** | In development | `$FACTORY_PROJECTS_DIR/SampleProject/` | ✅ Complete |
| 5 | **CoreLib** | Base library | `$FACTORY_PROJECTS_DIR/CoreLib/` | ✅ Complete |

## Factory Structure per Project

Each project follows the v1.3.0 structure:

```
$FACTORY_ROOT/
└── <Project>/
    ├── docs/
    │   ├── INDEX.md              ← Entry point
    │   ├── GUIDE.md              ← Conventions and standards
    │   ├── domain/               ← DDD: context-map, bounded contexts
    │   ├── adr/                  ← Architecture Decision Records
    │   ├── api/                  ← Contracts (OpenAPI, GraphQL)
    │   ├── architecture/         ← System overview
    │   ├── database/             ← Schema, changelog, policies
    │   ├── design/               ← Feature specs
    │   ├── operations/           ← Runbooks, postmortems
    │   ├── security/             ← Policies, threat models
    │   ├── testing/              ← Testing strategy
    │   ├── decisions/            ← Product decisions (PO)
    │   ├── context/              ← active.md + progress.md
    │   └── backlog/              ← issues, improvements, tech-debt
    └── ...
```

## How to Use This Index

### For Agent

1. Read this file to identify available projects
2. Locate `$FACTORY_ROOT/<project>/docs/INDEX.md`
3. Follow initialization sequence in `init-sequence.md`

### For Product Owner (PO)

- Add new projects to this index
- Update status when changed
- Do not remove projects — mark as "Archived"

## Naming Conventions

- **Project name:** PascalCase (ex: ExampleProject)
- **Factory folder:** Same name in `$FACTORY_ROOT/`
- **Code folder:** Same name in `$FACTORY_PROJECTS_DIR/`
- **Source code:** In English
- **Documentation:** In English (base) with localization support

## Golden Rule

> `$FACTORY_ROOT/docs/` = PO exclusive territory
> Never create/modify without express authorization

---

*Factory skill v1.3.1 reference*
*Updated: 2026-05-14*
