# Message API

The Message API handles sending, delaying, and retrieving text-based messages between matched users.

## POST /messages

Sends a new message with a 12-hour delivery delay.

### Request

- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "matchId": "string",
    "content": "string", // Text message, max 1000 characters
    "emojis": ["string"] // Optional: Array of emoji codes
  }
  ```

### Response

- **Status**: 201 Created
- **Body**:
  ```json
  {
    "messageId": "string",
    "status": "queued",
    "deliveryTime": "ISO8601 timestamp"
  }
  ```

## GET /messages/{matchId}

Retrieves all messages for a match.

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  [
    {
      "messageId": "string",
      "content": "string",
      "senderId": "string",
      "timestamp": "ISO8601 timestamp"
    }
  ]
  ```

### Example

```bash
curl -X POST https://us-central1-globetalk.cloudfunctions.net/api/messages \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"matchId": "abc123", "content": "Hello from Brazil! 😊"}'
```