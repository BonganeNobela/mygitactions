# Moderation API

The Moderation API handles reporting users, managing bans, and reviewing flagged content.

## POST /report

Reports a message or user for inappropriate content.

### Request

- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "messageId": "string", // Optional
    "userId": "string", // Optional
    "reason": "string" // e.g., "Inappropriate language"
  }
  ```

### Response

- **Status**: 201 Created
- **Body**:
  ```json
  {
    "reportId": "string",
    "status": "submitted"
  }
  ```

## POST /block

Blocks a user to prevent further communication.

### Request

- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "userId": "string"
  }
  ```

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  {
    "blocked": true
  }
  ```