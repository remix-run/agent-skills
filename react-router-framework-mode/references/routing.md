# Routing in React Router Framework Mode

Routes are configured in `app/routes.ts` using the route helpers.

## Basic Route Configuration

```ts
import { type RouteConfig, route } from "@react-router/dev/routes";

export default [
  route("about", "./routes/about.tsx"),
  route("products/:id", "./routes/product.tsx"),
] satisfies RouteConfig;
```

## Nested Routes

Use the third argument to nest routes:

```ts
import { type RouteConfig, route, index } from "@react-router/dev/routes";

export default [
  route("dashboard", "./routes/dashboard.tsx", [
    index("./routes/dashboard-home.tsx"),
    route("settings", "./routes/dashboard-settings.tsx"),
  ]),
] satisfies RouteConfig;
```

The parent renders an `<Outlet />` for child routes:

```tsx
// app/routes/dashboard.tsx
import { Outlet } from "react-router";

export default function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <Outlet />
    </div>
  );
}
```

## Layout Routes

Routes without a path that only provide shared UI:

```ts
import { type RouteConfig, route, layout } from "@react-router/dev/routes";

export default [
  layout("./routes/marketing-layout.tsx", [
    route("about", "./routes/about.tsx"),
    route("pricing", "./routes/pricing.tsx"),
  ]),
] satisfies RouteConfig;
```

## Index Routes

Default child route when parent path matches exactly:

```ts
import { type RouteConfig, route, index } from "@react-router/dev/routes";

export default [
  route("dashboard", "./routes/dashboard.tsx", [
    index("./routes/dashboard-home.tsx"), // matches /dashboard
    route("settings", "./routes/settings.tsx"), // matches /dashboard/settings
  ]),
] satisfies RouteConfig;
```

## Route Prefixes

Group routes under a path without a layout:

```ts
import { type RouteConfig, route, prefix } from "@react-router/dev/routes";

export default [
  prefix("api", [
    route("users", "./routes/api/users.tsx"),
    route("posts", "./routes/api/posts.tsx"),
  ]),
] satisfies RouteConfig;
```

## Dynamic Segments

Capture URL parameters with `:param`:

```ts
route("products/:productId", "./routes/product.tsx")
route("users/:userId/posts/:postId", "./routes/post.tsx")
```

Access in loaders and components:

```tsx
export async function loader({ params }: Route.LoaderArgs) {
  const product = await db.getProduct(params.productId);
  return product;
}

export default function Product({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.name}</h1>;
}
```

## Optional Segments

Make segments optional with `?`:

```ts
route("products/:category?", "./routes/products.tsx")
```

Matches both `/products` and `/products/electronics`.

## Splat Routes (Catch-All)

Match any remaining path with `*`:

```ts
route("files/*", "./routes/files.tsx")
```

Access the matched path via `params["*"]`:

```tsx
export async function loader({ params }: Route.LoaderArgs) {
  const filePath = params["*"]; // e.g., "docs/intro.md"
  return getFile(filePath);
}
```

## File-Based Route Conventions

Optionally use `@react-router/fs-routes` for file-based routing:

```ts
// app/routes.ts
import { flatRoutes } from "@react-router/fs-routes";

export default flatRoutes();
```

File naming conventions:
- `_index.tsx` → Index route
- `about.tsx` → `/about`
- `products.$id.tsx` → `/products/:id`
- `products_.new.tsx` → `/products/new` (escapes nesting)
- `_auth.login.tsx` → `/login` (pathless layout)
- `$.tsx` → Splat route
