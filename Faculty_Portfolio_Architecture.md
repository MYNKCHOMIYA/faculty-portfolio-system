# Generalized Faculty Portfolio Management System
## Team Execution Plan & Parallel Roadmap

This document outlines the distributed system architecture, specific technology stacks, team roles, working methods, and a "Learn & Build" timeline designed for your 3-member team.

---

## 1. Team Allocation & Roles

*   **Frontend Development**: Manan Garg
    *   *Focus*: UI/UX, responsive design, data fetching, global state, mapping backend APIs to the user interface.
*   **Backend Development**: Kunal Gupta
    *   *Focus*: API design, database schema, authentication, file storage logic, serving as the bridge between frontend and ML.
*   **Machine Learning & Data Analysis**: Mayank Kumar
    *   *Focus*: NLP topic extraction, data pipelines, recommendation algorithms, trend analysis Python service.

---

## 2. Distributed Working Method

To efficiently build a distributed system in parallel without blocking each other, the team will use the **"API Contract-First"** approach:

1.  **API Contracts**: Before writing code, Manan and Kunal agree on the exact JSON structures for requests/responses (e.g., using Postman or Swagger). Kunal then provides "Mock APIs" so Manan can build the UI immediately while Kunal builds the actual database logic behind the scenes.
2.  **Git Version Control**: 
    *   One unified GitHub organization/repository.
    *   Branch naming convention: `frontend/feature-name`, `backend/feature-name`, `ml/feature-name`.
    *   Never push directly to `main`. Always use Pull Requests (PRs).
3.  **Communication & Sync**: Two short weekly meetings (15-30 mins). One to plan weekly goals, one to integrate the pieces (Frontend + Backend + ML).
4.  **Containerization (Docker)**: Use Docker so Kunal and Mayank can run their services locally without "it works on my machine" issues.

---

## 3. Tech Stack & Learning Focus

### Frontend (Manan Garg)
*   **Stack**: React.js / Next.js, Tailwind CSS, Axios/Fetch.
*   **To Learn**: 
    1. Basics of React Hooks (`useState`, `useEffect`).
    2. Tailwind CSS grid and flexbox for responsive layouts.
    3. How to make API requests to a backend using Axios or the Fetch API.

### Backend (Kunal Gupta)
*   **Stack**: Node.js with Express.js, PostgreSQL, Prisma ORM (or Sequelize).
*   **To Learn**:
    1. Building RESTful APIs (GET, POST, PUT, DELETE) with Express.js.
    2. Connecting an Express app to a PostgreSQL database using Prisma.
    3. JWT (JSON Web Token) authentication for secure login.

### ML Model & Analysis (Mayank Kumar)
*   **Stack**: Python, FastAPI (for the microservice), Scikit-Learn, spaCy/Transformers (NLP).
*   **To Learn**:
    1. Creating quick web APIs using Python FastAPI.
    2. Text preprocessing and basic NLP for extracting keywords from publication abstracts.
    3. Cosine similarity for recommendation systems.

---

## 4. Parallel "Learn & Build" Roadmap (12 Weeks)

This timeline allows each member to learn the necessary tech while immediately applying it to the project.

### Phase 1: Foundations & Setup (Weeks 1-3)
*   **Goal**: Establish the base projects and learn core frameworks.
*   **Manan (Frontend)**: 
    *   *Learn*: React basics and styling. 
    *   *Build*: Initialize Next.js/React project. Build static UI screens (Login Page, Registration Page, static empty Dashboard).
*   **Kunal (Backend)**:
    *   *Learn*: Express API routing and PostgreSQL basics.
    *   *Build*: Initialize Node.js project. Design database schema. Build the `POST /register` and `POST /login` APIs.
*   **Mayank (ML)**:
    *   *Learn*: Python FastAPI and basic NLP libraries.
    *   *Build*: Setup a simple FastAPI server. Create a mock endpoint that takes a "string" (abstract) and returns 3 dummy keywords.

*Integration Check*: Manan successfully logs in using Kunal's API.

---

### Phase 2: Core System & Parallel Development (Weeks 4-6)
*   **Goal**: Implement the core portfolio CRUD (Create, Read, Update, Delete) features.
*   **Manan (Frontend)**: 
    *   *Learn*: API Data Fetching.
    *   *Build*: Forms to Add/Edit Publications, Projects, and Profile Details. Display list of publications.
*   **Kunal (Backend)**:
    *   *Learn*: File uploads and Relational queries.
    *   *Build*: APIs to handle Profile updates, adding Publications, uploading profile pictures or PDFs (using AWS S3 or disk storage).
*   **Mayank (ML)**:
    *   *Learn*: Scikit-Learn clustering and text-embedding techniques.
    *   *Build*: Write the actual script to process an abstract and categorize it/extract research interests.

*Integration Check*: Manan can submit a new publication form, Kunal saves it to the DB.

---

### Phase 3: The "Distributed System" Integration (Weeks 7-9)
*   **Goal**: Connect the core Node.js backend with the Python ML service.
*   **Manan (Frontend)**: 
    *   *Build*: Admin Analytics Dashboard UI and the "Recommended Collaborators" UI.
*   **Kunal (Backend)**:
    *   *Learn*: Microservice communication.
    *   *Build*: The Main Backend API calls Mayank's FastAPI ML service to get data, formats it, and sends it to Manan's Frontend.
*   **Mayank (ML)**:
    *   *Build*: Deploy the topic extraction and "similarity recommendation" models onto the FastAPI endpoints.

*Integration Check*: Frontend requests "Collaborators" -> Node Backend forwards request to -> Python ML API -> computes -> Node sends back to Frontend.

---

### Phase 4: Polish & Deployment (Weeks 10-12)
*   **Goal**: Make the system production-ready and host it on the internet.
*   **Manan (Frontend)**: Polish UI, fix mobile-responsiveness, handle error states (e.g., showing a red toast if an API fails).
*   **Kunal (Backend)**: Add input validation, secure API endpoints, ensure database indexes exist for performance.
*   **Mayank (ML)**: Optimize the ML models so they respond quickly over the API.
*   **Deployment (All Together)**: 
    *   Manan deploys to **Vercel** or Netlify.
    *   Kunal deploys database to **Supabase/Render** and API to **Render/Railway**.
    *   Mayank deploys FastAPI service to **Render/Railway**.
