# Foundry Cast Agent Skill

`foundry-cast` is an Agent Skills-compatible skill that helps coding agents use Foundry `cast` safely and effectively for EVM investigation and contract interaction.

## Install

From this repository:

```bash
npx skills add <owner>/<repo> --skill foundry-cast
```

Or install all skills from the repo:

```bash
npx skills add <owner>/<repo>
```

## Skill Path

- `skills/foundry-cast/SKILL.md`

## Local Validation

```bash
# List discoverable skills in this repo
npx skills add . --list

# Install this skill for specific agents
npx skills add . --skill foundry-cast -a opencode -a codex -y --copy
```

## What It Covers

- Revert decoding and selector/signature/topic lookup
- Token metadata and ERC20 inspection
- Chain/RPC reads, logs, storage, and debug workflows
- Transaction safety gates for state-changing actions

## Repository Layout

```text
skills/
  foundry-cast/
    SKILL.md
    references/
      command-map.md
      revert-and-signatures.md
      token-introspection.md
      tx-safety-and-rpc.md
```
