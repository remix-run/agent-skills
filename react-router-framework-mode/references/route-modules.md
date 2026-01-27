# Route Modules

A route module is a file that defines the behavior for a route. It can export various functions and components.

## Component Export (default)

The default export renders the route's UI:

```tsx
export default function MyRoute({ loaderData }: Route.ComponentProps) {
  return <div>{loaderData.message}</div>;
}
```

## loader

Provides data to the component on the server:

```tsx
export async function loader({ params, request }: Route.LoaderArgs) {
  const product = await db.getProduct(params.id);
  if (!product) {
    throw new Response("Not Found", { status: 404 });
  }
  return product;
}
```

## clientLoader

Runs in the browser. Can work alongside or replace `loader`:

```tsx
export async function clientLoader({ params, serverLoader }: Route.ClientLoaderArgs) {
  // Option 1: Call server loader and augment
  const serverData = await serverLoader();
  const clientData = await fetchFromClient();
  return { ...serverData, ...clientData };
}

// Force clientLoader to run on initial hydration
clientLoader.hydrate = true;
```

## action

Handles form submissions and mutations on the server:

```tsx
export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const title = formData.get("title");
  
  await db.createProject({ title });
  return redirect("/projects");
}
```

## clientAction

Handles mutations in the browser:

```tsx
export async function clientAction({ request, serverAction }: Route.ClientActionArgs) {
  const formData = await request.formData();
  
  // Can call serverAction or handle entirely on client
  const result = await serverAction();
  return result;
}
```

## ErrorBoundary

Catches errors from loader, action, or rendering:

```tsx
import { isRouteErrorResponse, useRouteError } from "react-router";

export function ErrorBoundary() {
  const error = useRouteError();

  if (isRouteErrorResponse(error)) {
    return (
      <div>
        <h1>{error.status} {error.statusText}</h1>
        <p>{error.data}</p>
      </div>
    );
  }

  return (
    <div>
      <h1>Error</h1>
      <p>{error instanceof Error ? error.message : "Unknown error"}</p>
    </div>
  );
}
```

## HydrateFallback

Shows while `clientLoader.hydrate` is running:

```tsx
export function HydrateFallback() {
  return <div>Loading...</div>;
}
```

## meta

Sets document metadata:

```tsx
export function meta({ data }: Route.MetaArgs) {
  return [
    { title: data.product.name },
    { name: "description", content: data.product.description },
  ];
}
```

## links

Adds `<link>` elements to the document head:

```tsx
export function links() {
  return [
    { rel: "stylesheet", href: "/styles/product.css" },
    { rel: "icon", href: "/favicon.png" },
  ];
}
```

## headers

Sets HTTP headers for the response:

```tsx
export function headers({ loaderHeaders }: Route.HeadersArgs) {
  return {
    "Cache-Control": loaderHeaders.get("Cache-Control") ?? "max-age=3600",
  };
}
```

## handle

Expose arbitrary data for use with `useMatches`:

```tsx
export const handle = {
  breadcrumb: "Products",
  scrollToTop: true,
};
```

Access in parent routes:

```tsx
import { useMatches } from "react-router";

function Breadcrumbs() {
  const matches = useMatches();
  const breadcrumbs = matches
    .filter(m => m.handle?.breadcrumb)
    .map(m => m.handle.breadcrumb);
  
  return <nav>{breadcrumbs.join(" > ")}</nav>;
}
```

## shouldRevalidate

Optimize when loaders rerun:

```tsx
export function shouldRevalidate({ currentUrl, nextUrl, formAction }) {
  // Don't revalidate if only the search params changed
  if (currentUrl.pathname === nextUrl.pathname) {
    return false;
  }
  return true;
}
```
