# ECD Reuse Mode

## Purpose

Use Reuse Mode when the user asks whether an implementation, component, module, workflow, template, script, or document pattern can be reused, copied into another project, packaged, shared, or turned into a reusable asset.

Common triggers:

- "Can this component be reused?"
- "Make this reusable."
- "How should another project reuse this?"
- "Write reuse documentation."
- "Package this as a reusable component."
- "Copy this into another project."
- "Create a reuse mode / reusable pattern."

Reuse Mode must not produce vague architecture advice. It must produce an executable reuse path that another developer can follow without guessing.

## Non-Negotiable Rule: Reuse Must Be Executable

Every reuse path must be implementable. Do not write aspirations, slogans, or future-tense wishes such as:

- "This can be extracted into a component library later."
- "Future projects can reuse it as needed."
- "Adjust based on business requirements."
- "Consider unified packaging."

Instead, each reuse path must include:

- Applicable scenario.
- Exact files or package names.
- Required dependencies and install commands.
- Required configuration and where to put it.
- Required reusable style/assets files included in the reusable unit or package.
- Minimal invocation code.
- Verification commands.
- Expected verification results.
- Known limitations.

If any of those items cannot be provided, say what is missing and why.

The `## 复用方式` section must be detailed enough for a brand-new project to adopt the asset without knowing the original project. It must explain how to introduce the asset from zero: prerequisites, copy/install steps, dependency installation, import paths, configuration, required data shape, event/callback wiring, reusable style/assets files, layout/container requirements, and verification.

## Non-Negotiable Rule: Reuse Is File/Module/Package Based

Reuse is not asking users to copy scattered code. A valid reuse deliverable must be organized as reusable files, folders, modules, tools, components, or packages that a consumer can copy or install as a unit, then import, configure, and call.

Bad reuse instructions:

- "Copy these classes from `admin.css`."
- "Copy these functions from lines 100-180 of `Foo.vue`."
- "Move these code snippets into your project."
- "Find similar code in the old project and migrate it manually."

Good reuse instructions:

- "Copy `src/components/map/` as a reusable module directory."
- "Import `{ LeafletProviderMap }` from `src/components/map`."
- "The reusable directory includes `style.css`, provider config, components, and exports."
- "Install declared dependencies, configure environment variables, and call the exported component/API."
- "Package `packages/map-components/` and consume it through a versioned dependency."

If reusable behavior currently depends on scattered CSS, helper functions, constants, assets, interfaces, API clients, or project-specific snippets, first extract those dependencies into the reusable directory/package/tool file. Only write the reuse guide after the reusable boundary exists.

A reuse path may say "copy this reusable directory/file set". It must not say "open an old project file and copy these lines/classes". If style is required, provide a dedicated `style.css` or documented style entrypoint inside the reusable directory/package. If helpers are required, provide utility files in the reusable directory/package. If interfaces or data adapters are required, provide them as exported files or documented contracts.

## First Decide Whether Reuse Is Real

Before documenting reuse, inspect whether the target is actually reusable:

- Can it run outside the current page/module?
- Are inputs explicit through props, params, config, API contracts, CLI flags, or files?
- Are outputs explicit through events, return values, files, callbacks, API responses, or emitted artifacts?
- Does it depend on a route, global store, global CSS, hard-coded domain, hard-coded secret, organization name, scattered source snippets, or page-specific DOM?
- Can a new project use it by changing configuration instead of editing source code?
- Are third-party service keys injected through configuration rather than hard-coded?
- Is there a stable export entrypoint such as `index.js`, `index.ts`, package exports, CLI entrypoint, or documented file path?

If the target is already reusable enough, do not interrupt the user with an approval question. Analyze the project and directly produce the reuse documentation.

If the target is not reusable, only partially reusable, or has obvious reuse gaps, tell the user clearly before writing the reuse documentation. Explain why it is not ready for reuse and propose concrete changes. Continue with reusable refactoring only when the user confirms they want to proceed.

After the user confirms, make the current project reusable first, then write the reuse documentation. Typical reusable refactoring may include:

- Extracting page-specific logic into reusable components.
- Moving provider/service definitions into configuration modules.
- Moving shared calculations into utilities.
- Creating stable export entrypoints such as `index.js` or package exports.
- Splitting UI-only components from lifecycle/stateful containers.
- Removing hard-coded business data, route assumptions, domains, tokens, or environment-specific values.
- Extracting required styles, assets, helpers, constants, adapters, and interfaces into the reusable unit.
- Adding explicit props, params, events, return contracts, config objects, adapters, or extension points.
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
      -> refactor into reusable modules/components/utilities/config
      -> validate
      -> write reuse documentation
```

## Required Reuse Boundaries

A reusable asset should have clear boundaries:

- UI boundary: visual component and interaction only.
- Data boundary: business data enters through explicit props, params, files, or adapters.
- Service boundary: external URLs, tokens, CRS, credentials, and provider definitions live in config/provider files.
- Style boundary: required CSS classes, design tokens, assets, and theme assumptions live inside the reusable unit/package or are exposed through a stable style entrypoint.
- Export boundary: consumers have one stable import or package entrypoint.
- Validation boundary: consumers know which command or manual check proves the reuse works.

## Avoid Fake Reuse

Do not label something reusable when:

- It hard-codes current-page business state.
- It requires a specific page to work.
- It stores secrets in UI code or browser storage by default.
- Its styles, helpers, constants, or assets are scattered across unrelated files instead of being extracted into a reusable unit.
- A new project must edit internal source code just to use it.
- The documentation lacks a reusable file/directory list, dependency list, config list, invocation example, and verification path.

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
> **复用对象**：说明被复用的组件、模块、工具、流程或模板名称
> **来源项目/路径**：说明来源项目和关键源码路径
> **适用范围**：说明适合复用到哪些项目或场景
> **维护说明**：说明后续修改、升级、验证或注意事项

## 介绍

说明复用对象是什么、解决什么问题、当前具备哪些能力。

## 实现原理

说明核心分层、状态流转、依赖关系、配置注入方式、扩展点，以及为什么这种设计具备复用性。

## 详细原理

详细说明当前项目如何实现它，包括目录结构、模块职责、核心流程、参数、事件、配置项、关键契约和可复用边界。

## 分析

### 可能的优势

说明这种设计对复用、扩展、测试、迁移、维护或版本治理的好处。

### 可能存在的缺陷

诚实说明当前耦合点、缺少的扩展点、缺少的测试、样式依赖、构建工具假设或打包缺口。

## 复用方式

提供可执行的复用路径。不能写成愿景，必须写清楚如何拷贝、如何安装、如何配置、如何调用、如何验证。
```

If the user has already supplied a required heading format, use their headings exactly. Still enforce executable reuse content.

## File/Directory Reuse Path

Use this path when projects are few, the reusable asset is still evolving, or no shared package system exists. The consumer must copy a complete reusable file set or directory as a unit, not scattered source snippets.

Must include:

- New-project prerequisites, including framework version, build tool, runtime assumptions, and browser/server requirements when relevant.
- Reusable files/directories to copy as a complete unit.
- Optional reusable files/directories to copy as complete units.
- Dependencies to install.
- Reusable style/assets files included in the copied directory/package.
- Environment variables/configuration.
- Target import path after copying.
- Minimal invocation example.
- Required input data shape.
- Events/callbacks/outputs the consumer must handle.
- Layout/container requirements such as width, height, parent positioning, or required host elements.
- Build-tool differences.
- Verification steps and expected results.

Template:

````markdown
### 方式一：文件/目录复用

适用场景：

* ...

新项目前置条件：

* 框架/运行时：...
* 构建工具：...
* 浏览器或服务端要求：...

1. 复制完整复用文件/目录：

```text
src/components/example/ExampleComponent.vue
src/components/example/index.js
```

2. 安装依赖：

```bash
npm install example-dependency
```

3. 确认复用单元内置样式入口：

```text
src/components/example/style.css
```

4. 配置运行时参数：

```env
EXAMPLE_TOKEN=...
```

5. 调用组件或模块：

```vue
<ExampleComponent :items="items" @change="onChange" />
```

6. 准备输入数据：

```js
const items = [
  { id: 1, name: 'Example' }
]
```

7. 处理输出事件：

```js
function onChange(payload) {
  // handle payload
}
```

8. 确认容器或布局要求：

```css
.example-host {
  width: 100%;
  height: 400px;
}
```

9. 验证：

```bash
npm run lint
npm run build
```

预期结果：构建通过，并且在 `...` 页面能看到目标功能正常工作。
````

## Package Reuse Path

Use this path when multiple projects need the same asset, versioning matters, or future upgrades should be centrally managed.

Must include:

- Suggested package directory.
- Package entrypoint.
- Peer dependencies.
- Style output strategy.
- Installation command.
- Import example.
- Minimal consumer code for a new project.
- Required config/env setup.
- Required input data shape.
- Events/callbacks/outputs.
- Layout/container requirements when applicable.
- Versioning and breaking-change note.
- Verification steps.

Template:

````markdown
### 方式二：打包调用复用

建议包结构：

```text
packages/example/
├── src/
│   └── index.ts
├── package.json
└── README.md
```

Peer dependencies：

```json
{
  "peerDependencies": {
    "vue": "^3.2.13"
  }
}
```

业务项目调用：

```js
import { ExampleComponent } from "@company/example"
import "@company/example/style.css"
```

业务页面最小示例：

```vue
<ExampleComponent :items="items" @change="onChange" />
```

业务项目配置：

```env
EXAMPLE_TOKEN=...
```

业务数据结构：

```js
const items = [
  { id: 1, name: 'Example' }
]
```

验证：

```bash
npm run build
npm pack
npm install ../packages/example/company-example-1.0.0.tgz
npm run build
```
````

## Reuse Acceptance Checklist

Before finishing Reuse Mode, confirm:

- The reusable boundary exists in code or the missing boundary is explicitly documented.
- The reuse guide is placed under `docs/reuse/` unless the user explicitly requested another location.
- The reuse guide starts with metadata lines for generation time, reusable target, source path, applicable scope, and maintenance notes.
- The reuse guide includes file/directory reuse and package reuse when applicable.
- Each reuse method is executable, not aspirational.
- Required reusable file sets, dependencies, style/assets entrypoints, config, import paths, invocation code, input data shape, event/callback wiring, layout requirements, and verification steps are listed.
- Secrets are injected through configuration, not hard-coded.
- Existing behavior is validated with lint/build/tests/manual checks where feasible.
- Current limitations are stated honestly.

## Final Response Requirements

In the final response, include:

- Whether the target is currently reusable.
- What was changed to improve reuse.
- The reuse guide path.
- The ECD work/review record path if ECD delivery rules require one.
- Validation commands and results.
- Remaining limitations.



