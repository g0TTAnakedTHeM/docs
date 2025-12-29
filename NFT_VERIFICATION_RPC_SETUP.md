# Solana RPC Setup and Configuration Guide

## Overview

The NFT verification system requires access to a Solana RPC (Remote Procedure Call) endpoint to query the blockchain for NFT ownership data. This guide explains how to configure RPC endpoints, obtain API keys, and optimize performance.

## Table of Contents

- [Understanding RPC Endpoints](#understanding-rpc-endpoints)
- [RPC Provider Options](#rpc-provider-options)
- [Helius Setup (Recommended)](#helius-setup-recommended)
- [QuickNode Setup](#quicknode-setup)
- [Alchemy Setup](#alchemy-setup)
- [Public RPC Endpoints](#public-rpc-endpoints)
- [Environment Configuration](#environment-configuration)
- [Multiple RPC Endpoints](#multiple-rpc-endpoints)
- [Rate Limits and Optimization](#rate-limits-and-optimization)
- [Troubleshooting](#troubleshooting)

## Understanding RPC Endpoints

### What is an RPC Endpoint?

An RPC endpoint is a URL that allows the bot to communicate with the Solana blockchain. The bot uses RPC endpoints to:

- Verify wallet signatures
- Query NFT ownership
- Fetch NFT metadata
- Validate collection addresses

### Why Do I Need an API Key?

While public RPC endpoints exist, they have strict rate limits and can be unreliable. API keys from RPC providers offer:

- **Higher rate limits** - More requests per second
- **Better reliability** - Dedicated infrastructure
- **Enhanced features** - DAS API for efficient NFT queries
- **Priority support** - Faster response times

### Free vs Paid Plans

Most RPC providers offer free tiers suitable for small to medium servers:

| Server Size | Recommended Plan | Estimated Cost |
|-------------|------------------|----------------|
| < 50 verified users | Free tier | $0/month |
| 50-500 users | Free tier or Basic | $0-$50/month |
| 500+ users | Paid tier | $50-$200/month |

## RPC Provider Options

### Comparison Table

| Provider | Free Tier | DAS API | Setup Difficulty | Recommended For |
|----------|-----------|---------|------------------|-----------------|
| **Helius** | 100 req/sec | ✅ Yes | Easy | Most users (recommended) |
| **QuickNode** | 25 req/sec | ✅ Yes | Easy | Alternative option |
| **Alchemy** | 300 req/day | ✅ Yes | Medium | Small servers |
| **Public RPC** | Limited | ❌ No | None | Testing only |

## Helius Setup (Recommended)

Helius is recommended because it offers the best free tier and excellent DAS (Digital Asset Standard) API support.

### Step 1: Create Account

1. Go to https://helius.dev
2. Click "Sign Up" or "Get Started"
3. Create an account with your email
4. Verify your email address

### Step 2: Create API Key

1. Log in to your Helius dashboard
2. Click "Create New Project" or "New API Key"
3. Name your project (e.g., "Discord NFT Bot")
4. Select "Mainnet" network
5. Click "Create"
6. Copy your API key (starts with a long string of characters)

### Step 3: Get RPC URL

Your Helius RPC URL format:
```
https://mainnet.helius-rpc.com/?api-key=YOUR_API_KEY_HERE
```

Replace `YOUR_API_KEY_HERE` with your actual API key.

### Step 4: Configure Bot

Add to your `.env` file:

```env
SOLANA_RPC_ENDPOINT=https://mainnet.helius-rpc.com/?api-key=YOUR_API_KEY_HERE
HELIUS_API_KEY=YOUR_API_KEY_HERE
```

### Helius Free Tier Limits

- **100 requests/second**
- **Unlimited requests/month**
- **DAS API included**
- **Sufficient for most Discord servers**

### Upgrading Helius

If you need more:

1. Go to Helius dashboard
2. Click "Upgrade Plan"
3. Choose a paid tier:
   - **Developer:** $50/month - 500 req/sec
   - **Professional:** $200/month - 1000 req/sec
   - **Enterprise:** Custom pricing

## QuickNode Setup

QuickNode is a reliable alternative with good performance.

### Step 1: Create Account

1. Go to https://quicknode.com
2. Click "Sign Up"
3. Create an account
4. Verify your email

### Step 2: Create Endpoint

1. Log in to QuickNode dashboard
2. Click "Create Endpoint"
3. Select "Solana"
4. Select "Mainnet"
5. Choose your plan (Start with free tier)
6. Click "Create Endpoint"

### Step 3: Get RPC URL

1. Click on your endpoint
2. Copy the HTTP Provider URL
3. It looks like: `https://xxx.solana-mainnet.quiknode.pro/YOUR_TOKEN/`

### Step 4: Configure Bot

Add to your `.env` file:

```env
SOLANA_RPC_ENDPOINT=https://xxx.solana-mainnet.quiknode.pro/YOUR_TOKEN/
```

### QuickNode Free Tier Limits

- **25 requests/second**
- **10M requests/month**
- **DAS API included**
- **Good for small to medium servers**

### Upgrading QuickNode

Paid plans start at $49/month with higher limits.

## Alchemy Setup

Alchemy offers good free tier limits but with daily caps.

### Step 1: Create Account

1. Go to https://alchemy.com
2. Click "Sign Up"
3. Create an account
4. Verify your email

### Step 2: Create App

1. Log in to Alchemy dashboard
2. Click "Create App"
3. Name your app (e.g., "Discord NFT Bot")
4. Select "Solana" as the chain
5. Select "Mainnet" as the network
6. Click "Create App"

### Step 3: Get API Key

1. Click on your app
2. Click "View Key"
3. Copy the API Key

### Step 4: Get RPC URL

Your Alchemy RPC URL format:
```
https://solana-mainnet.g.alchemy.com/v2/YOUR_API_KEY
```

### Step 5: Configure Bot

Add to your `.env` file:

```env
SOLANA_RPC_ENDPOINT=https://solana-mainnet.g.alchemy.com/v2/YOUR_API_KEY
```

### Alchemy Free Tier Limits

- **300 compute units/second**
- **300M compute units/month**
- **DAS API included**
- **Good for small servers**

## Public RPC Endpoints

Public endpoints are available but **NOT recommended** for production use.

### Available Public Endpoints

```
https://api.mainnet-beta.solana.com
https://solana-api.projectserum.com
```

### Limitations

- ❌ Very strict rate limits
- ❌ Unreliable during high network usage
- ❌ No DAS API support (slower NFT queries)
- ❌ No guaranteed uptime
- ❌ Shared with thousands of users

### When to Use

- Testing and development only
- Temporary fallback
- Very small servers (< 10 users)

### Configuration

Add to your `.env` file:

```env
SOLANA_RPC_ENDPOINT=https://api.mainnet-beta.solana.com
```

## Environment Configuration

### Basic Configuration

Create or edit your `.env` file in the bot's root directory:

```env
# Primary RPC endpoint (required)
SOLANA_RPC_ENDPOINT=https://mainnet.helius-rpc.com/?api-key=YOUR_KEY

# Helius API key (optional, for enhanced features)
HELIUS_API_KEY=YOUR_KEY

# Discord bot token (required)
DISCORD_TOKEN=your_discord_token_here

# Other bot configuration...
```

### Advanced Configuration

For better reliability, configure multiple endpoints:

```env
# Primary RPC endpoint
SOLANA_RPC_ENDPOINT=https://mainnet.helius-rpc.com/?api-key=YOUR_HELIUS_KEY

# Fallback RPC endpoints (comma-separated)
SOLANA_RPC_FALLBACK=https://xxx.solana-mainnet.quiknode.pro/YOUR_TOKEN/,https://api.mainnet-beta.solana.com

# Helius API key
HELIUS_API_KEY=YOUR_HELIUS_KEY
```

### Environment Variables Reference

| Variable | Required | Description | Example |
|----------|----------|-------------|---------|
| `SOLANA_RPC_ENDPOINT` | Yes | Primary RPC endpoint | `https://mainnet.helius-rpc.com/?api-key=...` |
| `HELIUS_API_KEY` | No | Helius API key for DAS | `abc123...` |
| `SOLANA_RPC_FALLBACK` | No | Backup endpoints | `https://endpoint1,https://endpoint2` |

## Multiple RPC Endpoints

### Why Use Multiple Endpoints?

- **Reliability:** Automatic failover if primary fails
- **Load balancing:** Distribute requests across endpoints
- **Rate limit management:** Switch when limits are hit

### Configuration

```env
SOLANA_RPC_ENDPOINT=https://mainnet.helius-rpc.com/?api-key=KEY1
SOLANA_RPC_FALLBACK=https://xxx.quiknode.pro/KEY2/,https://api.mainnet-beta.solana.com
```

### How It Works

1. Bot uses primary endpoint (`SOLANA_RPC_ENDPOINT`)
2. If primary fails, tries first fallback
3. If first fallback fails, tries second fallback
4. After successful request, returns to primary
5. Implements exponential backoff between retries

### Best Practice

Configure at least 2 endpoints:
- **Primary:** Paid or free tier with good limits (Helius/QuickNode)
- **Fallback:** Public RPC or alternative provider

## Rate Limits and Optimization

### Understanding Rate Limits

Rate limits restrict how many requests you can make per second/minute/month.

**Common limits:**
- Helius Free: 100 req/sec
- QuickNode Free: 25 req/sec
- Public RPC: ~10 req/sec (unofficial)

### Bot's Built-in Optimization

The bot automatically:
- ✅ Caches collection metadata (24 hours)
- ✅ Batches NFT queries when possible
- ✅ Implements exponential backoff on failures
- ✅ Queues requests to avoid rate limits
- ✅ Uses DAS API for efficient NFT queries

### Reducing RPC Usage

If you're hitting rate limits:

1. **Increase check interval:**
   ```
   /nft-config set-check-interval 48
   ```

2. **Limit collections:** Only configure essential collections

3. **Upgrade RPC plan:** Consider paid tier

4. **Use Helius:** Best free tier limits

### Monitoring Usage

Check your RPC provider's dashboard to monitor:
- Requests per second
- Total requests per day/month
- Error rates
- Response times

## Troubleshooting

### "RPC request failed" Error

**Possible causes:**
- Invalid API key
- Rate limit exceeded
- RPC endpoint down
- Network connectivity issues

**Solutions:**
1. Verify API key is correct in `.env`
2. Check RPC provider dashboard for status
3. Wait a few minutes and try again
4. Configure fallback endpoints
5. Check bot logs for detailed error messages

### "Invalid RPC endpoint" Error

**Possible causes:**
- Typo in RPC URL
- Missing API key in URL
- Wrong network (devnet vs mainnet)

**Solutions:**
1. Double-check RPC URL format
2. Ensure API key is included
3. Verify you're using mainnet endpoint
4. Test endpoint with curl:
   ```bash
   curl -X POST YOUR_RPC_URL \
     -H "Content-Type: application/json" \
     -d '{"jsonrpc":"2.0","id":1,"method":"getHealth"}'
   ```

### Rate Limit Errors

**Symptoms:**
- "Rate limit exceeded" messages
- Slow verification checks
- Timeouts

**Solutions:**
1. Check current usage in provider dashboard
2. Increase check interval: `/nft-config set-check-interval 48`
3. Upgrade to paid plan
4. Configure multiple RPC endpoints
5. Reduce number of verified users (if testing)

### Slow Response Times

**Symptoms:**
- Verification takes > 30 seconds
- Timeouts during checks

**Solutions:**
1. Check Solana network status: https://status.solana.com
2. Try different RPC provider
3. Verify your internet connection
4. Check RPC provider's status page
5. Use Helius (generally fastest)

### Connection Timeouts

**Symptoms:**
- "Connection timeout" errors
- No response from RPC

**Solutions:**
1. Check your firewall settings
2. Verify bot can make outbound HTTPS requests
3. Test RPC endpoint from command line
4. Try different RPC provider
5. Check if RPC endpoint is down

### Testing Your Configuration

Test your RPC endpoint:

```bash
# Test basic connectivity
curl -X POST YOUR_RPC_ENDPOINT \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getHealth"
  }'

# Expected response: {"jsonrpc":"2.0","result":"ok","id":1}
```

Test DAS API (Helius/QuickNode):

```bash
curl -X POST YOUR_RPC_ENDPOINT \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getAssetsByOwner",
    "params": {
      "ownerAddress": "SOME_WALLET_ADDRESS",
      "page": 1,
      "limit": 10
    }
  }'
```

## Security Best Practices

### Protecting Your API Keys

1. **Never commit `.env` to git:**
   ```bash
   # Add to .gitignore
   echo ".env" >> .gitignore
   ```

2. **Use environment variables:** Don't hardcode keys in code

3. **Rotate keys regularly:** Generate new keys every few months

4. **Limit key permissions:** Use read-only keys if available

5. **Monitor usage:** Watch for unusual activity in provider dashboard

### API Key Storage

- ✅ Store in `.env` file
- ✅ Use environment variables in production
- ✅ Use secrets management in cloud deployments
- ❌ Don't commit to version control
- ❌ Don't share in Discord/public channels
- ❌ Don't hardcode in source files

## Cost Estimation

### Free Tier Capacity

**Helius Free (100 req/sec):**
- ~50-100 verified users with 24h checks
- ~200+ users with 48h checks
- Unlimited for manual checks only

**QuickNode Free (25 req/sec):**
- ~25-50 verified users with 24h checks
- ~100 users with 48h checks

### When to Upgrade

Consider upgrading when:
- Consistently hitting rate limits
- Verification checks failing frequently
- Server has > 100 verified users
- Need faster response times
- Want guaranteed uptime

### Cost Examples

**Small server (50 users):**
- Free tier sufficient
- Cost: $0/month

**Medium server (200 users):**
- Free tier with 48h checks, or
- Helius Developer ($50/month)

**Large server (1000+ users):**
- Helius Professional ($200/month) or
- QuickNode paid tier ($49-$299/month)

## Support and Resources

### RPC Provider Documentation

- **Helius:** https://docs.helius.dev
- **QuickNode:** https://www.quicknode.com/docs/solana
- **Alchemy:** https://docs.alchemy.com/reference/solana-api-quickstart

### Solana Resources

- **Solana Status:** https://status.solana.com
- **Solana Docs:** https://docs.solana.com
- **RPC API Reference:** https://docs.solana.com/api

### Getting Help

1. Check provider's status page
2. Review provider's documentation
3. Test with curl commands above
4. Check bot logs for detailed errors
5. Contact provider support if needed

---

**Recommended Setup:** Start with Helius free tier. It's easy to set up, has generous limits, and works great for most Discord servers.
