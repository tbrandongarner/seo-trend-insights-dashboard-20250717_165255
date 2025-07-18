# SEO Trend Insights

A multi-tenant React application for marketers and agencies that delivers personalized SEO trend insights by combining an LLM-based SEO Trends API with Google Analytics and Search Console. It features interactive dashboards, scheduled PDF/CSV reports, real-time alerts, workspace management, role-based access control, and comprehensive audit logging—all within a secure, responsive UI.

---

## Table of Contents

- [Overview](#overview)  
- [Features](#features)  
- [Architecture](#architecture)  
- [Installation](#installation)  
- [Usage](#usage)  
- [Components](#components)  
- [Dependencies](#dependencies)  
- [Environment Variables](#environment-variables)  
- [Development & Testing](#development--testing)  
- [CI/CD & Deployment](#cicd--deployment)  
- [Contributing](#contributing)  
- [License](#license)  

---

## Overview

SEO Trend Insights Dashboard lets agencies and in-house marketing teams:

- Authenticate via email/password or OAuth2 (Google, Microsoft).  
- Onboard new users with niche selection and Google Analytics/Search Console data connection.  
- Explore interactive, filterable trend charts with drill-down capabilities.  
- Schedule recurring PDF/CSV reports delivered by email or webhook.  
- Configure real-time alerts on custom thresholds.  
- Manage multi-tenant workspaces and user roles.  
- View custom 403/404 pages, global error handling, and loading states.  
- Track every action with tenant-aware audit logs.  
- Operate in light/dark mode with WCAG 2.1 AA compliance.

---

## Features

- Secure JWT session management with OAuth2 connectors  
- Row-level multi-tenant isolation  
- Real-time ingestion of LLM SEO trends & Google API data with caching & circuit breakers  
- Interactive, drill-down dashboards  
- Customizable PDF/CSV report scheduling  
- Robust email and webhook alerts with exponential backoff  
- Workspace and user role administration  
- Comprehensive audit logging  
- Global error boundaries, custom 404/403, loading spinners  
- Responsive, accessible design (dark mode support)

---

## Architecture

• **Entry Point**  
  - `index.js`: ReactDOM render, global error handler setup  

• **Root**  
  - `app.jsx`: Main component, React Router configuration  

• **Layout**  
  - `header.tsx`: Top nav, global search, theme toggle  
  - `sidebar.tsx`: Primary nav (Dashboard, Reports, Alerts, Workspaces)  

• **Page Components**  
  - `authentication.tsx`  
  - `onboarding.tsx`  
  - `dashboard.tsx`  
  - `reportscheduler.tsx`  
  - `alertsettings.tsx`  
  - `workspacemanager.tsx`  

• **Shared UI**  
  - `errorboundary.tsx`  
  - `loadingspinner.tsx`  
  - `notfound.tsx`  
  - `accessdenied.tsx`  

• **Service Layer**  
  - `apiclient.ts`  
  - `authservice.ts`  
  - `dataingestionservice.ts`  
  - `scoringservice.ts`  
  - `reportservice.ts`  
  - `workspaceservice.ts`  
  - `notificationservice.ts`  
  - `auditservice.ts`  

• **State Management**  
  - Local React state & Context API  

• **Styling & Accessibility**  
  - Responsive design, dark mode, WCAG 2.1 AA  

• **Observability**  
  - Logging in services, client errors via `ErrorBoundary`  

---

## Installation

1. Clone the repository  
   ```bash
   git clone https://github.com/your-org/seo-trend-insights.git
   cd seo-trend-insights
   ```

2. Install dependencies  
   ```bash
   npm install
   ```

3. Copy and configure environment variables  
   ```bash
   cp .env.example .env
   # Edit .env with your API URLs, OAuth credentials, AWS keys, etc.
   ```

4. Start development server  
   ```bash
   npm run dev
   ```

5. Build for production  
   ```bash
   npm run build
   ```

---

## Usage

### Running Locally

```bash
npm run dev
# Visit http://localhost:3000
```

### Building & Starting

```bash
npm run build
npm start
```

### Running Tests

```bash
npm test
```

### Example: Fetching SEO Trends Programmatically

```ts
import apiClient from './services/apiclient';
import { TrendFetchParams } from './services/dataingestionservice';

async function getTrends() {
  const params: TrendFetchParams = { tenantId: 'abc123', niche: 'technology', timeframe: '7days' };
  const response = await apiClient.get('/trends', { params });
  console.log(response.data);
}
```

---

## Components

### Service Layer

- **apiclient.ts**  
  Central HTTP client using Axios with retry, circuit breakers, and tenant header injection.

- **authservice.ts**  
  Handles signup, login, token refresh, OAuth2 connectors, and role checks.

- **dataingestionservice.ts**  
  Fetches & caches LLM SEO trends and Google Analytics/Search Console data.

- **scoringservice.ts**  
  Implements SEO topic scoring (trend velocity, niche relevance, historic performance).

- **reportservice.ts**  
  Generates, schedules, and delivers PDF/CSV reports via S3 + SES/SendGrid.

- **workspaceservice.ts**  
  CRUD for multi-tenant workspaces, user roles, and data isolation logic.

- **notificationservice.ts**  
  Sends email/webhook notifications with exponential backoff retry logic.

- **auditservice.ts**  
  Logs tenant-aware actions for compliance and audit trails.

### Page Components

- **authentication.tsx**  
  Email/password login, OAuth2 connectors, JWT handling.

- **onboarding.tsx**  
  User profile setup, niche selection, GA/Search Console OAuth flow.

- **dashboard.tsx**  
  Filterable trend charts, drill-downs, loading skeletons.

- **reportscheduler.tsx**  
  Visual report builder, recurrence settings, PDF/CSV export.

- **alertsettings.tsx**  
  Real-time alert configuration via email/webhooks.

- **workspacemanager.tsx**  
  Workspace creation, user roles, and access control.

### Layout & Shared UI

- **header.tsx** / **sidebar.tsx**  
  App chrome with navigation, search, theme toggle.

- **errorboundary.tsx**  
  Global React error catcher with fallback UI.

- **loadingspinner.tsx**  
  Universal loading indicator.

- **notfound.tsx** / **accessdenied.tsx**  
  Custom 404 and 403 pages.

---

## Dependencies

- React 18+  
- React Router DOM  
- Axios  
- Chart.js (or D3.js)  
- Tailwind CSS / Styled-Components  
- JWT Decode  
- Dotenv  
- AWS SDK (S3, SES)  
- SendGrid (optional)  
- Jest & React Testing Library  

See full list in `package.json`.

---

## Environment Variables

```ini
REACT_APP_API_BASE_URL=https://api.yourdomain.com
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
OAUTH_REDIRECT_URI=https://app.yourdomain.com/oauth/callback
AWS_S3_BUCKET=seo-trend-reports
AWS_REGION=us-east-1
AWS_ACCESS_KEY_ID=XXX
AWS_SECRET_ACCESS_KEY=XXX
SENDGRID_API_KEY=XXX
```

---

## Development & Testing

- **Linting**  
  ```bash
  npm run lint
  ```

- **Unit Tests**  
  ```bash
  npm test
  ```

- **Type Checking**  
  ```bash
  npm run typecheck
  ```

---

## CI/CD & Deployment

- **Pre-commit hooks:** ESLint, Prettier  
- **GitHub Actions:**  
  - `lint`  
  - `test`  
  - `build`  
  - `deploy` to staging/production  

See `.github/workflows/` for pipeline definitions.

---

## Contributing

1. Fork the repo  
2. Create a feature branch (`git checkout -b feature/awesome`)  
3. Commit your changes (`git commit -m 'Add awesome feature'`)  
4. Push to your branch (`git push origin feature/awesome`)  
5. Open a Pull Request  

Please follow the [Code of Conduct](CODE_OF_CONDUCT.md) and fill out our [PR template](.github/PULL_REQUEST_TEMPLATE.md).

---

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---

**Enjoy building better SEO strategies with real-time, data-driven insights!**