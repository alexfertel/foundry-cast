# Foundry Cast Command Map

Use this map to translate user intent into concrete commands.

## No-RPC Commands

- Function selector from signature:
  - `cast sig "transfer(address,uint256)"`
- Keccak hash:
  - `cast keccak "hello"`
- ABI encode calldata:
  - `cast calldata "transfer(address,uint256)" <to> <amount>`
- Decode calldata with known signature:
  - `cast decode-calldata "transfer(address,uint256)" <calldata>`

## RPC Read-Only Commands

- Current block number:
  - `cast block-number --rpc-url <RPC_URL>`
- Wallet balance:
  - `cast balance <address_or_ens> --rpc-url <RPC_URL>`
  - Ether format: add `--ether`
- Contract code and size:
  - `cast code <address> --rpc-url <RPC_URL>`
  - `cast codesize <address> --rpc-url <RPC_URL>`
- Contract call:
  - `cast call <contract> "<fn_sig>" <args...> --rpc-url <RPC_URL>`

## Token Quick Checks

- Decimals:
  - `cast call <token> "decimals()(uint8)" --rpc-url <RPC_URL>`
- Symbol:
  - `cast call <token> "symbol()(string)" --rpc-url <RPC_URL>`
- Name:
  - `cast call <token> "name()(string)" --rpc-url <RPC_URL>`
- Total supply:
  - `cast call <token> "totalSupply()(uint256)" --rpc-url <RPC_URL>`

Prefer the `cast erc20` namespace when available and clearer.

## Event/Log Inspection

- Query logs by event signature and address:
  - `cast logs --address <contract> "Transfer(address,address,uint256)" --rpc-url <RPC_URL>`
- Decode event payload with known signature:
  - `cast decode-event --sig "Transfer(address,address,uint256)" <data>`

## Storage and Proofs

- Read raw slot:
  - `cast storage <address> <slot> --rpc-url <RPC_URL>`
- Generate state proof:
  - `cast proof <address> <slot1> <slot2> --rpc-url <RPC_URL>`

## Debug and Replay

- Trace existing transaction in a local replay environment:
  - `cast run <tx_hash> --rpc-url <RPC_URL>`
- Read receipt:
  - `cast receipt <tx_hash> --rpc-url <RPC_URL>`

## State-Changing Commands (Confirmation Required)

- Send transaction:
  - `cast send <to> "<fn_sig>" <args...> --rpc-url <RPC_URL> --private-key <KEY>`
- ERC20 transfer:
  - `cast erc20 transfer <token> <to> <amount> --rpc-url <RPC_URL> --private-key <KEY>`
- ERC20 approve:
  - `cast erc20 approve <token> <spender> <amount> --rpc-url <RPC_URL> --private-key <KEY>`

Always run preflight + explicit confirmation from the user immediately before execution.
