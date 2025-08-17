# Usage

Learn how to use GlobeTalk as a user or developer.

## For Users

1. **Sign Up**
   - Create an account using Firebase Authentication (Google).
   - Set up your profile with region, hobbies, and languages.

2. **Find a Pen Pal**
   - Go to the Match Screen and select preferences (e.g., language: Spanish, time zone: UTC-3).
   - Choose one-time or ongoing correspondence.
   - Example: Match with a user from Argentina for ongoing chats.

3. **Send a Message**
   - In the Compose Letter section, write a message (e.g., "Hello! What's your favorite holiday? 😊").
   - Submit to queue it for delivery in 12 hours.

4. **Explore Cultural Facts**
   - Visit the Cultural Explorer to view facts about your pen pal’s region.
   - Example: Learn about Diwali if matched with an Indian user.

5. **Stay Safe**
   - Use the Settings & Safety section to block or report users.
   - Example: Report a message for inappropriate content.

## For Developers

1. **API Integration**
   - Authenticate with Firebase to get a token.
   - Use the [Matchmaking API](./api/matchmaking.md) to connect users.
   - Example: Send a `POST /match` request to pair a user with a Spanish-speaking pen pal.

2. **Local Testing**
   - Run Firebase emulators to test API endpoints locally.
   - Use Postman to send requests to `http://localhost:5001/globetalk/us-central1/api`.

3. **Database Access**
   - Query the `users` collection to retrieve profiles.
   - Example: Fetch a pen pal’s profile with `GET /profile/<userId>`.

## Example Workflow

1. Sign up and set your profile to "Region: Europe, Language: French."
2. Match with a pen pal from France.
3. Send a message: "Bonjour! Tell me about French culture! 🇫🇷"
4. After 12 hours, receive a reply and view their cultural fact about Bastille Day.