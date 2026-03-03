# Revert and Signature Playbook

## Revert Message Triage

1. If user provides tx hash, fetch receipt first:
   - `cast receipt <tx_hash> --rpc-url <RPC_URL>`
2. If revert data is present, decode custom error:
   - `cast decode-error <error_data>`
3. If decode is unknown, try 4byte lookup:
   - `cast 4byte <selector>`
4. If calldata is available, decode intent:
   - `cast 4byte-calldata <calldata>`
   - Or `cast decode-calldata "<fn_sig>" <calldata>` when signature is known.

Explain both the likely semantic cause and the decoded technical cause.

## Function Signature Questions

- "What function is this selector?"
  - `cast 4byte <selector>`
- "What selector is this signature?"
  - `cast sig "<function_signature>"`
- "What event is this topic0?"
  - `cast 4byte-event <topic0>`

If multiple candidates appear, list candidates and provide one follow-up query to disambiguate.

## Calldata and ABI Decode Patterns

- Decode unknown calldata quickly:
  - `cast 4byte-calldata <calldata>`
- Decode with known ABI/function signature:
  - `cast decode-calldata "<fn_sig>" <calldata>`
- Decode ABI return payload:
  - `cast decode-abi "<fn_sig>" <hex_output>`
- Decode event data with known event signature:
  - `cast decode-event --sig "<event_sig>" <data>`

## Common Pitfalls

- Selector length must be 4 bytes (`0x` + 8 hex chars).
- Topic0 is 32 bytes (`0x` + 64 hex chars).
- Revert data can be empty for low-level failures.
- ERC20 failures may use custom errors, panic codes, or generic reverts depending on implementation.
