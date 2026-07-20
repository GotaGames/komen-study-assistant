# Komen: The Intelligent Study Assistant

> **Note on repository visibility:** Komen is an actively developed commercial product. To protect intellectual property, the source code is kept private. This repository serves as a technical showcase, documenting the system architecture, engineering decisions, and the development journey behind the application.

---

## Download & Try Komen

The application is currently available for Android devices. Experience the gamified study flow firsthand by downloading it directly from the Google Play Store:

<a href="https://play.google.com/store/apps/details?id=com.gotagames.komen" target="_blank">
  <img src="https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png" alt="Get it on Google Play" height="60"/>
</a>

---
## The Vision

Studying is often treated as a test of endurance. Students struggle with organization, lose motivation over time, and rarely get immediate, personalized feedback on their progress. Traditional methods are repetitive and disconnected from how modern learning actually works.

I built Komen to change that dynamic. It is not just another task manager. Komen bridges the gap between artificial intelligence, study management, and gamification to create an ecosystem where students actually want to learn. It handles the heavy lifting of routine organization, generates tailored quizzes on the fly, evaluates answers using AI, and—most importantly—works perfectly offline.


---

### Product Screenshots

<p align="center">
  <img src="assets/dashboard.jpg" width="30%" alt="Komen Dashboard" />
  <img src="assets/study.jpg" width="30%" alt="Study Session" />
  <img src="assets/analytics.jpg" width="30%" alt="Analytics and Progress" />
</p>

## Under the Hood

To deliver a seamless, cross-platform experience with complex backend requirements, I chose a modern and highly scalable stack:

* **Frontend:** Flutter & Dart (Focusing on a fluid, 60fps user interface)
* **Backend API:** FastAPI & Python (Handling complex business logic and AI processing)
* **Database:** Supabase / PostgreSQL (Relational data modeling and scalability)
* **Authentication:** Firebase Authentication (Secure, frictionless user access)

### System Architecture

Instead of connecting the mobile app directly to the database, I implemented a decoupled architecture. The Flutter client communicates exclusively with the FastAPI backend, which acts as the single source of truth for business rules, data validation, and AI service integration before interacting with Supabase.

```text
Mobile Client (Flutter) 
      │
      ▼ (RESTful API Requests)
Backend Service (FastAPI) 
      │
      ▼ (Data Validation & Business Logic)
Database & Storage (Supabase)

```

---

## The Engineering Journey: Challenges & Solutions

Building Komen was a massive undertaking in system design. Transitioning from theoretical concepts to a production-ready application required solving several complex engineering hurdles.

### 1. The Offline-First Reality

Students don't always have reliable internet access, so requiring a constant connection was a dealbreaker. I designed an offline-first architecture where data is stored and managed locally on the device. The challenge was building a custom synchronization engine. Whenever connectivity is restored, the app intelligently syncs the local cache with the FastAPI backend, resolving conflicts and ensuring data consistency without interrupting the user experience.

### 2. Taming the AI Integration

Integrating AI features—like dynamic quiz generation and answer evaluation—was not just about making API calls. The real challenge was performance and UX. AI generation takes time, so I had to design backend endpoints that process these requests efficiently, implement timeouts, and handle potential network failures gracefully so the app never feels unresponsive.

### 3. State Management and UI Performance

As the app grew to include study diaries, progress tracking, and gamified elements, managing the state across dozens of screens became complex. I structured the Flutter project using clean architecture principles, separating the UI from the domain and data layers. This kept the codebase maintainable, allowed for reusable components, and ensured the UI remained highly responsive.

### 4. Database Modeling for the Future

Designing the Supabase schema required forward-thinking. I had to model relations for users, study sessions, dynamic AI content, and progress tracking in a way that remains performant today but is flexible enough to support future features like learning analytics and team collaboration.

### System Architecture Diagram

### System Architecture Diagram

```mermaid
graph TD
    %% Styling Classes for a Clean Technical Look
    classDef client fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef backend fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef storage fill:#efebe9,stroke:#5d4037,stroke-width:2px;
    classDef external fill:#ede7f6,stroke:#5e35b1,stroke-width:2px;

    %% Mobile Client Nodes
    subgraph Mobile_Client [Mobile Client - Flutter]
        UI[Flutter UI / Widgets]
        Sync[Sync Engine]
        Cache[(Local SQLite & Shared Preferences)]
        WV[WebView Environment]
    end
    class Mobile_Client,UI,Sync,Cache,WV client;

    %% Backend Nodes
    subgraph Backend_Services [Backend Architecture - FastAPI]
        Auth[Auth Verification Engine]
        Router[REST Endpoints]
        JSONParser[JSON Response Standardizer]
    end
    class Backend_Services,Auth,Router,JSONParser backend;

    %% Data and External Services Nodes
    subgraph Data_Cloud [Cloud & Data Layer]
        Supa[(Supabase / PostgreSQL)]
    end
    subgraph AI_Cloud [External Services]
        OpenAI[AI Provider API]
    end
    class Data_Cloud,Supa storage;
    class AI_Cloud,OpenAI external;

    %% Flows and Relationships (Strictly HTTPS)
    UI -->|1. Triggers Action| Sync
    Sync <-->|2. Read/Write Local Cache| Cache
    Sync ==>|3. HTTPS Request with Auth token| Auth
    Auth --> Router
    
    %% Relational Data Flow
    Router <-->|4. Database Operations| Supa
    
    %% AI Flow and Dynamic Rendering
    Router ==>|5. Text Prompt Processing| OpenAI
    OpenAI ==>|6. Raw AI Response| JSONParser
    JSONParser ==>|7. Structured JSON Payload| Router
    
    %% Response to App
    Router ==>|8. HTTPS Secure Response| Sync
    Sync ==>|9. Dynamic Widget Building| UI

    %% Internal Navigation Link
    UI --> WV ```



What's Next?

Komen is in active development. The current roadmap focuses on expanding the platform's reach and intelligence:

* **Web & Desktop Versions** for a unified cross-platform ecosystem.
* **Advanced AI Tutor** capabilities for deeper, conversational learning.
* **Learning Analytics** to provide students with data-driven insights into their study habits.

---

**Built by Denzel Gota**

Computer Engineering Student | Mozambique
