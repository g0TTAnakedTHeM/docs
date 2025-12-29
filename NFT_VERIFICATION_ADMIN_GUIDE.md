# NFT Verification Admin Guide

## Overview

This guide is for server administrators who want to configure and manage the NFT verification system. You'll learn how to set up NFT collections, manage role assignments, monitor verification activity, and troubleshoot common issues.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Initial Setup](#initial-setup)
- [Configuring NFT Collections](#configuring-nft-collections)
- [Managing Verification Settings](#managing-verification-settings)
- [Monitoring and Statistics](#monitoring-and-statistics)
- [Admin Commands Reference](#admin-commands-reference)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Prerequisites

Before setting up NFT verification, ensure:

1. **Bot Permissions:** The bot needs the following permissions:
   - `MANAGE_ROLES` - To assign and remove NFT-based roles
   - `SEND_MESSAGES` - To respond to commands
   - `EMBED_LINKS` - To display rich embeds
   - `READ_MESSAGE_HISTORY` - For command interactions

2. **Role Hierarchy:** The bot's role must be **higher** than any NFT-based roles it manages. Discord's role hierarchy prevents bots from managing roles above their own position.

3. **RPC Configuration:** Ensure the bot has access to a Solana RPC endpoint (see RPC Setup Guide).

## Initial Setup

### Step 1: Enable NFT Verification

Enable the NFT verification feature for your server:

```
/nft-config enable
```

To disable it later:

```
/nft-config disable
```

### Step 2: Create Roles

Create the roles you want to assign based on NFT ownership:

1. Go to Server Settings → Roles
2. Create a new role (e.g., "NFT Holder", "Diamond Hands", etc.)
3. Configure role permissions and appearance
4. Ensure the bot's role is positioned **above** this role

### Step 3: Add NFT Collections

Link NFT collections to roles:

```
/nft-config add-collection <collection-address> <role>
```

Example:
```
/nft-config add-collection ABC123def456... @NFT Holder
```

**Finding Collection Addresses:**
- Use Solana explorers like Solscan or Solana Explorer
- Check the collection's official website or Discord
- Look at the "Collection" field in an NFT's metadata

### Step 4: Configure Check Interval

Set how often the bot checks NFT ownership (in hours):

```
/nft-config set-check-interval 24
```

Recommended intervals:
- **24 hours** - Standard (default)
- **12 hours** - More frequent updates
- **48 hours** - Less frequent (reduces RPC usage)

## Configuring NFT Collections

### Adding Collections

```
/nft-config add-collection <collection-address> <role>
```

**Parameters:**
- `collection-address` - The Solana address of the NFT collection
- `role` - The Discord role to assign (mention or ID)

**What happens:**
- The bot validates the collection address
- The collection is linked to the specified role
- Users who own NFTs from this collection will receive the role
- Existing verified users are checked immediately

### Removing Collections

```
/nft-config remove-collection <collection-address>
```

**What happens:**
- The collection-to-role mapping is removed
- Roles are **NOT** automatically removed from users
- Use `/nft-force-check @user` to update specific users
- The next periodic check will remove roles from all users

### Listing Collections

```
/nft-config list-collections
```

Displays:
- All configured collections
- Associated roles
- Collection names (if metadata is available)
- When each collection was added

## Managing Verification Settings

### Check Interval

Control how often the bot automatically verifies NFT ownership:

```
/nft-config set-check-interval <hours>
```

**Considerations:**
- Lower intervals = more up-to-date roles, but higher RPC usage
- Higher intervals = less RPC usage, but slower role updates
- Users can always trigger manual checks with `/check-verification`

### Enabling/Disabling

Temporarily disable NFT verification without losing configuration:

```
/nft-config disable
```

Re-enable when ready:

```
/nft-config enable
```

**Note:** Disabling does NOT remove existing roles or verification data.

## Monitoring and Statistics

### View Statistics

Get an overview of NFT verification activity:

```
/nft-stats
```

Shows:
- Total verified users
- Total roles assigned
- Roles per collection
- Recent verification activity
- Last periodic check time

### Force Check User

Manually trigger verification for a specific user:

```
/nft-force-check @user
```

Use cases:
- User reports role issues
- Testing after configuration changes
- Immediate role update after user purchases NFT

### View User Status

Check verification status for any user:

```
/verification-status @user
```

Shows:
- User's linked wallet
- Verification timestamp
- Last check timestamp
- Current NFT-based roles
- NFTs owned from each collection

### Admin Unlink

Remove a user's wallet link (admin override):

```
/nft-unlink @user
```

Use cases:
- User requests unlink but can't do it themselves
- Resolving verification issues
- Removing inactive verifications

**Warning:** This removes all NFT-based roles from the user.

## Admin Commands Reference

### Configuration Commands

| Command | Description | Permission Required |
|---------|-------------|---------------------|
| `/nft-config enable` | Enable NFT verification | MANAGE_ROLES |
| `/nft-config disable` | Disable NFT verification | MANAGE_ROLES |
| `/nft-config add-collection <address> <role>` | Add NFT collection | MANAGE_ROLES |
| `/nft-config remove-collection <address>` | Remove NFT collection | MANAGE_ROLES |
| `/nft-config list-collections` | List all collections | MANAGE_ROLES |
| `/nft-config set-check-interval <hours>` | Set check frequency | MANAGE_ROLES |

### Monitoring Commands

| Command | Description | Permission Required |
|---------|-------------|---------------------|
| `/nft-stats` | View verification statistics | MANAGE_ROLES |
| `/nft-force-check @user` | Force check for user | MANAGE_ROLES |
| `/verification-status @user` | View user's status | MANAGE_ROLES |
| `/nft-unlink @user` | Unlink user's wallet | MANAGE_ROLES |

## Best Practices

### Role Management

1. **Role Hierarchy:** Always keep the bot's role above NFT-based roles
2. **Role Names:** Use clear, descriptive names (e.g., "DeGods Holder" not just "Holder")
3. **Role Colors:** Use distinct colors to make NFT roles easily identifiable
4. **Permissions:** Be careful with permissions granted to NFT roles

### Collection Configuration

1. **Verify Addresses:** Always double-check collection addresses before adding
2. **Test First:** Test with a small collection or test role before full deployment
3. **Document Collections:** Keep a list of which collections grant which roles
4. **Regular Audits:** Periodically review configured collections

### Monitoring

1. **Check Logs:** Regularly review the log channel for verification events
2. **Monitor Stats:** Use `/nft-stats` to track adoption and usage
3. **User Feedback:** Encourage users to report issues
4. **RPC Health:** Monitor RPC endpoint performance and rate limits

### Communication

1. **Announce Feature:** Let users know NFT verification is available
2. **Provide Instructions:** Share the user guide with your community
3. **List Collections:** Regularly post which NFTs grant access
4. **Update Users:** Notify when adding/removing collections

### Security

1. **Protect Admin Commands:** Only give MANAGE_ROLES to trusted admins
2. **Monitor Activity:** Watch for unusual verification patterns
3. **Audit Logs:** Review admin actions in the log channel
4. **Backup Config:** Keep a backup of your collection configurations

## Troubleshooting

### Bot Can't Assign Roles

**Symptoms:** Bot confirms verification but roles aren't assigned

**Solutions:**
1. Check bot's role position in Server Settings → Roles
2. Ensure bot has MANAGE_ROLES permission
3. Verify the bot's role is above the NFT role
4. Check bot permissions in the specific channel

### Collection Address Invalid

**Symptoms:** Error when adding collection: "Invalid collection address"

**Solutions:**
1. Verify the address is correct (check on Solscan)
2. Ensure it's a collection address, not an individual NFT mint
3. Check that the collection exists on-chain
4. Try again in a few minutes (RPC might be slow)

### Users Not Receiving Roles

**Symptoms:** User verified but didn't receive expected roles

**Solutions:**
1. Verify user actually owns NFTs from configured collections
2. Use `/nft-force-check @user` to manually trigger check
3. Check if collection is properly configured with `/nft-config list-collections`
4. Verify the NFT is from the correct collection (check metadata)

### Periodic Checks Not Running

**Symptoms:** Roles not updating automatically

**Solutions:**
1. Check if NFT verification is enabled: `/nft-config list-collections`
2. Verify check interval is set: should show in config
3. Check bot logs for errors
4. Restart the bot if necessary

### RPC Errors

**Symptoms:** "Network error" or "RPC request failed" messages

**Solutions:**
1. Check RPC endpoint configuration (see RPC Setup Guide)
2. Verify API key is valid (if using Helius/QuickNode)
3. Check rate limits on your RPC provider
4. Configure fallback RPC endpoints
5. Wait a few minutes and try again

### Verification Taking Too Long

**Symptoms:** Verification or checks timing out

**Solutions:**
1. Check Solana network status (solana.com/status)
2. Verify RPC endpoint is responding
3. Reduce check interval to lower load
4. Consider upgrading RPC plan for higher rate limits

### Duplicate Wallet Error

**Symptoms:** "Wallet already linked to another account"

**Solutions:**
1. Find which account has the wallet: search verification logs
2. Use `/nft-unlink @user` on the old account
3. Have user verify again with the new account
4. This is a security feature to prevent abuse

### Missing Permissions

**Symptoms:** "I don't have permission to manage roles"

**Solutions:**
1. Grant bot MANAGE_ROLES permission
2. Check role hierarchy (bot role must be higher)
3. Verify bot has permission in the specific channel
4. Check server-wide permission overrides

## Logging and Audit Trail

The bot logs all verification events to your configured log channel:

**Logged Events:**
- User verifications (wallet linked)
- Role assignments and removals
- Configuration changes (collections added/removed)
- Admin actions (force checks, unlinks)
- Errors and failures
- Periodic check summaries

**Log Format:**
```
[NFT Verification] User @username verified wallet ABC...XYZ
[NFT Verification] Added role @NFT Holder to @username (owns 3 NFTs from Collection)
[NFT Verification] Admin @admin added collection ABC...XYZ → @Role
[NFT Verification] Periodic check complete: 45 users checked, 3 roles added, 1 removed
```

## Performance Optimization

### For Large Servers

If you have many verified users (100+):

1. **Increase Check Interval:** Use 48 hours instead of 24
2. **Stagger Checks:** The bot automatically staggers checks to avoid rate limits
3. **Upgrade RPC:** Consider a paid RPC plan with higher rate limits
4. **Monitor Usage:** Use `/nft-stats` to track RPC usage patterns

### RPC Rate Limits

**Free Tier Limits (Helius):**
- 100 requests/second
- Sufficient for most servers

**If hitting limits:**
1. Increase check interval
2. Reduce number of collections
3. Upgrade to paid RPC plan
4. Configure multiple RPC endpoints for load balancing

## Support and Resources

- **User Guide:** Share with your community
- **RPC Setup Guide:** For configuring Solana RPC endpoints
- **Troubleshooting Guide:** Detailed solutions for common issues
- **Bot Logs:** Check your configured log channel
- **Solana Status:** https://status.solana.com

---

**Need Help?** Check the troubleshooting section or consult the bot's error messages for specific guidance.
