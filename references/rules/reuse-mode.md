# ECD Reuse Mode

## Purpose

Use Reuse Mode when the user asks whether any development asset can be reused, made reusable, copied into another project, packaged, shared, or documented as a reusable asset.

Reuse Mode is domain-agnostic. It applies to frontend, backend, mobile, desktop, data/ML, database, infrastructure, automation, developer tooling, testing, documentation systems, and any other software delivery surface.

Common reusable assets include, but are not limited to:

- Frontend UI components, hooks/composables, design tokens, styles, assets, routers, state modules, and page-level tools.
- Backend services, application use cases, domain modules, API controllers, SDK/API clients, middleware, jobs, queues, adapters, repositories, and validation rules.
- CLI tools, scripts, automation workflows, code generators, project scaffolds, and developer utilities.
- Data/ML pipelines, feature builders, model wrappers, notebook-to-script assets, ETL jobs, dataset validators, and report generators.
- Database schemas, migrations, seed data, stored procedures, indexes, query templates, and data access modules.
- Infrastructure/IaC modules, deployment charts, Dockerfiles, CI/CD workflows, monitoring dashboards, alert rules, and runtime configuration templates.
- Test harnesses, fixtures, mocks, contract tests, E2E helpers, performance benchmark tools, and QA checklists.
- Documentation templates, runbooks, product/spec templates, release templates, and knowledge-base patterns.

Common triggers:

- "Can this be reused?"
- "Make this reusable."
- "How should another project reuse this?"
- "Write reuse documentation."
- "Package this as a reusable component/service/library/script/tool/template/module."
- "Copy this into another project."
- "Create a reuse mode / reusable pattern."

Reuse Mode must not produce vague architecture advice. It must produce an executable reuse path that another developer can follow without guessing.

## Non-Negotiable Rule: Reuse Must Be Executable

Every reuse path must be implementable. Do not write aspirations, slogans, or future-tense wishes such as:

- "This can be extracted later."
- "Future projects can reuse it as needed."
- "Adjust based on business requirements."
- "Consider unified packaging."

Instead, each reuse path must include the applicable subset of:

- Applicable scenario and non-applicable scenario.
- Exact reusable files, directories, package names, images, charts, templates, or artifacts.
- Required dependencies and install commands.
- Required configuration, environment variables, secrets, permissions, and where to put them.
- Required reusable styles, assets, schemas, migrations, fixtures, config templates, or runtime files included in the reusable unit/package.
- Stable import/call/execute/deploy entrypoint.
- Minimal invocation, command, deployment, or integration example.
- Required input data shape, API contract, CLI flags, file format, database contract, infrastructure variables, or configuration schema.
- Output contract such as return value, event, callback, emitted file, API response, database state, deployed resource, log, metric, or generated artifact.
- Runtime, deployment, storage, network, permission, security, and observability requirements where relevant.
- Verification commands and manual checks.
- Expected verification results.
- Known limitations and unsafe reuse scenarios.

If any of those items cannot be provided, say what is missing and why.

The `## 复用方式` section must be detailed enough for a brand-new project to adopt the asset without knowing the original project. It must explain how to introduce the asset from zero: prerequisites, copy/install steps, dependency installation, import/call/execute/deploy paths, configuration, required data or contract shape, output handling, reusable asset files, runtime/deployment requirements, permissions/secrets, and verification.

## Non-Negotiable Rule: Reuse Is Asset/Module/Package Based

Reuse is not asking users to copy scattered code. A valid reuse deliverable must be organized as reusable files, folders, modules, tools, services, libraries, packages, templates, images, charts, IaC modules, datasets, or artifacts that a consumer can copy or install as a unit, then configure, import/call/execute/deploy, and verify.

Bad reuse instructions:

- "Copy these classes from `admin.css`."
- "Copy these functions from lines 100-180 of `Foo.vue`."
- "Copy these service methods from this controller."
- "Move these SQL snippets into your project."
- "Find similar CI code in the old project and migrate it manually."
- "Open the old project and copy the scattered helper functions."

Good reuse instructions:

- "Copy `src/components/map/` as a reusable frontend module directory."
- "Copy `src/services/audit-log/` as a reusable backend service module and import its public entrypoint."
- "Install `@company/order-rules` and call `validateOrder(order, config)`."
- "Copy `scripts/import-users/` and run `node ./scripts/import-users/index.js --input users.csv`."
- "Copy `db/migrations/customer-tags/` and apply migrations with the target project's migration runner."
- "Use `infra/modules/postgres-read-replica/` as a Terraform module with the documented variables."
- "Install the internal Docker image or Helm chart and configure the documented values."
- "Copy `docs/templates/release-note/` and render it with the documented input fields."

If reusable behavior currently depends on scattered CSS, helper functions, constants, schemas, migrations, fixtures, API clients, service adapters, infrastructure snippets, CI steps, environment files, documentation fragments, or project-specific snippets, first extract those dependencies into the reusable directory/package/tool/template/artifact. Only write the reuse guide after the reusable boundary exists.

A reuse path may say "copy this reusable directory/file set" or "install this package/chart/image/module". It must not say "open an old project file and copy these lines/classes/functions/SQL snippets". If styles are required, provide a dedicated style entrypoint. If helpers are required, provide utility files. If interfaces, schemas, migrations, data adapters, infrastructure variables, or workflow contracts are required, provide them as exported files or documented contracts inside the reusable unit.

## First Decide Whether Reuse Is Real

Before documenting reuse, inspect whether the target is actually reusable:

- Can it run outside the current page, service, job, repository, deployment, dataset, or workflow?
- Are inputs explicit through props, params, config, API contracts, CLI flags, files, schemas, environment variables, infrastructure variables, or adapters?
- Are outputs explicit through events, return values, files, callbacks, API responses, database state, deployed resources, generated artifacts, logs, metrics, or emitted messages?
- Does it depend on a route, global store, global CSS, hard-coded domain, hard-coded secret, organization name, current database, current queue/topic, current cloud account, page-specific DOM, scattered source snippets, or undocumented local state?
- Can a new project use it by changing configuration rather than editing internal source code?
- Are secrets, credentials, service keys, tokens, and cloud/account identifiers injected through configuration rather than hard-coded?
- Is there a stable entrypoint such as `index.js`, `index.ts`, package exports, API module, CLI entrypoint, script command, migration directory, Docker image, Helm chart, Terraform module, workflow file, template path, or documented artifact path?
- Is there a validation path that proves the asset works in a fresh consumer project or an equivalent isolated environment?

If the target is already reusable enough, do not interrupt the user with an approval question. Analyze the project and directly produce the reuse documentation.

If the target is not reusable, only partially reusable, or has obvious reuse gaps, tell the user clearly before writing the reuse documentation. Explain why it is not ready for reuse and propose concrete changes. Continue with reusable refactoring only when the user confirms they want to proceed.

After the user confirms, make the current project reusable first, then write the reuse documentation. Typical reusable refactoring may include:

- Extracting page-specific, service-specific, job-specific, deployment-specific, or dataset-specific logic into reusable modules.
- Moving provider/service definitions into configuration modules.
- Moving shared calculations, validation rules, adapters, schemas, migrations, fixtures, and commands into reusable files/directories.
- Creating stable entrypoints such as `index.js`, package exports, CLI commands, API exports, migration roots, Docker image tags, Helm chart values, Terraform module variables, or template render commands.
- Splitting UI-only parts, domain logic, application use cases, infrastructure adapters, persistence adapters, and runtime wrappers according to the current stack's conventions.
- Removing hard-coded business data, route assumptions, database names, queue/topic names, domains, tokens, cloud account IDs, organization names, or environment-specific values.
- Extracting required styles, assets, helpers, constants, schemas, migrations, fixtures, adapters, interfaces, config templates, and deployment/runtime files into the reusable unit.
- Adding explicit props, params, events, return contracts, config objects, adapters, schemas, CLI flags, API contracts, environment variables, infrastructure variables, or extension points.
- Preserving backward compatibility wrappers when existing callers depend on old names.

Only after the reusable boundary exists should the agent create the `[R]复用-XXXXX说明.md` artifact.

Decision flow:

```text
Inspect target
  -> reusable enough
      -> analyze project
      -> write reuse documentation directly
  -> not reusable / incomplete reuse boundary
      -> tell user the gaps
      -> propose concrete reusable refactor
      -> wait for user confirmation
      -> refactor into reusable assets/modules/tools/packages/templates/artifacts
      -> validate
      -> write reuse documentation
```

## Required Reuse Boundaries

A reusable asset should have clear boundaries. Select the boundaries that apply to the target; do not force frontend-only or backend-only concepts onto unrelated assets.

- Product/workflow boundary: the user scenario, business rule, or operational workflow is explicit and not hidden in a page, service, or job.
- UI boundary: visual components, interactions, design tokens, assets, and layout requirements are explicit when the asset has a UI surface.
- API/service boundary: public methods, endpoints, DTOs, error shapes, retries, timeouts, idempotency, and caller responsibilities are explicit when the asset exposes a service surface.
- Domain boundary: business rules, invariants, validations, and state transitions are separated from thin entrypoints and adapters.
- Data boundary: input/output data shapes, schemas, migrations, seed data, fixtures, storage assumptions, and compatibility rules are explicit.
- Runtime/deployment boundary: process model, commands, containers, charts, IaC variables, CI/CD steps, network access, storage, scaling, and observability are explicit.
- Config/secrets boundary: external URLs, tokens, credentials, cloud/account IDs, feature flags, and provider definitions live in config or secret stores, not hard-coded internals.
- Asset/style boundary: required CSS, design tokens, static assets, templates, reports, generated files, or documentation fragments live inside the reusable unit/package or are exposed through stable entrypoints.
- Export/entrypoint boundary: consumers have one or more stable import, call, command, deploy, migration, template, or package entrypoints.
- Validation boundary: consumers know which command, test, fixture, environment, or manual check proves the reuse works.

## Avoid Fake Reuse

Do not label something reusable when:

- It hard-codes current-page, current-service, current-job, current-database, current-cloud-account, or current-organization state.
- It requires a specific page, route, service, job, deployment, dataset, database, queue/topic, cloud resource, or CI runner to work without documenting that requirement.
- It stores secrets in UI code, repository files, scripts, images, logs, browser storage, plain text config, or generated docs by default.
- Its styles, helpers, constants, schemas, migrations, fixtures, adapters, infrastructure files, or workflow steps are scattered across unrelated files instead of being extracted into a reusable unit.
- A new project must edit internal source code just to use it.
- The documentation lacks a reusable file/directory/package/artifact list, dependency list, config list, invocation/deployment example, input/output contract, and verification path.

## Required Documentation Output Format

Skill-internal rules stay in English, but project-facing reuse documentation must be written in Chinese by default because it is a delivery artifact for the user and their team.

When the user asks for reuse documentation, create it under the project reuse documentation directory:

```text
docs/reuse/
```

Do not place reuse documentation directly under `docs/` unless the user explicitly requests a different location.

Name the file:

```text
[R]复用-XXXXX说明.md
```

If the user explicitly requests another naming convention or language for the artifact, follow the user's requested artifact format. Skill-internal references should remain English.

Use this Chinese structure unless the user gives a stricter one:

```markdown
# XXXXX的复用说明

> **文档生成时间**：YYYY-MM-DD HH:mm:ss
> **复用对象**：说明被复用的组件、模块、工具、流程、模板、服务、脚本、数据资产、基础设施模块或其它开发资产名称
> **来源项目/路径**：说明来源项目和关键源码/配置/模板/脚本/资产路径
> **适用范围**：说明适合复用到哪些项目、系统、服务、流程、环境或场景
> **维护说明**：说明后续修改、升级、验证、版本治理或注意事项

## 介绍

说明复用对象是什么、解决什么问题、当前具备哪些能力。

## 实现原理

说明核心分层、状态流转、依赖关系、配置注入方式、输入输出契约、扩展点，以及为什么这种设计具备复用性。

## 详细原理

详细说明当前项目如何实现它，包括目录结构、模块职责、核心流程、参数、事件、接口、数据结构、配置项、运行方式、部署要求、关键契约和可复用边界。

## 分析

### 可能的优势

说明这种设计对复用、扩展、测试、迁移、维护、性能、部署、治理或版本管理的好处。

### 可能存在的缺陷

诚实说明当前耦合点、缺少的扩展点、缺少的测试、运行时假设、数据/状态风险、样式/资产依赖、构建工具假设、部署限制或打包缺口。

## 复用方式

提供可执行的复用路径。不能写成愿景，必须写清楚如何复制/安装、如何配置、如何调用/执行/部署、如何接入数据或契约、如何处理输出、如何验证。
```

If the user has already supplied a required heading format, use their headings exactly. Still enforce executable reuse content.

## File/Directory/Artifact Reuse Path

Use this path when projects are few, the reusable asset is still evolving, or no shared package/artifact system exists. The consumer must copy a complete reusable file set, directory, template, script bundle, migration set, chart, IaC module, or artifact as a unit, not scattered source snippets.

Must include the applicable subset of:

- New-project prerequisites, including framework/runtime version, language version, build tool, database, cloud provider, OS, browser/server requirements, CLI/runtime assumptions, and permission requirements when relevant.
- Reusable files/directories/artifacts to copy as a complete unit.
- Optional reusable files/directories/artifacts to copy as complete units.
- Dependencies to install.
- Reusable styles/assets/schemas/migrations/fixtures/config templates/runtime files included in the copied unit.
- Environment variables, secrets, permissions, runtime configuration, infrastructure variables, or feature flags.
- Target import/call/command/deploy/migration/template path after copying.
- Minimal invocation, command, API call, deployment, migration, or integration example.
- Required input data shape, API contract, CLI flags, file format, database contract, infrastructure variable schema, or configuration schema.
- Outputs/events/callbacks/files/API responses/database changes/deployed resources/logs/metrics the consumer must handle.
- Runtime, layout, storage, network, deployment, scheduling, observability, or security requirements when applicable.
- Build-tool or environment differences.
- Verification steps and expected results.

Template:

````markdown
### 方式一：文件/目录/资产复用

适用场景：

* ...

新项目前置条件：

* 运行时/框架/语言：...
* 构建或执行工具：...
* 数据库/云资源/网络/权限要求：...

1. 复制完整复用文件/目录/资产：

```text
src/modules/example/
scripts/example-tool/
db/migrations/example/
infra/modules/example/
```

2. 安装依赖：

```bash
npm install example-dependency
# 或 pip install ... / mvn ... / go get ... / terraform init ...
```

3. 确认复用单元内置资源：

```text
src/modules/example/index.ts
src/modules/example/schema.ts
src/modules/example/config.example.env
src/modules/example/README-contract.md
```

4. 配置运行时参数：

```env
EXAMPLE_ENDPOINT=...
EXAMPLE_TOKEN=...
```

5. 调用、执行或部署：

```bash
node ./scripts/example-tool/index.js --input ./data/input.csv
```

```js
import { runExample } from '@/modules/example'

const result = await runExample(input, config)
```

6. 准备输入契约：

```js
const input = {
  id: 'example-1',
  payload: {}
}
```

7. 处理输出契约：

```js
function handleResult(result) {
  // handle return value / emitted file / API response / database state / deployed resource
}
```

8. 确认运行、部署或资源要求：

```text
需要能访问目标 API；需要数据库迁移权限；需要写入 ./output 目录；需要配置监控指标。
```

9. 验证：

```bash
npm run lint
npm run test
npm run build
```

预期结果：构建或测试通过，并且在目标项目/环境中能看到目标功能、脚本、服务、迁移、部署或模板正常工作。
````

## Package/Artifact Reuse Path

Use this path when multiple projects need the same asset, versioning matters, or future upgrades should be centrally managed. The packaged asset may be an npm package, Maven/Gradle package, Python package, Go module, NuGet package, Docker image, Helm chart, Terraform module, internal CLI, migration package, template package, or other versioned artifact.

Must include the applicable subset of:

- Suggested package/artifact directory.
- Public entrypoint, command, deploy path, migration root, or template renderer.
- Peer/runtime dependencies.
- Bundled resource strategy for styles, assets, schemas, migrations, templates, fixtures, config examples, Dockerfiles, charts, or IaC files.
- Installation command.
- Import/call/execute/deploy example.
- Minimal consumer code or command for a new project.
- Required config/env/secrets/permissions setup.
- Required input contract.
- Output contract.
- Runtime/deployment/storage/network/observability requirements when applicable.
- Versioning and breaking-change note.
- Verification steps.

Template:

````markdown
### 方式二：打包/制品复用

建议包或制品结构：

```text
packages/example/
├── src/
│   └── index.ts
├── config/
│   └── example.env
├── package.json
└── README.md
```

依赖与运行约束：

```json
{
  "peerDependencies": {
    "runtime-or-framework": "版本范围"
  }
}
```

业务项目安装：

```bash
npm install @company/example
# 或 pip install company-example / docker pull ... / terraform source=... / helm repo add ...
```

业务项目调用、执行或部署：

```js
import { runExample } from '@company/example'

const result = await runExample(input, config)
```

```bash
company-example --config ./example.config.json
```

业务项目配置：

```env
EXAMPLE_ENDPOINT=...
EXAMPLE_TOKEN=...
```

业务输入契约：

```js
const input = {
  id: 'example-1',
  payload: {}
}
```

验证：

```bash
npm run build
npm pack
npm install ../packages/example/company-example-1.0.0.tgz
npm run test
```
````

## Reuse Acceptance Checklist

Before finishing Reuse Mode, confirm:

- The target has been assessed for real reusable properties across its actual domain, not forced into a frontend-only or backend-only model.
- The reusable boundary exists in code/config/assets/templates/artifacts, or the missing boundary is explicitly documented.
- The reuse guide is placed under `docs/reuse/` unless the user explicitly requested another location.
- The reuse guide starts with metadata lines for generation time, reusable target, source path, applicable scope, and maintenance notes.
- The reuse guide includes file/directory/artifact reuse and package/artifact reuse when applicable.
- Each reuse method is executable, not aspirational.
- Required reusable file sets, dependencies, resource entrypoints, config, import/call/execute/deploy paths, invocation code or command, input contract, output handling, runtime/deployment requirements, permissions/secrets, and verification steps are listed.
- Secrets are injected through configuration or secret management, not hard-coded.
- Existing behavior is validated with lint/build/tests/manual checks, isolated execution, dry-run, migration validation, deployment plan, or equivalent checks where feasible.
- Current limitations are stated honestly.

## Final Response Requirements

In the final response, include:

- Whether the target is currently reusable.
- What was changed to improve reuse.
- The reuse guide path when a guide was created.
- The ECD work/review record path if ECD delivery rules require one.
- Validation commands and results.
- Remaining limitations.
