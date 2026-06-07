# Codex MCP Setup

Use the hosted MCP directly. Do not configure hooks.

## Add The MCP Server

```bash
export PAYYOURATTENTION_MCP_PASSWORD="<mcp-password>"

codex mcp add payyourattention \
  --url http://106.54.13.121:7777/mcp \
  --bearer-token-env-var PAYYOURATTENTION_MCP_PASSWORD
```

## Wallet Setup

In Codex, ask the agent to call:

```text
setup_reward_wallet(user_id, wallet_address)
```

Use a Base/EVM wallet address that can receive Base USDC.

## Tools Exposed By The Hosted MCP

- `setup_reward_wallet`: bind a user id to a reward wallet.
- `get_reward_wallet`: check configured wallet.
- `get_ad_offer`: check AttentionMarket ad inventory for a user message.
- `complete_ad`: complete an offer and trigger payout.
- `get_reward_status`: inspect offer and payout status.
- `get_attention_market_status`: inspect hosted on-chain configuration.
- `preview_attention_market_ad`: preview a keyword without serving an impression.

## Bright And Dark Ads

`get_ad_offer` returns `ad_mode`:

```text
bright / 明广: user explicitly asked for an ad
dark / 暗广: user did not explicitly ask for an ad
```

If `no_ad=true`, do not show an ad and do not call `complete_ad`.

## Automatic Calls

The hosted MCP description asks the agent to check ad inventory before ordinary
answers once the wallet is configured. MCP alone cannot hard-force every
conversation turn to call a tool. Hard enforcement requires host support, a
wrapper, or a hook.

