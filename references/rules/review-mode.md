# ECD Review Mode

## Purpose

Use Review Mode when the user asks ECD to review code, current git changes, a PR, a commit range, a patch, an architecture change, or a reusable asset before adoption. Review Mode brings the `code-review-expert` workflow into ECD while preserving ECD's broader enterprise checks.

Review Mode is review-first. Do not implement fixes during the same response unless the user explicitly asks for fixes after receiving the review findings.

## Trigger Examples

- "ECD review"
- "用 ECD review 一下"
- "按 ECD 的 review 模式检查"
- "review current changes"
- "code review this PR"
- "检查这次改动有没有问题"
- "看看有没有 SOLID / 安全 / 质量问题"
- "review 这个复用改造"

## Review Contract

- User-facing review output must be Chinese.
- Findings must come first, ordered by severity.
- Use concrete file and line references when possible.
- Prefer actionable defects over generic advice.
- Do not rewrite code during Review Mode unless the user explicitly asks for implementation after the review.
- If no issues are found, say that clearly and list residual risks or unverified areas.
- If the diff is too large, review by module/feature area and state what was covered.
- If there are no unstaged changes, ask whether to review staged changes, a branch diff, a commit range, or specific files.

## Severity Levels

| Level | Name | Blocks? | Description |
| --- | --- | --- | --- |
| P0 | Critical | Must block | Security vulnerability, data loss, irreversible corruption, production outage risk, severe correctness bug |
| P1 | High | Should block | Logic error, broken contract, significant SOLID/architecture violation, performance regression, auth/data consistency risk |
| P2 | Medium | Usually fix or track | Maintainability concern, minor SOLID violation, missing edge case, weak validation, test gap with realistic risk |
| P3 | Low | Optional | Naming, style, clarity, small cleanup, low-risk suggestion |

## Preflight

Start by scoping the review:

```bash
git status -sb
git diff --stat
git diff
```

Also inspect staged or range diffs when relevant:

```bash
git diff --cached
git diff main...HEAD
git show --stat <commit>
git show <commit>
```

Use `rg` to find related modules, call sites, contracts, migrations, configs, tests, and docs.

Identify:

- Files changed and approximate lines changed.
- Entry points and public contracts.
- Ownership/module boundaries.
- Critical paths: auth, permissions, money, data writes, migrations, secrets, deployment, concurrency, external APIs, jobs, queues, storage, runtime config.
- Test/build/validation signals already present or missing.

## Review Dimensions

### 1. Correctness And Behavior

Check:

- Logic errors and broken assumptions.
- Changed behavior not reflected in callers, tests, docs, or config.
- Public contract compatibility: APIs, DTOs, CLI flags, emitted events, schemas, migration order, file formats.
- Boundary cases: null/undefined, empty collections, zero/negative numbers, large inputs, off-by-one, pagination, Unicode/long strings.
- Error propagation and user-visible failure modes.

### 2. SOLID And Architecture

Check:

- SRP: unrelated responsibilities in one module, class, component, handler, service, job, or script.
- OCP: adding variants requires repeated edits to core logic instead of extension points.
- LSP: subtype or adapter breaks caller expectations.
- ISP: broad interfaces with unused methods or empty implementations.
- DIP: domain/application logic coupled directly to infrastructure, network, storage, framework, or vendor SDK.
- Local architecture fit: does the change follow existing module boundaries and naming conventions?
- Avoid over-refactoring: propose incremental splits when risk is non-trivial.

### 3. Security And Privacy

Check:

- XSS, SQL/NoSQL/command/GraphQL injection, SSRF, path traversal, unsafe deserialization, prototype pollution.
- Missing AuthN/AuthZ, tenant/ownership checks, IDOR, trusting client-provided roles or IDs.
- Secrets, tokens, credentials, PII, or internal stack traces in code, logs, config, generated files, or client-visible bundles.
- JWT/token validation: algorithm, expiration, issuer, audience, weak or hardcoded secrets.
- CORS, security headers, dependency/supply-chain risk.
- Sensitive data masking and least-privilege assumptions.

### 4. Reliability And Data Integrity

Check:

- Missing transactions, partial writes, inconsistent state, weak validation before persistence.
- Missing idempotency for retries, webhooks, jobs, payments, imports, or migrations.
- Race conditions: check-then-act, TOCTOU, read-modify-write, missing locks/version checks, concurrent inserts.
- Distributed systems: stale cache, event ordering, duplicate messages, missing distributed lock where necessary.
- External calls without timeouts, retries, circuit breaking, fallback, or rate limiting.

### 5. Performance And Resource Use

Check:

- N+1 queries, unbounded loops, loading large files/datasets into memory, missing pagination.
- CPU-heavy operations in hot paths.
- Sync/blocking I/O in async/request paths.
- Cache without TTL, invalidation, tenant scoping, or safe keying.
- Bundle size or rendering cost for frontend changes.
- 10x/100x data and concurrency behavior.

### 6. Error Handling And Observability

Check:

- Swallowed exceptions, overly broad catch, only logging without recovery/propagation.
- Async errors, promise rejections, missing error boundaries.
- User-facing errors leaking internals.
- Missing diagnostic context: requestId, userId, tenantId, entityId, jobId, traceId, config key, resource name.
- Missing logs, metrics, traces, alert signals, or deployment checks for risky paths.

### 7. Tests And Validation

Check:

- Tests cover changed normal, edge, and failure paths.
- Contract tests exist for public APIs/CLI/file/schema/migration interfaces.
- UI changes have interaction/visual/accessibility or browser verification where feasible.
- Data/ML/automation changes have fixture-based deterministic checks.
- Infrastructure/deployment changes have plan/dry-run/validation output.
- Missing tests are called out with risk, not as generic "add tests" noise.

### 8. Removal And Iteration Candidates

Identify:

- Dead code, unused exports, disabled feature branches, duplicate utilities, obsolete configs.
- Safe removal now vs defer with migration/telemetry/owner/checkpoints.
- Follow-up plan when immediate removal is risky.

## Handling Large Or Mixed Diffs

- If diff is large, summarize by file/module first.
- Group findings by logical feature area, not only file order.
- Review high-risk paths first.
- State any areas not reviewed because of size, missing context, or unavailable validation.

## Output Format

Use this shape by default:

```markdown
## Code Review Summary

**Files reviewed**: X files, Y lines changed
**Overall assessment**: APPROVE / REQUEST_CHANGES / COMMENT

## Findings

### P0 - Critical

(none or findings)

### P1 - High

1. **[path/to/file.ext:line] Title**
   - 问题：...
   - 影响：...
   - 建议：...

### P2 - Medium

2. **[path/to/file.ext:line] Title**
   - 问题：...
   - 影响：...
   - 建议：...

### P3 - Low

(none or findings)

## Removal / Iteration Plan

(only when applicable)

## Additional Suggestions

(optional)

## Coverage And Residual Risk

- 已检查：...
- 未覆盖：...
- 剩余风险：...
```

If no issues are found:

```markdown
## Code Review Summary

**Overall assessment**: APPROVE

未发现需要阻塞的代码问题。

已检查：...
未覆盖：...
剩余风险：...
```

## Inline Comments

When the environment supports inline comments and a finding is tied to a specific code line, emit `::code-comment{...}` directives in addition to the review body. Keep each comment tight and actionable.

Use priority mapping:

- P0 -> priority=0
- P1 -> priority=1
- P2 -> priority=2
- P3 -> priority=3

Example:

```text
::code-comment{title="[P1] Missing tenant check" body="该查询使用客户端传入的 tenantId，但没有校验当前用户是否属于该租户，可能造成越权读取。建议从认证上下文取 tenantId，并在仓储层强制过滤。" file="/abs/path/service.ts" start=42 priority=1}
```

## Next Steps

End the review by asking how to proceed only if findings exist:

```markdown
## Next Steps

我发现 X 个问题（P0: _, P1: _, P2: _, P3: _）。

你希望我下一步怎么处理？

1. 修复全部问题
2. 只修复 P0/P1
3. 修复指定问题
4. 暂不修改，仅保留 review 结论
```

Do not implement until the user explicitly chooses a fix path.

## ECD Interaction Rules

- Review Mode overrides ECD's normal "implement after design" tendency: review first, fix only after explicit user confirmation.
- Review Mode still uses ECD inspection discipline: understand modules, contracts, risk, tests, and blast radius before judging.
- When a review itself recommends Medium+ changes, treat follow-up implementation as normal ECD coding work with the applicable design/confirmation gates.
- If the review target is a reusable asset, also apply Reuse Mode checks where relevant: reusable boundary, stable entrypoint, config/secrets, executable reuse path, and validation.
