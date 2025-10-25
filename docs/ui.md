# UI Modules

GlobeTalk's user interface is built with HTML, CSS, and Vanilla JavaScript, designed for simplicity and responsiveness.

## Match Screen

- Displays options to find a new pen pal with filters (language, time zone, one-time vs. ongoing).
- Example: Select "French" and "UTC+1" to match with a user from France.

## Message Inbox

- Shows active conversations with pen pals.
- Displays message status (queued, delivered) and timestamps.
- Example: View a thread with a pen pal from Japan, showing delayed messages.

## Compose Letter

- A text editor for writing messages with emoji support.
- Shows a preview of the message and delivery delay (12 hours).
- Example: Write "Greetings from Canada! 🍁" and schedule delivery.

## Cultural Explorer

- Displays region-based facts about your pen pal (e.g., holidays, sayings).
- Example: Learn about Brazil’s Carnival or a Swedish Midsummer tradition.

## Settings & Safety

- Adjust preferences (e.g., language, notifications).
- Block or report users and view app policies.
- Example: Block a user or report a message for inappropriate content.

---
# UI/UX DESIGN EXPLANATION FOR OUR WEBSITE

## Introduction
The UI/UX design for GlobeTalk focuses on simplicity, accessibility, and a visually engaging experience to facilitate global connections between users through pen pal interactions. The design prioritizes clarity, user guidance, and real-time interactivity, ensuring that users of varying technical skills can navigate the platform with ease.

## Design Philosophy
The GlobeTalk interface emphasizes:
- Clean, modern aesthetics: A blue gradient header provides a welcoming and calm visual, reflecting trustworthiness and global connectivity.
- Card-based layouts: User profiles, pen pal suggestions, and active pen pal lists are presented in cards, balancing readability with visual appeal.
- Mobile-first, responsive design: All pages are designed to function seamlessly across desktop and mobile devices.

**Benefit:** Users can intuitively explore profiles and features without cognitive overload, making it easy to engage with the platform.

## Navigation and Layout

### Navigation Bar:
- Located at the top of each page, featuring links to Home, Find Pen Pal, Chats, My Penpals, and Settings & Safety.
- The Logout button is placed on the top-right for quick access.
- Consistent placement across pages ensures users can navigate intuitively.

### Dashboard Layout (Home Page):
- **User Card:** Displays the logged-in user’s profile with username, timezone, and a short bio.
- **Pen Pal Suggestions:** Each suggested pen pal is presented as a card with region, hobbies, and languages.
- **Active Pen Pals List:** Scrollable card layout showing all active connections, making it easy to view and access conversations.

**Benefit:** Users can quickly identify personal information, find new pen pals, and manage existing connections from a single dashboard.

## Find Pen Pal Page

### Purpose:
Allows users to set preferences to find compatible pen pals.

### Form-Based User Input:
- **Language Preference:** Dropdown menu for selecting communication language.
- **Region/Timezone:** Dropdown for selecting compatible time zones.
- **Find My Pen Pal Button:** Submits preferences and generates suggested matches.

### Guidance & Accessibility:
Clear placeholder text and sub-label instructions help users understand required input.

**Benefit:** The form simplifies matching with like-minded users across the globe, making the experience intuitive and efficient.

### Interface Elements:
- **Cards:** Used throughout the platform to display users, suggestions, and active pen pals.
  - Include profile avatar, username, region/timezone, hobbies, and languages.
  - Structured hierarchically to emphasize the most relevant information first.
- **Buttons:**
  - Styled with a gradient and hover effects to highlight interactivity.
  - Clearly labelled actions, such as Find My Pen Pal, enhance usability.

### Accessibility and Usability
- **Colour Contrast:** Blue and white gradients ensure readability for users with visual impairments.
- **Typography:** Bold, sans-serif fonts are used for clarity.
- **Responsive Design:** Layout adapts to various screen sizes, ensuring functional and visual consistency across devices.

**Benefit:** The design ensures users from different regions and technical backgrounds can comfortably use the platform.

## Chats Page Interface
The Chats page adopts a split view layout that balances functionality and simplicity:

- **Left Panel Conversation List:** Vertically scrollable list of active chats, with the selected chat highlighted.
- **Right Panel Chat Window:** Clean message interface with distinguishable chat bubbles.
  - Outgoing messages use a vibrant gradient tone.
  - Incoming messages remain neutral for contrast.
- **Top Action Bar:** Includes the recipient’s username and a Block button for safety.
- **Message Input Area:** Includes emoji icon and Send button styled in purple.

**Benefit:** The two-column structure allows users to manage multiple conversations efficiently, mirroring modern messaging experiences.

## Penpals Page Interface
Focuses on managing user connections through search and request tools.

- **Search Section:** At the top, users can search by username using a labelled input field and purple “Search” button.
- **Your Penpals Card:** Displays current pen pals; when empty, a friendly prompt encourages users to connect.
- **Requests Section:**
  - **Requests To You:** Cards showing requester’s username with Accept / Decline buttons.
  - **Requests You Sent:** Displays pending outgoing requests.

The gradient blue background provides visual separation, while white cards maintain contrast and readability.

**Benefit:** Users get a clear overview of their social interactions in one place.

## User Feedback and Accessibility
Across all pages:
- Responsive layouts for all devices
- Buttons and inputs provide hover/focus feedback
- Error messages and confirmations appear in context

**Benefit:** Ensures an inclusive and straightforward experience that supports global friendships through intuitive communication.

---

## Profile Customization Interface
The Profile Page allows users to define personal characteristics and interests.

### Layout and Design Elements:
- **Form Based Structure:** Vertically oriented form with clear labels (Age Range, Gender, Interests, Short Bio).
- **Dropdown Menus:** Used for Age Range and Gender for consistency.
- **Text Fields:** Allow flexible self-expression.
- **Primary Action Button:** “Save Change” button uses purple gradient and is clearly visible.

### User Experience Considerations:
- Simplicity and guidance through placeholder text
- Visual confirmation after saving
- Consistent spacing, rounded elements, and typography

**Benefit:** Promotes quick setup with easy personalization.

---

## Settings and Safety Interface
Allows users to manage key preferences and access safety information.

### Layout and Navigation:
- **Two-Section Layout:**  
  1. Policy Access Cards  
  2. Profile Information  
- **Policy Cards:** Privacy Policy and Terms of Service displayed as interactive cards.
- **Profile Information Section:** Dropdowns for Language and Region/Time Zone.

### Visual Design:
- Clean sans-serif typography
- Blue-to-teal gradients for trust and calmness
- Light borders, rounded corners for clarity and touch-friendliness

---

## Overall UX Integration
GlobeTalk’s UX is guided by principles of:
- Consistency
- Clarity
- Accessibility
- Feedback
- User control

A unified visual language (gradient colors, card layouts, structured spacing) reduces cognitive load. Real-time interaction responses boost confidence. Responsive layouts ensure functionality across devices.

Users maintain full control over connections, preferences, and personal data—empowering comfortable global communication in an inclusive, user-centered environment.
