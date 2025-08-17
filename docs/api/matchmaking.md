# Matchmaking API

The Matchmaking API connects users with random pen pals based on filters like language or time zone.

## POST /match

Matches the authenticated user with a new pen pal.

### Request

- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "language": "string", // Optional: e.g., "Spanish"
    "timezone": "string", // Optional: e.g., "UTC+1"
    "oneTime": boolean // true for one-time message, false for ongoing
  }
  ```

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  {
    "matchId": "string",
    "penPal": {
      "userId": "string",
      "region": "string",
      "language": "string"
    }
  }
  ```

### Example

```bash
curl -X POST https://us-central1-globetalk.cloudfunctions.net/api/match \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"language": "Spanish", "oneTime": false}'
```