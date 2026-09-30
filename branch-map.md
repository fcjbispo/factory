# Mapa de Branches — Factory

**Criado em:** 2026-09-10
**Última atualização:** 2026-09-30 — contagens re-verificadas via `git rev-list v1.3.0..<branch>`; seção de Ações adicionada para acompanhar as implementações da v1.4.0.
**Produção:** `v1.3.0` (também é `origin/HEAD`)
**Objetivo:** situar o estado do repositório e servir de registro das ações necessárias — cada implementação feita para a v1.4.0 (ou correção em `master`/`v1.3.0`) entra na tabela **Ações** com status.

## Linhagem

```
v1.0.0 ── v1.1.0 ── v1.2.0 (tag) ── v1.3.0  ← PRODUÇÃO (origin/HEAD)
                                       │
                                       ├── v1.3.1 (16 commits)
                                       │        └── master (19 commits: +demo add/revert, sobra CLAUDE.md)
                                       │                 └── v1.4.0 (24 commits: +skill agnóstica)
                                       │
                                       └── v1.3.0-en_us (3 commits: tradução in-place paralela)
```

## Resumo por branch

| Branch | Commits à frente de v1.3.0 | Último commit | Resumo |
|---|---|---|---|
| `v1.0.0` | 4 | 2026-04-29 | Release empacotada. ⚠️ Não é ancestral puro: a branch tem 4 commits de release fora da linha principal |
| `v1.1.0` | 4 | 2026-04-29 | Release empacotada. ⚠️ Idem — 4 commits de release fora da linha principal |
| `v1.2.0` (branch) | 0 | 2026-04-19 | Branch = tag `v1.2.0` + 1 commit (README/tarball). ⚠️ Tag e branch divergem — `git refname 'v1.2.0' is ambiguous` (ainda presente em 2026-09-30) |
| `v1.3.0` | 0 (base) | 2026-04-29 | **Produção.** Gestão de backlog de produção, comandos `update`/`sync`, auth GitHub para repos privados, comparação de versões melhorada, helper/release no Makefile, shellcheck limpo |
| `v1.3.1` | 16 | 2026-05-14 | **Reestruturação + sistema i18n.** Detalhes abaixo |
| `master` | 19 | 2026-05-31 | v1.3.1 + ciclo demo (add → revert). Sobrou só `CLAUDE.md` novo (97 linhas), que rege o repo hoje |
| `v1.4.0` | 24 | 2026-05-31 | master + 5 commits: merge, skill runtime-agnóstica em `src/skill/`, pacote pt_BR. Branch mais avançada |
| `v1.3.0-en_us` | 3 | 2026-05-31 | **Tradução in-place concorrente** sobre v1.3.0, sem reestruturação `src/`. Detalhes abaixo |

## v1.3.1 — reestruturação + sistema i18n (16 commits)

- **Estrutura:** `factory-init.sh` → `src/factory-init.sh` (repo vira fonte, `dist/` vira saída de build)
- **i18n (parcial):** sistema de localização com build multi-idioma — `src/locales/{pt,en}.env` (~175 linhas cada).
  Estado real verificado em 2026-09-30: só `factory-init.sh` (strings embutidas no build) e `docs/templates/INDEX.md` (45 marcadores) usam `$i18n_*`;
  FACTORY-GUIDE, GUIDE e agent profiles foram traduzidos **in-place para EN** no `src/`, não via build. A afirmação original de "traduzidos via build" estava errada.
- **Makefile:** target `build` corrigido, `help` adicionado, duplicata `build-all` removida, cleanup de staging dirs após packaging
- **Build:** versão auto-detectada da branch; README ganha seção de workflow de manutenção
- **Higiene:** `.obsidian/` removida do repo e ignorada; patterns completos de Obsidian/staging no `.gitignore`
- **Agentes:** renomeia `arquiteto-senior` → `senior-architect`
- **Templates novos:** `backlog/` (7 categorias: dependencies, documentation, improvements, issues, performance, security, tech-debt), `domain/` (DDD: bounded-context, ubiquitous-language, context-map), `decisions/`, `design/`, `security/threat-models/`, `testing/test-strategy.md`, `operations/postmortems/`
- **Volume:** 94 arquivos, +3.821 / −2.570 linhas

## master — demo adicionado e revertido (19 commits)

Contém tudo de v1.3.1 + 3 commits:

- `ab0e023` add demo (use cases e apresentações PT/EN)
- `7684615` adding demo
- `4773b43` rollback removendo demo

**Efeito líquido vs v1.3.1: apenas `CLAUDE.md` novo (97 linhas)** — o guia de manutenção do framework.

## v1.4.0 — skill runtime-agnóstica (24 commits)

Tudo de master + 5 commits (`master..v1.4.0`): skill movida para `src/skill/` e distribuída no tarball:

- `01d7139` Merge branch 'master' into v1.3.1x
- `af259d7` / `8e53ae5` skill totalmente runtime-agnóstica
- `02b882c` gera pacote pt_BR v1.3.1
- `4b55ef2` skill para `src/skill/` e distribuída no tarball

Conteúdo:

- `src/skill/SKILL.md` (168 linhas)
- `src/skill/config/model-router-base.json` (249 linhas) + `model-router-schema.json` (147 linhas) — roteamento de modelos LLM
- `src/skill/references/`: `init-sequence.md`, `model-router-base.md`, `model-router-examples.md`, `model-router-override.md`, `projects-index.md`
- `src/factory-init.sh` ganha ~45 linhas (instalação da skill)
- locales enriquecidos (+15 linhas cada); Makefile ajustado; pacote `dist/factory-1.3.1.pt.tar.gz`
- **Volume vs master:** 17 arquivos, +1.061 / −13 linhas

## v1.3.0-en_us — tradução in-place concorrente (3 commits)

- Traduz `docs/templates/` inteiro, agent profiles e README **diretamente sobre v1.3.0** — sem reestruturação `src/`, sem sistema de locales
- Também renomeia `senior-architect`; inclui `translation.log`
- Tarball `factory-1.3.0.tar.gz` re-gerado em EN
- **Volume:** 47 arquivos, +1.620 / −1.572 linhas (quase 1:1 = troca de idioma)

## ⚠️ Decisão pendente: duas abordagens de i18n competindo

| | v1.3.1 / v1.4.0 | v1.3.0-en_us |
|---|---|---|
| Estratégia | Marcadores `$i18n_*` + locales no build (parcialmente implementada) | Repo traduzido in-place (fica EN) |
| Fonte da verdade | `src/` + `src/locales/` | `docs/` (EN) |
| Pró-re | Um código, N idiomas no build | Simples, zero tooling |
| Contra | Só 2 arquivos usam marcadores; um bug impede a substituição | Fork do conteúdo, sync manual |

## Bugs conhecidos (verificados em 2026-09-30, branch `master`)

1. **`envsubst '$i18n_'` não substitui `$i18n_*`** (`src/Makefile:73,79`) — só casa com a variável literal `i18n_`. Resultado: os dois tarballs publicados (`dist/factory-1.3.0.{en,pt}.tar.gz`) entregam `docs/templates/INDEX.md` com **45 marcadores `$i18n_*` cru** — o primeiro arquivo lido por todo agente.
2. **`src/docs/guides/FACTORY-GUIDE.md.pt` é órfão** — nenhum Makefile o referencia; o pacote "pt" carrega o guia em inglês.
3. **Agents/GUIDE sem i18n** — só EN in-place; os locales PT não têm efeito sobre eles.
4. **`CLAUDE.md` raiz e `src/CLAUDE.md` desatualizados** — citam "guia v1.1.0" e o layout antigo (`factory-init.sh`/`docs/` na raiz; hoje sob `src/`).
5. **`make clean` apaga todo `dist/`**, inclusive os `.tar.gz` rastreados pelo git.
6. **`shellcheck` indisponível** nesta máquina — a validação prevista no CLAUDE.md não roda.

## Ações

Registrar aqui cada implementação (v1.4.0 ou correção em master/v1.3.0) conforme for feita.
Status: `pendente` | `em-progresso` | `concluído` (anexar commit) | `rejeitado`

| ID | Ação | Branch alvo | Status | Observação / commit |
|---|---|---|---|---|
| A1 | Corrigir `envsubst` no `src/Makefile` e reconstruir `dist/` | v1.4.0 | pendente | Bug 1 — índice cru nos 2 pacotes publicados |
| A2 | Integrar `FACTORY-GUIDE.md.pt` no build (ou remover) | v1.4.0 | pendente | Bug 2 |
| A3 | Decidir estratégia de i18n: locales-build vs in-place (`v1.3.0-en_us`) | v1.4.0 | pendente | Pré-requisito de A1/A2 definitivos |
| A4 | Atualizar `CLAUDE.md` raiz e `src/CLAUDE.md` (versão do guia + layout `src/`) | master | pendente | Bug 4 |
| A5 | Tornar `make clean` seguro para os tarballs rastreados | v1.4.0 | pendente | Bug 5 |
| A6 | Instalar `shellcheck` e validar `src/factory-init.sh` | master | pendente | Bug 6 |
| A7 | Alinhar `master` com `v1.4.0` (ou promover v1.4.0 a principal) | — | pendente | master está 5 commits atrás |
| A8 | Merge/arquivar `v1.3.0-en_us` conforme A3 | — | pendente | Evita duplicação de tradução |
| A9 | Resolver ambiguidade da ref `v1.2.0` (tag vs branch) | — | pendente | `git refname 'v1.2.0' is ambiguous` |
| A10 | Remover/renomear `v1.0.0` e `v1.1.0` (commits de release fora da linha) | — | pendente | Não são ancestrais puros |
