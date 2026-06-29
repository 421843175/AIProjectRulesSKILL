---
name: enterprise-code-design
description: Enterprise software delivery workflow for complex code tasks across frontend, backend, mobile, desktop, data/ML, infrastructure, automation, developer tooling, documentation systems, reusable components, reusable modules, and code review; also called ECD or the ECD skill. Use whenever the user explicitly says "ECD", "调用ECD", "调用ECD技能", "使用ECD", "使用ECD技能", "按ECD执行", "规范开发", or asks for enterprise-standard/规范化 development. Also use for non-trivial implementation, refactor, public contracts, UI/user workflows, data/storage, deployment/runtime, core paths, concurrency, performance, security, migrations, architecture-sensitive work, code review work such as "ECD review", "用 ECD review", "按 ECD review 模式检查", "review current changes", "code review this PR", or reuse work across any development surface such as "can this be reused", "make this reusable", "write reuse documentation", "copy this to another project", "turn this into a reusable tool/module/template", or "package this as a reusable component/service/library/script/infra module". Requires project inspection, Chinese design proposal when risk requires it, user confirmation before Medium+ coding, minimal blast radius, no unrelated edits, Chinese comments, robust code, tests, validation, and a Chinese Markdown review document.
---

# Enterprise Code Design

## Contract

- All user-facing replies, designs, plans, test reports, final summaries, review docs, and new code comments must be Chinese.
- Mandatory: think and design with enterprise software discipline before coding: product/user impact, clear layers, module boundaries, responsibility separation, contracts, state/data flow, consistency, extensibility, compatibility, observability, security, maintainability, minimal blast radius, and no over-engineering.
- Keep execution concise; this skill is a checklist, not a policy essay.

## Use When

Complex coding across any software surface: new product behavior, UI/user workflow, backend/service logic, mobile/desktop behavior, data/ML pipeline, infrastructure/deployment, automation/tooling, docs-as-product, cross-module edits, public contract changes, state/storage changes, core paths, concurrency/performance/security, migration, refactor, unclear blast radius. For small local edits, use a lighter flow but still preserve style, tests, and change isolation.

## Reference Modes

- **Review Mode**: when the user asks ECD to review code, current git changes, a PR, a commit range, a patch, a reusable asset, or says "ECD review", "用 ECD review", "按 ECD review 模式检查", or similar, read `references/rules/review-mode.md` before reviewing. Review Mode incorporates the `code-review-expert` workflow into ECD: inspect git scope, review correctness, SOLID/architecture, security/privacy, reliability/data integrity, performance, error handling/observability, tests, and removal candidates. Review Mode is review-first: present findings ordered by P0-P3 severity with file/line references when possible, state coverage and residual risk, and do not implement fixes until the user explicitly confirms a fix path.
- **Reuse Mode**: when the user asks whether anything can be reused across any development surface, asks to make an implementation reusable, asks for reusable asset documentation, asks how to copy it into another project, or asks how to package it for reuse, read `references/rules/reuse-mode.md` before designing or editing. Reuse Mode applies to frontend components, backend services, domain modules, SDK/API clients, CLI tools, scripts, automation workflows, data/ML pipelines, database schema/migration assets, infrastructure/IaC modules, configuration templates, test harnesses, documentation templates, and other software delivery assets. Reuse Mode first assesses whether the target has real reusable properties; if it is reusable, proceed toward reusable-asset marking and documentation. If it is not reusable or incomplete, explain the gaps and reusable refactoring requirements, perform the required refactor after any ECD design-confirmation gate that applies, validate the changed code/config/artifact, then stop and ask the user to confirm the refactored result and approve adding it as a reusable asset. Only after the user confirms should Reuse Mode mark the confirmed files/artifacts as reusable assets with explicit metadata such as `@asset` and `@reusable true`, then create the reuse documentation. Reuse is not asking users to copy scattered code snippets: organize reuse around self-contained files, folders, tool modules, services, libraries, templates, packages, images, charts, IaC modules, or artifacts that can be copied/installed, imported/called/executed/deployed, configured, and verified. Reuse Mode requires executable reuse paths, not aspirational advice, and the `## 复用方式` section must be detailed enough for a brand-new project to introduce the asset through copy/install, import/call/execute/deploy, config, data/contracts, outputs/events/files/API responses, permissions/secrets, runtime/deployment requirements, and verification steps.

## Reusable Asset Marker Guard

When inspecting or editing any file that contains `@reusable true`, treat it as a shared reusable asset with elevated blast radius. Before changing it, read the file's asset header, follow its `@notice`, inspect the linked reuse documentation under `docs/reuse/` when present, identify known consumers, and update the reuse documentation if the change affects behavior, contracts, configuration, usage, validation, or limitations.

## Gates

### 0. Risk

Classify first:

- Low: local only, no public contract, user workflow, persisted state/data, runtime/deployment, permission/security, or core-path impact; brief note then proceed.
- Medium: new behavior, UI flow, integration, state handling, automation, docs-as-product, or multi-file change; design + acceptance criteria + tests.
- High: legacy/public contract, storage/data migration, auth/permission/privacy, concurrency/performance, core path, deployment/runtime, infrastructure, or compatibility risk; confirmed design + blast radius + rollback + checks.
- Prod-critical: production stability, user trust, money, permission, privacy, compliance, safety, irreversible data, release/deployment, or business-critical workflow risk; implementation plan before coding + rollout/monitoring/rollback/acceptance.

### 1. Inspect Project

Before design/editing, inspect and summarize in Chinese:

- Structure, modules/packages, entrypoints, build/run scripts, deployment/runtime surfaces.
- Stack, framework, dependencies, tests, logging, config, observability, release conventions.
- Architecture/patterns: component/view-model, state management, layered, MVC/MVVM, DDD, event, repository, factory, strategy, adapter, pipeline, workflow, IaC, plugin/extension patterns.
- Naming/code style: files, components, classes/functions, commands, jobs, schemas, APIs/contracts, models/DTOs, errors, returns, validation, constants, comments.
- Product/data/test boundaries: UI/UX flows, accessibility, client/server contracts, DB/cache/storage, queues/events, external APIs, files, jobs, models, infrastructure, legacy interfaces, unit/integration/E2E/visual/manual tests.

### 2. Design Before Code

For complex tasks, do not edit task-related implementation code, configuration, workflows, schemas, infrastructure, or product/documentation surfaces until the required Chinese design is confirmed. Include:

- Requirement: goal, user/workflow impact, I/O, state changes, edge cases, unknowns.
- Current project state and constraints.
- Tech/dependency choice: prefer existing stack; new deps need necessity, alternatives, maintenance, License, security, cost.
- Enterprise architecture thinking: layers, packages/modules, responsibility boundaries, state/data flow, UI/API/CLI/file/event/config contracts as applicable, consistency, errors, extension points, compatibility/migration (if applicable), observability, security, maintainability, minimal blast radius.
- Options + recommendation.
- Acceptance criteria: normal/error paths, boundaries, performance, compatibility, visible/user-observable success.
- Confirmation points.

### 3. Evaluate

For each option, or the only option, state: time/complexity cost; scalability/performance under realistic load, large data, many users, or long-running workflows; blast radius across product flows, UI, clients, services, public contracts, storage, cache, config, jobs, messages/events, models, infrastructure, tests, release/deployment, and documentation.

### 4. Enterprise Checks

For high/prod-critical risk, add concise conclusions:

- Rollout/rollback: gray release, Feature Flag, compatibility, migration, rollback, observation.
- Security/privacy: auth, validation, injection, secrets, masking, supply-chain risk; never log passwords, Tokens, keys, sensitive data.
- State/data: local state, persisted data, files, cache, migrations, indexes, history, transactions, idempotency, rollback, consistency verification.
- Observability: log fields, metrics, Trace, user-visible errors, error rate, latency, throughput, alerts; if no metrics, logs must locate issues.
- Performance/experience: baseline, target, bottleneck, validation/load test, responsiveness, accessibility/interaction quality where applicable.
- Tech debt: quick/temporary choice records cause, impact, cleanup plan.

### 5. Very Complex Work

For multi-stage, storage/data migration, infrastructure/deployment, cross-platform, cross-system, or core workflow work, create a Chinese implementation plan before coding: phases, goals, file-level changes, risks, validation, order. Continue only after confirmation.

## Coding Rules

### Isolation

- Modify only task-related code/config/tests/docs.
- Do not touch unrelated code, comments, formatting, names, deps, configs, docs.
- No incidental cleanup, style normalization, comment cleanup, or formatting churn.
- Do not break existing behavior; if risk exists, design must cover blast radius, compatibility, validation, rollback.
- Unrelated issues are notes only unless user explicitly authorizes fixes.

### Chinese Comments

- Chinese line comment above every new/modified code line.
- Chinese block comment above every new/modified method, function, component, command, workflow, job, or script: purpose, params/inputs, return/output, exceptions/failure modes, side effects.
- Chinese block comment at new/modified class/component/module/file top: responsibility, layer/surface, use case, notes.
- Explain business/design intent, not syntax.
- If this conflicts with project style, explain and ask before coding.

### Robustness

- Validate null/type/bounds/enums/permission/state.
- Catch core errors, classify, log context, degrade gracefully; never swallow silently.
- Log or expose diagnostic context on key paths with requestId/userId/sessionId/taskId/traceId/entityId/componentId/jobId or local equivalent.
- No hard code: constants/enums/config for magic values, paths, statuses, switches, timeouts.
- Writes and side effects need transaction or consistency strategy, idempotency, concurrency, retry, rollback, or user-safe recovery as applicable.
- Sanitize external input/files/network/SQL/commands/permissions/URLs/events/prompts/config.

### Organization

Follow current architecture/package boundaries. Keep domain/business rules out of thin entrypoints, route handlers, UI glue, scripts, workflow wrappers, and infrastructure adapters unless the project pattern intentionally places them there. Split by local conventions: components/views, hooks/view-models, commands, application/use cases, domain, adapters, repositories/storage, infrastructure, schemas/models, config, constants, tests, docs. Avoid over-engineering. Add abstractions only for real complexity, extension, reuse, or local pattern. New public contracts need compatibility, versioning, error shape, caller/user migration, and documentation as applicable.

## Tests And Delivery

- Add automated tests when possible using project framework.
- Core logic covers normal, invalid params, boundaries, exceptions, idempotency/concurrency.
- UI/product changes need component/interaction/E2E/visual/accessibility checks or browser/device verification.
- API/CLI/file/event/schema/storage/infrastructure changes need integration tests or executable contract/config/migration checks.
- Data/ML/automation changes need fixture-based checks, deterministic sample runs, or documented reproducibility checks.
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
- Prod-critical: create all sections 1-11 with explicit rollout, monitoring, rollback, state/data/security, user/business impact, and owner handoff details.

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
* **核心改动**：说明具体改了哪些组件、模块、流程、配置、脚本、数据/状态处理、接口契约或文档。
* **注释与健壮性**：[例如：已补全关键方法/组件的中文说明，对关键入参、状态、权限或异常路径进行了防御性校验]

## 5. 影响范围
> [Medium及以上必填] 评估本次修改的爆炸半径：是否影响用户流程、现有公共契约、上游/下游调用方、状态或数据一致性、历史兼容、后台任务、部署运行时、权限安全、可观测性或交互体验。

## 6. 验收标准与验证
* **测试用例与覆盖**：列出已通过的正常、异常、边界单测或集成验证场景。
* **手动验证命令/数据/日志/界面路径**：提供可执行的检查步骤及预期输出。

## 7. 风险说明
> [High/Prod-critical必填] 线上发布或执行时可能引发的衍生风险，例如用户流程中断、瞬时高负载、数据/状态不一致、权限误放大、兼容性破坏、部署失败或体验退化。

## 8. 回滚方案
> [High/Prod-critical必填] 明确、可落地的回滚步骤。例如：1. 关闭特性开关（Feature Flag）；2. 执行 Git 变更回滚命令；3. 按顺序还原配置、数据/状态、部署资源或任务开关。

## 9. 维护建议
> [High/Prod-critical必填] 后续运维或交接给其他同学时的注意事项，例如配置阈值建议、重点监控指标、用户反馈入口、日志排查入口、运行手册或值班观察窗口。

## 10. 当前设计的不足
> [按风险矩阵必填；Low及以上均需保留] 结合当前时间窗口、技术栈限制或老代码结构，诚实列出当前落地实现中的工程取舍、代码耦合或潜在技术债。

## 11. 可能的更好的设计
> [Medium及以上必填] 如果抛开当前工期限制或历史包袱，或者用户规模/数据量/调用量再翻 10 倍，说明未来应如何演进该模块或体验，例如重构为状态机、抽离为独立服务、引入消息队列解耦、组件化设计系统、独立数据管道、IaC 模块或自动化治理。
```

Final reply must include review doc path, or full Markdown content if no file was created.

## Reply Shape

Design: `项目现状`, `方案设计`, `评估维度`, `影响范围`, `推荐方案`, `验收标准`, `请确认`.

Final: changed summary, key files, tests/reason, validation results, review doc path/content, remaining risks.








