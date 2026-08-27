# ShortJob — MVP, SRS and Microservices Diagrams

> Repository basis: `Ajay120503/ShortJob` (reviewed 27 August 2026).  
> Architecture note: the repository currently contains one Express backend with modular routes/controllers. The microservices diagrams below describe a proposed target architecture.

## 1. Product System Context

```mermaid
flowchart TB
    Visitor["Visitor"]
    Member["Registered Member"]
    Recruiter["Job Poster / Recruiter"]
    Admin["Admin / Super Admin"]
    SJ["ShortJob Platform"]
    Email["Email / SMTP"]
    Media["Cloudinary Media"]
    DB[("MongoDB")]

    Visitor -->|"Browse approved content and jobs"| SJ
    Member -->|"Network, post, apply and chat"| SJ
    Recruiter -->|"Publish jobs and manage applicants"| SJ
    Admin -->|"Moderate content and manage users"| SJ
    SJ -->|"OTP and account messages"| Email
    SJ -->|"Upload and delete media"| Media
    SJ -->|"Persist platform data"| DB
```

## 2. MVP Scope

```mermaid
flowchart TB
    Goal["MVP Goal: connect members with short-term jobs and professional contacts"]

    subgraph Must["MUST — MVP Release"]
        A["Secure registration, OTP, login and profile"]
        B["Create, browse, filter and view jobs"]
        C["Apply or quick-apply to a job"]
        D["Recruiter views applicants and updates status"]
        E["Social feed: create, view, like and comment"]
        F["One-to-one chat and notifications"]
        G["Admin moderation and user controls"]
    end

    subgraph Next["SHOULD — Post-MVP"]
        H["Stories, saved posts and skill endorsements"]
        I["Matched jobs, job Q&A and reach analytics"]
        J["Applicant export, kanban and profile strength"]
        K["Login audit with location and face photo"]
    end

    subgraph Later["COULD — Future"]
        L["ML recommendations and semantic search"]
        M["Organization accounts and recruiter teams"]
        N["Video interviews and calendar integration"]
        O["Payments, subscriptions and premium features"]
    end

    Goal --> Must
    Must -->|"Validated adoption"| Next
    Next -->|"Scale and monetize"| Later
```

## 3. MVP Success Flow

```mermaid
journey
    title Core ShortJob MVP journey
    section Join
      Register and verify OTP: 4: Member
      Complete professional profile: 4: Member
    section Discover
      Browse or filter opportunities: 5: Member
      Open matched job details: 5: Member
    section Apply
      Apply or quick-apply: 5: Member
      Receive status notification: 4: Member
    section Recruit
      Review applicants: 4: Recruiter
      Shortlist or select applicant: 5: Recruiter
    section Connect
      Start one-to-one chat: 5: Member, Recruiter
```

## 4. SRS — Actors and Use Cases

```mermaid
flowchart LR
    Visitor["Visitor"]
    Member["Member"]
    Recruiter["Job Poster"]
    Admin["Admin"]
    Super["Super Admin"]

    subgraph System["ShortJob Functional Boundary"]
        UC1(["Register / Login"])
        UC2(["Manage Profile and Network"])
        UC3(["Browse Feed, Stories and Jobs"])
        UC4(["Create Social Content"])
        UC5(["Create and Manage Jobs"])
        UC6(["Apply and Track Application"])
        UC7(["Chat and Receive Notifications"])
        UC8(["Review Moderation Queue"])
        UC9(["Manage Users, Roles and Settings"])
    end

    Visitor --> UC1
    Visitor --> UC3
    Member --> UC2
    Member --> UC3
    Member --> UC4
    Member --> UC6
    Member --> UC7
    Recruiter --> UC5
    Recruiter --> UC7
    Admin --> UC8
    Admin --> UC9
    Super --> UC9
```

## 5. SRS — Functional Requirement Map

```mermaid
flowchart TB
    SRS["ShortJob Functional Requirements"]

    SRS --> AUTH["FR-01 Authentication"]
    AUTH --> AUTH1["OTP registration and email verification"]
    AUTH --> AUTH2["JWT cookie login, refresh and logout"]
    AUTH --> AUTH3["Password recovery and account deletion"]

    SRS --> USER["FR-02 Profile and Network"]
    USER --> USER1["Profile, skills, qualifications and timeline"]
    USER --> USER2["Search, follow and skill endorsement"]
    USER --> USER3["Presence privacy and opportunity status"]

    SRS --> SOCIAL["FR-03 Social Content"]
    SOCIAL --> SOCIAL1["Posts, images, types, likes and saves"]
    SOCIAL --> SOCIAL2["Nested comments and replies"]
    SOCIAL --> SOCIAL3["Stories with views and expiry"]

    SRS --> JOB["FR-04 Jobs and Applications"]
    JOB --> JOB1["Publish, edit, filter and remove jobs"]
    JOB --> JOB2["Apply, quick-apply and prevent duplicates"]
    JOB --> JOB3["Applicant status, Q&A, matching and statistics"]

    SRS --> COMMS["FR-05 Communication"]
    COMMS --> COMMS1["Conversation and text/media/file messages"]
    COMMS --> COMMS2["Typing, reactions, read receipts and presence"]
    COMMS --> COMMS3["Persistent and real-time notifications"]

    SRS --> ADMIN["FR-06 Administration"]
    ADMIN --> ADMIN1["Moderation queue and rule-based checks"]
    ADMIN --> ADMIN2["Approve/reject posts, jobs and stories"]
    ADMIN --> ADMIN3["Roles, blocks, badges, settings and audit records"]
```

## 6. SRS — Role and Permission Model

```mermaid
flowchart TB
    Public["Public Access"] --> P1["View approved feed, profiles and jobs"]
    Member["Authenticated Member"] --> M1["Public access + create, interact, apply and chat"]
    Admin["Admin"] --> A1["Member access + moderate and inspect users"]
    Super["Super Admin"] --> S1["Admin access + roles, blocks, deletion, badges and settings"]

    Public --> Member
    Member --> Admin
    Admin --> Super
```

## 7. SRS — Non-Functional Requirements

```mermaid
mindmap
  root((ShortJob Quality Requirements))
    Security
      JWT in httpOnly cookies
      Password hashing
      Helmet and CORS
      Role-based authorization
      Rate limiting
      Input and file validation
    Performance
      Indexed MongoDB queries
      Paginated feeds and messages
      API target under 2 seconds p95
      Real-time events under 1 second normally
    Reliability
      Idempotent event consumers
      Retry external email and media operations
      Health checks and graceful failures
      Backups and point-in-time recovery
    Scalability
      Stateless API replicas
      Horizontally scaled Socket.IO
      Service-owned data and event bus
      CDN-hosted media
    Usability
      Responsive mobile-first PWA
      Accessible navigation and forms
      Clear validation and empty states
    Maintainability
      Versioned APIs
      Automated tests and CI
      Central logging and tracing
      Documented service contracts
```

## 8. SRS — Core Domain Data Model

```mermaid
erDiagram
    USER ||--o{ POST : authors
    USER ||--o{ STORY : authors
    USER ||--o{ JOB_POST : publishes
    USER ||--o{ APPLICATION : submits
    USER }o--o{ USER : follows
    USER }o--o{ CONVERSATION : participates
    USER ||--o{ MESSAGE : sends
    USER ||--o{ NOTIFICATION : receives
    USER ||--o{ LOGIN_RECORD : generates

    POST ||--o{ COMMENT : contains
    POST o|--o| JOB_POST : links
    JOB_POST ||--o{ APPLICATION : receives
    CONVERSATION ||--o{ MESSAGE : contains
    COMMENT o|--o{ COMMENT : has_replies

    USER {
      ObjectId id PK
      string name
      string email UK
      string role
      array skills
      array followers
      boolean openToOpportunities
    }
    JOB_POST {
      ObjectId id PK
      ObjectId postedBy FK
      string title
      string opportunityType
      string locationType
      date deadline
      string status
    }
    APPLICATION {
      ObjectId id PK
      ObjectId jobPost FK
      ObjectId applicant FK
      string status
    }
    POST {
      ObjectId id PK
      ObjectId author FK
      string type
      string content
      string status
    }
    COMMENT {
      ObjectId id PK
      ObjectId post FK
      ObjectId author FK
      ObjectId parentComment FK
    }
    STORY {
      ObjectId id PK
      ObjectId author FK
      string content
      string status
    }
    CONVERSATION {
      ObjectId id PK
      array participants
      ObjectId lastMessage FK
    }
    MESSAGE {
      ObjectId id PK
      ObjectId conversation FK
      ObjectId sender FK
      string messageType
    }
    NOTIFICATION {
      ObjectId id PK
      ObjectId recipient FK
      string type
      boolean isRead
    }
    LOGIN_RECORD {
      ObjectId id PK
      ObjectId user FK
      datetime loginAt
      object location
      object photo
    }
```

## 9. Current Repository Architecture

```mermaid
flowchart TB
    Web["React 19 + Vite PWA"]
    API["Express Modular Monolith"]
    Socket["Socket.IO in Express Process"]
    Timer["In-process Moderation and Cleanup Timers"]
    Mongo[("Shared MongoDB")]
    Cloud["Cloudinary"]
    SMTP["Nodemailer / SMTP"]

    Web -->|"REST /api"| API
    Web <-->|"Real-time events"| Socket
    API --- Socket
    API --- Timer
    API --> Mongo
    API --> Cloud
    API --> SMTP
```

## 10. Proposed Microservices Architecture

```mermaid
flowchart TB
    Client["React PWA / Future Mobile App"]
    Gateway["API Gateway / BFF"]
    Realtime["Realtime Gateway"]
    Bus[("Event Bus")]

    Auth["Identity Service"]
    Profile["Profile and Network Service"]
    Content["Content Service"]
    Jobs["Jobs and Application Service"]
    Chat["Chat Service"]
    Notify["Notification Service"]
    Moderate["Moderation Service"]
    Media["Media Service"]
    Admin["Admin Service"]

    Client -->|"HTTPS"| Gateway
    Client <-->|"WebSocket"| Realtime
    Gateway --> Auth
    Gateway --> Profile
    Gateway --> Content
    Gateway --> Jobs
    Gateway --> Chat
    Gateway --> Admin
    Realtime --> Chat
    Realtime --> Notify

    Auth <--> Bus
    Profile <--> Bus
    Content <--> Bus
    Jobs <--> Bus
    Chat <--> Bus
    Notify <--> Bus
    Moderate <--> Bus
    Media <--> Bus
    Admin <--> Bus
```

## 11. Microservice Responsibility and Data Ownership

```mermaid
flowchart LR
    subgraph Identity["Identity Service"]
        IAPI["OTP, JWT, refresh, recovery"]
        IDB[("Users and Sessions DB")]
        IAPI --> IDB
    end

    subgraph Profile["Profile and Network Service"]
        PAPI["Profiles, follow, badges, endorsements"]
        PDB[("Profile Graph DB")]
        PAPI --> PDB
    end

    subgraph Content["Content Service"]
        CAPI["Posts, comments, stories, saves"]
        CDB[("Content DB")]
        CAPI --> CDB
    end

    subgraph Jobs["Jobs and Application Service"]
        JAPI["Jobs, applications, matching, Q&A"]
        JDB[("Jobs DB")]
        JAPI --> JDB
    end

    subgraph Chat["Chat Service"]
        CHAPI["Conversations and messages"]
        CHDB[("Chat DB")]
        CHAPI --> CHDB
    end

    subgraph Platform["Platform Services"]
        N["Notifications"]
        M["Moderation"]
        F["Media"]
        A["Admin and Audit"]
    end
```

## 12. Job Application Event Flow

```mermaid
sequenceDiagram
    actor Member
    participant GW as API Gateway
    participant Jobs as Jobs Service
    participant Profile as Profile Service
    participant Bus as Event Bus
    participant Notify as Notification Service
    participant Recruiter

    Member->>GW: Apply to job
    GW->>Jobs: Authenticated application command
    Jobs->>Profile: Read applicant summary
    Profile-->>Jobs: Skills, qualification and contact policy
    Jobs->>Jobs: Validate deadline and duplicate
    Jobs-->>GW: Application accepted
    GW-->>Member: 201 Created
    Jobs-->>Bus: ApplicationSubmitted
    Bus-->>Notify: Consume event
    Notify-->>Recruiter: Real-time and persistent notification
```

## 13. Moderated Content Publishing Flow

```mermaid
sequenceDiagram
    actor Member
    participant Content as Content Service
    participant Bus as Event Bus
    participant Moderate as Moderation Service
    participant Admin as Admin Service
    participant Notify as Notification Service

    Member->>Content: Create post, job or story
    Content->>Content: Store as pending_review
    Content-->>Bus: ContentSubmitted
    Bus-->>Moderate: Run rule-based checks
    Moderate->>Moderate: Score risks and flags
    alt Safe and auto-approval enabled
        Moderate-->>Bus: ContentApproved
    else Unsafe and auto-rejection enabled
        Moderate-->>Bus: ContentRejected
    else Manual review required
        Moderate-->>Admin: Add to review queue
        Admin-->>Bus: AdminDecision
    end
    Bus-->>Content: Update publication status
    Bus-->>Notify: Notify creator of decision
```

## 14. Real-Time Chat Flow

```mermaid
sequenceDiagram
    actor Sender
    participant RT as Realtime Gateway
    participant Chat as Chat Service
    participant Bus as Event Bus
    participant Notify as Notification Service
    actor Recipient

    Sender->>RT: Send message
    RT->>Chat: Persist authenticated message
    Chat-->>RT: Message stored
    RT-->>Recipient: Emit new_message when online
    Chat-->>Bus: MessageCreated
    Bus-->>Notify: Create unread notification
    Notify-->>Recipient: Push unread update
    Recipient->>RT: Mark message read
    RT->>Chat: Update read state
    RT-->>Sender: Emit message_read
```

## 15. Target Deployment Flow

```mermaid
flowchart TB
    CDN["CDN / Static Hosting"]
    LB["Load Balancer and API Gateway"]
    K8S["Container Platform"]
    Services["Stateless Service Replicas"]
    Workers["Moderation, Email and Cleanup Workers"]
    Broker[("Event Broker")]
    Redis[("Redis: cache, rate limits and Socket.IO adapter")]
    Databases[("Service-owned MongoDB databases")]
    Observability["Central logs, metrics and traces"]
    External["Cloudinary and SMTP"]

    CDN -->|"App and assets"| LB
    LB --> K8S
    K8S --> Services
    K8S --> Workers
    Services <--> Broker
    Workers <--> Broker
    Services --> Redis
    Services --> Databases
    Workers --> Databases
    Services --> External
    Workers --> External
    Services --> Observability
    Workers --> Observability
```

## 16. Safe Migration from Monolith to Microservices

```mermaid
flowchart TB
    P1["Phase 1: keep monolith; define API and event contracts"]
    P2["Phase 2: extract notifications and background workers"]
    P3["Phase 3: extract chat and realtime gateway"]
    P4["Phase 4: extract jobs and applications"]
    P5["Phase 5: extract content and moderation"]
    P6["Phase 6: isolate identity, profiles and service databases"]

    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> P5
    P5 --> P6
```
