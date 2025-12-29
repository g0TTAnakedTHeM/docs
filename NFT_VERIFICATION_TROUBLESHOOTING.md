# NFT Verification Troubleshooting Guide

## Overview

This comprehensive troubleshooting guide covers common issues with the NFT verification system, their causes, and step-by-step solutions. Use this guide to diagnose and resolve problems quickly.

## Table of Contents

- [Quick Diagnostics](#quick-diagnostics)
- [User Issues](#user-issues)
- [Admin Issues](#admin-issues)
- [Technical Issues](#technical-issues)
- [Performance Issues](#performance-issues)
- [Security Issues](#security-issues)
- [Error Messages Reference](#error-messages-reference)

## Quick Diagnostics

### Is the bot working at all?

1. Can the bot respond to basic commands? Try `/ping` or `/help`
2. Is the bot online in the member list?
3. Check bot logs for startup errors

### Is NFT verification enabled?

```
/nft-config list-collections
```

If you get "NFT verification is disabled", run:
```
/nft-config enable
```

### Are collections configured?

```
/nft-config list-collections
```

Should show at least one collection. If empty, add collections:
```
/nft-config add-collection <address> <role>
```

### Is the RPC endpoint working?

Check bot logs for RPC errors. Test your endpoint:
```bash
curl -X POST YOUR_RPC_ENDPOINT \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"getHealth"}'
```

## User Issues

### Issue: Can't Receive DM from Bot

**Symptoms:**
- User runs `/verify` but doesn't receive a DM
- Bot says "I couldn't send you a DM"

**Causes:**
- User has DMs disabled from server members
- User has blocked the bot
- User's DMs are full

**Solutions:**

1. **Enable DMs from server:**
   - Right-click the server icon
   - Select "Privacy Settings"
   - Enable "Direct Messages"
   - Try `/verify` again

2. **Check if bot is blocked:**
   - Go to User Settings → Privacy & Safety
   - Check "Blocked Users" list
   - Unblock the bot if present

3. **Clear DM space:**
   - Close some DM conversations
   - Try again

**Alternative:** Some bots support in-channel verification. Check with your admin.

---

### Issue: Signature Verification Failed

**Symptoms:**
- User submits signature but gets "Invalid signature" error
- Verification fails after signing message

**Causes:**
- Wrong message signed
- Signature copied incorrectly
- Wrong wallet used
- Expired verification message (> 10 minutes old)

**Solutions:**

1. **Start fresh:**
   - Run `/verify` again to get a new message
   - Copy the ENTIRE message exactly as shown
   - Don't modify or add anything

2. **Sign correctly:**
   - Use the "Sign Message" feature in your wallet
   - Don't use "Sign Transaction"
   - Paste the exact message from the bot

3. **Copy signature carefully:**
   - Copy the entire signature string
   - Don't include extra spaces or line breaks
   - Use Ctrl+C / Cmd+C to copy

4. **Use correct wallet:**
   - Make sure you're signing with the wallet you want to link
   - Check wallet address matches what you're submitting

5. **Don't wait too long:**
   - Sign and submit within 10 minutes
   - If expired, run `/verify` again

**Step-by-step verification:**
```
1. Run /verify
2. Copy ENTIRE message from DM (including "Please sign this message...")
3. Open wallet → Sign Message (NOT Sign Transaction)
4. Paste message → Sign
5. Copy signature from wallet
6. Submit to bot within 10 minutes
```

---

### Issue: Wallet Already Linked Error

**Symptoms:**
- Error: "This wallet is already linked to another Discord account"
- Can't verify with desired wallet

**Causes:**
- Wallet is linked to a different Discord account
- User previously verified with an alt account
- Someone else is using the same wallet

**Solutions:**

1. **If it's your wallet on another account:**
   - Log into the other Discord account
   - Run `/unverify` to unlink
   - Return to current account and verify

2. **If you don't have access to the other account:**
   - Contact a server administrator
   - Admin can use `/nft-unlink @user` to remove the link
   - Provide proof of wallet ownership if requested

3. **If someone else is using your wallet:**
   - This is a security issue
   - Contact server administrators immediately
   - Change your wallet if compromised

**Prevention:** Only verify with wallets you personally control.

---

### Issue: Verified But No Roles Received

**Symptoms:**
- Verification successful
- No roles assigned
- Message says "You don't own any configured NFTs"

**Causes:**
- User doesn't own NFTs from configured collections
- NFTs are in a different wallet
- Collection not configured by admins
- NFT metadata doesn't match collection address

**Solutions:**

1. **Check which collections grant roles:**
   ```
   /nft-collections
   ```
   This shows which NFT collections are configured

2. **Verify you own NFTs from those collections:**
   - Check your wallet on Solscan or Solana Explorer
   - Look at the "Collection" field in NFT metadata
   - Ensure collection address matches

3. **Check correct wallet:**
   - Verify you linked the wallet that contains the NFTs
   - Check verification status:
     ```
     /verification-status
     ```

4. **Wait for metadata to update:**
   - If you just minted/bought the NFT, wait 5-10 minutes
   - Run manual check:
     ```
     /check-verification
     ```

5. **Contact admin:**
   - Ask if your collection is configured
   - Provide collection address
   - Admin can add it with `/nft-config add-collection`

---

### Issue: Roles Not Updating After NFT Purchase

**Symptoms:**
- Bought new NFT
- Still don't have the role
- Verification status shows old data

**Causes:**
- Periodic check hasn't run yet
- NFT metadata not updated on-chain
- Collection not configured

**Solutions:**

1. **Trigger manual check:**
   ```
   /check-verification
   ```
   This immediately checks your current NFT holdings

2. **Wait for metadata:**
   - If you just bought/minted, wait 5-10 minutes
   - Solana needs time to update metadata
   - Try manual check again

3. **Verify collection is configured:**
   ```
   /nft-collections
   ```
   Ensure your NFT's collection is listed

4. **Check wallet:**
   - Verify NFT is in the linked wallet
   - Check on Solscan: https://solscan.io
   - Confirm collection address matches

---

### Issue: Roles Removed After Selling NFT

**Symptoms:**
- Sold/transferred NFT
- Role was removed
- Want to keep role

**Cause:**
- This is expected behavior
- Bot detected you no longer own the NFT

**Solutions:**

1. **This is working as intended:**
   - NFT-based roles require ongoing ownership
   - Roles are automatically removed when NFTs are sold

2. **If you still own the NFT:**
   - Check if it's in the correct wallet
   - Run `/check-verification` to update
   - Verify on Solscan that you still own it

3. **If you want permanent roles:**
   - Ask server admins about alternative role systems
   - Some servers offer "OG" roles that don't require ongoing ownership

---

### Issue: Can't Unverify

**Symptoms:**
- `/unverify` command doesn't work
- Error when trying to unlink wallet

**Causes:**
- Bot permission issues
- Database error
- Command not responding

**Solutions:**

1. **Try again:**
   - Wait a few seconds
   - Run `/unverify` again

2. **Check bot status:**
   - Is bot online?
   - Can it respond to other commands?

3. **Contact admin:**
   - Admin can force unlink with `/nft-unlink @you`

4. **Check logs:**
   - Ask admin to check bot logs for errors

## Admin Issues

### Issue: Can't Add Collection

**Symptoms:**
- `/nft-config add-collection` fails
- Error: "Invalid collection address"
- Error: "Collection not found"

**Causes:**
- Invalid collection address
- Typo in address
- Collection doesn't exist on-chain
- RPC endpoint issues

**Solutions:**

1. **Verify collection address:**
   - Check on Solscan: https://solscan.io
   - Look for "Collection" or "Verified Collection" field
   - Copy the exact address

2. **Common mistakes:**
   - Using NFT mint address instead of collection address
   - Using devnet address on mainnet
   - Extra spaces or characters

3. **Test the address:**
   - Look it up on Solscan
   - Verify it's a valid Solana address
   - Check it's on mainnet (not devnet)

4. **Check RPC:**
   - Verify RPC endpoint is working
   - Check bot logs for RPC errors
   - Try again in a few minutes

5. **Format check:**
   ```
   /nft-config add-collection ABC123def456ghi789... @Role Name
   ```
   - Address should be 32-44 characters
   - Role should be mentioned with @

---

### Issue: Bot Can't Assign Roles

**Symptoms:**
- Verification succeeds
- Bot says roles assigned
- But user doesn't have the role

**Causes:**
- Bot missing MANAGE_ROLES permission
- Bot's role is below the NFT role in hierarchy
- Role was deleted
- Discord API issues

**Solutions:**

1. **Check bot permissions:**
   - Server Settings → Roles
   - Find bot's role
   - Ensure "Manage Roles" is enabled

2. **Check role hierarchy:**
   - Server Settings → Roles
   - Drag bot's role ABOVE all NFT roles
   - Bot can only manage roles below it

3. **Verify role exists:**
   - Check if the role still exists
   - If deleted, create it again
   - Update collection config if needed

4. **Check channel permissions:**
   - Bot needs permissions in the channel
   - Check channel-specific overrides

5. **Test with admin:**
   - Try manually assigning the role
   - If you can't, it's a Discord permission issue

**Correct hierarchy:**
```
@Admin
@Bot Role          ← Bot's role
@NFT Holder        ← NFT roles (bot can manage these)
@Verified
@everyone
```

---

### Issue: Periodic Checks Not Running

**Symptoms:**
- Roles not updating automatically
- Last check time not changing
- Users report outdated roles

**Causes:**
- Bot restarted and scheduler not started
- Check interval set too high
- Scheduler crashed
- Bot offline during check time

**Solutions:**

1. **Verify check interval:**
   ```
   /nft-config list-collections
   ```
   Should show check interval (e.g., "24 hours")

2. **Check bot uptime:**
   - Verify bot has been online continuously
   - Scheduler resets on bot restart

3. **Restart bot:**
   - Restart the bot process
   - Scheduler will start fresh

4. **Check logs:**
   - Look for "Periodic verification complete" messages
   - Check for scheduler errors

5. **Force manual check:**
   ```
   /nft-force-check @user
   ```
   Test if verification works manually

6. **Verify configuration:**
   - Ensure NFT verification is enabled
   - Check collections are configured
   - Verify RPC endpoint is working

---

### Issue: Can't Remove Collection

**Symptoms:**
- `/nft-config remove-collection` doesn't work
- Collection still appears in list
- Error when removing

**Causes:**
- Wrong collection address
- Database error
- Permission issues

**Solutions:**

1. **Get exact address:**
   ```
   /nft-config list-collections
   ```
   Copy the address exactly as shown

2. **Use correct command:**
   ```
   /nft-config remove-collection ABC123def456...
   ```
   Don't include the role

3. **Check permissions:**
   - Ensure you have MANAGE_ROLES permission
   - Try with a different admin

4. **Manual cleanup:**
   - If persistent, may need to edit config file
   - Contact bot developer for assistance

---

### Issue: Stats Not Showing Correctly

**Symptoms:**
- `/nft-stats` shows wrong numbers
- Counts don't match reality
- Missing data

**Causes:**
- Cache not updated
- Database inconsistency
- Recent configuration changes

**Solutions:**

1. **Wait for next periodic check:**
   - Stats update after each periodic verification
   - Check interval determines update frequency

2. **Force checks:**
   - Run `/nft-force-check` on several users
   - Stats will update after checks

3. **Restart bot:**
   - Restart to clear caches
   - Stats will recalculate

4. **Verify data:**
   - Manually count verified users
   - Check if numbers make sense
   - Report if significantly off

## Technical Issues

### Issue: RPC Endpoint Errors

**Symptoms:**
- "RPC request failed" errors
- "Network error" messages
- Timeouts during verification

**Causes:**
- Invalid API key
- Rate limit exceeded
- RPC endpoint down
- Network connectivity issues
- Firewall blocking requests

**Solutions:**

1. **Verify API key:**
   - Check `.env` file
   - Ensure API key is correct
   - No extra spaces or quotes

2. **Test endpoint:**
   ```bash
   curl -X POST YOUR_RPC_ENDPOINT \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","id":1,"method":"getHealth"}'
   ```
   Should return: `{"jsonrpc":"2.0","result":"ok","id":1}`

3. **Check rate limits:**
   - Log into RPC provider dashboard
   - Check current usage
   - Upgrade plan if needed

4. **Configure fallback:**
   ```env
   SOLANA_RPC_FALLBACK=https://backup-endpoint.com
   ```

5. **Check provider status:**
   - Helius: https://status.helius.dev
   - QuickNode: https://status.quicknode.com
   - Solana: https://status.solana.com

6. **Firewall check:**
   - Ensure outbound HTTPS (port 443) is allowed
   - Test from server command line

---

### Issue: Database/Storage Errors

**Symptoms:**
- "Failed to save verification" errors
- Configuration not persisting
- Data loss after restart

**Causes:**
- File permission issues
- Disk full
- Corrupted config file
- Concurrent write conflicts

**Solutions:**

1. **Check file permissions:**
   ```bash
   ls -la config/servers/
   ```
   Bot needs read/write access

2. **Check disk space:**
   ```bash
   df -h
   ```
   Ensure sufficient space available

3. **Backup and repair:**
   ```bash
   # Backup config
   cp config/servers/SERVER_ID.json config/servers/SERVER_ID.json.backup
   
   # Check if valid JSON
   cat config/servers/SERVER_ID.json | jq .
   ```

4. **Fix permissions:**
   ```bash
   chmod 644 config/servers/*.json
   chown bot-user:bot-group config/servers/*.json
   ```

5. **Restore from backup:**
   - If config corrupted, restore from backup
   - Reconfigure collections if needed

---

### Issue: Memory/Performance Issues

**Symptoms:**
- Bot slow to respond
- High memory usage
- Crashes during verification
- Timeouts

**Causes:**
- Too many verified users
- Memory leak
- Insufficient resources
- Large NFT queries

**Solutions:**

1. **Check resource usage:**
   ```bash
   top
   # or
   htop
   ```
   Look at bot's CPU and memory usage

2. **Increase check interval:**
   ```
   /nft-config set-check-interval 48
   ```
   Reduces load on bot and RPC

3. **Restart bot:**
   - Clears memory leaks
   - Resets connections

4. **Upgrade server:**
   - More RAM if consistently high usage
   - Faster CPU for large servers

5. **Optimize configuration:**
   - Reduce number of collections
   - Use better RPC endpoint
   - Enable caching

6. **Check for loops:**
   - Review bot logs
   - Look for repeated errors
   - May indicate bug

---

### Issue: Command Registration Failed

**Symptoms:**
- Commands don't appear in Discord
- Slash commands not working
- "Unknown command" errors

**Causes:**
- Commands not registered with Discord
- Bot missing APPLICATION_COMMANDS permission
- Discord API issues
- Bot in too many servers (100+ limit for global commands)

**Solutions:**

1. **Re-register commands:**
   ```bash
   npm run register-commands
   # or
   node dist/register-commands.js
   ```

2. **Check bot permissions:**
   - Bot needs `applications.commands` scope
   - Re-invite bot with correct permissions
   - Use Discord's permission calculator

3. **Wait for propagation:**
   - Commands can take up to 1 hour to appear
   - Try in a different server/channel

4. **Check bot token:**
   - Ensure DISCORD_TOKEN is correct in `.env`
   - Token must have proper scopes

5. **Guild vs Global:**
   - Register as guild commands for instant updates
   - Global commands take longer to propagate

---

### Issue: Signature Verification Failing (Technical)

**Symptoms:**
- All signature verifications fail
- "Invalid signature" for valid signatures
- Technical error in logs

**Causes:**
- Incorrect signature verification implementation
- Wrong message format
- Encoding issues
- Library version mismatch

**Solutions:**

1. **Check message format:**
   - Must match exactly what user signed
   - No extra whitespace or characters
   - Correct encoding (UTF-8)

2. **Verify library versions:**
   ```bash
   npm list @solana/web3.js
   ```
   Ensure compatible version

3. **Test with known good signature:**
   - Create test case with valid signature
   - Verify it works

4. **Check logs:**
   - Look for detailed error messages
   - May indicate specific issue

5. **Update dependencies:**
   ```bash
   npm update @solana/web3.js bs58
   ```

## Performance Issues

### Issue: Slow Verification

**Symptoms:**
- Verification takes > 30 seconds
- Users complain about wait times
- Timeouts

**Causes:**
- Slow RPC endpoint
- Network latency
- Solana network congestion
- Too many NFTs to query

**Solutions:**

1. **Use faster RPC:**
   - Switch to Helius or QuickNode
   - Upgrade to paid tier
   - Configure multiple endpoints

2. **Check Solana network:**
   - Visit https://status.solana.com
   - Wait if network is congested

3. **Optimize queries:**
   - Ensure using DAS API
   - Batch requests when possible
   - Cache metadata

4. **Check bot server:**
   - Ensure good internet connection
   - Low latency to RPC endpoint
   - Sufficient resources

---

### Issue: Rate Limit Errors

**Symptoms:**
- "Rate limit exceeded" errors
- Verification fails during peak times
- Periodic checks incomplete

**Causes:**
- Too many requests to RPC
- Free tier limits exceeded
- Many users verifying simultaneously
- Check interval too short

**Solutions:**

1. **Upgrade RPC plan:**
   - Move to paid tier
   - Higher rate limits

2. **Increase check interval:**
   ```
   /nft-config set-check-interval 48
   ```

3. **Configure fallback endpoints:**
   - Distribute load across multiple RPCs
   - Automatic failover

4. **Stagger checks:**
   - Bot should automatically stagger
   - Check logs for timing

5. **Monitor usage:**
   - Check RPC provider dashboard
   - Identify peak times
   - Adjust configuration

## Security Issues

### Issue: Suspicious Verification Activity

**Symptoms:**
- Multiple failed verification attempts
- Same wallet trying different accounts
- Unusual patterns in logs

**Causes:**
- Attempted abuse
- Bot testing
- User confusion
- Malicious activity

**Solutions:**

1. **Review logs:**
   - Check verification attempts
   - Look for patterns
   - Identify suspicious users

2. **Rate limiting:**
   - Bot should have built-in rate limits
   - Verify they're working

3. **Block if necessary:**
   - Use Discord's ban feature
   - Document the issue

4. **Notify admins:**
   - Alert other admins
   - Monitor for continued attempts

5. **Review security:**
   - Ensure signature verification is working
   - Check for vulnerabilities

---

### Issue: Wallet Reuse Concerns

**Symptoms:**
- Same wallet on multiple accounts
- Users sharing wallets
- Verification conflicts

**Cause:**
- System prevents this by design
- One wallet per account per server

**Solutions:**

1. **This is working correctly:**
   - System prevents wallet reuse
   - Security feature, not a bug

2. **If legitimate need:**
   - User must unlink from old account first
   - Then verify with new account

3. **If abuse suspected:**
   - Review both accounts
   - Check for ban evasion
   - Take appropriate action

## Error Messages Reference

### User-Facing Errors

| Error Message | Meaning | Solution |
|---------------|---------|----------|
| "I couldn't send you a DM" | DMs disabled | Enable DMs from server |
| "Invalid signature" | Signature verification failed | Re-verify with correct signature |
| "Wallet already linked" | Wallet in use by another account | Unlink from other account first |
| "You don't own any configured NFTs" | No NFTs from configured collections | Check `/nft-collections` |
| "Verification expired" | Message too old (>10 min) | Run `/verify` again |
| "You are not verified" | No wallet linked | Run `/verify` first |
| "Rate limit exceeded" | Too many requests | Wait and try again |

### Admin Errors

| Error Message | Meaning | Solution |
|---------------|---------|----------|
| "Invalid collection address" | Address format wrong or doesn't exist | Verify address on Solscan |
| "Missing permissions" | Bot can't manage roles | Check role hierarchy and permissions |
| "Collection not found" | Collection doesn't exist on-chain | Double-check address |
| "Role not found" | Specified role doesn't exist | Create role first |
| "NFT verification is disabled" | Feature turned off | Run `/nft-config enable` |

### Technical Errors

| Error Message | Meaning | Solution |
|---------------|---------|----------|
| "RPC request failed" | Can't connect to Solana | Check RPC configuration |
| "Network error" | Connection issues | Check internet/firewall |
| "Database error" | Can't save/load data | Check file permissions |
| "Timeout" | Request took too long | Check RPC performance |
| "Invalid response from RPC" | Unexpected RPC response | Check RPC endpoint health |

## Getting Additional Help

### Before Asking for Help

1. **Check this guide** - Most issues are covered here
2. **Review bot logs** - Often contain specific error details
3. **Test basic functionality** - Isolate the problem
4. **Document the issue** - Steps to reproduce, error messages, screenshots

### Information to Provide

When asking for help, include:

- **What you're trying to do**
- **What's happening instead**
- **Error messages** (exact text)
- **Steps to reproduce**
- **Bot logs** (relevant sections)
- **Configuration** (RPC provider, check interval, etc.)
- **Server size** (number of verified users)

### Where to Get Help

1. **Bot documentation** - Check all guides
2. **Bot logs** - Often self-explanatory
3. **RPC provider support** - For RPC issues
4. **Discord API docs** - For Discord-specific issues
5. **Bot developer** - For bugs or feature requests

---

**Remember:** Most issues are configuration-related and can be resolved by carefully following the setup guides and checking permissions.
