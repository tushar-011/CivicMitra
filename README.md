# 🏛️ CivicMitra

> **A citizen-centric civic issue reporting and tracking web application built with React, Vite, Tailwind CSS, Node.js, Express, MySQL, and Leaflet.**

**CivicMitra** explores a digital way for citizens to report public-infrastructure concerns, attach supporting images, identify their location, and track a complaint with a generated ID. The project also includes administrator-facing report and hotspot-map interfaces, India-specific address datasets, multilingual resources, and a Progressive Web App (PWA) configuration.

> **Project scope:** An academic/hackathon and portfolio prototype. The repository includes a working application structure and extensive design documentation; production readiness, deployment, authentication, and some end-to-end functionality still require review. This README describes implementation visible in the source code rather than claiming all features have been fully tested.

---

## ✨ Highlights

- 🌐 Citizen-focused landing page and navigation
- 📝 Civic complaint submission with category, description, priority, and location
- 📷 Photo/file attachments through a backend upload endpoint
- 🧾 Generated complaint tracking IDs (`CIVYYYYMMDD-00001` style)
- 🗺️ Interactive Leaflet maps and geographic report views
- 🧑‍💼 Administrator report-list and hotspot-map interfaces
- 📊 Reports statistics API (total, pending, resolved, cities covered)
- 🗄️ MySQL-backed report records
- 🇮🇳 State, district, city, and PIN-code data resources
- 🌐 English and Hindi translation resources
- 🌗 Light/dark theme context
- 📱 Responsive UI and installable PWA configuration
- 📚 Hackathon documentation, architecture and data-flow diagrams

---

# 📌 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#️-tech-stack)
- [Project Structure](#-project-structure)
- [Application Flow](#-application-flow)
- [Data and Integrations](#-data-and-integrations)
- [Requirements](#-requirements)
- [Service Setup](#-service-setup)
- [Configuration](#️-configuration)
- [Running the Application](#️-running-the-application)
- [Major Application Modules](#-major-application-modules)
- [Project Screenshots](#️-project-screenshots)
- [Documentation and Diagrams](#-documentation-and-diagrams)
- [API Endpoints](#-api-endpoints)
- [Testing Checklist](#-testing-checklist)
- [Security Notes](#-security-notes)
- [Current Limitations](#️-current-limitations)
- [Future Improvements](#-future-improvements)
- [GitHub Setup](#-github-setup)
- [Project Status](#-project-status)
- [Development Notes](#-development-notes)
- [Author](#-author)

---

# 📖 About the Project

CivicMitra addresses a common civic-service problem: citizens often lack a convenient, centralized way to report local problems and retain a reference for follow-up. The application connects a React-based citizen interface to an Express API that persists report information in MySQL.

A typical intended user journey is:

1. Open CivicMitra and choose to report a civic issue.
2. Select the type of issue and describe the problem.
3. Provide location/address information and optionally attach images.
4. Submit the issue to the backend.
5. Receive a tracking ID generated from the submission date and database row ID.
6. Use the tracking interface to retrieve the submitted report.
7. View report collections and map-based information through the available citizen/admin pages.

The included `SIH-31/` directory contains research, design materials, and internal hackathon documentation. Those assets explain the intended wider governance and routing vision, which may go beyond the implemented API.

---

# 🚀 Features

## 🌐 Landing Page

- CivicMitra introduction, feature descriptions, and navigation
- How-it-works and installation sections
- Dedicated entry points for reporting and exploring issues

## 📝 Civic Issue Reporting

- Civic issue category selection
- Problem description and priority selection
- Location and address capture
- Optional supporting media files
- Multipart form upload to the Express backend

## 🧾 Report Tracking

- Generated identifiers such as `CIV20261017-00001`
- Retrieval through `GET /api/reports/:trackingId`
- A separate frontend tracking page

> **Note:** Tracking IDs are derived at read time from `createdAt` and the report ID; they are not stored as a separate database column in the supplied API.

## 🗺️ Maps and Location

- Interactive maps using Leaflet and React Leaflet
- Map markers, filters, popups, bounds updates, and legends
- India-specific location data to support address selection
- An administrator hotspot-map page

## 🧑‍💼 Administration Interfaces

- Report-list page with reusable cards, filters, and detail components
- Statistics cards and loading/empty states
- Assignment modal component present in the frontend
- Hotspot map for geographic visualization

> **Important:** An assignment UI component does not mean report assignment is persisted; backend assignment/update endpoints were not found in the uploaded source. Admin routes do not currently establish secure server-side authorization.

## 📊 Report Statistics

The backend exposes summarized counts for total reports, resolved reports, pending reports, and distinct cities. The returned figures depend on the underlying MySQL table and its records.

## 🌗 Theme, Languages and PWA

- React theme context for visual preferences
- English/Hindi i18next translation files
- Vite PWA manifest with install icons and standalone display mode

> PWA setup is present; comprehensive offline reporting and background synchronization should be independently verified before claiming full offline support.

---

# 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 19 |
| Development/build tool | Vite 7 |
| Styling | Tailwind CSS 4 |
| Client-side routing | React Router |
| HTTP client | Axios / browser requests |
| UI icons | Lucide React |
| Mapping | Leaflet, React Leaflet |
| Language support | i18next, react-i18next |
| PWA | vite-plugin-pwa |
| Browser storage dependency | IndexedDB (`idb`) |
| Backend | Node.js |
| API framework | Express 5 |
| Database | MySQL with `mysql2/promise` |
| Multipart uploads | Multer |
| Environment configuration | dotenv |
| Version control | Git and GitHub |

**Architecture note:** The source includes a conventional React frontend and Express/MySQL backend. Hackathon documentation may describe wider planned services; it should not be confused with functionality already implemented.

---

# 📁 Project Structure

```text
CivicMitra/
│
├── README.md
├── .gitignore                        # Recommended: add before publishing
│
├── backend/
│   ├── db.js                         # MySQL connection pool
│   ├── server.js                     # Express app and API mounting
│   ├── routes/
│   │   └── reportRoutes.js           # Report creation, retrieval and stats
│   ├── uploads/                      # Runtime user-uploaded files (ignore contents)
│   ├── .env                          # Local secrets (DO NOT COMMIT)
│   ├── .env.example                  # Safe configuration template (recommended)
│   ├── package.json
│   └── package-lock.json
│
├── frontend/
│   ├── public/                       # Icons and PWA assets
│   ├── src/
│   │   ├── admin/pages/              # Admin report list and hotspot map
│   │   ├── components/
│   │   │   ├── admin/                # Admin UI building blocks
│   │   │   ├── common/               # Navbar, footer, install UI
│   │   │   ├── Landing/              # Landing-page sections
│   │   │   ├── map/                  # Map components
│   │   │   └── report/               # Reporting form components
│   │   ├── context/                  # Theme and language state
│   │   ├── data/india/               # State/district/city/PIN-code JSON
│   │   ├── i18n/                     # Translation setup/resources
│   │   ├── pages/                    # Citizen pages
│   │   ├── utils/                    # Map utilities
│   │   ├── App.jsx                   # React route definitions
│   │   └── main.jsx                  # React entry point
│   ├── vite.config.js
│   ├── package.json
│   └── package-lock.json
│
└── SIH-31/
    ├── Assets/                       # Wireframes, API YAML and design assets
    ├── CivicMitra_Docs/              # Requirements, research and design PDFs
    └── Diagrams/
        └── Internal Hackathon Diagrams/
            ├── CivicMitra_Architecutre_Diagram.png
            ├── CivicMitra_Dfd.png
            ├── CivicMitra_ER-Diagram.png
            ├── CivicMitra_IntegrationArchitecureDiagram.png
            ├── CivicMitra_Admin_Workflow.png
            └── CivicMitra_User_Workflow.jpeg
```

> `node_modules/`, the actual `.env`, and user-uploaded data should not be committed. The tree above omits generated dependencies and many individual datasets/documents for readability.

---

# 🔄 Application Flow

```text
Citizen / Browser
       │
       ▼
React + Vite Frontend
       │
       ├── Landing / Dashboard / Report List
       │
       ├── Report Form ──────┐
       │                     ▼
       │               POST /api/reports
       │                     │
       │                     ▼
       │             Express + Multer
       │                     │
       │              ┌──────┴──────┐
       │              ▼             ▼
       │         MySQL reports   Uploaded files
       │              │
       │              ▼
       │         Tracking ID
       │
       ├── Track Report ──► GET /api/reports/:trackingId
       │
       └── Admin Views ───► GET /api/reports
                              GET /api/reports/stats
```

### Report submission data flow

```text
Category + Description + Priority + Location + Address + Media
                           │
                           ▼
                  Frontend report form
                           │
                           ▼
                Multipart HTTP request
                           │
                           ▼
                 Express report route
                           │
                   Parse + validate
                           │
                 ┌─────────┴─────────┐
                 ▼                   ▼
           Store file names     Insert report
           and upload files     into MySQL
                 └─────────┬─────────┘
                           ▼
                 Success + Tracking ID
```

---

# 🗄️ Data and Integrations

| Component | Responsibility |
|---|---|
| React pages | Capture issue details and display reports |
| Express routes | Accept report submissions and serve stored report data |
| Multer | Receive and write attached files to `backend/uploads/` |
| MySQL `reports` table | Persist issue details, timestamps, status and other report fields |
| India JSON datasets | Support state/district/city/PIN-code selection |
| Leaflet | Visualize report locations on maps |
| i18next | Provide multilingual UI resources |
| PWA plugin | Configure installability and service-worker behavior |

### Database fields referenced in the backend

The provided API inserts `category`, `description`, `lat`, `lng`, `address`, `city`, `district`, `state`, `stateCode`, `country`, `pincode`, `mediaFiles`, and `priority`.

It also reads `id`, `createdAt`, and `status` when retrieving records or computing statistics. **A complete SQL `CREATE TABLE` migration was not found in the uploaded project**, so the exact types, defaults, constraints, and indexes must be documented separately before another developer can reliably initialize the database.

---

# ✅ Requirements

- Node.js and npm versions compatible with the project's installed Vite/React packages
- A running MySQL server
- A MySQL user/database with permission to read/write the reports table
- A modern web browser
- Git and a GitHub account for publishing
- Optional: VS Code or another JavaScript editor

---

# 🔌 Service Setup

## 1. MySQL

Create a local MySQL database and configure the credentials in `backend/.env`. You must also create a `reports` table compatible with the queries in `backend/routes/reportRoutes.js`.

> **Schema warning:** This repository does not include a verified database migration. Do not assume a fresh installation will work with only a database name; the required table, keys, columns, and defaults must be established first.

## 2. Backend Dependencies

From the project root:

```powershell
cd backend
npm install
```

## 3. Frontend Dependencies

In a separate terminal, from the project root:

```powershell
cd frontend
npm install
```

---

# ⚙️ Configuration

Create `backend/.env` locally with values appropriate to your MySQL installation:

```dotenv
PORT=5000
DB_HOST=localhost
DB_USER=your_mysql_username
DB_PASS=your_mysql_password
DB_NAME=civicmitra
```

Use the same variable names as `backend/db.js`: `DB_HOST`, `DB_USER`, `DB_PASS`, and `DB_NAME`.

**Never commit the real `.env` file.** Add `backend/.env` to `.gitignore` and commit only a placeholder `.env.example`.

The frontend currently contains references to `http://localhost:5000/api/reports` in multiple source files. The `frontend/vite.config.js` development proxy also needs review. For deployment or use on another machine, centralize the API URL in a frontend configuration variable and verify requests and CORS behavior.

---

# ▶️ Running the Application

Open **two terminals** from the project root.

### Terminal 1 — Backend

```powershell
cd backend
npm install
npm run dev
```

The backend defaults to `http://localhost:5000` when `PORT` is not provided.

### Terminal 2 — Frontend

```powershell
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite (commonly `http://localhost:5173`).

### Production-style frontend build

```powershell
cd frontend
npm run build
npm run preview
```

> The frontend build does not automatically deploy the API or MySQL database. Report submission and tracking require a correctly configured, running backend and database schema.

---

# 🧩 Major Application Modules

```text
CivicMitra
│
├── Citizen Experience
│   ├── Landing Page
│   ├── Citizen Dashboard
│   ├── Create Report
│   ├── Report List
│   └── Track Report
│
├── Map Experience
│   ├── Interactive Map
│   ├── Markers + Popups
│   └── Location Filters
│
├── Admin Experience
│   ├── Report List and Filters
│   ├── Report Details
│   ├── Statistics Cards
│   ├── Assignment UI (not API-backed)
│   └── Hotspot Map
│
├── Backend
│   ├── Report Creation
│   ├── Report Retrieval
│   ├── Report Statistics
│   ├── MySQL Connection
│   └── Image Uploads
│
└── Supporting Assets
    ├── India Location Datasets
    ├── Translation Resources
    ├── PWA Configuration
    └── SIH-31 Documentation + Diagrams
```

---

# 🖼️ Project Screenshots

The original archive contains wireframes and design assets, but I did not confirm a dedicated set of final application screenshots suitable for embedding in the README. Add screenshots of the **actual running application** to a new directory such as `docs/screenshots/` and enable the examples below after the files exist.

```markdown
<!-- Add these after taking and committing real screenshots:
![CivicMitra landing page](docs/screenshots/landing.png)
![CivicMitra report form](docs/screenshots/report-form.png)
![CivicMitra tracking page](docs/screenshots/tracking.png)
![CivicMitra admin map](docs/screenshots/admin-map.png)
-->
```

---

# 🧭 Documentation and Diagrams

The project includes the following **existing** materials:

| Artifact | Repository file |
|---|---|
| System architecture | [`CivicMitra_Architecutre_Diagram.png`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_Architecutre_Diagram.png) |
| Data flow diagram | [`CivicMitra_Dfd.png`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_Dfd.png) |
| ER diagram | [`CivicMitra_ER-Diagram.png`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_ER-Diagram.png) |
| Integration architecture | [`CivicMitra_IntegrationArchitecureDiagram.png`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_IntegrationArchitecureDiagram.png) |
| Admin workflow | [`CivicMitra_Admin_Workflow.png`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_Admin_Workflow.png) |
| User workflow | [`CivicMitra_User_Workflow.jpeg`](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_User_Workflow.jpeg) |
| API reference document | [`Civic Mitra APi Doc.pdf`](SIH-31/CivicMitra_Docs/Civic%20Mitra%20APi%20Doc.pdf) |
| Requirements analysis | [`Civic Mitra Requirement Analysis.pdf`](SIH-31/CivicMitra_Docs/Civic%20Mitra%20Requirement%20Analysis.pdf) |
| Problem definition | [`Civic Mitra Problem Definition.pdf`](SIH-31/CivicMitra_Docs/Civic%20Mitra%20Problem%20Definition.pdf) |
| Routing design | [`CivicMitra Problem Routing System - Department Mapping.pdf`](SIH-31/CivicMitra_Docs/CivicMitra%20Problem%20Routing%20System%20-%20Department%20Mapping.pdf) |

### System Architecture Diagram

![CivicMitra architecture diagram](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_Architecutre_Diagram.png)

### Data Flow Diagram

![CivicMitra data flow diagram](SIH-31/Diagrams/Internal%20Hackathon%20Diagrams/CivicMitra_Dfd.png)

> The diagrams reflect hackathon design work; individual flows may represent planned rather than implemented operations.

---

# 🔗 API Endpoints

The following routes are implemented in `backend/routes/reportRoutes.js`:

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/reports` | Submit a report with optional `mediaFiles` uploads |
| `GET` | `/api/reports` | Retrieve all reports; optional `?trackingId=` filter |
| `GET` | `/api/reports/stats` | Return summarized report counts |
| `GET` | `/api/reports/:trackingId` | Look up one report by tracking ID |

**Example response shape for report creation:**

```json
{
  "success": true,
  "message": "Report submitted successfully",
  "reportId": 1,
  "trackingId": "CIV20261017-00001"
}
```

This is an **illustrative** example, not a live API response. No report update, delete, assignment, login, or role-management API endpoints were found in the uploaded backend.

---

# 🧪 Testing Checklist

- [ ] Landing page loads and navigation works
- [ ] Dark/light theme behaves as expected
- [ ] Language selection displays appropriate translated strings
- [ ] Report form validates required inputs
- [ ] India address and location selectors work
- [ ] Photo attachments upload to `backend/uploads/`
- [ ] New records appear in the MySQL `reports` table
- [ ] Tracking ID is returned after successful submission
- [ ] Tracking page retrieves a matching report
- [ ] Invalid/nonexistent tracking IDs return appropriate errors
- [ ] Report-list page displays backend results
- [ ] Statistics endpoint returns accurate counts
- [ ] Admin report and hotspot-map views render properly
- [ ] Map markers and filters work with actual records
- [ ] Frontend build completes (`npm run build`)
- [ ] Responsive views function on mobile and desktop
- [ ] No credentials or personal report data are accidentally exposed
- [ ] Failed backend/database requests produce useful UI errors

**Note:** This is a recommended validation checklist, **not** a claim that all tests have passed.

---

# 🔒 Security Notes

CivicMitra should be treated as an **academic/hackathon prototype**, not a production-secured public reporting service.

Before committing or deploying:

- Exclude `backend/.env` and rotate any credentials previously shared or committed.
- Ignore `node_modules/`, build outputs, logs, and actual user uploads.
- Review `SIH-31/` documentation, screenshots, and YAML assets for private information or secrets.
- Add genuine authentication and role-based authorization for administrative operations.
- Restrict public read access to complaint records; civic reports can contain sensitive location and personal information.
- Validate data types, coordinates, file extensions, file sizes and request bodies server-side.
- Harden Multer filename handling and serve uploads with appropriate access rules.
- Restrict CORS for deployed environments; the current Express app uses permissive `cors()`.
- Add API request throttling, error monitoring, and audit logging.
- Protect against unsafe rendering of untrusted content and follow applicable privacy rules.

**Do not upload database passwords, real complaint images, API secrets, or private user data to a public GitHub repository.**

---

# ⚠️ Current Limitations

- The MySQL schema/migration required for a fresh installation is not included in the archive.
- The frontend contains hardcoded localhost backend URLs and a development proxy requiring review.
- No backend authentication, administrator authorization, or access-control rules were found.
- The available backend routes support creation and retrieval, not report status changes, assignment, or deletion.
- The assignment modal is a UI component, not evidence of a persisted assignment workflow.
- Some citizen-facing dashboard figures appear to be demonstration values.
- A PWA configuration exists, but complete offline submission/synchronization is not verified.
- Automated integration/security tests and a complete deployment pipeline were not identified.
- Tracking IDs are generated from dates and IDs, rather than stored independently; robustness around date validation should be reviewed.

---

# 🔮 Future Improvements

- Secure citizen accounts and admin role-based access
- API endpoints for assignment, status updates, resolution and escalation
- Department-specific complaint routing and ownership
- Database schema migration scripts and seed data
- Centralized environment-aware frontend API configuration
- Live dashboard analytics using authenticated backend data
- Better file-upload validation and cloud storage options
- Verified offline draft queue and background synchronization
- Map-based hotspot clustering and prioritization analytics
- Citizen notifications and clear status timelines
- Accessibility and localization coverage improvements
- Unit, API and end-to-end tests with CI/CD
- Documented deployment to suitable frontend, API and database hosts
- Data retention, reporting abuse controls and privacy policies

---

# 🌐 GitHub Setup

Create an **empty** public repository called `CivicMitra` under [tushar-011](https://github.com/tushar-011), and make sure you have a suitable `.gitignore` and a safe `backend/.env.example`.

From the **project root**:

```powershell
git init
git branch -M main
git remote add origin https://github.com/tushar-011/CivicMitra.git

# Confirm the sensitive backend environment file is excluded
git check-ignore backend/.env

# Review tracked files before committing
git add .
git status
git diff --cached --stat

git commit -m "Initial commit: CivicMitra civic reporting platform"
git push -u origin main
```

For later updates:

```powershell
git status
git add .
git commit -m "Update CivicMitra documentation and app"
git push
```

> If the repository is already initialized or has a remote, check `git remote -v` before re-running initialization commands. Never force-push solely to bypass an unexpected remote history.

---

# 🏁 Project Status

### ✅ Academic/hackathon prototype and supporting assets available

The uploaded codebase demonstrates:

- React-based reporting and dashboard UIs
- Express/MySQL report API design
- Location-aware forms and Leaflet mapping components
- Attachment upload plumbing
- Complaint ID-based retrieval
- Multilingual and theme resources
- PWA installation configuration
- Extensive hackathon research, workflow and system diagrams

**Current stage:** Repository cleanup, environment/database documentation, end-to-end verification, and GitHub/portfolio publication.

---

# 👨‍💻 Development Notes

CivicMitra was built as a civic-technology academic/hackathon project, exploring how accessible interfaces, mapping and structured reports can support digital public-service workflows.

It can serve as a portfolio demonstration of **full-stack JavaScript development, REST API design, relational data storage, location-aware interfaces, and civic-tech system planning**.

---

<div align="center">

## 🏛️ CivicMitra

**Report • Track • Map • Improve**

Built with **React + Vite + Tailwind CSS + Node.js + Express + MySQL + Leaflet**

</div>

---

# 👨‍💻 Author

### Tushar Thakur

**MCA Data Science**  
**Chandigarh University**

[![GitHub](https://img.shields.io/badge/GitHub-tushar--011-181717?logo=github)](https://github.com/tushar-011)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Tushar%20Thakur-0A66C2?logo=linkedin)](https://www.linkedin.com/in/tushar-thakur-8848a7396/)

[![Email](https://img.shields.io/badge/Email-artificial.thakur%40gmail.com-EA4335?logo=gmail)](mailto:artificial.thakur@gmail.com)
