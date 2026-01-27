# Route Module Types

React Router generates route-specific types that provide type inference for URL params, loader data, action data, and more.

## Is Typegen Set Up?

Check for these indicators:

1. `.react-router/types/` directory exists (or appears after running `dev`)
2. `tsconfig.json` includes `.react-router/types/**/*`
3. Imports like `import type { Route } from "./+types/my-route"` resolve

**If not set up:** See https://reactrouter.com/how-to/route-module-type-safety

## Importing Types

Import the `Route` namespace from the `+types` directory relative to your route file:

```tsx
import type { Route } from "./+types/my-route";
```

## Available Types

| Type                             | Used For                                       |
| -------------------------------- | ---------------------------------------------- |
| `Route.LoaderArgs`               | Server `loader` function arguments             |
| `Route.ClientLoaderArgs`         | Client `clientLoader` function arguments       |
| `Route.ActionArgs`               | Server `action` function arguments             |
| `Route.ClientActionArgs`         | Client `clientAction` function arguments       |
| `Route.ComponentProps`           | Component props (loaderData, actionData, etc.) |
| `Route.ErrorBoundaryProps`       | `ErrorBoundary` component props                |
| `Route.HydrateFallbackProps`     | `HydrateFallback` component props              |
| `Route.MetaArgs`                 | `meta` function arguments                      |
| `Route.HeadersArgs`              | `headers` function arguments                   |
| `Route.MiddlewareFunction`       | Server middleware function type                |
| `Route.ClientMiddlewareFunction` | Client middleware function type                |

## What Gets Typed

Types are inferred from your route configuration:

- **URL params**: From dynamic segments (`:id`, `:slug`)
- **Loader data**: From `loader` return → `ComponentProps.loaderData`
- **Action data**: From `action` return → `ComponentProps.actionData`
- **Parent data**: Matches include typed data from parent routes

## Example

```tsx
import type { Route } from "./+types/products.$id";

// params.id typed as string (from :id segment)
export async function loader({ params }: Route.LoaderArgs) {
  const product = await db.products.find(params.id);
  return { product };
}

// loaderData.product typed from loader return
export default function Product({ loaderData }: Route.ComponentProps) {
  return <h1>{loaderData.product.name}</h1>;
}
```

## Type-Safe URLs with `href`

The `href` utility generates type-safe URL paths:

```tsx
import { href } from "react-router";

// Type-safe path generation with params
const aboutUrl = href("/:lang?/about", { lang: "en" });
// → "/en/about"

// Use with Link
<Link to={href("/products/:id", { id: "abc123" })} />;
```

## See Also

- [Route Module Type Safety](https://reactrouter.com/how-to/route-module-type-safety)
