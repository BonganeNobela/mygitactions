# TECHSTACK
 
## Frontend: HTML, CSS, and JavaScript with Figma for UI Design

For the frontend of GlobeTalk we use core web technologies HTML, CSS, and 
JavaScript to build an accessible and lightweight user interface. This allows us 
to rapidly prototype key views such as the matchmaking screen, cultural 
profiles, and chat interface without the overhead of frameworks. By converting 
designs made in Figma directly into code our team ensures that the chat page, 
profile views, and cultural explorer match the intended UI/UX design. Since the 
project emphasizes simplicity, accessibility, and cross-browser compatibility, 
sticking with raw web technologies aligns well with our goals. 
Backend: Node.js with Express + Firebase 
The backend logic for GlobeTalk is handled using a hybrid approach: 
- Node.js with Express hosts custom APIs (e.g., Moderation API) deployed 
on Render. These handle tasks like reporting users, blocking/unblocking, 
and moderation logs. 
- Firebase Cloud Functions are used for other serverless tasks, such as 
simulating message delivery delays (to mimic postal mail). 
This setup gives us both control (for our custom APIs) and scalability 
(through Firebase’s serverless infrastructure). 
Database: Firebase Firestore 
GlobeTalk stores its core data in Firebase Firestore, which supports the 
project’s dynamic and semi-structured needs: 
- User Profiles (region, language, hobbies, cultural facts). 
- Match Records (pairings between pen pals with timestamps). 
- Messages (stored with delays, sender/receiver IDs, and text content). 
- Moderation Logs (reports, blocks, flags). 
Firestore’s real-time syncing also allows chat updates and profile data to be 
retrieved instantly, which is essential for asynchronous messaging and user 
interaction. 
 
## Hosting: Render + GitHub Pages 
- The APIs (e.g., Moderation API, future Messaging API) are deployed on 
Render, which provides free hosting with environment variable support 
for Firebase keys. 
- The frontend is hosted via GitHub Pages, making the static HTML/CSS/JS 
easily accessible for end-users and simple to redeploy during 
development. 
This hybrid setup keeps deployment costs low while giving us flexibility 
and scalability. 
 
## Authentication and Security: Firebase Authentication 
User accounts are managed with Firebase Authentication, which enables 
anonymous matching while protecting real user data. Users never share 
personal details (names or emails) instead, unique identifiers are generated. 
Firebase Auth also integrates directly with Firestore, so security rules can 
enforce that users only access their own profiles and message threads. 
 
## DevOps & CI/CD: GitHub Actions with Render Deployment 
For CI/CD, we use GitHub Actions to automate builds and deployments: 
- Every push to the main branch triggers builds and redeployment to 
Render for backend APIs. 
- GitHub Pages auto-deploys frontend changes directly from the 
repository. 
This ensures that team members can work collaboratively, and new 
features are pushed live without manual intervention. 
 
## Testing: Postman, Jest, and User Feedback 
- Postman is used to test API endpoints (/report, /block, /unblock, 
/listBlocked). 
- Jest is considered for frontend unit tests (e.g., input validation, emoji 
picker logic). 
- Google Forms User Feedback complements technical testing, providing 
real-world usability insights. 
This combination ensures both technical correctness and positive user 
experience, which is critical for GlobeTalk. 