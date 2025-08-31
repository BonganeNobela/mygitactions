# GlobeTalk - Diagrams

# Chatflow sequence 
```mermaid
sequenceDiagram
    autonumber
    participant UA as User A
    participant System as System
    participant DB as Firestore
    participant Trans as Translation API
    participant FCM as Firebase Cloud Messaging
    participant UB as User B
    
    UA->>System: Send message to User B
    System->>DB: Store original message
    System->>Trans: Request translation (if needed)
    Trans-->>System: Return translated message
    System->>DB: Store translated message
    System->>FCM: Send push notification
    FCM-->>UB: Deliver notification
    System-->>UA: Confirm message sent
    System-->>UB: Real-time message update
    UB->>System: Send reaction/emoji
    System->>DB: Store reaction
    System-->>UA: Real-time reaction update
    Note right of System: All messages stored with timestamp
    Note left of UB: User receives both original and translated text
    
```

# Admin Moderation sequence

```mermaid

sequenceDiagram
    autonumber
    participant User as Reporting User
    participant System as System
    participant ContentAPI as Content Moderation API
    participant Admin as Administrator
    participant DB as Firestore
    participant Target as Reported User
    User->>System: Report inappropriate content
    System->>DB: Store report with metadata
    System->>ContentAPI: Analyze flagged content
    ContentAPI-->>System: Return moderation score
    alt Auto-moderate (high confidence)
        System->>DB: Auto-delete offensive content
        System->>DB: Log moderation action
        System-->>User: Notify action taken
    else Requires human review
        System->>Admin: Flag for manual review
        Admin->>System: Access Admin Dashboard
        System->>DB: Fetch report details
        DB-->>Admin: Display report and content
        Admin->>System: Make moderation decision
        alt Ban user
            System->>DB: Update user status to banned
            System->>Target: Send account suspension notice
        else Warn user
            System->>Target: Send warning notification
            System->>DB: Log warning
        else Dismiss report
            System->>DB: Mark report as dismissed
        end
        System->>DB: Update report status
        System-->>User: Notify resolution
        System->>DB: Log admin action
    end
    Note over System: All actions logged for audit trail
    Note over Admin: Admin can generate reports for analytics
```
# Friendship System Sequence

```mermaid
sequenceDiagram
    autonumber
    participant UA as User A
    participant System as System
    participant MatchAPI as Matchmaking API
    participant FriendAPI as Friendship API
    participant DB as Firestore
    participant FCM as Push Notifications
    participant UB as User B
    
    UA->>System: Access contact search
    System->>MatchAPI: Request user suggestions
    MatchAPI->>DB: Query compatible users
    DB-->>MatchAPI: Return filtered users
    MatchAPI-->>System: Provide suggestions
    System-->>UA: Display potential contacts
    
    UA->>System: Send friend request to User B
    System->>FriendAPI: Process friend request
    FriendAPI->>DB: Check existing relationship
    
    alt No existing relationship
        FriendAPI->>DB: Create friendship record (pending)
        DB-->>FriendAPI: Confirm creation
        FriendAPI->>FCM: Send notification to User B
        FCM-->>UB: Friend request notification
        FriendAPI-->>System: Request sent confirmation
        System-->>UA: Show request pending
        
        UB->>System: View friend requests
        System->>DB: Fetch pending requests
        DB-->>System: Return requests list
        System-->>UB: Display friend requests
        
        UB->>System: Accept friend request
        System->>FriendAPI: Process acceptance
        FriendAPI->>DB: Update friendship status to accepted
        FriendAPI->>FCM: Notify User A of acceptance
        FCM-->>UA: Friend request accepted notification
        FriendAPI-->>System: Friendship established
        System-->>UB: Show new contact in list
        
        Note over System: Both users can now chat
        
    else Relationship exists
        FriendAPI-->>System: Return existing status
        System-->>UA: Show current relationship status
    end
    
    Note over DB: Mutual friendship enforced by Cloud Functions
    Note over System: All friendship changes trigger real-time updates
```

# UseCase Diagram 
```mermaid
graph TD
    User[User/Pen Pal]
    
    %% User Use Cases
    UC1[Sign Up / Log In]
    UC2[Complete Onboarding]
    UC3[View Home/Dashboard]
    UC4[Search Contacts]
    UC5[Send Friend Request]
    UC6[Chat with Contact]
    UC7[Edit Profile]
    UC8[Manage Settings]
    UC9[View Notifications]
    UC10[Block/Report User]
    UC11[Auto-Translate Messages]
    
    %% User connections
    User --> UC1
    User --> UC2
    User --> UC3
    User --> UC4
    User --> UC5
    User --> UC6
    User --> UC7
    User --> UC8
    User --> UC9
    User --> UC10
    
    %% Include/Extend
    UC6 -.->|includes| UC11
    UC6 -.->|includes| UC10
    UC4 -.->|extends| UC5
    
    %% Styling
    classDef userClass fill:#e1f5fe
    class User userClass
    
```
```mermaid
graph TD
    Admin[Administrator/Moderator]
    System[System]
    
    %% Admin Use Cases
    AC1[Admin Log In]
    AC2[View Admin Dashboard]
    AC3[Manage Users]
    AC4[Moderate Chat]
    AC5[View Analytics]
    AC6[Generate Reports]
    AC7[Manage Admin Settings]
    
    %% System Use Cases
    SC1[Process Matchmaking]
    SC2[Handle Realtime Updates]
    SC3[Auto-Moderate Content]
    SC4[Aggregate Analytics]
    
    %% Admin connections
    Admin --> AC1
    Admin --> AC2
    Admin --> AC3
    Admin --> AC4
    Admin --> AC5
    Admin --> AC6
    Admin --> AC7
    
    %% System connections
    System --> SC1
    System --> SC2
    System --> SC3
    System --> SC4
    
    %% Include relationships
    AC4 -.->|includes| SC3
    
    %% Styling
    classDef adminClass fill:#fff3e0
    classDef systemClass fill:#f3e5f5
    
    class Admin adminClass
    class System systemClass
```

# Class Diagram

```mermaid
classDiagram
    class User {
        +String userID
        +String email
        +String displayName
        +Boolean activeStatus
        +DateTime lastSeen
        +Boolean isAdmin
        +signUp()
        +logIn()
        +updateProfile()
        +deleteAccount()
        +deactivateAccount()
    }
    class Profile {
        +String userID
        +Int age
        +String gender
        +String residence
        +String bio
        +Array languages
        +Array interests
        +String timezone
        +Boolean hideActiveStatus
        +DateTime createdAt
        +updateProfile()
        +getProfile()
    }
    class Message {
        +String messageID
        +String senderID
        +String receiverID
        +String content
        +String translatedContent
        +DateTime timestamp
        +Array reactions
        +Boolean isRead
        +Boolean isFlagged
        +sendMessage()
        +addReaction()
        +markAsRead()
        +flagMessage()
    }
    class Friendship {
        +String friendshipID
        +String requesterID
        +String receiverID
        +String status
        +DateTime requestedAt
        +DateTime acceptedAt
        +sendRequest()
        +acceptRequest()
        +rejectRequest()
        +removeFriend()
    }
    class Notification {
        +String notificationID
        +String userID
        +String type
        +String content
        +Boolean isRead
        +DateTime createdAt
        +sendNotification()
        +markAsRead()
    }
    class Report {
        +String reportID
        +String reporterID
        +String reportedUserID
        +String reportedMessageID
        +String reason
        +String status
        +DateTime createdAt
        +DateTime resolvedAt
        +submitReport()
        +reviewReport()
        +resolveReport()
    }
    class Admin {
        +String adminID
        +Array permissions
        +DateTime lastLogin
        +viewDashboard()
        +manageUsers()
        +moderateContent()
        +generateReports()
        +viewAnalytics()
    }
    class Analytics {
        +String analyticsID
        +Date date
        +Int activeUsers
        +Int messagesSent
        +Int flaggedReports
        +Int newSignups
        +aggregateDaily()
        +generateCSV()
        +generatePDF()
    }
    User "1" --o "1" Profile : has
    User "1" --o "0..*" Message : sends
    User "1" --o "0..*" Message : receives
    User "1" --o "0..*" Friendship : initiates
    User "1" --o "0..*" Friendship : receives
    User "1" --o "0..*" Notification : receives
    User "1" --o "0..*" Report : files
    User "1" --o "0..*" Report : reported_in
    Admin "1" --|> User : extends
    Admin "1" --o "0..*" Report : reviews
    Admin "1" --o "0..*" Analytics : generates
    Message "1" --o "0..*" Report : flagged_in
```

# Architecture diagram

```mermaid
graph TB
    subgraph "Client Layer"
        Web[Web App]
        Mobile[Mobile App]
    end
    subgraph "Authentication"
        Auth[Firebase Authentication]
        OAuth[Third-party OAuth]
    end
    subgraph "API Layer"
        Gateway[API Gateway]
        UserAPI[User Management API]
        ChatAPI[Chat API]
        MatchAPI[Matchmaking API]
        FriendAPI[Friendship System API]
        ModAPI[Moderation API]
        TransAPI[Translation API]
        SearchAPI[Search API]
        PrivacyAPI[Privacy Settings API]
        RoleAPI[Role-based Access Control API]
    end
    subgraph "Business Logic"
        Functions[Cloud Functions]
        MatchEngine[Matching Engine]
        ContentMod[Content Moderation]
        Analytics[Analytics Engine]
        NotificationService[Notification Service]
    end
    subgraph "Database Layer"
        Firestore[(Firestore)]
        Storage[Firebase Storage]
    end
    subgraph "External Services"
        FCM[Firebase Cloud Messaging]
        Azure[Azure Content Safety]
        Perspective[Perspective API]
        TranslationSvc[Translation Service]
    end
    subgraph "Admin Interface"
        AdminWeb[Admin Dashboard]
        Reports[Report Generator]
    end
    Web --> Gateway
    Mobile --> Gateway
    AdminWeb --> Gateway
    Web --> Auth
    Mobile --> Auth
    AdminWeb --> Auth
    Auth --> OAuth
    Gateway --> UserAPI
    Gateway --> ChatAPI
    Gateway --> MatchAPI
    Gateway --> FriendAPI
    Gateway --> ModAPI
    Gateway --> TransAPI
    Gateway --> SearchAPI
    Gateway --> PrivacyAPI
    Gateway --> RoleAPI
    UserAPI --> Functions
    ChatAPI --> Functions
    MatchAPI --> MatchEngine
    FriendAPI --> Functions
    ModAPI --> ContentMod
    Functions --> Firestore
    MatchEngine --> Firestore
    ContentMod --> Firestore
    Analytics --> Firestore
    UserAPI --> Storage
    NotificationService --> FCM
    ContentMod --> Azure
    ContentMod --> Perspective
    TransAPI --> TranslationSvc
    Functions --> FCM
    Reports --> Functions
    Analytics --> Reports
    Firestore -.->|Real-time listeners| Web
    Firestore -.->|Real-time listeners| Mobile
    classDef clientLayer fill:#e3f2fd
    classDef apiLayer fill:#f3e5f5
    classDef businessLayer fill:#e8f5e8
    classDef dataLayer fill:#fff3e0
    classDef externalLayer fill:#fce4ec
    classDef adminLayer fill:#f1f8e9
    class Web,Mobile clientLayer
    class Gateway,UserAPI,ChatAPI,MatchAPI,FriendAPI,ModAPI,TransAPI,SearchAPI,PrivacyAPI,RoleAPI apiLayer
    class Functions,MatchEngine,ContentMod,Analytics,NotificationService businessLayer
    class Firestore,Storage dataLayer
    class FCM,Azure,Perspective,TranslationSvc externalLayer
    class AdminWeb,Reports adminLayer
```

# Data Flow Diagram

```mermaid
graph TD
    %% External Entities
    User[User]
    Admin[Administrator]
    
    %% Processes
    P1[1.0 User Authentication]
    P2[2.0 Profile Management]
    P3[3.0 Matchmaking System]
    P4[4.0 Chat Processing]
    P5[5.0 Friendship Management]
    P6[6.0 Content Moderation]
    P7[7.0 Notification System]
    P8[8.0 Analytics Processing]
    
    %% Data Stores
    DS1[(D1: User Profiles)]
    DS2[(D2: Messages)]
    DS3[(D3: Friendships)]
    DS4[(D4: Reports)]
    DS5[(D5: Moderation Logs)]
    DS6[(D6: Analytics)]
    DS7[(D7: Notifications)]
    
    %% External Systems
    EXT1[Translation API]
    EXT2[Content Moderation API]
    EXT3[Push Notification Service]
    
    %% User flows
    User -->|Login credentials| P1
    P1 -->|Authentication token| User
    P1 -->|User session| P2
    
    User -->|Profile data| P2
    P2 -->|Profile info| DS1
    DS1 -->|Profile details| P2
    P2 -->|Updated profile| User
    
    User -->|Search filters| P3
    P3 -->|User preferences| DS1
    DS1 -->|User data| P3
    P3 -->|Match suggestions| User
    
    User -->|Friend request| P5
    P5 -->|Friendship data| DS3
    DS3 -->|Friend status| P5
    P5 -->|Request result| User
    P5 -->|Friendship notification| P7
    
    User -->|Chat message| P4
    P4 -->|Translation request| EXT1
    EXT1 -->|Translated text| P4
    P4 -->|Message data| DS2
    DS2 -->|Chat history| P4
    P4 -->|Message delivery| User
    P4 -->|New message notification| P7
    
    User -->|Report content| P6
    P6 -->|Moderation request| EXT2
    EXT2 -->|Content analysis| P6
    P6 -->|Report data| DS4
    P6 -->|Moderation log| DS5
    
    %% Admin flows
    Admin -->|Moderation action| P6
    P6 -->|Action result| Admin
    DS4 -->|Report details| P6
    DS5 -->|Moderation history| P6
    
    Admin -->|Analytics request| P8
    P8 -->|Usage data| DS1
    P8 -->|Message stats| DS2
    P8 -->|Report stats| DS4
    DS6 -->|Analytics data| P8
    P8 -->|Analytics report| Admin
    
    %% Notification flows
    P7 -->|Notification data| DS7
    P7 -->|Push notification| EXT3
    EXT3 -->|Notification delivery| User
    DS7 -->|Notification history| P7
    
    %% System data flows
    P4 -->|Usage metrics| P8
    P5 -->|Activity metrics| P8
    P6 -->|Moderation metrics| P8
    P8 -->|Aggregated data| DS6
    
    classDef entity fill:#ffeb3b
    classDef process fill:#4caf50
    classDef datastore fill:#2196f3
    classDef external fill:#ff9800
    
    class User,Admin entity
    class P1,P2,P3,P4,P5,P6,P7,P8 process
    class DS1,DS2,DS3,DS4,DS5,DS6,DS7 datastore
    class EXT1,EXT2,EXT3 external
```

# Component Diagram
```mermaid
graph TB
    subgraph "Frontend Components"
        WebApp[Web Application]
        MobileApp[Mobile Application]
        AdminDash[Admin Dashboard]
        
        subgraph "Web App Modules"
            AuthComp[Authentication Component]
            ChatComp[Chat Component]
            ProfileComp[Profile Component]
            SearchComp[Search Component]
            NotifComp[Notification Component]
        end
        
        subgraph "Mobile App Modules"
            MAuthComp[Mobile Auth Component]
            MChatComp[Mobile Chat Component]
            MProfileComp[Mobile Profile Component]
            MSearchComp[Mobile Search Component]
            MNotifComp[Mobile Notification Component]
        end
        
        subgraph "Admin Modules"
            AdminAuth[Admin Authentication]
            UserMgmt[User Management]
            ContentMod[Content Moderation]
            Analytics[Analytics Dashboard]
            ReportGen[Report Generator]
        end
    end
    
    subgraph "Backend Components"
        APIGateway[API Gateway]
        
        subgraph "Core Services"
            UserService[User Service]
            ChatService[Chat Service]
            MatchService[Matchmaking Service]
            FriendService[Friendship Service]
            ModerationService[Moderation Service]
            NotificationService[Notification Service]
            AnalyticsService[Analytics Service]
        end
        
        subgraph "Infrastructure Components"
            AuthProvider[Firebase Auth Provider]
            DatabaseLayer[Database Access Layer]
            StorageLayer[Storage Access Layer]
            CloudFunctions[Cloud Functions Runtime]
        end
    end
    
    subgraph "External Services"
        TranslationAPI[Translation API]
        ContentModerationAPI[Content Moderation API]
        PushService[Push Notification Service]
        ThirdPartyAuth[OAuth Providers]
    end
    
    subgraph "Data Layer"
        Firestore[(Firestore Database)]
        FirebaseStorage[Firebase Storage]
        Cache[Redis Cache]
    end
    
    %% Frontend to Backend connections
    WebApp --> APIGateway
    MobileApp --> APIGateway
    AdminDash --> APIGateway
    
    %% Component internal connections
    WebApp --> AuthComp
    WebApp --> ChatComp
    WebApp --> ProfileComp
    WebApp --> SearchComp
    WebApp --> NotifComp
    
    MobileApp --> MAuthComp
    MobileApp --> MChatComp
    MobileApp --> MProfileComp
    MobileApp --> MSearchComp
    MobileApp --> MNotifComp
    
    AdminDash --> AdminAuth
    AdminDash --> UserMgmt
    AdminDash --> ContentMod
    AdminDash --> Analytics
    AdminDash --> ReportGen
    
    %% API Gateway to Services
    APIGateway --> UserService
    APIGateway --> ChatService
    APIGateway --> MatchService
    APIGateway --> FriendService
    APIGateway --> ModerationService
    APIGateway --> NotificationService
    APIGateway --> AnalyticsService
    
    %% Services to Infrastructure
    UserService --> AuthProvider
    UserService --> DatabaseLayer
    ChatService --> DatabaseLayer
    ChatService --> CloudFunctions
    MatchService --> DatabaseLayer
    FriendService --> DatabaseLayer
    FriendService --> CloudFunctions
    ModerationService --> DatabaseLayer
    ModerationService --> CloudFunctions
    NotificationService --> CloudFunctions
    AnalyticsService --> DatabaseLayer
    AnalyticsService --> CloudFunctions
    
    %% Infrastructure to Data Layer
    DatabaseLayer --> Firestore
    StorageLayer --> FirebaseStorage
    DatabaseLayer --> Cache
    
    %% External Service connections
    AuthProvider --> ThirdPartyAuth
    ChatService --> TranslationAPI
    ModerationService --> ContentModerationAPI
    NotificationService --> PushService
    CloudFunctions --> PushService
    
    %% Real-time connections
    Firestore -.->|Real-time listeners| ChatComp
    Firestore -.->|Real-time listeners| MChatComp
    Firestore -.->|Real-time listeners| NotifComp
    Firestore -.->|Real-time listeners| MNotifComp
    
    classDef frontend fill:#e3f2fd
    classDef backend fill:#e8f5e8
    classDef external fill:#fff3e0
    classDef data fill:#f3e5f5
    
    class WebApp,MobileApp,AdminDash,AuthComp,ChatComp,ProfileComp,SearchComp,NotifComp,MAuthComp,MChatComp,MProfileComp,MSearchComp,MNotifComp,AdminAuth,UserMgmt,ContentMod,Analytics,ReportGen frontend
    class APIGateway,UserService,ChatService,MatchService,FriendService,ModerationService,NotificationService,AnalyticsService,AuthProvider,DatabaseLayer,StorageLayer,CloudFunctions backend
    class TranslationAPI,ContentModerationAPI,PushService,ThirdPartyAuth external
    class Firestore,FirebaseStorage,Cache data
```

# Deployment diagram

```mermaid
graph TB
    subgraph "Client Devices"
        subgraph "User Devices"
            Browser[Web Browser]
            MobileDevice[Mobile Device]
        end
        
        subgraph "Admin Devices"
            AdminBrowser[Admin Web Browser]
        end
    end
    
    subgraph "CDN / Load Balancer"
        CDN[Content Delivery Network]
        LoadBalancer[Load Balancer]
    end
    
    subgraph "Google Cloud Platform"
        subgraph "Firebase Hosting"
            WebServer[Static Web Assets]
            AdminWeb[Admin Interface]
        end
        
        subgraph "App Engine / Cloud Run"
            APIServer1[API Server Instance 1]
            APIServer2[API Server Instance 2]
            APIGateway[API Gateway]
        end
        
        subgraph "Cloud Functions"
            CF1[Matchmaking Functions]
            CF2[Moderation Functions]
            CF3[Analytics Functions]
            CF4[Notification Functions]
        end
        
        subgraph "Firebase Services"
            FireAuth[Firebase Authentication]
            FireDB[(Firestore Database)]
            FireStorage[Firebase Storage]
            FCM[Firebase Cloud Messaging]
        end
        
        subgraph "Monitoring & Logging"
            CloudLogging[Cloud Logging]
            CloudMonitoring[Cloud Monitoring]
            ErrorReporting[Error Reporting]
        end
    end
    
    subgraph "External Services"
        TranslateAPI[Google Translate API]
        AzureContent[Azure Content Safety]
        PerspectiveAPI[Perspective API]
        OAuth[OAuth Providers<br/>Google, Facebook, etc.]
    end
    
    subgraph "Mobile App Stores"
        AppStore[Apple App Store]
        PlayStore[Google Play Store]
    end
    
    %% Client connections
    Browser -->|HTTPS| CDN
    MobileDevice -->|HTTPS/WSS| LoadBalancer
    AdminBrowser -->|HTTPS| CDN
    
    %% CDN and Load Balancer
    CDN --> WebServer
    CDN --> AdminWeb
    LoadBalancer --> APIGateway
    
    %% API Gateway routing
    APIGateway --> APIServer1
    APIGateway --> APIServer2
    
    %% API Server connections
    APIServer1 --> CF1
    APIServer1 --> CF2
    APIServer1 --> CF3
    APIServer1 --> CF4
    APIServer2 --> CF1
    APIServer2 --> CF2
    APIServer2 --> CF3
    APIServer2 --> CF4
    
    %% Cloud Functions to Firebase
    CF1 --> FireDB
    CF2 --> FireDB
    CF3 --> FireDB
    CF4 --> FireDB
    CF4 --> FCM
    
    %% Firebase connections
    APIServer1 --> FireAuth
    APIServer2 --> FireAuth
    APIServer1 --> FireDB
    APIServer2 --> FireDB
    APIServer1 --> FireStorage
    APIServer2 --> FireStorage
    
    %% External API connections
    CF1 --> TranslateAPI
    CF2 --> AzureContent
    CF2 --> PerspectiveAPI
    FireAuth --> OAuth
    
    %% Mobile app distribution
    MobileDevice -.->|Download/Update| AppStore
    MobileDevice -.->|Download/Update| PlayStore
    
    %% Real-time connections
    FireDB -.->|Real-time updates| Browser
    FireDB -.->|Real-time updates| MobileDevice
    FCM -.->|Push notifications| MobileDevice
    
    %% Monitoring connections
    APIServer1 --> CloudLogging
    APIServer2 --> CloudLogging
    CF1 --> CloudMonitoring
    CF2 --> CloudMonitoring
    CF3 --> CloudMonitoring
    CF4 --> CloudMonitoring
    APIServer1 --> ErrorReporting
    APIServer2 --> ErrorReporting
    
    %% Deployment specifications
    WebServer -.->|"Static Files<br/>React/Vue Build"| Browser
    AdminWeb -.->|"Admin SPA<br/>React Dashboard"| AdminBrowser
    APIServer1 -.->|"Node.js/Python<br/>Auto-scaling"| APIGateway
    APIServer2 -.->|"Node.js/Python<br/>Auto-scaling"| APIGateway
    
    classDef client fill:#e3f2fd
    classDef hosting fill:#e8f5e8
    classDef compute fill:#fff3e0
    classDef database fill:#f3e5f5
    classDef external fill:#fce4ec
    classDef monitoring fill:#f1f8e9
    
    class Browser,MobileDevice,AdminBrowser,AppStore,PlayStore client
    class CDN,LoadBalancer,WebServer,AdminWeb hosting
    class APIServer1,APIServer2,APIGateway,CF1,CF2,CF3,CF4 compute
    class FireAuth,FireDB,FireStorage,FCM database
    class TranslateAPI,AzureContent,PerspectiveAPI,OAuth external
    class CloudLogging,CloudMonitoring,ErrorReporting monitoring
```
# User State Diagram
```mermaid
stateDiagram-v2
    [*] --> Guest: User visits app
    
    Guest --> Registering: Click Sign Up
    Guest --> LoggingIn: Click Sign In
    
    Registering --> Onboarding: Registration successful
    LoggingIn --> Onboarding: First time login
    LoggingIn --> Active: Returning user login
    
    Onboarding --> ProfileSetup: Begin profile creation
    ProfileSetup --> Active: Profile completed
    
    state Active {
        [*] --> Online
        Online --> Browsing: Browse contacts
        Online --> Chatting: Open chat
        Online --> Managing: Access settings
        
        Browsing --> Online: Return to home
        Browsing --> RequestSent: Send friend request
        
        RequestSent --> Online: Request processed
        RequestSent --> Connected: Request accepted
        
        Connected --> Chatting: Start conversation
        
        Chatting --> Online: Close chat
        Chatting --> Typing: Composing message
        Chatting --> Reacting: Add emoji reaction
        
        Typing --> Chatting: Send message
        Reacting --> Chatting: Reaction added
        
        Managing --> Online: Settings saved
        Managing --> ProfileEditing: Edit profile
        Managing --> PrivacySettings: Adjust privacy
        
        ProfileEditing --> Managing: Profile updated
        PrivacySettings --> Managing: Privacy updated
        
        Online --> Idle: No activity (5 min)
        Idle --> Online: User interaction
        Idle --> Offline: Extended inactivity
        Offline --> Online: User returns
    }
    
    Active --> Reported: User gets reported
    Active --> Blocked: User blocks someone
    Active --> Suspended: Admin action
    Active --> Deactivated: User deactivates
    Active --> Deleted: User deletes account
    
    Reported --> UnderReview: Admin reviews report
    UnderReview --> Active: Report dismissed
    UnderReview --> Warned: Warning issued
    UnderReview --> Suspended: Violation confirmed
    
    Warned --> Active: Warning acknowledged
    
    Suspended --> Active: Suspension lifted
    Suspended --> Banned: Repeated violations
    
    Blocked --> Active: User can unblock
    
    Deactivated --> Active: User reactivates
    Deactivated --> Deleted: Auto-delete after 30 days
    
    Banned --> [*]: Account permanently closed
    Deleted --> [*]: Account permanently removed
    
    note right of Active
        User can perform all app functions:
        - Send/receive messages
        - Make friend requests
        - Update profile
        - Access all features
    end note
    
    note right of Suspended
        Limited access:
        - Cannot send messages
        - Cannot make friend requests
        - Read-only access
    end note
    
    note right of Banned
        No access:
        - Account locked
        - Cannot log in
        - All data preserved for audit
    end note
```
# Chat State Diagram

```mermaid
stateDiagram-v2
    [*] --> NoConversation: User has no active chats
    
    NoConversation --> SearchingContacts: User searches for contacts
    NoConversation --> ViewingRequests: User checks friend requests
    
    SearchingContacts --> RequestSent: Send friend request
    SearchingContacts --> NoConversation: Cancel search
    
    ViewingRequests --> RequestAccepted: Accept friend request
    ViewingRequests --> RequestRejected: Reject friend request
    ViewingRequests --> NoConversation: Close requests
    
    RequestSent --> Pending: Waiting for response
    RequestAccepted --> Connected: Friendship established
    RequestRejected --> NoConversation: Request declined
    
    Pending --> Connected: Request accepted by other user
    Pending --> RequestRejected: Request declined by other user
    Pending --> NoConversation: Cancel pending request
    
    Connected --> ChatActive: Open chat window
    
    state ChatActive {
        [*] --> Viewing
        
        Viewing --> Composing: Start typing message
        Viewing --> Reacting: Click emoji reaction
        Viewing --> MenuOpen: Open chat menu
        
        Composing --> Sending: Send message
        Composing --> Viewing: Cancel message
        
        Sending --> MessageSent: Message delivered
        MessageSent --> Viewing: Return to chat
        
        Reacting --> ReactionSent: Reaction added
        ReactionSent --> Viewing: Return to chat
        
        MenuOpen --> Viewing: Close menu
        MenuOpen --> Blocking: Block user
        MenuOpen --> Reporting: Report user
        MenuOpen --> ViewingProfile: View user profile
        
        ViewingProfile --> Viewing: Return to chat
        
        Blocking --> [*]: Chat blocked, exit to home
        Reporting --> [*]: Report submitted, exit to home
    }
    
    ChatActive --> Connected: Close chat window
    ChatActive --> Blocked: User blocked
    ChatActive --> Reported: User reported
    
    Connected --> Unfriended: Remove friend
    Unfriended --> NoConversation: Friendship ended
    
    Blocked --> [*]: Chat permanently closed
    Reported --> UnderReview: Report submitted for moderation
    
    UnderReview --> Connected: Report dismissed
    UnderReview --> Blocked: Report confirmed
    
    %% Message status substates
    state MessageSent {
        Delivered --> Read: Recipient opens chat
        Delivered --> Translated: Auto-translation applied
        Translated --> Read: Recipient reads translated message
    }
    
    note right of Connected
        Users can exchange messages,
        see online status,
        react with emojis
    end note
    
    note right of Blocked
        No further communication possible,
        chat history preserved
    end note
    
    note left of Pending
        Friend request sent,
        waiting for other user's response
    end note
```




# Activity Diagrams



# Activity Diagram (User Registration Flow)
```mermaid
flowchart TD
    Start([User starts registration])
    
    ChooseAuth{Choose authentication method}
    EmailAuth[Enter email and password]
    OAuthAuth[Select OAuth provider]
    
    ValidateEmail{Email valid?}
    ValidateOAuth{OAuth successful?}
    
    CreateAccount[Create Firebase account]
    AccountExists{Account already exists?}
    
    SendVerification[Send verification email]
    WaitVerification[Wait for email verification]
    CheckVerification{Email verified?}
    
    StartOnboarding[Begin onboarding process]
    
    %% Profile Setup Flow
    EnterBasicInfo[Enter age and gender]
    ValidateBasic{Information valid?}
    
    EnterLocation[Enter residence/location]
    ValidateLocation{Location valid?}
    
    EnterLanguages[Select spoken languages]
    ValidateLanguages{At least one language?}
    
    EnterInterests[Add interests and hobbies]
    
    WriteBio[Write personal bio]
    ValidateBio{Bio appropriate?}
    
    SetPrivacy[Configure privacy settings]
    
    ReviewProfile[Review profile information]
    ConfirmProfile{Confirm profile?}
    
    SaveProfile[Save to Firestore]
    ProfileSaved{Profile saved successfully?}
    
    GenerateMatches[Generate initial matches]
    
    ShowWelcome[Show welcome message]
    RedirectHome[Redirect to Home/Dashboard]
    
    End([Registration complete])
    
    %% Error handling
    ShowEmailError[Show email format error]
    ShowOAuthError[Show OAuth error]
    ShowLoginOption[Show login option]
    ShowVerificationError[Resend verification email]
    ShowValidationError[Show field validation error]
    ShowBioError[Show inappropriate content warning]
    ShowSaveError[Show save error, retry]
    
    %% Flow connections
    Start --> ChooseAuth
    
    ChooseAuth -->|Email| EmailAuth
    ChooseAuth -->|OAuth| OAuthAuth
    
    EmailAuth --> ValidateEmail
    ValidateEmail -->|No| ShowEmailError
    ShowEmailError --> EmailAuth
    ValidateEmail -->|Yes| CreateAccount
    
    OAuthAuth --> ValidateOAuth
    ValidateOAuth -->|No| ShowOAuthError
    ShowOAuthError --> ChooseAuth
    ValidateOAuth -->|Yes| CreateAccount
    
    CreateAccount --> AccountExists
    AccountExists -->|Yes| ShowLoginOption
    ShowLoginOption --> ChooseAuth
    AccountExists -->|No| SendVerification
    
    SendVerification --> WaitVerification
    WaitVerification --> CheckVerification
    CheckVerification -->|No| ShowVerificationError
    ShowVerificationError --> WaitVerification
    CheckVerification -->|Yes| StartOnboarding
    
    StartOnboarding --> EnterBasicInfo
    EnterBasicInfo --> ValidateBasic
    ValidateBasic -->|No| ShowValidationError
    ShowValidationError --> EnterBasicInfo
    ValidateBasic -->|Yes| EnterLocation
    
    EnterLocation --> ValidateLocation
    ValidateLocation -->|No| ShowValidationError
    ValidateLocation -->|Yes| EnterLanguages
    
    EnterLanguages --> ValidateLanguages
    ValidateLanguages -->|No| ShowValidationError
    ValidateLanguages -->|Yes| EnterInterests
    
    EnterInterests --> WriteBio
    WriteBio --> ValidateBio
    ValidateBio -->|No| ShowBioError
    ShowBioError --> WriteBio
    ValidateBio -->|Yes| SetPrivacy
    
    SetPrivacy --> ReviewProfile
    ReviewProfile --> ConfirmProfile
    ConfirmProfile -->|No| EnterBasicInfo
    ConfirmProfile -->|Yes| SaveProfile
    
    SaveProfile --> ProfileSaved
    ProfileSaved -->|No| ShowSaveError
    ShowSaveError --> SaveProfile
    ProfileSaved -->|Yes| GenerateMatches
    
    GenerateMatches --> ShowWelcome
    ShowWelcome --> RedirectHome
    RedirectHome --> End
    
    classDef startEnd fill:#ffcdd2
    classDef process fill:#c8e6c9
    classDef decision fill:#fff9c4
    classDef error fill:#ffcccb
    
    class Start,End startEnd
    class EmailAuth,OAuthAuth,CreateAccount,SendVerification,StartOnboarding,EnterBasicInfo,EnterLocation,EnterLanguages,EnterInterests,WriteBio,SetPrivacy,ReviewProfile,SaveProfile,GenerateMatches,ShowWelcome,RedirectHome process
    class ChooseAuth,ValidateEmail,ValidateOAuth,AccountExists,CheckVerification,ValidateBasic,ValidateLocation,ValidateLanguages,ValidateBio,ConfirmProfile,ProfileSaved decision
    class ShowEmailError,ShowOAuthError,ShowLoginOption,ShowVerificationError,ShowValidationError,ShowBioError,ShowSaveError error
```



# Message Processing Activity Diagram

```mermaid
flowchart TD
    Start([User sends message])
    
    ValidateInput{Message content valid?}
    ShowError[Show validation error]
    
    CheckFriendship{Users are friends?}
    ShowAuthError[Show authorization error]
    
    StoreMessage[Store original message in Firestore]
    StorageSuccess{Storage successful?}
    ShowStorageError[Show storage error]
    
    CheckTranslation{Translation needed?}
    
    %% Translation branch
    CallTranslationAPI[Call Translation API]
    TranslationSuccess{Translation successful?}
    StoreTranslation[Store translated message]
    UseOriginal[Use original message only]
    
    %% Content moderation branch
    parallel1{{Parallel Processing}}
    
    CheckContent[Analyze content with Moderation API]
    ContentClean{Content appropriate?}
    FlagContent[Flag message for review]
    AutoDelete[Auto-delete offensive content]
    NotifyModerators[Notify moderators]
    
    %% Notification branch
    PrepareNotification[Prepare push notification]
    CheckRecipientStatus{Recipient online?}
    SendPushNotification[Send FCM push notification]
    SkipPush[Skip push notification]
    
    %% Real-time update branch
    UpdateFirestore[Update Firestore with real-time trigger]
    NotifyRecipient[Send real-time update to recipient]
    UpdateSender[Confirm delivery to sender]
    
    %% Analytics branch
    LogAnalytics[Update message analytics]
    IncrementCount[Increment daily message count]
    
    End([Message processing complete])
    
    %% Error retry paths
    RetryStorage[Retry storage operation]
    RetryTranslation[Retry translation]
    RetryNotification[Retry notification]
    
    %% Main flow
    Start --> ValidateInput
    ValidateInput -->|No| ShowError
    ShowError --> Start
    ValidateInput -->|Yes| CheckFriendship
    
    CheckFriendship -->|No| ShowAuthError
    ShowAuthError --> End
    CheckFriendship -->|Yes| StoreMessage
    
    StoreMessage --> StorageSuccess
    StorageSuccess -->|No| ShowStorageError
    ShowStorageError --> RetryStorage
    RetryStorage --> StoreMessage
    StorageSuccess -->|Yes| CheckTranslation
    
    CheckTranslation -->|Yes| CallTranslationAPI
    CheckTranslation -->|No| parallel1
    
    CallTranslationAPI --> TranslationSuccess
    TranslationSuccess -->|Yes| StoreTranslation
    TranslationSuccess -->|No| RetryTranslation
    RetryTranslation --> CallTranslationAPI
    StoreTranslation --> parallel1
    TranslationSuccess -->|Retry failed| UseOriginal
    UseOriginal --> parallel1
    
    %% Parallel processing branches
    parallel1 --> CheckContent
    parallel1 --> PrepareNotification
    parallel1 --> UpdateFirestore
    parallel1 --> LogAnalytics
    
    %% Content moderation flow
    CheckContent --> ContentClean
    ContentClean -->|Yes| UpdateFirestore
    ContentClean -->|No| FlagContent
    FlagContent --> AutoDelete
    AutoDelete --> NotifyModerators
    NotifyModerators --> End
    
    %% Notification flow
    PrepareNotification --> CheckRecipientStatus
    CheckRecipientStatus -->|Offline| SendPushNotification
    CheckRecipientStatus -->|Online| SkipPush
    SendPushNotification --> NotifyRecipient
    SkipPush --> NotifyRecipient
    
    %% Real-time update flow
    UpdateFirestore --> NotifyRecipient
    NotifyRecipient --> UpdateSender
    UpdateSender --> End
    
    %% Analytics flow
    LogAnalytics --> IncrementCount
    IncrementCount --> End
    
    classDef startEnd fill:#ffcdd2
    classDef process fill:#c8e6c9
    classDef decision fill:#fff9c4
    classDef error fill:#ffcccb
    classDef parallel fill:#e1bee7
    
    class Start,End startEnd
    class StoreMessage,CallTranslationAPI,StoreTranslation,CheckContent,PrepareNotification,UpdateFirestore,SendPushNotification,NotifyRecipient,UpdateSender,LogAnalytics,IncrementCount,FlagContent,AutoDelete,NotifyModerators process
    class ValidateInput,CheckFriendship,StorageSuccess,CheckTranslation,TranslationSuccess,ContentClean,CheckRecipientStatus decision
    class ShowError,ShowAuthError,ShowStorageError,RetryStorage,RetryTranslation,RetryNotification error
    class parallel1 parallel
```

# Admin Moderation Activity Diagram

# GlobeTalk - Admin Moderation Activity Diagram

```mermaid
flowchart TD
    Start([Admin starts moderation review])
    
    AccessDashboard[Access Admin Dashboard]
    CheckReports{Pending reports available?}
    
    NoReports[No reports to review]
    SelectReport[Select report to review]
    
    LoadReportDetails[Load report details from Firestore]
    ReviewContent[Review flagged content]
    CheckContext[Check conversation context]
    ReviewUserHistory[Review user's moderation history]
    
    MakeDecision{Moderation decision}
    
    %% Decision branches
    DismissReport[Dismiss report as invalid]
    WarnUser[Issue warning to user]
    SuspendUser[Suspend user account]
    BanUser[Permanently ban user]
    DeleteContent[Delete specific content]
    
    %% Dismiss flow
    UpdateReportStatus1[Update report status to dismissed]
    NotifyReporter1[Notify reporter of decision]
    LogAction1[Log dismissal action]
    
    %% Warning flow
    SendWarningNotification[Send warning notification to user]
    UpdateReportStatus2[Update report status to resolved]
    LogAction2[Log warning action]
    NotifyReporter2[Notify reporter of action taken]
    
    %% Suspension flow
    UpdateUserStatus1[Update user status to suspended]
    SetSuspensionDuration[Set suspension duration]
    SendSuspensionNotification[Send suspension notification]
    UpdateReportStatus3[Update report status to resolved]
    LogAction3[Log suspension action]
    NotifyReporter3[Notify reporter of action taken]
    
    %% Ban flow
    UpdateUserStatus2[Update user status to banned]
    SendBanNotification[Send ban notification]
    UpdateReportStatus4[Update report status to resolved]
    LogAction4[Log ban action]
    NotifyReporter4[Notify reporter of action taken]
    ArchiveUserData[Archive user data for audit]
    
    %% Delete content flow
    RemoveOffensiveContent[Remove flagged content]
    UpdateMessageStatus[Mark message as deleted]
    UpdateReportStatus5[Update report status to resolved]
    LogAction5[Log content deletion]
    NotifyReporter5[Notify reporter of action taken]
    
    %% Analytics update
    UpdateAnalytics[Update moderation analytics]
    
    CheckMoreReports{More reports to review?}
    
    GenerateReport{Generate summary report?}
    CreateCSVReport[Generate CSV report]
    CreatePDFReport[Generate PDF report]
    DownloadReport[Download report file]
    
    End([Moderation session complete])
    
    %% Flow connections
    Start --> AccessDashboard
    AccessDashboard --> CheckReports
    
    CheckReports -->|No| NoReports
    CheckReports -->|Yes| SelectReport
    NoReports --> End
    
    SelectReport --> LoadReportDetails
    LoadReportDetails --> ReviewContent
    ReviewContent --> CheckContext
    CheckContext --> ReviewUserHistory
    ReviewUserHistory --> MakeDecision
    
    MakeDecision -->|Dismiss| DismissReport
    MakeDecision -->|Warning| WarnUser
    MakeDecision -->|Suspend| SuspendUser
    MakeDecision -->|Ban| BanUser
    MakeDecision -->|Delete Content| DeleteContent
    
    %% Dismiss path
    DismissReport --> UpdateReportStatus1
    UpdateReportStatus1 --> NotifyReporter1
    NotifyReporter1 --> LogAction1
    LogAction1 --> UpdateAnalytics
    
    %% Warning path
    WarnUser --> SendWarningNotification
    SendWarningNotification --> UpdateReportStatus2
    UpdateReportStatus2 --> LogAction2
    LogAction2 --> NotifyReporter2
    NotifyReporter2 --> UpdateAnalytics
    
    %% Suspension path
    SuspendUser --> UpdateUserStatus1
    UpdateUserStatus1 --> SetSuspensionDuration
    SetSuspensionDuration --> SendSuspensionNotification
    SendSuspensionNotification --> UpdateReportStatus3
    UpdateReportStatus3 --> LogAction3
    LogAction3 --> NotifyReporter3
    NotifyReporter3 --> UpdateAnalytics
    
    %% Ban path
    BanUser --> UpdateUserStatus2
    UpdateUserStatus2 --> SendBanNotification
    SendBanNotification --> UpdateReportStatus4
    UpdateReportStatus4 --> LogAction4
    LogAction4 --> NotifyReporter4
    NotifyReporter4 --> ArchiveUserData
    ArchiveUserData --> UpdateAnalytics
    
    %% Delete content path
    DeleteContent --> RemoveOffensiveContent
    RemoveOffensiveContent --> UpdateMessageStatus
    UpdateMessageStatus --> UpdateReportStatus5
    UpdateReportStatus5 --> LogAction5
    LogAction5 --> NotifyReporter5
    NotifyReporter5 --> UpdateAnalytics
    
    %% Continue flow
    UpdateAnalytics --> CheckMoreReports
    CheckMoreReports -->|Yes| SelectReport
    CheckMoreReports -->|No| GenerateReport
    
    GenerateReport -->|Yes CSV| CreateCSVReport
    GenerateReport -->|Yes PDF| CreatePDFReport
    GenerateReport -->|No| End
    
    CreateCSVReport --> DownloadReport
    CreatePDFReport --> DownloadReport
    DownloadReport --> End
    
    classDef startEnd fill:#ffcdd2
    classDef process fill:#c8e6c9
    classDef decision fill:#fff9c4
    classDef action fill:#fff3e0
    
    class Start,End,NoReports startEnd
    class AccessDashboard,LoadReportDetails,ReviewContent,CheckContext,ReviewUserHistory,UpdateReportStatus1,UpdateReportStatus2,UpdateReportStatus3,UpdateReportStatus4,UpdateReportStatus5,UpdateAnalytics,CreateCSVReport,CreatePDFReport,DownloadReport process
    class CheckReports,MakeDecision,CheckMoreReports,GenerateReport decision
    class DismissReport,WarnUser,SuspendUser,BanUser,DeleteContent,SendWarningNotification,SendSuspensionNotification,SendBanNotification,RemoveOffensiveContent,UpdateUserStatus1,UpdateUserStatus2,LogAction1,LogAction2,LogAction3,LogAction4,LogAction5 action
```
