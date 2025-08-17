# Installation

Follow these steps to set up GlobeTalk locally for development.

## Prerequisites

- Node.js (v16 or higher)
- npm
- Firebase CLI
- Git

## Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/example/globetalk.git
   cd globetalk
   ```

2. **Initialize npm**
   ```bash
   npm init -y
   ```

3. **Install Dependencies**
   ```bash
   npm install eslint --save-dev
   npx eslint --init
   npm install jest --save-dev
   ```

4. **Set Up Firebase**
   - Install Firebase CLI:
     ```bash
     npm install -g firebase-tools
     ```
   - Log in to Firebase:
     ```bash
     firebase login
     ```
   - Initialize Firebase in the project:
     ```bash
     firebase init
     ```
   - Select Firestore, Functions, and Hosting, then follow the prompts.

5. **Run Locally**
   - Start the development server:
     ```bash
     firebase emulators:start
     ```
   - Open `http://localhost:5000` in your browser.

## Troubleshooting

- Ensure Firebase configuration (`firebase.json`) is correct.
- Check that Firestore security rules allow read/write access for testing.