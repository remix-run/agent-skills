# Getting Started with React Router Framework Mode

## Installation

Create a new React Router project:

```shellscript
npx create-react-router@latest my-app
cd my-app
npm install
npm run dev
```

The app runs at `http://localhost:5173`.

## Project Structure

A framework mode project has this structure:

```
my-app/
├── app/
│   ├── root.tsx          # Root layout, wraps all routes
│   ├── routes.ts         # Route configuration
│   └── routes/           # Route modules
├── react-router.config.ts # Framework configuration
├── vite.config.ts
└── package.json
```

## The Root Route (`app/root.tsx`)

The root route is the shell of your application:

```tsx
import {
  Links,
  Meta,
  Outlet,
  Scripts,
  ScrollRestoration,
} from "react-router";

export default function Root() {
  return (
    <html lang="en">
      <head>
        <meta charSet="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <Meta />
        <Links />
      </head>
      <body>
        <Outlet />
        <ScrollRestoration />
        <Scripts />
      </body>
    </html>
  );
}
```

Key components:
- `<Meta />` - Renders meta tags from route `meta` exports
- `<Links />` - Renders link tags from route `links` exports
- `<Outlet />` - Renders the matched child route
- `<Scripts />` - Renders the client-side JavaScript
- `<ScrollRestoration />` - Restores scroll position on navigation

## Configuration (`react-router.config.ts`)

```ts
import type { Config } from "@react-router/dev/config";

export default {
  // Enable/disable server-side rendering
  ssr: true,
  
  // Pre-render specific routes at build time
  // prerender: ["/", "/about"],
} satisfies Config;
```

## Framework Mode vs Other Modes

React Router has three modes:

| Mode | Use Case |
|------|----------|
| **Framework** | Full-stack apps with SSR, loaders, actions, file-based routing |
| **Data** | SPAs with data loading but no SSR, uses `createBrowserRouter` |
| **Declarative** | Simple client-side routing with `<Routes>` and `<Route>` |

Framework mode is the most feature-rich, providing:
- Automatic code splitting per route
- Server-side rendering
- Data loading with `loader`/`clientLoader`
- Mutations with `action`/`clientAction`
- Type-safe route params and data
