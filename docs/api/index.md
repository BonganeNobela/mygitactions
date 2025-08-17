# API Overview

GlobeTalk provides a set of RESTful APIs to manage matchmaking, messaging, user profiles, and moderation. These APIs are built with Firebase Functions and integrate with Firebase Firestore for data storage.

## API Modules

- **[Matchmaking API](./matchmaking.md)**: Connect users with available pen pals based on filters like language or time zone.
- **[Message API](./message.md)**: Handle sending, delaying, and retrieving text-based messages.
- **[Profile API](./profile.md)**: Manage user cultural profiles, including region and hobbies.
- **[Moderation API](./moderation.md)**: Report users, manage bans, and review flagged content.

## Authentication

All APIs require Firebase Authentication tokens. Include the token in the `Authorization` header as `Bearer <token>`.

## Base URL

```
https://us-central1-globetalk.cloudfunctions.net/api
```

## Getting Started

1. Set up Firebase Authentication for your app.
2. Obtain an access token for authenticated requests.
3. Use the endpoints below to interact with GlobeTalk's features.