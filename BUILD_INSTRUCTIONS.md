# SEO Trend Insights – Build Instructions

These instructions will guide you through setting up, building, testing, and deploying the **SEO Trend Insights** React application. Follow each section carefully to ensure a smooth development and production workflow.

---

## Table of Contents

1. [Prerequisites & Environment Setup](#prerequisites--environment-setup)  
2. [Step-by-Step Build Process](#step-by-step-build-process)  
3. [Deployment Instructions](#deployment-instructions)  
4. [Testing Procedures](#testing-procedures)  
5. [Troubleshooting Common Issues](#troubleshooting-common-issues)  
6. [Special Considerations](#special-considerations)  

---

## 1. Prerequisites & Environment Setup

Before you begin, make sure you have the following installed:

- **Node.js** (v16.x or later)  
- **npm** (v8.x or later) or **Yarn** (v1.22.x or later)  
- **Git** (for version control)  
- A modern code editor (e.g., VS Code)

### 1.1. Clone the Repository

```bash
git clone https://github.com/your-org/seo-trend-insights.git
cd seo-trend-insights
```

### 1.2. Install Project Dependencies

Using npm:

```bash
npm install
```

Or using Yarn:

```bash
yarn install
```

### 1.3. Environment Variables

Create a `.env` file in the project root (it’s already listed as a dependency for `apiclient.ts`):

```bash
cp .env.example .env
```

Populate `.env` with:

```
REACT_APP_API_BASE_URL=https://api.yourdomain.com
REACT_APP_AUTH_CLIENT_ID=your-client-id
REACT_APP_AUTH_CLIENT_SECRET=your-secret
# Add any additional keys as needed
```

---

## 2. Step-by-Step Build Process

The components below are listed in **execution order**. Components marked **Fail** require manual fixes before successful build.

### 2.1. Execution Order 0

1. **package.json** (.json) – ✔ Pass  
   - Ensure all dependencies and scripts (`start`, `build`, `test`) are defined.

### 2.2. Execution Order 1

2. **apiclient.ts** (.ts) – ✔ Pass  
   - Compiles against `.env` variables.  
   - Exposes REST/GraphQL methods to other services.

3. **index.js** (.js) – ✔ Pass  
   - Application entry: renders `<App />` into DOM.

4. **header.tsx** (.tsx) – ✔ Pass  
   - Static header component, imports React and styles.

**Commands:**

```bash
npm run tsc -- apiclient.ts
npm run build-js -- index.js header.tsx
```

### 2.3. Execution Order 2

5. **authservice.ts** (.ts) – ✔ Pass  
   - Depends on `apiclient.ts`. Auth flows, token refresh.

6. **app.jsx** (.jsx) – ✖ Fail  
   - Main App wrapper.  
   - **Action Required:** Fix missing import of `BrowserRouter` from `react-router-dom` and remove unsupported JSX syntax.  

7. **sidebar.tsx** (.tsx) – ✔ Pass  
   - Side navigation component.

**Commands:**

```bash
# After fixing app.jsx:
npm run tsc -- authservice.ts app.jsx
npm run build-js -- sidebar.tsx
```

### 2.4. Execution Order 3

8. **dataingestionservice.ts** (.ts) – ✔ Pass  
   - Pulls data from third‐party APIs via `apiclient.ts`.

9. **errorboundary.tsx** (.tsx) – ✔ Pass  
   - React error boundary wrapper.

```bash
npm run tsc -- dataingestionservice.ts
npm run build-js -- errorboundary.tsx
```

### 2.5. Execution Order 4

10. **scoringservice.ts** (.ts) – ✔ Pass  
    - Processes and scores ingested data.

11. **loadingspinner.tsx** (.tsx) – ✔ Pass  
    - Loading indicator component.

```bash
npm run tsc -- scoringservice.ts
npm run build-js -- loadingspinner.tsx
```

### 2.6. Execution Order 5

12. **reportservice.ts** (.ts) – ✔ Pass  
    - Generates and exports report data.

13. **notfound.tsx** (.tsx) – ✔ Pass  
    - 404 component.

```bash
npm run tsc -- reportservice.ts
npm run build-js -- notfound.tsx
```

### 2.7. Execution Order 6

14. **workspaceservice.ts** (.ts) – ✖ Fail  
    - Depends on `authservice.ts`.  
    - **Action Required:** Correct the import path of `AuthService` and handle missing promise rejection.

15. **accessdenied.tsx** (.tsx) – ✔ Pass  
    - Renders “Access Denied” message.

```bash
# After fixes:
npm run tsc -- workspaceservice.ts
npm run build-js -- accessdenied.tsx
```

### 2.8. Execution Order 7

16. **authentication.tsx** (.tsx) – ✔ Pass  
    - Login/logout flow UI.

17. **notificationservice.ts** (.ts) – ✔ Pass  
    - Handles in-app notifications.

```bash
npm run tsc -- notificationservice.ts
npm run build-js -- authentication.tsx
```

### 2.9. Execution Order 8

18. **onboarding.tsx** (.tsx) – ✔ Pass  
    - New‐user wizard; depends on `authentication.tsx`.

19. **auditservice.ts** (.ts) – ✔ Pass  
    - Tracks user actions.

```bash
npm run tsc -- auditservice.ts
npm run build-js -- onboarding.tsx
```

### 2.10. Execution Order 9–12

20. **dashboard.tsx** (.tsx) – ✔ Pass  
21. **reportscheduler.tsx** (.tsx) – ✔ Pass  
22. **alertsettings.tsx** (.tsx) – ✔ Pass  
23. **workspacemanager.tsx** (.tsx) – ✔ Pass  

```bash
npm run build-js -- dashboard.tsx
npm run build-js -- reportscheduler.tsx alertsettings.tsx workspacemanager.tsx
```

### 2.11. Final Bundle

Use your build tool (CRA, Webpack, etc.):

```bash
npm run build
```

This produces a `build/` directory ready for deployment.

---

## 3. Deployment Instructions

1. **Serve Static Files**  
   - Upload the contents of `build/` to your static host (Netlify, Vercel, AWS S3 + CloudFront).

2. **Configure Redirects**  
   - For single-page routing, ensure all unknown paths redirect to `index.html`.  
   - Example (Netlify `_redirects` file):
     ```
     /*    /index.html   200
     ```

3. **Environment Variables**  
   - In your hosting dashboard, re-set the same `REACT_APP_...` keys as in your local `.env`.

4. **CDN & Caching**  
   - Invalidate caches after each deploy.  
   - Configure long-term caching for static assets (e.g. `*.js`, `*.css`).

---

## 4. Testing Procedures

- **Unit Tests** (Jest + React Testing Library):
  ```bash
  npm run test
  ```
- **Coverage Report**:
  ```bash
  npm run test -- --coverage
  ```
- **End-to-End Tests** (Cypress or Playwright):
  1. Start the dev server:
     ```bash
     npm start
     ```
  2. In a new terminal:
     ```bash
     npm run e2e
     ```

---

## 5. Troubleshooting Common Issues

- **Node Version Mismatch**  
  - Use [nvm](https://github.com/nvm-sh/nvm) to switch to the correct Node version.
- **Missing `.env` Variables**  
  - Builds will fail if `REACT_APP_*` env vars are undefined.
- **TypeScript Errors**  
  - Run `npm run tsc` to see detailed errors; adjust types or imports.
- **Module Not Found**  
  - Verify file names & relative import paths (case-sensitive on Linux).
- **Component Fails After Build**  
  - Check the browser console for runtime errors (missing props, undefined functions).

---

## 6. Special Considerations

- **TypeScript + React**  
  - Ensure `tsconfig.json` has `jsx: "react-jsx"` or `react-jsxdev`.
- **Environment Variables in React**  
  - Only `REACT_APP_` prefixed vars are embedded at build time.
- **Code Splitting & Lazy Loading**  
  - Utilize `React.lazy()` for large components (e.g., `dashboard.tsx`, `reportscheduler.tsx`).
- **Security**  
  - Never store sensitive secrets in the client. Keep them on your backend.
- **Performance**  
  - Audit bundle size with [source-map-explorer](https://github.com/danvk/source-map-explorer).

---

You are now ready to build, test, and deploy **SEO Trend Insights**. Happy coding! 🚀