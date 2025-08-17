# Profile API

The Profile API manages user cultural profiles, including region, hobbies, and languages.

## PUT /profile

Updates the authenticated user's profile.

### Request

- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "region": "string", // e.g., "South America"
    "hobbies": ["string"], // e.g., ["reading", "travel"]
    "languages": ["string"], // e.g., ["English", "Portuguese"]
    "bio": "string" // Max 200 characters
  }
  ```

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  {
    "userId": "string",
    "updated": true
  }
  ```

## GET /profile/{userId}

Retrieves a pen pal's anonymous profile.

### Response

- **Status**: 200 OK
- **Body**:
  ```json
  {
    "region": "string",
    "hobbies": ["string"],
    "languages": ["string"],
    "bio": "string",
    "funFact": "string" // e.g., "Celebrates Diwali"
  }
  ```