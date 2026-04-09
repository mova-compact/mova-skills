---
name: mova-connector
description: >
  Prepare connector and binding setup for MOVA contracts using the current
  MOVA Tool SDK and MCP ecosystem. Activate when a contract depends on
  external calls, when the user wants to understand what external systems are
  required, or when the user wants to move from mock or incomplete setup
  toward real MCP-backed integrations.
version: 1.0.0
tags: [mova, connectors, mcp, integration, setup]
---

# MOVA Connector

Connect external systems to MOVA contracts through the current connector and binding model.

This skill is not a magic “paste URL and token and everything is live” flow. Its job is to help the agent reason about:

- what external systems a contract actually needs
- what connector definitions already exist
- what bindings already exist
- what MCP server or upstream service should sit behind that binding

## Important boundary

Current MOVA public surface does support connector and binding objects, but it does not yet provide a polished end-user one-command live connector registration flow.

So this skill must stay honest:

- use the real SDK commands that exist today
- do not promise fully automated secret onboarding if it is not public yet
- do not promise `mova connectors test` or `mova connectors remove` if those commands do not exist

## When to trigger

Activate when the user:

- has a contract with external steps
- wants to understand required integrations
- wants to prepare real MCP-backed execution
- asks to connect ERP, OCR, CRM, bank, email, or other business systems
- wants to inspect existing connector and binding state

## What this skill does

1. Read the contract package, especially `runtime_binding_set.json`
2. Identify which external steps need connector support
3. Distinguish connector definitions from business bindings
4. Help the user choose a real MCP server or integration target
5. Use the existing SDK surfaces to create or inspect connectors and bindings
6. Explain what is still missing before live runtime execution can happen

## What this skill does NOT do

- it does not claim that secrets are safely onboarded through a finished public vault product
- it does not claim that a live MCP server is already proven just because a connector object exists
- it does not invent runtime test commands that are not implemented today

## Current SDK surfaces that are real

Use only these connector-related CLI groups:

- `mova connectors list`
- `mova connectors get [connector_id]`
- `mova connectors add --id ... --title ... --service-kind ... --auth-mode ... --actions ...`
- `mova bindings list`
- `mova bindings get [binding_id]`
- `mova bindings history [binding_id]`
- `mova bindings lineage [binding_id]`
- `mova bindings create ...`
- `mova bindings attach [binding_id]`
- `mova bindings rebind [binding_id] ...`
- `mova bindings activate [binding_id]`
- `mova bindings enable-steady-state [binding_id]`
- `mova bindings pause [binding_id]`
- `mova bindings disable [binding_id]`

Platform read surfaces that already exist:

- `GET /access/connectors`
- `GET /access/vault-bindings`

## Conceptual model

### 1. Contract package

The contract package says what the workflow needs.

Read:

- `runtime_binding_set.json`
- step classifications in `classification_results.json`
- `flow.json`

Focus on steps that are effectively external or require runtime connectivity.

### 2. Connector definition

A connector definition describes a type of service:

- what service kind it is
- what auth mode it expects
- what actions it supports
- what schemas may be associated with it

This is not yet the same thing as a live connected tenant-specific endpoint.

### 3. Business binding

A binding attaches a contract/runtime context to connector-related resources and execution posture.

This is the current operational object that matters for execution readiness.

### 4. MCP server or external upstream

The real external system sits behind the connector/binding contour:

- a user-operated MCP server
- or another upstream integration boundary that MOVA runtime can reach

The skill should help the user get to that point, not pretend that MOVA already hides every integration detail.

## Flow

### Step 1 — Read the contract

Inspect `runtime_binding_set.json`.

For each relevant external runtime item, determine:

- what capability is needed
- whether it is mock-only, unresolved, or already backed by a real service
- what downstream system category it belongs to

Present a plain-language summary:

> Your contract depends on these external capabilities:
> - OCR extraction
> - vendor or ERP lookup
> - VAT or identity validation
> - payment or posting target

### Step 2 — Check existing connector state

Use:

```bash
mova connectors list
mova bindings list
```

If useful, inspect one object:

```bash
mova connectors get [connector_id]
mova bindings get [binding_id]
```

Explain to the user:

> I am checking whether MOVA already has a connector definition and a binding for the capability your contract needs.

### Step 3 — Decide what must be created

There are three common cases.

#### Case A: connector exists, binding missing

Then the next work is binding creation or attachment.

#### Case B: connector missing

Then define a connector first with:

```bash
mova connectors add \
  --id [connector_id] \
  --title [human title] \
  --service-kind [service_kind] \
  --auth-mode [auth_mode] \
  --actions [comma,separated,actions]
```

Be honest:

This creates the connector definition layer. It does not by itself prove that a real user MCP server is already live.

#### Case C: connector and binding exist, but live runtime material is still unclear

Then the problem is not object creation but access/runtime integration readiness. Explain that clearly instead of pretending setup is complete.

### Step 4 — Choose the external target

If the user does not yet have a real MCP server, help them choose one:

- search the official MCP Registry
- identify an existing public server
- or identify that they need their own user-operated MCP server

Use the official registry and already published MOVA MCP Connector context when relevant.

Do not claim that the old `https://github.com/modelcontextprotocol/servers` list is the only registry source. The official registry now exists.

### Step 5 — Create or update binding objects

When the user is ready and the contract context is known, use the binding commands that actually exist:

```bash
mova bindings create ...
mova bindings attach [binding_id]
mova bindings rebind [binding_id] ...
mova bindings activate [binding_id]
```

Use `rebind`, `pause`, `disable`, or `enable-steady-state` only when that is operationally justified.

### Step 6 — Hand off to execution

When connector and binding posture is sufficiently prepared:

- explain what is ready
- explain what is still mock or unresolved
- if runtime execution is now possible, redirect to the Runner skill

## Rules

- NEVER claim that a connector definition alone means the integration is live
- NEVER promise non-existent commands like `mova connectors test`
- NEVER promise non-existent commands like `mova connectors remove`
- NEVER pretend the public connector UX is simpler than it currently is
- ALWAYS distinguish connector definitions from bindings
- ALWAYS distinguish metadata readiness from real runtime readiness
- ALWAYS tell the user when a real MCP server is still missing

## Command reference

| Command | Purpose |
|---|---|
| `mova connectors list` | List connector definitions |
| `mova connectors get [connector_id]` | Inspect one connector definition |
| `mova connectors add ...` | Create a connector definition |
| `mova bindings list` | List current bindings |
| `mova bindings get [binding_id]` | Inspect one binding |
| `mova bindings history [binding_id]` | Read binding history |
| `mova bindings lineage [binding_id]` | Read binding lineage |
| `mova bindings create ...` | Create a binding object |
| `mova bindings attach [binding_id]` | Attach a binding |
| `mova bindings rebind [binding_id] ...` | Replace or update a binding |
| `mova bindings activate [binding_id]` | Activate a binding |
| `mova bindings enable-steady-state [binding_id]` | Enable steady-state mode |
| `mova bindings pause [binding_id]` | Pause a binding |
| `mova bindings disable [binding_id]` | Disable a binding |

*MOVA Connector — connect external runtime reality to your contract honestly. Created by Sergii Miasoiedov.*
