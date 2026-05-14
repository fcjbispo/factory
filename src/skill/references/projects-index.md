# Factory Projects Index

*Reference for the factory skill — maintained manually or generated automatically*

## Active Projects (HOT)

| # | Project | Status | Code | Factory Docs |
|---|---------|--------|------|--------------|
| 1 | **MyGDriveSync** | Production | `~/Dev/Projects/MyGDriveSync/` | ✅ Complete |
| 2 | **FBNet** | Production | `~/Dev/Projects/FBNet/` | ✅ Complete |
| 3 | **MyBacklogManager** | In development | `~/Dev/Projects/MyBacklogManager/` | ✅ Complete |
| 4 | **MyCodebase** | In development | `~/Dev/Projects/MyCodebase/` | ✅ Complete |
| 5 | **FBPyUtils** | Base library | `~/Dev/Projects/FBPyUtils/` | ✅ Complete |

## Factory Structure per Project

Each project follows the v1.3.0 structure:

```
~/Dev/Factory/
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
2. Locate `~/Dev/Factory/<project>/docs/INDEX.md`
3. Follow initialization sequence in `init-sequence.md`

### For PO (Chicão)

- Add new projects to this index
- Update status when changed
- Do not remove projects — mark as "Archived"

## Naming Conventions

- **Project name:** PascalCase (ex: MyBacklogManager)
- **Factory folder:** Same name in `~/Dev/Factory/`
- **Code folder:** Same name in `~/Dev/Projects/`
- **Source code:** In English
- **Documentation:** In English (base) with localization support

## Golden Rule

> `~/Dev/Factory/docs/` = PO exclusive territory
> Never create/modify without express authorization

---

*Factory skill v1.3.1 reference*
*Updated: 2026-05-14*
