# ExecPlans: Self-Contained Execution Plans for Stateless Coding Agents

This document defines what an execution plan (“ExecPlan”) is and how to write, implement, and maintain one so that a complete novice (human or agent) can deliver a working, observable change using only the repository working tree and the single ExecPlan file.

An ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` are mandatory and must be kept correct as work proceeds.

## What an ExecPlan Is

An ExecPlan is a design-and-execution document that a coding agent can follow to produce a demonstrably working feature or system change.

“Stateless” means the executor has no memory of earlier conversations, earlier plans, or unwritten intent. They only have:
- The current repository working tree (files as they exist now).
- The single ExecPlan file you provide.

Therefore, an ExecPlan must contain all necessary context, definitions, commands, expected outputs, and recovery steps. If it is not written down in the ExecPlan, it does not exist.

## Relationship to PLANS.md

If `PLANS.md` exists in the repository, it is authoritative for process and formatting requirements. Before writing or implementing an ExecPlan, read `PLANS.md` from the repository root in full and follow it exactly. If any guidance here conflicts with `PLANS.md`, `PLANS.md` wins.

At the top of your ExecPlan, explicitly reference the repository-relative path to `PLANS.md` (for example, `PLANS.md` at repo root) and state that the ExecPlan must be maintained in accordance with it.

If `PLANS.md` does not exist, proceed using this document as the process contract and explicitly state in the ExecPlan that `PLANS.md` was not found at the time of writing.

## Non-Negotiable Requirements

Every ExecPlan must satisfy all of the following:

Self-contained and novice-guiding. The plan must include all needed information and must assume the reader knows nothing about the repository. It must name files by repository-relative paths, call out key entry points (modules/functions), and explain how the relevant parts fit together.

Outcome-focused and demonstrable. The plan must describe user-visible or externally verifiable behavior that will exist after completion, and how to prove it works (commands to run, requests to make, tests that pass, and what output to expect). Code changes that compile but do not produce meaningful behavior are not acceptable.

Plain language. Do not rely on jargon. If you use a technical term that is not ordinary English (for example “daemon”, “middleware”, “RPC”, “migration”), define it immediately in plain language and point to where it appears in this repository (files, commands, services, or logs).

No external dependencies for understanding. Do not point the reader to external blogs or documentation for essential knowledge. If external knowledge is required, embed the relevant explanation in your own words inside the ExecPlan.

Living and restartable. The plan must remain accurate as work progresses. A new contributor must be able to restart from only the current ExecPlan and the repository and still succeed.

## Required ExecPlan Envelope and Formatting

When delivering an ExecPlan as a chat response, the entire ExecPlan must be one single fenced code block labeled `md`, with no additional fenced code blocks inside it. When you need to show commands, transcripts, diffs, or code snippets inside the ExecPlan, present them as indented blocks (with leading spaces) rather than nested fences.

When writing an ExecPlan directly to a Markdown file where the file content is only the ExecPlan, omit the outer fence (the file itself is the plan). Still follow the “no nested fences” rule inside the file.

Use `#`, `##`, `###` headings. Use two newlines after every heading. Prefer prose over lists. Checklists are permitted only in the `Progress` section, where they are mandatory.

## Required Sections in Every ExecPlan

Every ExecPlan must include these sections, in this order, and keep them updated:

- Purpose / Big Picture: What a user can do after the change that they could not do before, and how to see it working.
- Progress: A checklist with timestamps that reflects the current, real state of work.
- Surprises & Discoveries: Unexpected behaviors, bugs, constraints, performance tradeoffs, or important learnings found during implementation, with concise evidence.
- Decision Log: Every key decision made, with rationale and date/author.
- Outcomes & Retrospective: What was achieved, what remains, and lessons learned (at major milestones and at completion).
- Context and Orientation: Repository-specific orientation for a novice.
- Plan of Work: The narrative sequence of edits/additions by file and location, with intent and minimal necessary detail.
- Concrete Steps: Exact commands to run (including working directory), and what to observe.
- Validation and Acceptance: How to prove the change works, expressed as human-verifiable behavior.
- Idempotence and Recovery: How to safely re-run steps; how to retry/rollback if something fails mid-way.
- Artifacts and Notes: Concise transcripts/diffs/log snippets that prove progress and success.
- Interfaces and Dependencies: Precise, prescriptive details of the public surfaces (types, signatures, modules, libraries) that must exist.

## Writing Style: How to Be Clear Without Being Redundant

Write so a novice can execute without guessing. Repeat only what is necessary for correctness when read in isolation (for example, assumptions, environment requirements, and how to validate). Avoid repeating the same rule in multiple sections unless it is essential at the point of use.

Prefer short paragraphs that answer:
- What changes and why it matters.
- Where in the repository the change happens.
- How to run it and what success looks like.
- What can go wrong and how to recover.

## Authoring an ExecPlan (Design Work)

When creating a new ExecPlan:

Start from the user outcome. In a few sentences, describe what becomes possible after completion and provide a concrete way to observe it working (an HTTP request and response, a CLI invocation, a UI flow, a before/after test outcome).

Orient the reader to the repository. In `Context and Orientation`, name the key files and explain how they relate. If the architecture is unfamiliar, describe only what is necessary to implement the change, using repository terms and paths.

Make assumptions explicit. Record environment assumptions (OS, required tools, env vars), and provide alternatives if reasonable. If you assume something exists (a service, a command, a config file), state how to verify it in the repository.

Resolve ambiguities in the plan. Do not outsource key decisions to the reader. When multiple reasonable options exist, choose one, explain why, and record it in the `Decision Log`. If feasibility is uncertain, design a milestone that proves feasibility with a small, testable prototype.

## Implementing an ExecPlan (Execution Work)

When implementing an ExecPlan:

Proceed milestone by milestone. Do not ask the user for “next steps”. If you must stop, update `Progress` to clearly mark what is done and what remains, including splitting partially completed items into “completed” and “remaining”.

Keep the plan correct. As you learn new information (real file paths, true command names, unexpected constraints), update the ExecPlan immediately so it stays self-contained and restartable.

Commit frequently. Prefer small, additive changes that keep tests passing where possible. If a risky change is required, provide a safe checkpoint and recovery instructions.

Record evidence. When you run commands or tests, capture a short, focused snippet of output in `Artifacts and Notes` so future readers can distinguish success from failure.

## Milestones: Structure and Purpose

Milestones are the story of delivery. Each milestone must describe:
- What will exist at the end of the milestone that did not exist before.
- The commands to run to demonstrate it.
- The acceptance you expect to observe.

Each milestone must be independently verifiable and must incrementally deliver the overall goal.

### Prototyping Milestones

Prototyping is allowed and often encouraged when it reduces risk. A prototyping milestone should be explicitly labeled as prototyping, should be additive and testable, and should include criteria for either promoting it into the final implementation or discarding it cleanly.

Parallel implementations (for example, a new path alongside an old path during migration) are acceptable when they reduce risk. If you do this, include how to validate both paths and a clear, safe procedure for removing the old path later, backed by tests.

## Validation and Acceptance: What “Done” Means

Validation is not optional. An ExecPlan must always include:
- The exact test command(s) for this repository and what a successful run looks like.
- At least one end-to-end or user-facing proof of behavior when applicable (a request/response transcript, a CLI output, a UI flow, or a reproducible scenario).
- Expected outputs and common failure signals so a novice can diagnose issues.

Acceptance must be phrased as behavior a human can verify. If a change is internal, explain how to demonstrate its impact (for example, a new test that fails before and passes after, or a measurable change in observable logs/metrics in a local run).

## Idempotence and Recovery

Write steps so they can be run more than once without causing damage or drift. If a step can fail partway through, include how to retry safely and how to roll back to a known-good state. If any operation is destructive (schema migration, data deletion), include backup instructions and a safe fallback.

## Required Change Notes When Revising an ExecPlan

Whenever you revise an ExecPlan (during implementation or review), ensure the changes are reflected consistently across all relevant sections. At the bottom of the ExecPlan, add a short note describing what changed and why, so future readers can understand the evolution without external context.

## Skeleton of a Good ExecPlan (Copy Headings, Then Fill In)

The headings below are the minimum structure. Your actual content must be prose-first, concrete, and repository-specific.

# <Short, action-oriented description>

This ExecPlan is a living document. The sections `Progress`, `Surprises & Discoveries`, `Decision Log`, and `Outcomes & Retrospective` must be kept up to date as work proceeds.

If `PLANS.md` is checked into the repo, reference its path from the repository root here and note that this document must be maintained in accordance with it.

## Purpose / Big Picture

Explain in a few sentences what someone gains after this change and how they can see it working. State the user-visible behavior you will enable.

## Progress

Use a list with checkboxes to summarize granular steps. Every stopping point must be documented here, even if it requires splitting a partially completed task into two (“done” vs. “remaining”). This section must always reflect the actual current state of the work.

- [x] (YYYY-MM-DD HH:MMZ) Example completed step.
- [ ] Example incomplete step.
- [ ] Example partially completed step (completed: X; remaining: Y).

Use timestamps to measure rates of progress.

## Surprises & Discoveries

Document unexpected behaviors, bugs, optimizations, constraints, or insights discovered during implementation. Provide concise evidence.

- Observation: …
  Evidence: …

## Decision Log

Record every decision made while working on the plan in the format:

- Decision: …
  Rationale: …
  Date/Author: …

## Outcomes & Retrospective

Summarize outcomes, gaps, and lessons learned at major milestones or at completion. Compare the result against the original purpose.

## Context and Orientation

Describe the current state relevant to this task as if the reader knows nothing. Name the key files and modules by full path. Define any non-obvious term you will use. Do not refer to prior plans.

## Plan of Work

Describe, in prose, the sequence of edits and additions. For each edit, name the file and location (function, module) and what to insert or change. Keep it concrete and minimal.

## Concrete Steps

State the exact commands to run and where to run them (working directory). When a command generates output, show a short expected transcript so the reader can compare. This section must be updated as work proceeds.

## Validation and Acceptance

Describe how to start or exercise the system and what to observe. Phrase acceptance as behavior, with specific inputs and outputs. If tests are involved, say what to run and what success looks like, including at least one test that demonstrates the new behavior.

## Idempotence and Recovery

Explain how steps can be repeated safely. If a step is risky, provide a safe retry or rollback path. Keep the environment clean after completion.

## Artifacts and Notes

Include the most important transcripts, diffs, or snippets as indented examples. Keep them concise and focused on what proves success.

## Interfaces and Dependencies

Be prescriptive. Name the libraries, modules, and services to use and why. Specify the types, traits/interfaces, and function signatures that must exist at the end. Prefer stable names and repository-relative paths.

When you revise this plan, ensure all sections remain consistent and self-contained, and add a short change note at the bottom describing what changed and why.
