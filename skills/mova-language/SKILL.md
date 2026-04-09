---
name: mova-language
description: >
  Teach this agent the MOVA language — Machine-Operable Verbal Actions.
  Activate when the user mentions MOVA, asks about contract-driven AI,
  structured agent actions, machine-operable agreements, or wants to
  understand MOVA schemas, envelopes, verbs, episodes, or the global
  semantic layer. This skill gives the agent deep knowledge of MOVA
  without requiring any external tools or plugins.
version: 1.0.0
tags: [mova, language, contracts, schemas, specification]
---

# MOVA Language Knowledge

You now have deep knowledge of MOVA — Machine-Operable Verbal Actions.

MOVA is a language for making agreements between humans, machines, and AI agents explicit, structured, and validatable. It is not a programming language. It contains no executable code. It is a contract catalog: structured data that describes what exists, what should happen, and what rules apply.

## Core Concepts

### What MOVA defines

- **Data structures** (`ds.*`) — JSON Schemas describing what objects exist and what shape they have
- **Verbs** — abstract operation types: `create`, `update`, `validate`, `extract`, `approve`, `route`, `explain`, `plan`
- **Envelopes** (`env.*`) — structured speech-acts that bind a verb to data, roles, and context
- **Episodes** — structured records of work that was actually performed
- **Policies and guardrails** — instruction profiles defining what is allowed, restricted, or requires human approval
- **Global semantic layer** (`global.*`) — shared vocabularies for roles, statuses, resources, categories, episode types, security events, and text channels

### Key principles

- **Dual-readable**: every MOVA document is readable by humans AND parseable by machines
- **Bidirectional**: describes what should happen (prospective) AND what already exists (retrospective)
- **Runtime-agnostic**: works with any executor — Python, Rust, Node.js, AI agent, or human
- **Vendor-independent**: works with any LLM, any cloud, any platform
- **Auditable by default**: every meaningful action can be recorded as a structured episode

### Data schemas (`ds.*`)

Each `ds.*` schema is a JSON Schema (draft 2020-12) defining field types, constraints, required vs optional fields, and allowed values. Schemas describe what data looks like, never how it is processed.

Red core schemas:
- `ds.mova_schema_core_v1` — core language for schemas themselves
- `ds.mova_episode_core_v1` — core episode frame
- `ds.security_event_episode_core_v1` — security event episode
- `ds.instruction_profile_core_v1` — policies and guardrails
- `ds.runtime_binding_core_v1` — runtime binding
- `ds.connector_core_v1` — connector contracts
- `ds.ui_text_bundle_core_v1` — UI text bundles
- `ds.mova4_core_catalog_v1` — core catalog model

### Envelopes (`env.*`)

Envelopes are structured requests, commands, and events. A typical envelope ties together:
- A verb (operation type)
- References to data schemas
- Roles (initiator, executor, receiver)
- Context and metadata

Core envelopes:
- `env.mova4_core_catalog_publish_v1`
- `env.instruction_profile_publish_v1`
- `env.security_event_store_v1`

### Verbs

Verbs describe types of operations. Examples: `create`, `update`, `delete`, `validate`, `route`, `record`, `aggregate`, `explain`, `plan`, `analyze`, `summarize`.

Verbs are abstract — different executors may implement the same verb differently but must honor the same input/output contracts.

### Global semantic layer (`global.*`)

Shared vocabularies:
- Roles: `user`, `agent`, `executor`
- Resources: `file_system`, `http_api`
- Statuses: `pending`, `completed`, `failed`
- Episode types, security event types, text channels

### Episodes and the genetic layer

Episodes are structured records of work steps: identifiers, timestamps, input references, executor identity, result status, output references, optional logs and metrics.

Episodes form the basis for audit, analytics, and the **genetic layer**.

Important honesty rule:

- the genetic layer is part of MOVA architecture
- but in the current public ecosystem it is not yet implemented as a fully automated production subsystem
- today it exists only partially and manually through lineage, evidence, audit, telemetry, and operator interpretation

### Security layer

- Instruction profiles: declarative policies and guardrails
- Security events: structured security episodes
- Security catalogs: event and action types
- Model versioning: tracks which security model a profile uses

### Four use patterns

1. **Describe and Execute** — define a process as a contract, AI executes it step by step
2. **Describe and Transfer** — capture semantics of existing systems, transfer between languages/platforms
3. **Describe and Verify** — formalize to find weak spots, test across implementations, audit compliance
4. **Execute, Learn, Evolve** — episodes produce patterns, patterns improve contracts, contracts generate training datasets

## How to help the user

When the user asks about MOVA:
- Explain concepts clearly without jargon overload
- Use concrete examples from business automation (invoices, approvals, workflows)
- Connect MOVA concepts to problems the user is trying to solve
- Reference the specification at https://github.com/mova-compact/mova-spec when deep detail is needed
- Explain the difference between MOVA and other approaches (prompt engineering, workflow tools, BPM systems)

When the user wants to work with MOVA:
- Help them identify which schemas, verbs, and envelopes are relevant
- Guide them through structuring their data and actions in MOVA format
- Validate their understanding against the specification
- Suggest the Intent Calibrator skill if they need help defining what they want to automate
- Suggest the Contract Writer skill if they have a clear intent and want to produce a MOVA contract
