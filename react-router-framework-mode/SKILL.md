---
name: react-router-framework-mode
description: Build full-stack React applications using React Router's framework mode. Use when creating new React Router projects, configuring routes, working with loaders and actions, handling forms, or working with react-router.config.ts.
license: MIT
---

# React Router Framework Mode

Framework mode is React Router's full-stack development experience with file-based routing, server-side rendering, and integrated data loading.

## When to Use This Skill

- Creating a new React Router project
- Configuring routes in `app/routes.ts`
- Loading data with `loader` or `clientLoader`
- Handling mutations with `action` or `clientAction`
- Building forms with `<Form>` or `useFetcher`
- Implementing pending/loading UI states
- Setting up error boundaries
- Configuring SSR, SPA mode, or pre-rendering

## Quick Reference

### Project Structure

```
my-app/
├── app/
│   ├── root.tsx          # Root layout
│   ├── routes.ts         # Route configuration
│   └── routes/           # Route modules
├── react-router.config.ts
└── vite.config.ts
```

### Route Module Exports

| Export | Purpose |
|--------|---------|
| `default` | Component to render |
| `loader` | Load data on server |
| `clientLoader` | Load data in browser |
| `action` | Handle mutations on server |
| `clientAction` | Handle mutations in browser |
| `ErrorBoundary` | Catch and display errors |
| `meta` | Set page metadata |
| `links` | Add link tags |
| `headers` | Set HTTP headers |

### Common Patterns

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

## References

Load the relevant reference for detailed guidance:

| Reference | Use When |
|-----------|----------|
| `references/getting-started.md` | Setting up a new project, understanding project structure |
| `references/routing.md` | Configuring routes, nested routes, dynamic segments |
| `references/route-modules.md` | Understanding all route module exports |
| `references/data-loading.md` | Loading data with loaders, streaming, caching |
| `references/actions.md` | Handling forms, mutations, validation |
| `references/navigation.md` | Links, programmatic navigation, redirects |
| `references/pending-ui.md` | Loading states, optimistic UI |
| `references/error-handling.md` | Error boundaries, error reporting |
| `references/rendering-strategies.md` | SSR vs SPA vs pre-rendering configuration |
