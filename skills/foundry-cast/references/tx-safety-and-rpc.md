# Transaction Safety and RPC Policy

## RPC Resolution Policy

Apply this order for read-only requests:

1. Explicit `--rpc-url` from the user request.
2. `ETH_RPC_URL` or `RPC_URL` environment variable.
3. Foundry config endpoint.
4. Ask the user for RPC URL or target network.

For state-changing requests, require explicit RPC URL and show it in preflight.

## Classification

- Read-only: `cast call`, `cast block`, `cast balance`, `cast logs`, `cast storage`, decode helpers.
- Dangerous: `cast send`, `cast publish-tx`, ERC20 transfer/approve/mint/burn, any private-key/hardware-wallet signing flow.

## Required Preflight Before Dangerous Actions

1. Resolve and display chain ID:
   - `cast chain-id --rpc-url <RPC_URL>`
2. Resolve and display sender address from signer configuration.
3. Display target address, function signature, args, and value.
4. If possible, run estimation/simulation first:
   - `cast estimate ... --rpc-url <RPC_URL>`
   - Optional: read-only `cast call ... --rpc-url <RPC_URL>` for expected return behavior.
5. Display gas strategy and nonce strategy.

Then ask for explicit confirmation.

## Confirmation Gate

Use this exact prompt:

```
This action can sign and broadcast a state-changing transaction.
Reply "confirm" to proceed.
```

If the user does not reply exactly with clear confirmation intent, do not execute the send.

## Post-Send Follow-up

- Return tx hash immediately.
- If user expects completion, fetch receipt:
  - `cast receipt <tx_hash> --rpc-url <RPC_URL>`
- If reverted, decode revert data and explain likely cause.

## Dangerous Defaults To Avoid

- Do not pick an arbitrary RPC for sending.
- Do not default to mainnet when network is unclear.
- Do not execute a send command based on implicit consent.
