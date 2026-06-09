---
name: enterprise-code-design
description: Enterprise workflow for complex code tasks. Use for non-trivial implementation, refactor, API/database/core-path/concurrency/performance/security changes, migrations, or architecture-sensitive work. Requires project inspection, Chinese design proposal, user confirmation before coding, minimal blast radius, no unrelated edits, Chinese comments, robust code, tests, validation, and a Chinese Markdown review document.
---

# Enterprise Code Design

## Contract

- All user-facing replies, designs, plans, test reports, final summaries, review docs, and new code comments must be Chinese.
- Mandatory: think and design with enterprise architecture discipline before coding: clear layers, module boundaries, responsibility separation, contracts, data flow, transaction/consistency, extensibility, compatibility, observability, security, maintainability, minimal blast radius, and no over-engineering.
- Keep execution concise; this skill is a checklist, not a policy essay.

## Use When

Complex coding: new business logic, cross-module edits, API/DB changes, core paths, concurrency/performance/security, migration, refactor, unclear blast radius. For small local edits, use a lighter flow but still preserve style, tests, and change isolation.

## Gates

### 0. Risk

Classify first:

- Low: local only, no API/DB/core impact; brief note then proceed.
- Medium: new logic or multi-file; design + acceptance criteria + tests.
- High: legacy API/DB/concurrency/performance/security/core path; confirmed design + blast radius + rollback + checks.
- Prod-critical: stability/data/money/permission/privacy risk; implementation plan before coding + rollout/monitoring/rollback/acceptance.

### 1. Inspect Project

Before design/editing, inspect and summarize in Chinese:

- Structure, modules/packages, entrypoints, build/run scripts.
- Stack, framework, dependencies, tests, logging, config.
- Architecture/patterns: layered, MVC, DDD, event, repository, factory, strategy, adapter.
- Naming/code style: class/method/API/DTO/VO/Entity/Service/Repository/Controller, errors, returns, validation, constants, comments.
- Data/test boundaries: DB, cache, queues, external APIs, legacy interfaces, unit/integration/E2E tests.

### 2. Design Before Code

For complex tasks, do not edit business code until user confirms the Chinese design. Include:

- Requirement: goal, I/O, edge cases, unknowns.
- Current project state and constraints.
- Tech/dependency choice: prefer existing stack; new deps need necessity, alternatives, maintenance, License, security, cost.
- Enterprise architecture thinking: layers, packages/modules, responsibility boundaries, data flow, API contract, transaction/consistency, errors, extension points, compatibility/migration (if applicable), observability, security, maintainability, minimal blast radius
- Options + recommendation.
- Acceptance criteria: normal/error paths, boundaries, performance, compatibility, visible success.
- Confirmation points.

### 3. Evaluate

For each option, or the only option, state: time/complexity cost; scalability/performance under high concurrency/large data; Blast Radius across modules, legacy APIs, DB, cache, config, jobs, messages, UI, tests, deployment.

### 4. Enterprise Checks

For high/prod-critical risk, add concise conclusions:

- Rollout/rollback: gray release, Feature Flag, compatibility, migration, rollback, observation.
- Security/privacy: auth, validation, injection, secrets, masking; never log passwords, Tokens, keys, sensitive data.
- Data: migration, indexes, history, transactions, idempotency, rollback, consistency verification.
- Observability: log fields, metrics, Trace, error rate, latency, throughput, alerts; if no metrics, logs must locate issues.
- Performance: baseline, target, bottleneck, validation/load test.
- Tech debt: quick/temporary choice records cause, impact, cleanup plan.

### 5. Very Complex Work

For multi-stage, DB migration, or multi-system work, create a Chinese implementation plan before coding: phases, goals, file-level changes, risks, validation, order. Continue only after confirmation.

## Coding Rules

### Isolation

- Modify only task-related code/config/tests/docs.
- Do not touch unrelated code, comments, formatting, names, deps, configs, docs.
- No incidental cleanup, style normalization, comment cleanup, or formatting churn.
- Do not break existing behavior; if risk exists, design must cover blast radius, compatibility, validation, rollback.
- Unrelated issues are notes only unless user explicitly authorizes fixes.

### Chinese Comments

- Chinese line comment above every new/modified code line.
- Chinese block comment above every method: purpose, params, return, exceptions, side effects.
- Chinese block comment at new/modified class/file top: responsibility, layer, use case, notes.
- Explain business/design intent, not syntax.
- If this conflicts with project style, explain and ask before coding.

### Robustness

- Validate null/type/bounds/enums/permission/state.
- Catch core errors, classify, log context, degrade gracefully; never swallow silently.
- Log key paths with orderId/userId/requestId/taskId/traceId/entityId or local equivalent.
- No hard code: constants/enums/config for magic values, paths, statuses, switches, timeouts.
- Writes need transaction, idempotency, concurrency, retry, rollback.
- Sanitize external input/files/network/SQL/commands/permissions.

### Organization

Follow current architecture/package boundaries. Keep business logic out of entrypoints/controllers/scripts/UI. Split by local conventions: API/Controller, Service/Application, Domain, Repository/DAO, Infrastructure, DTO, Config, Constants, Tests. Avoid over-engineering. Add abstractions only for real complexity, extension, reuse, or local pattern. New APIs need compatibility, versioning, error codes, response shape, caller migration.

## Tests And Delivery

- Add automated tests when possible using project framework.
- Core logic covers normal, invalid params, boundaries, exceptions, idempotency/concurrency.
- API/DB changes need integration tests or executable API/SQL checks.
- Frontend changes need component/interaction tests or browser verification.
- If tests cannot be written, explain why and give commands, data, steps, expected results.
- Run feasible validation: lint, typecheck, unit/integration tests, build, or project scripts.
- Final reply states changed files, tests, validation commands/results, risks.

DoD: design confirmed; code done; tests/test method provided; validation run; logs/errors/config extraction covered; rollout/rollback risk stated; review doc created or reason given.

## Review Doc

After testing, create a Chinese Markdown review doc in existing docs folder (`docs/`, `doc/`, `documentation/`, `设计文档/`). If none exists, ask before creating one or provide full Markdown in final reply with reason.

Name: `yyyyMMdd（做了XXX）文档.md` or `yyyyMMdd（做了XXX）任务记录.md`; use completion/test date + short Chinese action. Examples: `20260609新增RTCM解码任务记录.md`, `20260609（新增RTCM解码）任务记录.md`, `20260609（修复订单回调幂等）文档.md`.

Include: background; discussion core; architecture inspection; design/options/evaluation/acceptance/blast radius; implementation flow/files/config or commit info; tests/data/expected/actual; how to inspect UI/API/logs/DB/test reports; strengths/gaps/risks/tech debt/maintenance boundary; rollback/troubleshooting.

Final reply must include review doc path, or full Markdown content if no file was created.

## Reply Shape

Design: `项目现状`, `方案设计`, `评估维度`, `影响范围`, `推荐方案`, `验收标准`, `请确认`.

Final: changed summary, key files, tests/reason, validation results, review doc path/content, remaining risks.



