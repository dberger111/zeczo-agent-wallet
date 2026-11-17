# ZECZO Agent Wallet

The tool an AI agent runs to use a [ZECZO](https://zeczo.com) Agent Wallet: a separate Zcash wallet your agent controls by itself.

It runs on your own computer. The wallet's 24 words stay there and are never sent to ZECZO.

**For technical users for now.** The Mac installer is signed; a signed Windows installer is coming.

## What your agent can do

| Tool | What it does |
|---|---|
| `balance` | What the Agent Wallet holds |
| `address` | Its own shielded address, to receive ZEC |
| `history` | Recent payments in and out |
| `pay` | Pay a ZECZO @name or a Zcash address, in ZEC or dollars, with an optional note |

Try: "What's in my Agent Wallet?" · "Pay @maria $5" · "Show my last payments"

## Set up

**Mac (easiest):** download [ZECZO-Agent-Wallet-mac.pkg](../../releases/latest/download/ZECZO-Agent-Wallet-mac.pkg) and open it. It is signed by UpLevel Labs, Inc. and notarized by Apple, and installs the tool at `/usr/local/bin/zeczo-agent-wallet`.

**Windows or Linux:** download the file for your computer from [Releases](../../releases/latest). On Linux, make it runnable with `chmod +x`. The Windows file is not signed yet, so Windows will warn before running it.

Then, on any system:

1. Save the key once. In ZECZO open Agent Wallet > Key for agent > copy, then run:
   ```
   zeczo-agent-wallet setup
   ```
   Paste the 24 words, press Enter, then Ctrl-D (Ctrl-Z then Enter on Windows).
2. Add it to your agent as a local MCP server. For Claude desktop or Claude Code:
   ```json
   { "mcpServers": { "zeczo-agent-wallet": { "command": "/usr/local/bin/zeczo-agent-wallet" } } }
   ```
   Use the full path of wherever the file is on Windows or Linux.
3. In ZECZO, add money to the Agent Wallet. Only what you'd risk.

## What to know

- **Payments are final.** Nobody can reverse them, including ZECZO.
- **Fee:** each payment adds the 1% ZECZO fee. No fee to add money or take it all back in ZECZO.
- **Ceilings:** 0.1 ZEC a payment and 0.5 ZEC a day by default. Change them in `~/.zeczo-agent-wallet/limits.json`:
  ```json
  { "per_payment_zec": 0.1, "per_day_zec": 0.5 }
  ```
- **Checks:** before paying, the tool asks zeczo.com whether the address is blocked and, for an @name, what the member's address is. If ZECZO can't be reached, nothing is sent.
- **The key has no limits of its own.** Anyone who gets the 24 words can spend everything in that wallet. Keep little in it.
- **Rewards:** off by default. Turn on "Share fees with my inviters" in ZECZO and the tool reports each payment (amount, fee, time, recipient address) so part of the fee goes to the members who invited you. Off, it reports nothing.
- **Stop anytime:** in ZECZO, "Take it all back" returns the balance to your main wallet.
- **Works with** agents that run on a computer and can add a local tool. It does not work with the Claude, ChatGPT, Gemini or Grok phone apps or websites; use an Agent vault in ZECZO for those.

## Files on your computer

`~/.zeczo-agent-wallet/` holds `key` (readable only by you), `wallet.db`, `limits.json` and `payments.log`.

## Source

`zeczo_agent_wallet.rs` is the tool's own source, published so you can read what it does. It is built on ZECZO's wallet engine, which is not public, so it cannot be compiled from this repository alone.

© UpLevel Labs, Inc. All rights reserved.
