# Database Schema



## The ER Diagram

### 1. Entity Descriptions

**Users**  
Stores the core details of each pen pal, including personal info, language preferences, hobbies, and blocked users. This entity is the foundation for all matchmaking and messaging operations.

**Matches**  
Represents a pairing between two users for correspondence. Includes match metadata such as timestamps, status, and preferences.

**Messages**  
Stores the conversation between matched users. Each message references the sender, recipient, and time sent.

**Reports**  
Keeps track of user-reported issues such as harassment or inappropriate content, linking the reporting user, the reported user, and the context.

**CulturalProfiles**  
Holds optional cultural information about users (hobbies, cultural background, languages spoken) used to improve match recommendations.

### 2. Attributes List

#### Users Collection
| Field         | Type              | Constraints         | Description                        |
|--------------|-------------------|---------------------|------------------------------------|
| uid          | string            | Primary Key, Unique | Unique identifier for each user     |
| displayName  | string            | Optional            | User’s display name                |
| email        | string            | Required, Unique    | User’s email for login             |
| languages    | array of strings  | Optional            | Languages the user speaks          |
| hobbies      | array of strings  | Optional            | User interests                     |
| blockedUsers | array of strings  | Optional            | List of users blocked by this user |
| createdAt    | timestamp         | Default: now        | Account creation time              |

#### Matches Collection
| Field     | Type    | Constraints         | Description              |
|-----------|---------|---------------------|--------------------------|
| matchId   | string  | Primary Key         | Unique ID for each match |
| user1Id   | string  | Required            | UID of first user        |
| user2Id   | string  | Required            | UID of second user       |
| status    | string  | Optional            | e.g., active, completed  |
| matchedAt | timestamp | Default: now       | Time match was created   |

#### Messages Collection
| Field       | Type    | Constraints         | Description                |
|-------------|---------|---------------------|----------------------------|
| messageId   | string  | Primary Key         | Unique ID for each message |
| matchId     | string  | Required            | Reference to the match     |
| senderId    | string  | Required            | UID of sender              |
| recipientId | string  | Required            | UID of recipient           |
| text        | string  | Required            | Message content            |
| sentAt      | timestamp | Default: now       | Time message was sent      |

#### Reports Collection
| Field           | Type    | Constraints         | Description                |
|-----------------|---------|---------------------|----------------------------|
| reportId        | string  | Primary Key         | Unique ID for the report   |
| reportingUserId | string  | Required            | UID of reporting user      |
| reportedUserId  | string  | Required            | UID of reported user       |
| reason          | string  | Required            | Description of issue       |
| createdAt       | timestamp | Default: now       | Time report was filed      |

#### CulturalProfiles Collection
| Field         | Type             | Constraints         | Description                        |
|---------------|------------------|---------------------|------------------------------------|
| profileId     | string           | Primary Key         | Unique ID for the cultural profile |
| userId        | string           | Required            | UID of the user                    |
| languages     | array of strings | Optional            | Languages spoken                   |
| hobbies       | array of strings | Optional            | Hobbies and interests              |
| culturalNotes | string           | Optional            | Any additional cultural info        |

### 3. Relationships & Cardinalities
- **Users and Matches**: One User can participate in many Matches. Each Match connects exactly two Users. (One-to-Many)
- **Matches and Messages**: One Match can have many Messages. Each Message belongs to exactly one Match. (One-to-Many)
- **Users and Reports**: One User can file multiple Reports. Each Report references one reporting user and one reported user. (One-to-Many)
- **Users and CulturalProfiles**: One User can have one optional CulturalProfile. (One-to-One)

### 4. Indexes & Keys
- `uid` serves as the unique identifier for Users.
- `matchId`, `messageId`, `reportId`, `profileId` serve as document IDs for other collections.
- Composite indexes may be added for queries such as:
  - Retrieve all matches for a user (`user1Id` or `user2Id`)
  - Retrieve messages for a match (`matchId` + `sentAt`)

### 5. Deployment Information
- The database is deployed on **Firebase Firestore**, a NoSQL, document-based database.
- Collections are created dynamically when data is added.
- Backend access is secured via Firebase Admin SDK and service account credentials.
- Real-time updates allow chat messages and matches to sync instantly between users.

### 6. Choice Justification
- **Firestore Advantages for this Project:**
  - Real-time syncing – essential for messaging functionality.
  - Flexible schema – allows optional fields like hobbies and cultural notes without strict table constraints.
  - Seamless integration – works smoothly with Firebase Authentication and Cloud Functions.
  - Scalability – handles growing number of users and matches efficiently.

