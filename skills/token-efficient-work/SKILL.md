---
name: token-efficient-work
description: Minimize total token use on multi-step implementation, research, review, and planning by keeping context narrow, avoiding redundant work, and routing only suitable bounded subtasks to cost-efficient models. Use when the user asks to save tokens, split work by model, or apply a token-efficient workflow across projects.
---

# Token-Efficient Work

Optimize aggregate token use and cost per successfully completed task, including retries, corrections, and delegation overhead. Preserve correctness, safety, and the user's requested scope.

## Work compactly
1. Define the concrete deliverable and risk level before loading broad context.
2. Search narrowly first. Read only the files, ranges, references, and history needed for the current decision.
3. Reuse existing project instructions, scripts, tests, and artifacts instead of recreating them.
4. Keep updates, prompts, handoffs, and final responses concise. Pass paths, exact findings, and acceptance criteria rather than long background summaries.
5. Run verification proportional to the change. Prefer focused checks first; expand only when evidence or project rules require it.
6. Stop investigating a resolved branch of the problem. Do not repeat searches or tests without a new reason.

## Choose single-agent work by default

Delegation has context and coordination overhead. Do not create a subagent merely to use a cheaper model. Delegate only when a bounded, independent subtask is likely to reduce total work, can run usefully in parallel, or is required by the risk routing below.

When delegating:

- Fork no history or only the few turns necessary.
- Give the subagent one concrete output, explicit scope, relevant paths, and a stopping condition.
- Ask for compact evidence, not a narrative replay.
- Do not have multiple agents inspect the same area unless independent review is justified by risk.
- Keep integration and decisions that depend on the full user context with the primary agent only when the primary model meets the required tier below.

### Work with Default or an explicitly selected model

The app controls the primary model and reasoning effort, whether the user chooses Default or a specific model. This skill complements either mode by reducing context, duplicate work, retries, and unnecessary delegation. Default does not replace this skill's material-risk requirements.

This skill cannot itself switch the current primary model mid-turn. Respect the user's selection; do not change global defaults or operate the model picker merely to enforce the routing table. The routing below guides authorized delegated work and recommendations when a model change is needed. An Astra primary should handle suitable work directly rather than create a cheaper subagent solely to match the table.

When checking a required tier, use the effective model and effort exposed by the current runtime. The Default label and saved global configuration alone do not establish them. Do not assume Default analyzes each request and dynamically chooses the cheapest adequate model. If the effective tier cannot be verified, continue ungated work and use a qualifying, explicitly configured subagent for the gated step when available; otherwise explain the model and effort the user needs to select before that step.

### Escalate from a lower-tier primary

Classify difficulty and risk before material planning, implementation, or mutation. If the current primary model already meets the required tier, continue single-agent work; do not delegate merely to repeat the same work on another model.

If the required tier is higher than the current primary model, this skill authorizes bounded delegation to that tier when subagents and model overrides are available. Delegate the risk-bearing decision, implementation, or final review with the relevant user intent, constraints, paths, evidence requirements, and stopping condition. The lower-tier primary may coordinate artifacts and run deterministic checks, but must not replace the higher-tier agent's judgment on the gated decision.

For the material-risk categories below, the minimum is Sol high or a higher-capability model at high effort, including Astra high. A Luna or Terra primary must delegate the gated decision or mutation to a qualifying model; an Astra high primary already meets this requirement. If the work cannot be bounded safely, or no qualifying model or delegation is available, stop before the material step and explain the required model and effort. Routine planning does not require escalation unless the plan itself contains the listed architecture, integration, or material-risk decisions.

## Route models by difficulty and risk

Choose the model expected to complete the bounded assignment reliably at the lowest total cost, not simply the lowest token price. These are starting points, not a mandatory sequence:

- **GPT-5.6 Luna · medium:** mechanical search, inventory, formatting, simple classification, log extraction, and deterministic checks.
- **GPT-5.6 Terra · medium:** ordinary UI, API, documentation, tests, and well-scoped implementation.
- **GPT-5.6 Terra · high:** complex but bounded implementation or debugging that needs deeper reasoning without high-risk authority.
- **GPT-5.6 Sol · high:** architecture and integration decisions, ambiguous cross-system failures, final review of material risk, authentication, authorization, payments, secrets, RLS, destructive data changes, migrations, production release decisions, and security-sensitive work.
- **GPT-6 Astra · medium:** demanding multistep work across code, research, documents, or apps when stronger reasoning is likely to reduce retries and coordination. Use high for the hardest problems or any material-risk category above.
- **GPT-6 Astra · high:** the hardest cross-system reasoning and integration work, unresolved Sol-level problems, and material-risk work that benefits from stronger judgment. It satisfies the Sol-high minimum; do not add a Sol review merely to meet that minimum. Independently required reviews still apply.

Do not use `xhigh`, `max`, or `ultra` by default. Route material-risk work directly to Sol high or a qualifying higher-capability model. For other work, choose Astra directly when complexity or known task results justify it; do not require cheaper models to fail first. Otherwise escalate model or effort when concrete uncertainty, repeated failure, or insufficient evidence warrants it. Check the current runtime's supported models and effort levels when availability matters. If a named model is unavailable, choose the nearest current equivalent by capability and cost, except that material-risk work must not fall back below the required capability and effort without the user's explicit choice.

## Judge efficiency by completed work

- Include input and cache usage, output (including reasoning tokens where billed), retries, tool failures, user corrections, and all agents when comparing alternatives. Avoid double-counting nested usage fields. Prefer existing task evidence; do not run duplicate work merely to measure savings.
- Higher token prices do not necessarily mean higher task cost. Astra may finish difficult work with fewer output tokens or retries, but do not assume it is cheaper for the user's workload without evidence.
- Distinguish API billing from Codex subscription allowance. Subscription use also depends on model, reasoning, context, tools, retrieval, and caching; token counts alone do not establish allowance savings. Report only measurements actually available.

Routing is a local workflow recommendation. Recheck current capabilities and pricing when revising it; avoid hardcoding rates. Sources checked 2026-09-08: [Astra guidance](https://developers.openai.com/api/docs/guides/latest-model) and [Codex usage and pricing](https://learn.chatgpt.com/docs/pricing).

## Preserve safety and authority

Token efficiency never relaxes project instructions, validation gates, approval requirements, or release ownership. Do not omit a required check, make an unsafe assumption, or broaden permissions to save tokens. For high-risk work, saving tokens means narrowing inputs and avoiding duplication, not lowering the required review quality.
