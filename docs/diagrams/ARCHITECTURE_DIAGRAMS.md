# HQ Operations System - Detailed Architecture Diagrams

## 1. System Architecture Overview

```mermaid
graph TB
    subgraph "Client Layer"
        WEB["Next.js Web App<br/>React 19"]
        MOBILE["React Native Mobile<br/>iOS & Android"]
    end

    subgraph "Authentication & Security"
        AUTH["Oslo.js Auth<br/>JWT & OAuth2"]
        RBAC["Role-Based Access Control"]
    end

    subgraph "API Gateway & Serverless"
        VERCEL["Vercel<br/>Edge Functions"]
        INNGEST["Inngest<br/>Event-Driven Serverless"]
    end

    subgraph "Business Logic Services"
        BB["Battle Board<br/>Event Management"]
        SALES["Sales Handoff<br/>Pipeline & Deals"]
        KITCHEN["Kitchen Manager<br/>Orders & Inventory"]
        WAREHOUSE["Warehouse<br/>Stock & Shipments"]
        CRM["CRM System<br/>Contacts & Activities"]
        SCHED["Scheduling<br/>GoodShuffle/Nowsta"]
        TASKS["Task Management<br/>KannBahn"]
        AI["AI Assistant<br/>npcpy"]
    end

    subgraph "Data Layer"
        SUPABASE["Supabase<br/>PostgreSQL"]
        PRISMA["Prisma ORM<br/>Type-Safe"]
    end

    subgraph "External Integrations"
        STRIPE["Stripe<br/>Payments"]
        EMAIL["Resend<br/>Email Service"]
        S3["AWS S3<br/>File Storage"]
        GOODSHUFFLE["GoodShuffle API<br/>Scheduling"]
        NOWSTA["Nowsta API<br/>Staff Management"]
        KANNBAHN["KannBahn API<br/>Task Management"]
    end

    subgraph "Infrastructure & Deployment"
        CLOUDFLARE["Cloudflare<br/>Domain & CDN"]
        VERCEL_DEPLOY["Vercel Deployment"]
        DOCKER["Docker<br/>Containerization"]
    end

    subgraph "Validation & Schemas"
        ZOD["Zod<br/>TypeScript Validation"]
    end

    subgraph "UI & Styling"
        TAILWIND["Tailwind CSS"]
        SHADCN["shadcn/UI<br/>Components"]
    end

    %% Connections
    WEB --> AUTH
    MOBILE --> AUTH
    AUTH --> RBAC
    
    RBAC --> VERCEL
    VERCEL --> INNGEST
    
    INNGEST --> BB
    INNGEST --> SALES
    INNGEST --> KITCHEN
    INNGEST --> WAREHOUSE
    INNGEST --> CRM
    INNGEST --> SCHED
    INNGEST --> TASKS
    INNGEST --> AI
    
    BB --> SUPABASE
    SALES --> SUPABASE
    KITCHEN --> SUPABASE
    WAREHOUSE --> SUPABASE
    CRM --> SUPABASE
    SCHED --> SUPABASE
    TASKS --> SUPABASE
    AI --> SUPABASE
    
    SUPABASE --> PRISMA
    ZOD --> PRISMA
    
    BB --> STRIPE
    SALES --> STRIPE
    BB --> EMAIL
    SALES --> EMAIL
    KITCHEN --> S3
    WAREHOUSE --> S3
    
    SCHED --> GOODSHUFFLE
    SCHED --> NOWSTA
    TASKS --> KANNBAHN
    AI --> EMAIL
    
    VERCEL_DEPLOY --> VERCEL
    CLOUDFLARE --> VERCEL_DEPLOY
    DOCKER --> VERCEL_DEPLOY
    
    TAILWIND --> WEB
    SHADCN --> WEB

    style WEB fill:#0070f3,color:#fff
    style MOBILE fill:#0070f3,color:#fff
    style SUPABASE fill:#3ECF8E,color:#fff
    style STRIPE fill:#635BFF,color:#fff
    style EMAIL fill:#00B4D8,color:#fff
    style S3 fill:#FF9900,color:#fff
    style INNGEST fill:#000000,color:#fff
    style AUTH fill:#FF6B6B,color:#fff
    style RBAC fill:#FF6B6B,color:#fff
```

---

## 2. Data Flow: Order to Shipment

```mermaid
sequenceDiagram
    participant User as User / Mobile App
    participant Web as Web App
    participant API as Inngest API
    participant Kitchen as Kitchen Service
    participant Warehouse as Warehouse Service
    participant DB as Supabase DB
    participant Email as Resend Email
    participant Stripe as Stripe

    User->>Web: Creates Order
    Web->>API: POST /orders
    API->>Kitchen: Kitchen Order Event
    API->>DB: Save Order
    API->>Stripe: Process Payment
    
    Stripe->>API: Payment Webhook
    API->>Email: Send Confirmation
    Email->>User: Order Confirmation
    
    Kitchen->>DB: Update Order Status: Preparing
    Kitchen->>Web: Real-time Update
    
    Kitchen->>DB: Update Order Status: Ready
    API->>Warehouse: Pick & Pack Event
    
    Warehouse->>DB: Update Shipment Status: Picking
    Warehouse->>DB: Update Shipment Status: Packed
    Warehouse->>API: Shipment Ready
    
    API->>Email: Send Shipment Notice
    Email->>User: Shipment Notification
    
    User->>Web: Track Shipment
    Web->>DB: Query Shipment Status
    DB->>Web: Return Tracking Info
    Web->>User: Display Tracking
```

---

## 3. Real-Time Event Distribution

```mermaid
graph LR
    subgraph "Events"
        EV1["Shift Assignment"]
        EV2["Order Update"]
        EV3["Deal Stage Change"]
        EV4["Inventory Low"]
        EV5["Payment Received"]
    end

    subgraph "Inngest Queue"
        Q["Event Queue<br/>Processing & Distribution"]
    end

    subgraph "Listeners"
        L1["WebSocket Broadcaster"]
        L2["Email Notifier"]
        L3["SMS Alert"]
        L4["Slack Integration"]
        L5["Mobile Push"]
    end

    subgraph "Destinations"
        D1["Battle Board UI"]
        D2["Email Client"]
        D3["Mobile Device"]
        D4["Slack Channel"]
        D5["Dashboard"]
    end

    EV1 --> Q
    EV2 --> Q
    EV3 --> Q
    EV4 --> Q
    EV5 --> Q

    Q --> L1
    Q --> L2
    Q --> L3
    Q --> L4
    Q --> L5

    L1 --> D1
    L1 --> D5
    L2 --> D2
    L3 --> D3
    L4 --> D4
    L5 --> D3

    style Q fill:#000,color:#fff
    style L1 fill:#0070f3,color:#fff
    style L2 fill:#00B4D8,color:#fff
    style L3 fill:#FF6B6B,color:#fff
    style L4 fill:#FFA500,color:#fff
    style L5 fill:#3ECF8E,color:#fff
```

---

## 4. Module Integration & Data Sharing

```mermaid
graph TB
    subgraph "Core Modules"
        BB["Battle Board<br/>Events & Shifts"]
        SALES["Sales Handoff<br/>Deals & Contacts"]
        KITCHEN["Kitchen Manager<br/>Orders & Inventory"]
        WAREHOUSE["Warehouse<br/>Shipments & Stock"]
        CRM["CRM<br/>Contacts & Activities"]
    end

    subgraph "Supporting Modules"
        SCHED["Scheduling<br/>Shifts & Availability"]
        TASKS["Task Management<br/>Kanban Board"]
        AI["AI Assistant<br/>Insights"]
        PAYMENTS["Payments<br/>Stripe Integration"]
    end

    subgraph "Shared Data & Services"
        DB[(Shared Database<br/>PostgreSQL)]
        AUTH["Authentication<br/>Oslo.js"]
        NOTIFY["Notification Hub<br/>Multi-Channel"]
        AUDIT["Audit Log<br/>Compliance"]
        CACHE["Cache Layer<br/>Redis"]
    end

    %% Internal connections
    BB <--> DB
    SALES <--> DB
    KITCHEN <--> DB
    WAREHOUSE <--> DB
    CRM <--> DB
    SCHED <--> DB
    TASKS <--> DB
    AI <--> DB
    PAYMENTS <--> DB

    %% Auth connections
    BB --> AUTH
    SALES --> AUTH
    KITCHEN --> AUTH
    WAREHOUSE --> AUTH
    CRM --> AUTH
    SCHED --> AUTH
    TASKS --> AUTH
    AI --> AUTH
    PAYMENTS --> AUTH

    %% Notification connections
    BB --> NOTIFY
    SALES --> NOTIFY
    KITCHEN --> NOTIFY
    WAREHOUSE --> NOTIFY
    SCHED --> NOTIFY
    TASKS --> NOTIFY
    AI --> NOTIFY
    PAYMENTS --> NOTIFY

    %% Audit connections
    BB --> AUDIT
    SALES --> AUDIT
    KITCHEN --> AUDIT
    WAREHOUSE --> AUDIT
    CRM --> AUDIT
    SCHED --> AUDIT
    TASKS --> AUDIT
    PAYMENTS --> AUDIT

    %% Cross-module communication
    BB <--> SCHED
    SALES <--> CRM
    KITCHEN <--> WAREHOUSE
    SALES <--> PAYMENTS
    KITCHEN <--> PAYMENTS
    BB -.->|AI Insights| AI
    SALES -.->|AI Insights| AI
    KITCHEN -.->|AI Insights| AI

    %% Caching
    DB --> CACHE
    CACHE --> BB
    CACHE --> SALES
    CACHE --> KITCHEN

    style DB fill:#3ECF8E,color:#fff
    style AUTH fill:#FF6B6B,color:#fff
    style NOTIFY fill:#0070f3,color:#fff
    style AUDIT fill:#666,color:#fff
    style CACHE fill:#FFA500,color:#fff
```

---

## 5. Deployment Architecture

```mermaid
graph TB
    subgraph "Development"
        DEV["Local Dev<br/>pnpm dev<br/>Docker Compose"]
    end

    subgraph "Version Control"
        GIT["GitHub<br/>Feature Branches<br/>Pull Requests"]
    end

    subgraph "CI/CD Pipeline"
        LINT["Linting<br/>eslint"]
        TEST["Testing<br/>Jest/Vitest"]
        BUILD["Build<br/>Next.js Build"]
        SECURITY["Security Scan<br/>Dependency Check"]
    end

    subgraph "Staging Environment"
        STAGING["Vercel Staging<br/>Preview Deployments"]
    end

    subgraph "Production"
        PROD_WEB["Web App<br/>Vercel Production"]
        PROD_API["API<br/>Edge Functions"]
        PROD_MOBILE["Mobile<br/>App Store/Google Play"]
    end

    subgraph "Infrastructure"
        CLOUDFLARE["Cloudflare<br/>CDN & DNS"]
        SUPABASE["Supabase<br/>Database"]
        S3["AWS S3<br/>Static Assets"]
        MONITORING["Monitoring<br/>Sentry & Analytics"]
    end

    DEV --> GIT
    GIT --> LINT
    LINT --> TEST
    TEST --> BUILD
    BUILD --> SECURITY
    SECURITY --> STAGING
    STAGING -->|Approval| PROD_WEB
    STAGING -->|Approval| PROD_API
    STAGING -->|Approval| PROD_MOBILE

    PROD_WEB --> CLOUDFLARE
    PROD_API --> CLOUDFLARE
    PROD_WEB --> SUPABASE
    PROD_API --> SUPABASE
    PROD_WEB --> S3
    PROD_MOBILE --> SUPABASE
    PROD_WEB --> MONITORING
    PROD_API --> MONITORING

    style DEV fill:#666,color:#fff
    style GIT fill:#FF6B6B,color:#fff
    style STAGING fill:#FFA500,color:#fff
    style PROD_WEB fill:#00B4D8,color:#fff
    style PROD_API fill:#00B4D8,color:#fff
    style MONITORING fill:#0070f3,color:#fff
```

---

## 6. Battle Board Module - Detailed Flow

```mermaid
graph TB
    subgraph "UI Layer"
        CREATE["Create Event"]
        BOARD["Event Board View"]
        NOTIFY["Notification Center"]
    end

    subgraph "Business Logic"
        EVENTS["Event Service"]
        SHIFTS["Shift Service"]
        ALERTS["Alert Service"]
    end

    subgraph "Data Layer"
        EVDB[("Events Table")]
        SHIFTDB[("Shifts Table")]
        STAFFDB[("Staff Table")]
    end

    subgraph "External Services"
        NOTIF["Notification Hub<br/>Email/SMS/Push"]
        GOODSHUFFLE["GoodShuffle<br/>Scheduler"]
        NOWSTA["Nowsta<br/>Staff Mgmt"]
    end

    CREATE -->|POST /events| EVENTS
    EVENTS -->|Validate| ALERTS
    EVENTS -->|Save| EVDB
    EVENTS -->|Create Shift Template| SHIFTS

    SHIFTS -->|Save| SHIFTDB
    SHIFTS -->|Fetch Staff| STAFFDB
    SHIFTS -->|Suggest Schedule| GOODSHUFFLE
    
    GOODSHUFFLE -->|Optimize Schedule| SHIFTS
    SHIFTS -->|Assign Staff| NOWSTA
    NOWSTA -->|Send Notifications| NOTIF
    NOTIF -->|Update| BOARD

    BOARD -->|Query| EVDB
    BOARD -->|Query| SHIFTDB
    BOARD -->|Display| NOTIFY

    ALERTS -->|Alert on Changes| NOTIF

    style EVDB fill:#3ECF8E,color:#fff
    style SHIFTDB fill:#3ECF8E,color:#fff
    style STAFFDB fill:#3ECF8E,color:#fff
    style NOTIF fill:#0070f3,color:#fff
    style GOODSHUFFLE fill:#FFA500,color:#fff
    style NOWSTA fill:#FFA500,color:#fff
```

---

## 7. Database Relationships

```mermaid
erDiagram
    ORGANIZATION ||--o{ USER : has
    ORGANIZATION ||--o{ BATTLE_EVENT : manages
    ORGANIZATION ||--o{ DEAL : manages
    ORGANIZATION ||--o{ KITCHEN_ORDER : manages
    ORGANIZATION ||--o{ SHIPMENT : manages
    ORGANIZATION ||--o{ CONTACT : manages
    ORGANIZATION ||--o{ SHIFT : manages

    BATTLE_EVENT ||--o{ SHIFT : contains
    SHIFT ||--o{ USER : assigns
    SHIFT ||--o{ TASK : contains

    CONTACT ||--o{ DEAL : associated
    DEAL ||--o{ PAYMENT : generates
    
    KITCHEN_ORDER ||--o{ ORDER_ITEM : contains
    ORDER_ITEM ||--o{ INVENTORY_ITEM : references
    
    SHIPMENT ||--o{ SHIPMENT_ITEM : contains
    SHIPMENT_ITEM ||--o{ INVENTORY_ITEM : takes
    
    TASK ||--o{ COMMENT : has
    PAYMENT ||--o{ INVOICE : generates
    CONTACT ||--o{ ACTIVITY : has

    USER {
        string id PK
        string email UK
        string name
        enum role
        string organizationId FK
        datetime createdAt
    }

    ORGANIZATION {
        string id PK
        string name
        datetime createdAt
    }

    BATTLE_EVENT {
        string id PK
        string name
        enum status
        string organizationId FK
        datetime createdAt
    }

    SHIFT {
        string id PK
        string eventId FK
        enum status
        datetime startTime
        datetime endTime
        string organizationId FK
    }

    DEAL {
        string id PK
        string title
        float value
        enum stage
        string contactId FK
        string organizationId FK
    }

    KITCHEN_ORDER {
        string id PK
        string orderNumber UK
        enum status
        string organizationId FK
        datetime createdAt
    }

    SHIPMENT {
        string id PK
        enum status
        string organizationId FK
        datetime createdAt
    }

    CONTACT {
        string id PK
        string name
        string email
        string phone
        string organizationId FK
    }

    TASK {
        string id PK
        string title
        enum status
        string shiftId FK
        string assignedTo FK
    }

    PAYMENT {
        string id PK
        float amount
        enum status
        string dealId FK
        string invoiceId FK
    }

    INVOICE {
        string id PK
        string number UK
        float total
        string paymentId FK
    }

    ORDER_ITEM {
        string id PK
        string orderId FK
        string itemId FK
        int quantity
    }

    SHIPMENT_ITEM {
        string id PK
        string shipmentId FK
        string itemId FK
        int quantity
    }

    INVENTORY_ITEM {
        string id PK
        string sku UK
        string name
        int quantity
        float price
    }

    COMMENT {
        string id PK
        string taskId FK
        string userId FK
        string content
        datetime createdAt
    }

    ACTIVITY {
        string id PK
        string contactId FK
        string type
        string description
        datetime createdAt
    }
```

---

## 8. Authentication & Authorization Flow

```mermaid
sequenceDiagram
    participant User
    participant App as Next.js App
    participant Oslo as Oslo.js Auth
    participant DB as Supabase
    participant API as Inngest API

    User->>App: Login (email/password)
    App->>Oslo: verify credentials
    Oslo->>DB: Query User
    DB->>Oslo: Return User + Role
    Oslo->>App: Generate JWT Token
    App->>User: Store Token in Cookie
    User->>App: Request Protected Resource
    App->>API: Include JWT Token
    API->>Oslo: Verify Token
    Oslo->>DB: Check User Permissions
    DB->>Oslo: Return RBAC Rules
    Oslo->>API: Validate Access
    API->>DB: Fetch Resource (User Scoped)
    DB->>API: Return Filtered Data
    API->>App: Return Data
    App->>User: Display Resource

    Note over User,DB: User can only see<br/>their organization's data<br/>based on role
```

---

## 9. Inngest Event Processing Pipeline

```mermaid
graph LR
    subgraph "Event Sources"
        API["API Endpoints"]
        WEBHOOK["Webhooks"]
        CRON["Scheduled Tasks"]
    end

    subgraph "Inngest"
        QUEUE["Event Queue"]
        PROCESS["Processing Engine"]
        RETRY["Retry Logic<br/>Exponential Backoff"]
    end

    subgraph "Event Handlers"
        H1["Database Updates"]
        H2["Notifications"]
        H3["External APIs"]
        H4["Analytics"]
    end

    subgraph "Outcomes"
        SUCCESS["Success<br/>Acknowledge"]
        FAIL["Failure<br/>Dead Letter"]
        DEFER["Deferred<br/>Retry Later"]
    end

    API -->|Emit Event| QUEUE
    WEBHOOK -->|Emit Event| QUEUE
    CRON -->|Emit Event| QUEUE

    QUEUE --> PROCESS
    PROCESS --> H1
    PROCESS --> H2
    PROCESS --> H3
    PROCESS --> H4

    H1 --> SUCCESS
    H2 --> SUCCESS
    H3 --> RETRY
    H4 --> SUCCESS

    RETRY -->|Max Retries Exceeded| FAIL
    RETRY -->|Retry| PROCESS

    style QUEUE fill:#000,color:#fff
    style PROCESS fill:#0070f3,color:#fff
    style RETRY fill:#FFA500,color:#fff
    style SUCCESS fill:#3ECF8E,color:#fff
    style FAIL fill:#FF6B6B,color:#fff
```

