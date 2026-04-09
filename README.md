# MOVA Skills

MOVA Skills teach an AI agent to turn raw business intentions into executable MOVA contract packages.

```bash
/plugin marketplace add mova-compact/mova-skills
```

## Current Skill

- `mova-forge` — full contract authoring flow: intent crystallization, step classification, and canonical package generation

## What MOVA Forge Does

`mova-forge` is the first public skill in this repository.

It helps the agent:

- crystallize a vague business task into an explicit intent
- classify every step as `DETERMINISTIC`, `AI_ATOMIC`, `EXTERNAL_CALL`, or `HUMAN_GATE`
- shape the contract structure and runtime bindings
- generate a canonical MOVA contract package

This skill is a prompt/instruction layer. It does not execute the contract itself.

## Example

```text
You: "I want to automate invoice processing with OCR, duplicate check, and human approval"
→ MOVA Forge: clarifies scope and constraints
→ MOVA Forge: classifies the workflow step by step
→ MOVA Forge: produces a canonical contract package
→ Execute on MOVA Platform: via MOVA State API and MOVA Tool SDK
```

## Why Contracts, Not Prompts

A prompt is a wish. A contract is an obligation. MOVA contracts define the steps, data shapes, approval gates, and verification logic ahead of execution. The model follows the contract instead of improvising the workflow.

## Part Of The MOVA Ecosystem

- MOVA Spec: `https://github.com/mova-compact/mova-spec`
- MOVA Tool SDK: `https://github.com/mova-compact/mova-tool-sdk`
- MOVA MCP Connector: `https://github.com/mova-compact/mova-mcp-connector`
- MOVA State API: `https://mova-lab.eu`

## Status

This repository starts with `mova-forge` as the first published skill. Additional MOVA skills can be added here later without changing the marketplace structure.

Created and maintained by Sergii Miasoiedov.

MOVA: Machine-Operable Verbal Actions.
