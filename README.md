# SEO Trend Insights Dashboard (seo-trend-insights-dashboard-20250717_165255)

A multi-tenant React application for marketers and agencies. It combines an LLM-based SEO Trends API with Google Analytics and Search Console to deliver personalized SEO trend insights, interactive dashboards, scheduled PDF/CSV reports, real-time alerts, and workspace management?all within a secure, role-based environment.

Project Description:  
https://docs.google.com/document/d/18sXFI5lMcjd-66HGjv78CvW48b6g_WMweHjeByJ8faQ/

---

## Table of Contents
1. [Overview](#overview)  
2. [Key Features](#key-features)  
3. [Architecture & Project Structure](#architecture--project-structure)  
4. [Installation](#installation)  
5. [Usage](#usage)  
6. [Components](#components)  
7. [Dependencies](#dependencies)  
8. [Environment Variables](#environment-variables)  
9. [Scripts (CI/CD)](#scripts-cicd)  
10. [Contributing](#contributing)  
11. [License](#license)

---

## Overview

SEO Trend Insights Dashboard helps marketing teams and agencies to:

- Ingest and analyze real-time SEO trend data from a custom LLM-based SEO Trends API
- Combine data from Google Analytics and Search Console
- Visualize trends with interactive, filterable dashboards and drill-down charts
- Schedule automated PDF/CSV reports and deliver via email or webhooks
- Configure real-time alerts on trend thresholds
- Manage multiple client workspaces with role-based access control
- Maintain strict tenant isolation, audit logging, and global error handling

### User Flow

1. **Authentication**  
   Users sign in with email/password or OAuth2 connectors (Google, etc.).  
2. **Onboarding**  
   First-time users set up profiles, select niches, and connect Google Analytics/Search Console.  
3. **Dashboard**  
   View/filter SEO trend charts in real time.  
4. **Report Scheduler**  
   Build, preview, schedule, and export PDF/CSV reports.  
5. **Alerts**  
   Configure email/webhook notifications with retry/backoff logic.  
6. **Workspace Manager**  
   Create/switch workspaces, assign user roles, and isolate tenant data.  

---

## Key Features

- Secure JWT session management with OAuth2 connectors  
- Strict row-level multi-tenant isolation  
- Real-time data ingestion with caching and circuit breakers  
- Interactive, filterable dashboards with drill-down panels  
- Customizable PDF/CSV report scheduling and delivery  
- Robust email and webhook alerts with retry/backoff  
- Workspace and user role administration  
- Comprehensive audit logging of all tenant actions  
- Global error handling with custom 404/403 pages  
- Responsive, accessible design (dark mode, WCAG 2.1 AA)  
- Integrated logging and client-side error reporting  

---

## Architecture & Project Structure

- **Entry Point**  
  - `index.js`: Bootstrap and render React app, set up global error handler  

- **Root**  
  - `app.jsx`: Defines routes via React Router, renders layout  

- **Layout Components**  
  - `header.tsx`: Top navigation, global search, theme toggle  
  - `sidebar.tsx`: Collapsible navigation links (Dashboard, Reports, Alerts, Workspaces)  

- **Pages**  
  - `authentication.tsx`: Email/password login, OAuth2, JWT handling  
  - `onboarding.tsx`: Profile setup, niche selection, OAuth flow  
  - `dashboard.tsx`: Filterable trend charts, drill-downs  
  - `reportscheduler.tsx`: Visual report builder, scheduling, PDF/CSV export  
  - `alertsettings.tsx`: Real-time alert configuration (email/webhooks)  
  - `workspacemanager.tsx`: Workspace CRUD, user roles  

- **Shared UI**  
  - `errorboundary.tsx`: Global error catcher and fallback UI  
  - `loadingspinner.tsx`: Universal loading indicator  
  - `notfound.tsx`: Custom 404 page  
  - `accessdenied.tsx`: Custom 403 page  

- **Service Layer**  
  - `apiclient.ts`: HTTP client with retry, circuit breaker, tenant header injection  
  - `authservice.ts`: Authentication flows, token management, role checks  
  - `dataingestionservice.ts`: LLM SEO Trends & Google data ingestion, caching  
  - `scoringservice.ts`: Topic scoring and prioritization engine  
  - `reportservice.ts`: PDF/CSV generation, storage orchestration  
  - `workspaceservice.ts`: Workspace CRUD, multi-tenant isolation  
  - `notificationservice.ts`: Email/webhook dispatch with retry/backoff  
  - `auditservice.ts`: Tenant-aware action logging  

---

## Installation

1. Clone the repository  
   ```bash
   git clone https://github.com/your-org/seo-trend-insights-dashboard-20250717_165255.git
   cd seo-trend-insights-dashboard-20250717_165255
   ```

2. Install dependencies  
   ```bash
   npm install
   # or
   yarn install
   ```

3. Create a `.env` file in the root directory (see [Environment Variables](#environment-variables))  

4. Start in development mode  
   ```bash
   npm start
   # or
   yarn start
   ```

5. Open your browser at `http://localhost:3000`

---

## Usage

- **Sign In**  
  Navigate to the login page, sign in with email/password or via Google OAuth.

- **Onboarding**  
  New users will be guided through profile setup, niche selection, and GA/Search Console connection.

- **Dashboard Filters**  
  Use date pickers, keyword filters, and niche selectors to update charts in real time.

- **Schedule Reports**  
  Go to ?Reports? ? configure report, select recurrence, choose PDF/CSV, and save.  

- **Configure Alerts**  
  Go to ?Alerts? ? set thresholds, select channels (email/webhook), test webhook, and save.

- **Manage Workspaces**  
  Go to ?Workspaces? ? create new workspace, assign roles to team members, or switch contexts.

---

## Components

List of core files and their responsibilities:

- apiclient.ts  
  Central HTTP client (Axios) with retry, interceptors, tenant header injection.  
- authservice.ts  
  Login/logout, token storage/refresh, OAuth2 connectors, role checks.  
- dataingestionservice.ts  
  Fetches SEO Trends, Google Analytics, Search Console; caches responses.  
- scoringservice.ts  
  Scores and prioritizes topics based on trend velocity and relevance.  
- reportservice.ts  
  Generates and schedules PDF/CSV reports, stores to S3, dispatches via SES/SendGrid.  
- workspaceservice.ts  
  Manages multi-tenant workspace CRUD and user roles.  
- notificationservice.ts  
  Sends email/webhook notifications with exponential backoff.  
- auditservice.ts  
  Logs tenant actions for compliance and security.  

- authentication.tsx  
  UI for login/signup, password reset, and OAuth2 buttons.  
- onboarding.tsx  
  Guided niche selection and OAuth flow for GA/Search Console.  
- dashboard.tsx  
  Main dashboard with charts, filters, drill-downs.  
- reportscheduler.tsx  
  Build, preview, and schedule SEO reports.  
- alertsettings.tsx  
  Configure real-time alerts and test notifications.  
- workspacemanager.tsx  
  Admin UI for creating/switching workspaces and setting roles.  

- header.tsx, sidebar.tsx  
  Global navigation layout.  
- errorboundary.tsx, loadingspinner.tsx, notfound.tsx, accessdenied.tsx  
  Shared UI for errors, loading states, and access control.

---

## Dependencies

- React  
- React Router DOM  
- Axios  
- JWT Decode  
- Charting Library (e.g., recharts or chart.js)  
- date-fns (or Moment.js)  
- classnames  
- dotenv  
- Testing: Jest, React Testing Library  
- Linting: ESLint, Prettier  

Refer to `package.json` for full dependency list.

---

## Environment Variables

Create a `.env` in project root:

```
REACT_APP_API_BASE_URL=https://api.yourdomain.com
REACT_APP_GOOGLE_CLIENT_ID=your-google-client-id
REACT_APP_GOOGLE_CLIENT_SECRET=your-google-client-secret
REACT_APP_OAUTH_REDIRECT_URI=http://localhost:3000/oauth/callback
...
```

---

## Scripts (CI/CD)

Defined in `package.json`:

- `npm run lint` ? Lint code with ESLint  
- `npm run test` ? Run unit tests (Jest)  
- `npm run build` ? Create production build  
- `npm run start` ? Start development server  
- `npm run deploy` ? Trigger deployment pipeline (configure in CI)

---

## Contributing

1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/xyz`)  
3. Commit your changes (`git commit -m 'Add xyz feature'`)  
4. Push (`git push origin feature/xyz`)  
5. Open a Pull Request  

Please follow the existing code style, write unit tests for new features, and update documentation as needed.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.