# Command Reference

Azalea Gate provides **42+ commands** across verification, moderation, and configuration. This reference covers every command available.

---

## General Commands

| Command | Description |
| :--- | :--- |
| `/help` | View all available commands and features. Categories: NFT, Token, Moderation, Configuration. |
| `/pricing` | View subscription plans and pricing tiers. |

---

## Verification Commands (User)

These commands are available to all users to link their wallets.

| Command | Description |
| :--- | :--- |
| `/verify` | Link your **Solana** wallet. Opens the verification portal at `https://azaleagate.com/verify`. |
| `/verify-evm` | Link your **Ethereum/Polygon/Base** wallet. Supports all EVM chains. |
| `/verify-ton` | Link your **TON** wallet for The Open Network verification. |
| `/unverify` | Unlink your wallet from this server. Removes all verification roles. |
| `/check-verification` | Quick check if your wallet is currently verified. |
| `/verification-status` | View detailed info about your linked wallet(s) and assigned roles. |

---

## NFT Verification Commands (Admin)

Configure which NFT collections grant roles. **Requires:** `Manage Roles` permission.

| Command | Description |
| :--- | :--- |
| `/nft-config add-collection` | Add an NFT collection. Holders get the specified role. |
| `/nft-config remove-collection` | Remove an NFT collection from verification. |
| `/nft-config list-collections` | List all configured collections and their roles. |
| `/nft-config set-check-interval` | Set how often (hours) to re-check NFT ownership. |
| `/nft-config enable` | Enable NFT verification feature. |
| `/nft-config disable` | Disable NFT verification feature. |
| `/nft-collections` | Alias for listing collections. |
| `/nft-force-check [user]` | Force re-check a specific user's NFT holdings. |
| `/nft-unlink [user]` | Admin command to unlink a user's wallet. |
| `/nft-stats` | View server-wide verification statistics. |
| `/nft-list-wallets` | List all verified wallets in the server. |
| `/nft-list-holders [collection]` | List Discord members holding a specific collection. |
| `/nft-check-all` | Force re-check ALL verified users. (Use sparingly) |
| `/setup-verification-panel [channel]` | Create a "Verify Here" button panel in a channel. |

---

## EVM Commands (Admin)

Configure Ethereum, Polygon, Base, Optimism, and Arbitrum NFT verification.

| Command | Description |
| :--- | :--- |
| `/evm-config add-collection` | Add an ERC-721 NFT contract. Specify chain (eth/polygon/base/optimism/arb). |
| `/evm-config remove-collection` | Remove an EVM collection. |
| `/evm-config list` | List all configured EVM collections. |

---

## Token Gating Commands

Gate roles by SPL token balance (e.g., $BONK, $USDC).

| Command | Description |
| :--- | :--- |
| `/token-check` | (User) Check your token holdings and update roles. |
| `/token-config add` | Add a token requirement. Specify mint address, minimum amount, and role. |
| `/token-config remove` | Remove a token requirement. |
| `/token-config list` | List all token requirements. |
| `/token-config set-interval` | Set how often (hours) to check balances. |
| `/token-config enable` | Enable token gating. |
| `/token-config disable` | Disable token gating. |
| `/token-config status` | View current token gating status. |

---

## Staking Commands

Gate roles by staking status in supported protocols.

| Command | Description |
| :--- | :--- |
| `/staking-check` | (User) Check your staking status and update roles. |
| `/staking-config add` | Add a staking requirement. Specify protocol, min stake, and role. |
| `/staking-config remove` | Remove a staking requirement. |
| `/staking-config list` | List all staking requirements. |

---

## Verification Rules (Advanced)

Create complex multi-condition rules for role assignment.

| Command | Description |
| :--- | :--- |
| `/verification-rules create` | Create a new verification rule with multiple conditions. |
| `/verification-rules delete` | Delete a verification rule. |
| `/verification-rules list` | List all verification rules. |
| `/rule-check` | Check which rules a user matches. |

---

## Allowlist Commands

Manage wallet allowlists for presales, whitelist spots, etc.

| Command | Description |
| :--- | :--- |
| `/allowlist create [name]` | Create a new named allowlist. |
| `/allowlist add [name] [wallet]` | Add a wallet to an allowlist. |
| `/allowlist remove [name] [wallet]` | Remove a wallet from an allowlist. |
| `/allowlist list [name]` | View all wallets in an allowlist. |
| `/allowlist export [name]` | Export allowlist to CSV format. |

---

## Moderation Commands

Auto-moderation and content filtering. **Requires:** `Administrator` permission.

### Content Filtering
| Command | Description |
| :--- | :--- |
| `/add-prohibited [type] [value]` | Add a prohibited word or domain. Type: `word` or `domain`. |
| `/remove-prohibited [type] [value]` | Remove from the prohibited list. |
| `/list-prohibited` | View all prohibited words and domains. |

### Link Protection
| Command | Description |
| :--- | :--- |
| `/add-link-channel [channel]` | Whitelist a channel where links are allowed. |
| `/remove-link-channel [channel]` | Remove channel from whitelist. |
| `/list-link-channels` | List all whitelisted channels. |

### Mention Protection
| Command | Description |
| :--- | :--- |
| `/config-mentions` | Enable/disable mention spam protection. |
| `/add-protected-role [role]` | Protect a role from mass-ping attacks. |
| `/remove-protected-role [role]` | Remove role protection. |
| `/list-protected-roles` | List all protected roles. |

### Muting
| Command | Description |
| :--- | :--- |
| `/config-muted-role [role]` | Set the role to assign when muting users. |
| `/exempt-role [role]` | Exempt a role from all moderation filters. |

---

## Social Feeds Commands

Forward Twitter/X and Telegram content to Discord.

| Command | Description |
| :--- | :--- |
| `/feed add twitter [handle] [channel]` | Forward tweets from a Twitter account to a channel. |
| `/feed add telegram [channel_url] [channel]` | Forward Telegram channel posts to Discord. |
| `/feed remove [feed_id]` | Remove a configured feed. |
| `/feed list` | List all active feeds. |

---

## Setup & Configuration

Initial bot setup. **Requires:** `Administrator` permission.

| Command | Description |
| :--- | :--- |
| `/setup` | Initial bot configuration. Set log channel, monitoring period, violation actions. |
| `/config` | View current server configuration. |

---

## Verification Portal

Users verify their wallets through the web portal:

**Portal URL:** `https://azaleagate.com/verify`

The portal supports:
- **Solana**: Phantom, Solflare, Glow, Backpack
- **EVM (Ethereum/Polygon/Base)**: MetaMask, Rainbow, Coinbase Wallet
- **TON**: Tonkeeper, TON Space

Users sign a message (no transaction, no gas) to prove ownership.
