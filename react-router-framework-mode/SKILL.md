---
name: react-router-framework-mode
description: Build full-stack React applications using React Router's framework mode. Use when configuring routes, working with loaders and actions, handling forms, handling navigation, pending/optimistic UI, error boundaries, or working with react-router.config.ts or other react router conventions.
license: MIT
---

# React Router Framework Mode

Framework mode is React Router's full-stack development experience with file-based routing, server-side, client-side, and static rendering strategies, data loading and mutations, and type-safe route module API.

## When to Apply

- Configuring new routes (`app/routes.ts`)
- Loading data with `loader` or `clientLoader`
- Handling mutations with `action` or `clientAction`
- Navigating with `<Link>`, `<NavLink>`, `<Form>`, `redirect`, and `useNavigate`
- Implementing pending/loading UI states
- Configuring SSR, SPA mode, or pre-rendering (`react-router.config.ts`)

## References

Load the relevant reference for detailed guidance on the specific API/concept:

| Reference                            | Use When                                                        |
| ------------------------------------ | --------------------------------------------------------------- |
| `references/routing.md`              | Configuring routes, nested routes, dynamic segments             |
| `references/route-modules.md`        | Understanding all route module exports                          |
| `references/data-loading.md`         | Loading data with loaders, streaming, caching                   |
| `references/actions.md`              | Handling forms, mutations, validation                           |
| `references/navigation.md`           | Links, programmatic navigation, redirects                       |
| `references/pending-ui.md`           | Loading states, optimistic UI                                   |
| `references/error-handling.md`       | Error boundaries, error reporting                               |
| `references/rendering-strategies.md` | SSR vs SPA vs pre-rendering configuration                       |
| `references/middleware.md`           | Adding middleware to routes or using React Router's context API |
| `references/type-safety.md`          | Auto-generated route types, type imports, type safety           |

TODO: potential other references to consider:

- `references/special-files.md`
- `references/middleware.md`
- `references/revalidation.md`

## Quick Reference

### Project Structure

TODO: reference special files. Not sure if this is needed/should be at the top level.

```
my-app/
├── app/
│   ├── root.tsx          # Root layout
│   ├── routes.ts         # Route configuration
│   └── routes/           # Route modules
├── react-router.config.ts
└── vite.config.ts
```

### Configuring Routes

Load the `references/routing.md` reference for detailed guidance on configuring routes.

Here is a robust sample route config:

```ts filename="app/routes.ts"
import {
  type RouteConfig,
  route,
  index,
  layout,
  prefix,
} from "@react-router/dev/routes";

export default [
  index("./home.tsx"),
  route("about", "./about.tsx"),

  layout("./auth/layout.tsx", [
    route("login", "./auth/login.tsx"),
    route("register", "./auth/register.tsx"),
  ]),

  ...prefix("concerts", [
    index("./concerts/home.tsx"),
    route(":city", "./concerts/city.tsx"),
    route("trending", "./concerts/trending.tsx"),
  ]),
] satisfies RouteConfig;
```

If using `@react-router/fs-routes` see https://reactrouter.com/how-to/file-route-conventions

### Route Module Exports

Load the `references/route-modules.md` reference for detailed guidance on each export.

| Export             | Purpose                             | Runs On |
| ------------------ | ----------------------------------- | ------- |
| `default`          | Route component                     | Client  |
| `loader`           | Load data before render             | Server  |
| `clientLoader`     | Load data on client                 | Client  |
| `action`           | Handle form mutations               | Server  |
| `clientAction`     | Handle mutations on client          | Client  |
| `middleware`       | Pre/post request processing         | Server  |
| `clientMiddleware` | Client navigation processing        | Client  |
| `ErrorBoundary`    | Render on errors                    | Client  |
| `HydrateFallback`  | Show during client loader hydration | Client  |
| `headers`          | Set HTTP response headers           | Server  |
| `handle`           | Custom route metadata               | Both    |
| `links`            | Add `<link>` elements               | Both    |
| `meta`             | Add meta tags                       | Both    |
| `shouldRevalidate` | Control loader revalidation         | Client  |

### Common Patterns

TODO: will need to update this section to include a good summary of all the info. This might be a little too simplistic and not really helpful.

**Data loading:**

```tsx
export async function loader({ params }: Route.LoaderArgs) {
  return db.getProduct(params.id);
}

export default function Product({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.name}</h1>;
}
```

**Form submission:**

```tsx
import { Form, redirect } from "react-router";

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  await db.create({ title: formData.get("title") });
  return redirect("/items");
}

export default function New() {
  return (
    <Form method="post">
      <input name="title" />
      <button type="submit">Create</button>
    </Form>
  );
}
```

**Fetcher for non-navigating mutations:**

```tsx
import { useFetcher } from "react-router";

function LikeButton({ id }) {
  const fetcher = useFetcher();
  return (
    <fetcher.Form method="post" action={`/like/${id}`}>
      <button>{fetcher.state === "idle" ? "Like" : "..."}</button>
    </fetcher.Form>
  );
}
```

## Further Documentation

If anything related to React Router is not covered in these references, you can search the official documentation:

https://reactrouter.com/docs
