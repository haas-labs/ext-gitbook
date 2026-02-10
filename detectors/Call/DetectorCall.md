# Contract Call

Execute calls to a contract function (not a transaction), extract results, and check conditions.

## Functionality

- Calls a contract function on a schedule or per block.
- Extracts results and runs optional scripts/conditions.
- Emits alerts based on script output or errors.

## Configuration

![](../../.gitbook/assets/DetectorCall-Config.png)

NOTE: Screenshot should be max size 800px and clickable to expand to full size

- **Function**: Function to execute. It must be a full signature in the [foundry cast](https://getfoundry.sh/cast/reference/cast) format (e.g. `balanceOf(address)(uint256)`).
- **Parameters**: Parameters to pass to the function. Parameters can be separated with spaces or new lines. Tuples must be enclosed in `()`, arrays in `[]`, and strings in `""`. Refer to the cast documentation for syntax.
- **Script**: Script flow to execute on call result.
- **When**: When to execute script: `block` (every block) or `cron` (at cron intervals).
- **Cron**: Cron interval to execute.
- **Description**: Description pattern. Metadata fields from Alert can be used as substitutions and must be enclosed in `{}` (e.g. `{func}: {result}`).
- **Track Error**: Generate alerts on errors (in Contract Call or evaluation script).
- **Always Error**: Alert on every error. If unchecked, only new unique errors will be alerted and duplicates omitted.
- **Severity**: Alert severity level.

**Supported networks**: Ethereum, Ethereum Sepolia.
