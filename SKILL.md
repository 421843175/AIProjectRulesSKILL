---
name: enterprise-code-design
description: Enterprise workflow for complex code tasks, also called ECD or the ECD skill. Use whenever the user explicitly says "ECD", "调用ECD", "调用ECD技能", "使用ECD", "使用ECD技能", "按ECD执行", "规范开发", or asks for enterprise-standard/规范化 development. Also use for non-trivial implementation, refactor, API/database/core-path/concurrency/performance/security changes, migrations, or architecture-sensitive work. Requires project inspection, Chinese design proposal, user confirmation before coding, minimal blast radius, no unrelated edits, Chinese comments, robust code, tests, validation, and a Chinese Markdown review document.
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

DoD: Low-risk work has a brief risk note before coding; Medium and above work has confirmed Chinese design before coding; code done; tests/test method provided; validation run; logs/errors/config extraction covered; rollout/rollback risk stated when applicable; risk-rated review doc created under `docs/work/`.

## Review Doc

After testing, create the Chinese Markdown review doc under `docs/work/` for all risk levels, including Low-risk work. If `docs/work/` does not exist, create it. Do not place work review docs directly under `docs/` or other doc folders unless the user explicitly requests a different path.

Name: `WyyyyMMdd（做了XXX）文档.md` or `WyyyyMMdd（做了XXX）任务记录.md`; `W` means `work`, used to mark work review records. Use completion/test date + short Chinese action. Examples: `W20260609新增RTCM解码任务记录.md`, `W20260609（新增RTCM解码）任务记录.md`, `W20260609（修复订单回调幂等）文档.md`.

Use the Dynamic Matrix below to strictly trim the review doc by Gate 0 risk rating:

- Low: create sections 1, 4, 6, and 10. Omit all other sections unless the user explicitly asks for a fuller review doc or a concrete risk makes more sections useful.
- Medium: create sections 1-6, 10, and 11. Omit sections 7-9 unless a concrete release/rollback/maintenance risk exists.
- High: create all sections 1-11.
- Prod-critical: create all sections 1-11 with explicit rollout, monitoring, rollback, data/security, and owner handoff details.

Use this Chinese template; remove sections that the matrix says to omit, but do not renumber the remaining required sections:

```markdown
# 任务交付与评审记录（WyyyyMMdd）

## 1. 任务背景与改动概述
* **需求目标**：一句话说明本次改动解决什么业务问题。
* **风险定级**：[Low / Medium / High / Prod-critical]
* **变更文件列表**：
  * `path/to/FileA.java`（修改）
  * `path/to/FileB.java`（新增）

## 2. 项目现状
> [Medium及以上必填] 简述改动前的系统现状、已有逻辑约束，以及本次修改涉及的上下文边界。

## 3. 方案设计
> [Medium及以上必填] 至少列出主要方案；若有备选方案，用小标题区分。

### 3.1 方案一：[方案名称]
* **实现思路**：...
* **优点**：...
* **缺点**：...

### 3.2 方案二：[方案名称]（如有）
* **实现思路**：...
* **优点**：...
* **缺点**：...

* **最终推荐方案**：[明确指出最终采用了哪个方案及原因]

## 4. 实现内容
* **核心代码变动**：说明具体改了哪些核心类、配置或 SQL。
* **注释与健壮性**：[例如：已补全方法级中文块注释，对关键入参进行了防御性空校验]

## 5. 影响范围
> [Medium及以上必填] 评估本次修改的爆炸半径：是否影响现有 API 契约、上游调用方、缓存一致性、历史数据兼容、定时任务或前端交互。

## 6. 验收标准与验证
* **测试用例与覆盖**：列出已通过的正常、异常、边界单测或集成验证场景。
* **手动验证命令/SQL/日志**：提供可执行的检查步骤及预期输出。

## 7. 风险说明
> [High/Prod-critical必填] 线上发布或执行时可能引发的衍生风险，例如锁表、瞬时高负载、数据不一致、权限误放大或兼容性破坏。

## 8. 回滚方案
> [High/Prod-critical必填] 明确、可落地的回滚步骤。例如：1. 关闭特性开关（Feature Flag）；2. 执行 Git 变更回滚命令；3. 按顺序还原配置、数据或任务开关。

## 9. 维护建议
> [High/Prod-critical必填] 后续运维或交接给其他同学时的注意事项，例如配置阈值建议、重点监控指标、日志排查入口或值班观察窗口。

## 10. 当前设计的不足
> [按风险矩阵必填；Low及以上均需保留] 结合当前时间窗口、技术栈限制或老代码结构，诚实列出当前落地实现中的工程取舍、代码耦合或潜在技术债。

## 11. 可能的更好的设计
> [Medium及以上必填] 如果抛开当前工期限制或历史包袱，或者系统流量再翻 10 倍，说明未来应如何演进该模块，例如重构为状态机、抽离为独立服务、引入消息队列解耦。
```

Final reply must include review doc path, or full Markdown content if no file was created.

## Reply Shape

Design: `项目现状`, `方案设计`, `评估维度`, `影响范围`, `推荐方案`, `验收标准`, `请确认`.

Final: changed summary, key files, tests/reason, validation results, review doc path/content, remaining risks.





