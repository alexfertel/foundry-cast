# Token Introspection

Use this guide for token metadata and basic ERC20 checks.

## Core Metadata

- Decimals:
  - `cast call <token> "decimals()(uint8)" --rpc-url <RPC_URL>`
- Symbol:
  - `cast call <token> "symbol()(string)" --rpc-url <RPC_URL>`
- Name:
  - `cast call <token> "name()(string)" --rpc-url <RPC_URL>`
- Total supply:
  - `cast call <token> "totalSupply()(uint256)" --rpc-url <RPC_URL>`

If a call fails, verify the address has bytecode:

- `cast code <token> --rpc-url <RPC_URL>`

## Balance and Allowance

- Balance of holder:
  - `cast call <token> "balanceOf(address)(uint256)" <holder> --rpc-url <RPC_URL>`
- Allowance:
  - `cast call <token> "allowance(address,address)(uint256)" <owner> <spender> --rpc-url <RPC_URL>`

Or use dedicated subcommands:

- `cast erc20 balance <token> <holder> --rpc-url <RPC_URL>`
- `cast erc20 allowance <token> <owner> <spender> --rpc-url <RPC_URL>`

## Human-Readable Amounts

After fetching raw integer values:

- Convert smallest unit to decimal:
  - `cast format-units <value> <decimals>`
- Convert decimal user value to smallest unit:
  - `cast parse-units <value> <decimals>`

Always mention both raw units and normalized value when answering user questions.

## Sanity Checks

- If `decimals` is missing or non-standard, report uncertainty and avoid assumptions.
- If symbol/name revert, treat token as non-standard ERC20 and continue with available fields.
- If chain is ambiguous, fetch and report `cast chain-id --rpc-url <RPC_URL>` before interpreting values.
