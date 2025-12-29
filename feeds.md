# Social Feeds

Keep your community up-to-date by automatically forwarding content from Twitter/X and Telegram directly into your Discord channels.

## Twitter / X Feed
Forward tweets from any public account to a specific Discord channel.

**Command**:
```
/feed add twitter [username] [channel]
```
-   `username`: The Twitter handle (without @).
-   `channel`: The Discord channel to post updates in.

**Options**:
-   **Mention Role**: Ping a specific role when a tweet is posted.

## Telegram Feed
Forward messages from a public Telegram channel to Discord.

**Command**:
```
/feed add telegram [channel_link] [channel]
```

## Managing Feeds
To view your active feeds or remove them:

**List Feeds**:
```
/feed list
```

**Remove Feed**:
```
/feed remove [feed_id]
```
(You can find the `feed_id` from the list command).
