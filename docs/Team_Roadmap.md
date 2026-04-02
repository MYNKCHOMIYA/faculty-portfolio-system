# Generalized Faculty Portfolio Management System
## System Architecture, Folder Structure, & Team Roadmap

This document outlines the distributed system architecture, folder structure, technologies required for each role, and exactly where each technology is used within the product.

---

## 1. Project Folder Structure & Roles

The project is structured as a "monorepo" containing three separate applications, assigned to the three team members. I have created these three folders in your workspace.

```text
faculty-portfolio-system/
│
├── frontend/       (Manan Garg - UI & Client Logic)
│   ├── src/
│   │   ├── components/  # Reusable UI parts (Buttons, Navbars)
│   │   ├── pages/       # Next.js/React Routing pages (Dashboard, /login)
│   │   └── api/         # Axios config fetching data from Backend
│
├── backend/        (Kunal Gupta - Main API Server & Database)
│   ├── src/
│   │   ├── routes/      # Express API endpoints (/api/users)
│   │   ├── controllers/ # Logic for handling requests
│   │   ├── models/      # Database models (Prisma/TypeORM)
│   │   └── auth/        # JWT Authentication logic
│
└── ml-service/     (Mayank Kumar - Python AI Microservice)
    ├── app/
    │   ├── main.py      # FastAPI application entry
    │   ├── models.py    # NLP processing functions
    │   └── recommend.py # Cosine similarity logic
    └── requirements.txt
```

---

## 2. Technology Stack & Knowledge Requirements

To build the system effectively, here is exactly what each team member MUST know/learn, and where it is physically used in the project.

### 👩‍💻 Frontend: Manan Garg
**Role Focus**: Transforming API data into a beautiful, interactive user experience.

*   **HTML, CSS, JavaScript (ES6)**: Core web fundamentals.
    *   *Where used*: Foundational code in `frontend/src/`.
*   **React.js (and Hooks like `useState`, `useEffect`)**:
    *   *Where used*: Entire `frontend/` application. Creating dynamic forms, rendering lists of publications without reloading the page.
*   **Tailwind CSS**:
    *   *Where used*: In React components (e.g., `<button className="bg-blue-500 rounded p-2">`) to build professional, responsive layouts quickly instead of raw CSS files.
*   **Axios / Fetch API**:
    *   *Where used*: `frontend/src/api/`. Responsible for sending HTTP requests (like sending login credentials) to Kunal's Node.js server.

### 🛠 Backend: Kunal Gupta
**Role Focus**: Data persistence, security, and serving as the primary internet gateway.

*   **Node.js & Express.js**:
    *   *Where used*: Entire `backend/` folder. Creates the server that runs on `localhost:5000` listening for Manan's frontend requests (e.g., `app.post('/login')`).
*   **PostgreSQL & Prisma ORM** (or Sequelize):
    *   *Where used*: `backend/src/models/`. Connecting to the actual database structure to execute SQL queries efficiently (Saving a user's profile, fetching all faculty).
*   **JWT (JSON Web Tokens) & Bcrypt**:
    *   *Where used*: `backend/src/auth/`. Hashing faculty passwords for security (Bcrypt) and issuing a "logged-in passport" (JWT) so users don't have to keep logging in.

### 🧠 ML Model & Data Analysis: Mayank Kumar
**Role Focus**: Extracting intelligence from text data and powering analytical features.

*   **Python & FastAPI**:
    *   *Where used*: `ml-service/`. FastAPI is used to wrap ML models into a web API running on port `8000`. This allows Kunal's Node.js backend to query the ML engine over HTTP.
*   **Pandas & NumPy**:
    *   *Where used*: `ml-service/`. Cleaning and managing large JSON datasets of faculty publications.
*   **Basic NLP (spaCy / HuggingFace Transformers)**:
    *   *Where used*: Creating the *Faculty Research Analytics*. Given a string representing a publication abstract, the Python script uses NLP to extract the main topic keywords.
*   **Scikit-Learn (Cosine Similarity Algorithm)**:
    *   *Where used*: Creating the *Collaborator Recommendation System*. Comparing vectors of faculty interests to mathematically find similar researchers.

---

## 3. Distributed Working Method: How the 3 Systems Connect

Because you have 3 separate physical folders containing 3 separate projects, they connect over the network:

1.  **User Action**: Faculty member clicks "Find Collaborators" on Manan's React Frontend (`frontend/`).
2.  **Request Sent**: Frontend sends an HTTP `GET` request to Kunal's Backend (`backend/`).
3.  **Forwarding**: Kunal's Node.js Backend receives the request, realizes it needs AI processing, and forwards a request `GET /recommend/faculty-id` to Mayank's Python FastAPI Microservice (`ml-service/`).
4.  **Compute**: Mayank's ML service runs the Machine Learning math, outputting an array of recommended IDs back to Kunal.
5.  **Return**: Kunal enriches those IDs with actual Names via his PostgreSQL database, and sends the final JSON packet back to Manan's UI.
6.  **Render**: Manan's UI displays the results on the screen.

> [!TIP]
> **To Avoid Being Blocked**:
> Use the **"API Contract-First"** approach. Kunal and Manan must agree on the JSON request/response format *before* writing any code. Kunal can give Manan "Mock Fake Data" so Manan can build the UI immediately while Kunal builds the actual database logic behind the scenes.

---

## 4. The 12-Week Parallel Timeline

### Phase 1: Setup & Foundations (Weeks 1-3)
*   **Manan**: Initialize Frontend. Create basic UI screens (Login, Registration, Admin Dashboard visual shell). Learn React/Tailwind.
*   **Kunal**: Initialize Backend connecting to PostgreSQL. Build Auth endpoints `/login` and `/register`. Learn Express/JWT.
*   **Mayank**: Initialize `ml-service`. Create extremely basic `/test` FastAPI endpoint returning fake keywords. Learn Python API wrapping.

### Phase 2: Core System & Independent Dev (Weeks 4-6)
*   **Manan**: Fetch Auth data. Build forms to Add/Edit Publications and Projects. Store auth tokens in the browser.
*   **Kunal**: Build REST APIs for Publications, Profiles, Projects. Implement AWS S3 file upload for documents.
*   **Mayank**: Develop the NLP script that loads a CSV of dummy abstracts, cleans them, and extracts topic keywords using Scikit-Learn.

### Phase 3: The Integration Phase (Weeks 7-9)
*   **All**: The "Network Hookup". Integrating all 3 applications as described in Section 3.
*   **Manan**: Finalize data visualization graphs for trend analysis in the UI. 
*   **Kunal**: Finalize backend request forwarding to `ml-service`.
*   **Mayank**: Wrap the working NLP scripts into FastAPI HTTP endpoints so the Backend can query them live.

### Phase 4: Polish & Deployment (Weeks 10-12)
*   **Manan**: Deploys the static UI to **Vercel** or Netlify.
*   **Kunal**: Deploys PostgreSQL DB & Node.js API to **Render**, **Railway**, or **Supabase**.
*   **Mayank**: Deploys Python FastAPI Server to **Render** or **Railway**.
*   **All**: End-to-end bug testing, UI polish, making sure API URLs point to production instead of `localhost`.
