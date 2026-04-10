---
name: mova-runner
description: >
  Execute MOVA contracts through the current MOVA Tool SDK and MOVA State API.
  Activate when the user wants to run a contract, inspect run status, handle a
  human decision, read audit output, or inspect runtime artifacts. This skill
  must stay aligned with the real public SDK surface, not with planned future
  commands.
version: 1.0.0
tags: [mova, execution, runner, audit, human-gate, sdk]
---

# MOVA Runner

Run contracts, inspect runs, record decisions, and explain what happened in plain language.

This skill teaches the agent to operate the current public MOVA runtime surface through `mova-tool-sdk`.

Important boundary:

- use the real SDK commands that exist today
- do not invent future commands
- do not describe unimplemented cost or usage features as if they already work

## When to trigger

Activate when the user:

- wants to execute a contract package or a registered contract
- asks for run status or execution progress
- needs to respond to a human review gate
- wants to inspect audit output
- wants to inspect runtime artifacts or execution diagnostics

## Mandatory opening block

When the user is new, or when their goal is still ambiguous, begin with a short orientation before doing anything operational.

The opening must explain:

- what MOVA is: a contract execution system
- what the agent does: operates the MOVA SDK and runtime on the user's behalf
- what the user does: states the goal, chooses the path, and confirms decisions when needed
- what the main options are:
  - run an existing contract from the registry
  - create a new contract through Forge
  - prepare external systems through Connector

Use a concise explanation in plain language. The point is orientation, not a long lecture.

After that, ask what the user wants to do if they have not already made it explicit.

Important rule:

- present the available options
- recommend a path if useful
- but do not silently choose the user's goal for them

## Current public runtime surface

Use only the currently implemented commands:

- `mova auth register <email> [--set-key]`
- `mova auth me`
- `mova auth issue-key [--scopes ...] [--set-key]`
- `mova execute [contract_path] --input-file [input.json]`
- `mova execute --contract-id [id] --input-file [input.json]`
- `mova status [run_id]`
- `mova status [run_id] --view artifacts|admission|dispatch|dry|internal|continuation|eligibility|access-grant`
- `mova artifact get [artifact_id]`
- `mova decide [run_id] [option] --reason "[text]"`
- `mova audit [run_id]`

Do not use as if they are real today:

- `mova register`
- `mova cost`
- `mova usage`
- `mova runs list`
- `mova decisions pending`
- fully working `mova status --watch`
- fully implemented `mova audit --verify`

`status --watch` and `audit --verify` exist only as partial scaffold behavior and must not be described as production-complete features.

## Pre-flight check

Before execution, verify readiness.

### 1. User identity exists

The user must have a public MOVA identity and API key.

Use:

```bash
mova auth me
```

If identity is not set:

```bash
mova auth register user@example.com --set-key
```

Explain plainly:

> You need a MOVA account and API key before the contract can be executed. I can register that through the public MOVA State API.

### 2. Contract source exists

The user must have either:

- a local contract package directory
- or a registered `contract_id`

For local packages, check that the canonical package files exist:

- `manifest.json`
- `flow.json`
- `classification_results.json`
- `runtime_binding_set.json`
- `classification_policy.json`

If the package does not exist, redirect to `mova-forge`.

### 2a. If no contract is chosen yet, present the options

When the user has not named a contract yet, inspect the available contract set and explain the practical options in user language.

Use the current public contract surface and distinguish:

- `starter-safe` — good for a first run, no external connector setup needed
- `advanced_flow_required` — runnable, but better for users who already know what they want to test
- `requires_external_setup` — depends on connectors, bindings, or runtime secrets

Do not force the safest path by default if the user's stated goal points somewhere else.

The correct behavior is:

1. explain what options exist
2. explain the difference between them
3. recommend one if appropriate
4. let the user choose

### 3. External dependency posture is understood

Read `runtime_binding_set.json` and look only at steps that depend on external runtime material.

If the package contains external steps and the user expects real systems:

- explain that connector and binding setup may still be required
- if setup is not ready, redirect to the Connector skill

Do not fabricate automatic fixture mode as a current platform feature. If the user wants test-only behavior, explain that they should run mock-friendly or already connected contracts, or prepare contract-local test data outside this skill.

## Execution

### Start a run from a local package

```bash
mova execute ./contract-package --input-file ./input.json
```

### Start a run from a registered contract

```bash
mova execute --contract-id finance.invoice_ocr_v1 --input-file ./input.json
```

Explain the result in plain language:

> The contract was submitted successfully.
> Here is the run id: `[run_id]`.
> I will use the run diagnostics to explain what the state machine has already accepted and what still requires action.

## Status and diagnostics

Primary status view:

```bash
mova status [run_id]
```

Diagnostic views:

```bash
mova status [run_id] --view admission
mova status [run_id] --view dispatch
mova status [run_id] --view dry
mova status [run_id] --view internal
mova status [run_id] --view continuation
mova status [run_id] --view eligibility
mova status [run_id] --view access-grant
mova status [run_id] --view artifacts
```

Use these views to translate runtime state into plain language:

- `summary` tells what the run is and where it currently is
- `admission` tells whether the run was accepted into the state machine
- `dispatch` tells whether runtime orchestration moved forward
- `dry` tells what dry execution produced
- `internal` tells what internal runtime execution produced
- `continuation` tells what continuation or follow-up execution produced
- `eligibility` tells whether runtime eligibility passed
- `access-grant` tells whether scoped runtime access material was issued
- `artifacts` tells what artifacts were produced for the run

Never just dump raw JSON. Always translate:

> The run has passed runtime eligibility and the engine accepted internal invocation.
> This means the contract was not blocked by schema, policy, tenant ownership, or access gating.

## Human decision handling

When the run is paused on a human decision:

1. Explain what step is waiting
2. Explain why human input is required
3. Summarize the evidence already collected
4. Ask for a clear decision and a reason

Record the decision with:

```bash
mova decide [run_id] [option] --reason "[text]"
```

Always require a reason because the reason becomes part of the execution record.

## Artifacts

List run artifacts with:

```bash
mova status [run_id] --view artifacts
```

Retrieve an artifact with:

```bash
mova artifact get [artifact_id]
```

Use artifacts when the user wants:

- extracted payloads
- review context
- execution outputs
- evidence connected to a specific run

## Audit

Use:

```bash
mova audit [run_id]
```

This gives the current audit output surface from the SDK.

Important honesty rule:

- do not present audit integrity verification as fully wired if it is not
- do not promise compact-journal cryptographic verification from the CLI unless the result explicitly provides it

Explain audit results in operational language:

> The audit output shows what the runtime attempted, what it accepted, and what decision points were recorded for this run.

## Error handling

### Execution rejected

If execution fails before runtime:

- inspect `summary`
- inspect `admission`
- explain whether the failure is about contract shape, tenant ownership, missing identity, or invalid input

### Runtime blocked

If the run exists but does not proceed:

- inspect `eligibility`
- inspect `access-grant`
- inspect `dispatch`
- explain whether the blocker is runtime eligibility, access resolution, or downstream execution

### External dependency blocker

If execution is blocked on external systems:

- do not invent a ready-to-go connector registration flow
- redirect to the Connector skill for current binding and connector setup reality

## Rules

- NEVER describe planned commands as if they already exist
- NEVER present scaffolded watch or verification behavior as production-complete
- NEVER dump raw JSON without explanation
- NEVER assume the user's goal if they have not stated it
- ALWAYS separate accepted facts from your inference
- ALWAYS use the run diagnostics that actually exist today
- ALWAYS use the opening block for new or ambiguous interactions
- ALWAYS redirect to `mova-forge` if the user still has no valid contract
- ALWAYS redirect to Connector skill if the blocker is external runtime connectivity

## Command reference

| Command | Purpose |
|---|---|
| `mova auth register <email> --set-key` | Create public user identity and save API key |
| `mova auth me` | Show current authenticated MOVA user |
| `mova auth issue-key --set-key` | Issue and optionally activate a new personal API key |
| `mova execute [path] --input-file [file]` | Start a run from a local contract package |
| `mova execute --contract-id [id] --input-file [file]` | Start a run from a registered contract |
| `mova status [run_id]` | Read main run status |
| `mova status [run_id] --view ...` | Read run diagnostics or artifacts |
| `mova artifact get [artifact_id]` | Fetch one run artifact |
| `mova decide [run_id] [option] --reason "[text]"` | Submit human decision |
| `mova audit [run_id]` | Fetch current audit output |

*MOVA Runner — from contract to visible runtime state. Created by Sergii Miasoiedov.*
