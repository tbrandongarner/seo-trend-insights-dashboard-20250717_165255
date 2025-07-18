# seo-trend-insights-dashboard-20250717_165255

A multi-tenant React application for marketers and agencies that combines an LLM-based SEO Trends API with Google Analytics and Search Console. It delivers personalized SEO trend insights, interactive dashboards, scheduled PDF/CSV reports, real-time alerts, and workspace management?all within a secure, role-based environment.

---

## Table of Contents

1. [Overview](#overview)  
2. [Key Features](#key-features)  
3. [Architecture & Project Structure](#architecture--project-structure)  
4. [Installation](#installation)  
5. [Configuration](#configuration)  
6. [Usage](#usage)  
7. [Components & Services](#components--services)  
8. [Dependencies](#dependencies)  
9. [Environment Variables](#environment-variables)  
10. [Testing](#testing)  
11. [CI/CD & Deployment](#cicd--deployment)  
12. [Contributing](#contributing)  
13. [License](#license)  

---

## Overview

SEO Trend Insights Dashboard provides:

- Secure JWT session management with OAuth2 connectors  
- Row-level multi-tenant isolation  
- Real-time data ingestion (LLM SEO Trends + Google APIs) with caching and circuit breakers  
- Interactive, filterable trend dashboards with drill-downs  
- Customizable PDF/CSV report scheduling and delivery  
- Robust email and webhook alerts with retry/backoff  
- Workspace and user-role administration UI  
- Comprehensive audit logging  
- Global error handling, custom 404/403 pages  
- Responsive design, dark mode, WCAG 2.1 AA compliant  

---

## Key Features

- **Authentication & Authorization**  
  Email/password login, OAuth2 (Google), JWT handling, role checks  
- **Onboarding**  
  Profile setup, niche selection, GA/Search Console OAuth flow  
- **Dashboard**  
  Trend charts, filters, drill-downs, loading states  
- **Report Scheduler**  
  Visual report builder, recurrence settings, PDF/CSV export  
- **Alert Settings**  
  Real-time alert configuration via email/webhooks, retry logic  
- **Workspace Manager**  
  Multi-tenant workspace CRUD, user roles, access control  
- **Global UI**  
  Error boundary, loading spinner, 404/403 pages  

---

## Architecture & Project Structure

- **Entry Point**  
  - `index.js`  
- **Main App**  
  - `app.jsx` (React Router, route definitions)  
- **Layout Components**  
  - `header.tsx` (top navigation, global search, theme toggle)  
  - `sidebar.tsx` (primary navigation links)  
- **Page Components**  
  - `authentication.tsx`  
  - `onboarding.tsx`  
  - `dashboard.tsx`  
  - `reportscheduler.tsx`  
  - `alertsettings.tsx`  
  - `workspacemanager.tsx`  
- **Shared UI**  
  - `errorboundary.tsx`  
  - `loadingspinner.tsx`  
  - `notfound.tsx`  
  - `accessdenied.tsx`  
- **Service Layer** (`.ts` files)  
  - `apiclient.ts`  
  - `authservice.ts`  
  - `dataingestionservice.ts`  
  - `scoringservice.ts`  
  - `reportservice.ts`  
  - `workspaceservice.ts`  
  - `notificationservice.ts`  
  - `auditservice.ts`  
- **Config & Scripts**  
  - `package.json`  
  - `.env`  

---

## Installation

1. Clone the repo  
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

3. Create a `.env` file in the project root (see [Environment Variables](#environment-variables))

---

## Configuration

Required environment variables:

```
REACT_APP_API_BASE_URL=https://api.yourdomain.com
REACT_APP_OAUTH_CLIENT_ID=your-google-oauth-client-id
REACT_APP_OAUTH_REDIRECT_URI=https://app.yourdomain.com/oauth-callback
AWS_S3_BUCKET=your-s3-bucket
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
EMAIL_SERVICE_API_KEY=your-email-service-key
```

---

## Usage

### Development

```bash
npm run start
# or
yarn start
```
Open http://localhost:3000

### Building for Production

```bash
npm run build
```

### Running Tests

```bash
npm run test
```

### Linting & Formatting

```bash
npm run lint
npm run format
```

---

## Components & Services

### Page Components

- **Authentication** (`authentication.tsx`)  
  Login, signup, password reset, OAuth2 UI  
- **Onboarding** (`onboarding.tsx`)  
  User profile setup, niche selection, GA/Search Console OAuth  
- **Dashboard** (`dashboard.tsx`)  
  Filterable trend charts, summary cards, drill-downs  
- **Report Scheduler** (`reportscheduler.tsx`)  
  Build and schedule PDF/CSV reports, recurrence settings  
- **Alert Settings** (`alertsettings.tsx`)  
  Configure real-time alerts, email/webhook notifications  
- **Workspace Manager** (`workspacemanager.tsx`)  
  Create workspaces, assign roles, switch tenants  

### Layout & Shared UI

- **Header** (`header.tsx`)  
- **Sidebar** (`sidebar.tsx`)  
- **ErrorBoundary** (`errorboundary.tsx`)  
- **LoadingSpinner** (`loadingspinner.tsx`)  
- **NotFound (404)** (`notfound.tsx`)  
- **AccessDenied (403)** (`accessdenied.tsx`)  

### Services

- **ApiClient** (`apiclient.ts`)  
  Axios HTTP client with retry, circuit breaker, tenant header  
- **AuthService** (`authservice.ts`)  
  Login, logout, token refresh, role checks  
- **DataIngestionService** (`dataingestionservice.ts`)  
  Fetch & cache LLM SEO Trends, GA & Search Console data  
- **ScoringService** (`scoringservice.ts`)  
  Topic scoring & prioritization algorithm  
- **ReportService** (`reportservice.ts`)  
  PDF/CSV generation, scheduling, S3 storage  
- **WorkspaceService** (`workspaceservice.ts`)  
  Multi-tenant workspace CRUD & isolation  
- **NotificationService** (`notificationservice.ts`)  
  Email/webhook dispatch with backoff  
- **AuditService** (`auditservice.ts`)  
  Tenant-aware action logging  

---

## Dependencies

- React 18  
- TypeScript  
- React Router  
- Axios  
- Chart.js / Recharts / D3 (choose one)  
- JWT decode  
- OAuth2 client  
- AWS SDK (S3, SES) / SendGrid SDK  
- classnames  
- styled-components / Tailwind CSS  
- Jest & React Testing Library  
- ESLint & Prettier  

See `package.json` for full list.

---

## Environment Variables

Create a `.env` file:

```bash
REACT_APP_API_BASE_URL=
REACT_APP_OAUTH_CLIENT_ID=
REACT_APP_OAUTH_REDIRECT_URI=
AWS_S3_BUCKET=
AWS_REGION=
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
EMAIL_SERVICE_API_KEY=
```

---

## Testing

- **Unit Tests**: `npm run test`  
- **Coverage**: ensure >80% coverage  

---

## CI/CD & Deployment

- **Lint & Test** on each PR  
- **Build** & **Deploy** to staging/production via GitHub Actions or your preferred pipeline  
- **Auto-invalidate** CDN cache on new release  

---

## Contributing

1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/xyz`)  
3. Commit your changes (`git commit -m "feat: add xyz"`)  
4. Push to branch (`git push origin feature/xyz`)  
5. Open a Pull Request  

Please read our [CONTRIBUTING.md] if available.

---

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.