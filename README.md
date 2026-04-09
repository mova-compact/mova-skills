# MOVA Skills

MOVA Skills teach an AI agent to understand MOVA, turn business intentions into contract packages, prepare integrations, and operate the current MOVA runtime.

```bash
/plugin marketplace add mova-compact/mova-skills
```

## Included Skills

- `mova-language` — teaches the agent the MOVA language and ecosystem concepts
- `mova-forge` — full contract authoring flow: intent crystallization, step classification, and canonical package generation
- `mova-runner` — executes contracts through the real current MOVA Tool SDK and State API surface
- `mova-connector` — prepares connector and binding setup for external MCP-backed systems

## What You Get

This repository now covers the core public MOVA skill set:

- understand the language and conceptual model
- author canonical contract packages
- prepare connector and binding posture honestly
- run contracts through the live SDK/API contour

`mova-forge` remains the authoring anchor. `mova-runner` and `mova-connector` were aligned to the current public implementation so they do not promise commands or flows that do not yet exist.

## Example

```text
You: "I want to automate invoice processing with OCR, duplicate check, and human approval"
→ MOVA Forge: clarifies scope and constraints
→ MOVA Forge: classifies the workflow step by step
→ MOVA Forge: produces a canonical contract package
→ MOVA Connector: prepares external runtime connectivity if needed
→ MOVA Runner: executes through MOVA State API and MOVA Tool SDK
```

## Why Contracts, Not Prompts

A prompt is a wish. A contract is an obligation. MOVA contracts define the steps, data shapes, approval gates, and verification logic ahead of execution. The model follows the contract instead of improvising the workflow.

## Part Of The MOVA Ecosystem

- MOVA Spec: `https://github.com/mova-compact/mova-spec`
- MOVA Tool SDK: `https://github.com/mova-compact/mova-tool-sdk`
- MOVA MCP Connector: `https://github.com/mova-compact/mova-mcp-connector`
- MOVA State API: `https://mova-lab.eu`

## Status

This repository now publishes the first public MOVA skill pack around the real current platform:

- language
- forge
- runner
- connector

The skills are intentionally aligned with the current implementation, not with future product promises.

Created and maintained by Sergii Miasoiedov.

MOVA: Machine-Operable Verbal Actions.
