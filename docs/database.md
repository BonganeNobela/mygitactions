# Database Components

GlobeTalk uses Firebase Firestore for scalable, real-time data storage. Below are the main collections and their schemas.

## User Profiles

Stores user preferences and cultural information.

- **Collection**: `users`
- **Document**: `<userId>`
- **Fields**:
  ```json
  {
    "region": "string", // e.g., "Asia"
    "hobbies": ["string"], // e.g., ["music", "cooking"]
    "languages": ["string"], // e.g., ["English", "Mandarin"]
    "bio": "string", // Max 200 characters
    "createdAt": "timestamp"
  }
  ```

## Match Records

Tracks user pairings.

- **Collection**: `matches`
- **Document**: `<matchId>`
- **Fields**:
  ```json
  {
    "user1Id": "string",
    "user2Id": "string",
    "createdAt": "timestamp",
    "oneTime": boolean
  }
  ```

## Message Storage

Stores messages with delivery delays.

- **Collection**: `messages`
- **Document**: `<messageId>`
- **Fields**:
  ```json
  {
    "matchId": "string",
    "senderId": "string",
    "content": "string", // Max 1000 characters
    "emojis": ["string"],
    "queuedAt": "timestamp",
    "deliveryAt": "timestamp"
  }
  ```

## Moderation Logs

Tracks reports and bans.

- **Collection**: `moderation`
- **Document**: `<reportId>`
- **Fields**:
  ```json
  {
    "userId": "string", // Optional
    "messageId": "string", // Optional
    "reason": "string",
    "status": "string", // e.g., "pending", "resolved"
    "createdAt": "timestamp"
  }
  ```