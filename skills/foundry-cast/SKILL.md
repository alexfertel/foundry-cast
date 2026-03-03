---
name: foundry-cast
description: Use Foundry Cast effectively for EVM investigation and contract interaction. Trigger when users ask to decode revert messages, identify function signatures/selectors/topics, inspect token metadata like decimals/symbol/name, query chain state, inspect logs or storage, build cast commands, or troubleshoot RPC/ABI issues. Also use for transaction preparation with strict safety checks and explicit confirmation before any signing or broadcast.
---

# Foundry Cast

Use this skill to convert user intent into safe, correct `cast` commands and to interpret command output quickly.

## Workflow

1. Classify the request as one of: decode/signature work, token inspection, chain/RPC query, logs/storage/debug, or transaction sending.
2. Choose the smallest command set that answers the question.
3. Resolve RPC requirements with this policy:
   - If no RPC is needed, run directly.
   - If RPC is needed for read-only work, use this order: explicit `--rpc-url` -> `ETH_RPC_URL`/`RPC_URL` env -> foundry config endpoint -> ask for RPC/network.
   - If action is state-changing, require explicit RPC and run preflight checks before confirmation.
4. Prefer read-only verification commands before risky actions.
5. Explain results in plain language and include the exact command(s) used.

## Safety Rules

- Treat `cast send`, state-changing ERC20 actions, and any signed/broadcast transaction as dangerous.
- Before signing/sending, print a preflight summary with: `chain-id`, `rpc-url` source, `from`, `to`, `value`, `function`, `args`, gas settings, nonce mode.
- Ask for explicit confirmation immediately before execution.
- If confirmation is missing or ambiguous, do not sign/broadcast.
- When possible, run a read-only check first (`cast estimate`, `cast call`, or both).

## Ask Pattern For Dangerous Actions

Use this exact structure before state-changing execution:

```
Preflight summary:
- Chain ID: <id>
- RPC URL: <url or source>
- From: <address>
- To: <address>
- Value: <value>
- Action: <function/signature + args>
- Gas: <gas params>
- Nonce: <explicit or auto>

Reply "confirm" to proceed with signing/sending this transaction.
```

## Which Reference To Load

- For command selection and quick templates, read `references/command-map.md`.
- For revert decoding and selector/topic/signature identification, read `references/revert-and-signatures.md`.
- For token checks like decimals/symbol/name/supply/balance/allowance, read `references/token-introspection.md`.
- For transaction safety gates and RPC decision logic, read `references/tx-safety-and-rpc.md`.

## Output Style

- Provide both a short interpretation and the concrete `cast` command.
- When output is ambiguous, provide one follow-up command to disambiguate.
- Keep explanations concise; prioritize actionable next commands.
