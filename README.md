# payyourattention

User-facing setup for the hosted `payyourattention` MCP.

This repository does not contain the payout server source code. The hosted MCP
server is operated separately. Users only need to add the MCP endpoint to Codex
and register a Base/EVM wallet for receiving Base USDC rewards.

## Add To Codex

Set the MCP password in the environment that launches Codex:

```bash
export PAYYOURATTENTION_MCP_PASSWORD="<mcp-password>"
```

Add the hosted streamable HTTP MCP server:

```bash
codex mcp add payyourattention \
  --url http://106.54.13.121:7777/mcp \
  --bearer-token-env-var PAYYOURATTENTION_MCP_PASSWORD
```

This creates a Codex config entry equivalent to:

```toml
[mcp_servers.payyourattention]
url = "http://106.54.13.121:7777/mcp"
bearer_token_env_var = "PAYYOURATTENTION_MCP_PASSWORD"
```

## First Use

Ask the agent to register your reward wallet:

```text
Call setup_reward_wallet with user_id="local-user" and wallet_address="0xYourBaseWallet".
```

Use a Base/EVM wallet that can receive Base USDC.

## Runtime Behavior

When available, the agent should call:

```text
get_ad_offer(user_message, user_id)
```

If the response contains:

```json
{ "no_ad": true }
```

no ad is shown and no reward is payable.

If an offer is returned, it includes `ad_mode`:

```text
bright / 明广: the user explicitly asked for an ad
dark / 暗广: the user did not explicitly ask for an ad
```

After completion, the agent can call:

```text
complete_ad(offer_id, seconds_watched)
```

The hosted server pays Base USDC to the registered wallet.

## Notes

Direct MCP configuration exposes tools and instructions. It does not
protocol-force Codex to call the MCP before every conversation turn. A mandatory
pre-turn call requires host support, a wrapper, or a hook.

