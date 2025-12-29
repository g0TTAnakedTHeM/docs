# Moderation & Security

Azalea Gate provides automated moderation tools to keep your server safe from scams and spam.

## 1. Prohibited Words
Automatically delete messages containing specific words or phrases (e.g., "drainer", "free mint").

**Commands**:
-   `Add`: `/add-prohibited [word]`
-   `Remove`: `/remove-prohibited [word]`
-   `List`: `/list-prohibited`

## 2. Link Protection
Block all links except those you explicitly allow. This is highly effective against phishing.

**Commands**:
-   `Configure`: `/config-link-moderation` (Enable/Disable)
-   `Whitelist Channel`: `/add-link-channel [channel]` (Allow links in specific channels)

## 3. Mention Protection
Prevent users from mass-pinging valid roles or users.

**Command**:
-   `/config-mentions`: Enable/Disable mention limits.

## 4. Protected Roles
Prevent lower-ranked staff from moderating specific high-ranked roles (anti-nuke feature).

**Commands**:
-   `Add`: `/add-protected-role [role]`
-   `Remove`: `/remove-protected-role [role]`

## 5. Exempt Roles
Allow certain roles (e.g., Moderators) to bypass all moderation filters.

**Command**:
-   `/exempt-role [role]`
