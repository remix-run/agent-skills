TODO: needs to be reviewed

# Pending UI and Optimistic Updates

React Router provides hooks to show loading states and optimistic updates during navigation and form submissions.

## useNavigation

Track global navigation state:

```tsx
import { useNavigation } from "react-router";

export default function Root() {
  const navigation = useNavigation();
  const isNavigating = Boolean(navigation.location);

  return (
    <html>
      <body className={isNavigating ? "loading" : ""}>
        {isNavigating && <GlobalSpinner />}
        <Outlet />
      </body>
    </html>
  );
}
```

### Navigation States

- `navigation.state` - `"idle"`, `"loading"`, or `"submitting"`
- `navigation.location` - The location being navigated to (if any)
- `navigation.formData` - Form data being submitted (if any)
- `navigation.formMethod` - HTTP method of the submission

```tsx
const navigation = useNavigation();

// Check if navigating
const isNavigating = navigation.state !== "idle";

// Check if submitting a form
const isSubmitting = navigation.state === "submitting";

// Check if loading after submission
const isLoading = navigation.state === "loading";
```

## NavLink Pending State

Show pending state on the specific link being clicked:

```tsx
import { NavLink } from "react-router";

function Nav() {
  return (
    <NavLink to="/dashboard">
      {({ isPending }) => <span>Dashboard {isPending && <Spinner />}</span>}
    </NavLink>
  );
}
```

Or use className:

```tsx
<NavLink
  to="/dashboard"
  className={({ isPending }) => (isPending ? "pending" : "")}
>
  Dashboard
</NavLink>
```

## useFetcher for Local State

Each fetcher tracks its own state independently:

```tsx
import { useFetcher } from "react-router";

function LikeButton({ postId, liked }) {
  const fetcher = useFetcher();

  // Show pending state while submitting
  const isPending = fetcher.state !== "idle";

  return (
    <fetcher.Form method="post" action={`/posts/${postId}/like`}>
      <button disabled={isPending}>
        {isPending ? "..." : liked ? "Unlike" : "Like"}
      </button>
    </fetcher.Form>
  );
}
```

### Fetcher States

- `fetcher.state` - `"idle"`, `"submitting"`, or `"loading"`
- `fetcher.data` - Data returned from the action/loader
- `fetcher.formData` - Form data being submitted

## Optimistic UI

Show the expected result before the server responds:

```tsx
import { useFetcher } from "react-router";

function LikeButton({ postId, initialLiked }) {
  const fetcher = useFetcher();

  // Optimistically determine liked state
  const liked = fetcher.formData
    ? fetcher.formData.get("liked") === "true"
    : initialLiked;

  return (
    <fetcher.Form method="post" action={`/posts/${postId}/like`}>
      <input type="hidden" name="liked" value={String(!liked)} />
      <button>{liked ? "❤️" : "🤍"}</button>
    </fetcher.Form>
  );
}
```

## Optimistic UI with useNavigation

For form submissions that navigate:

```tsx
import { Form, useNavigation } from "react-router";

function NewProjectForm() {
  const navigation = useNavigation();

  // Get optimistic value from submission
  const optimisticTitle = navigation.formData?.get("title");
  const isSubmitting = navigation.state === "submitting";

  return (
    <Form method="post">
      <input type="text" name="title" />
      <button disabled={isSubmitting}>
        {isSubmitting ? "Creating..." : "Create"}
      </button>

      {optimisticTitle && <p>Creating "{optimisticTitle}"...</p>}
    </Form>
  );
}
```

## Skeleton Loading

Show skeletons while data loads:

```tsx
import { Suspense } from "react";
import { Await } from "react-router";

export async function loader() {
  return {
    fastData: await getFastData(),
    slowData: getSlowData(), // Don't await
  };
}

export default function Page({ loaderData }: Route.ComponentProps) {
  return (
    <div>
      <h1>{loaderData.fastData.title}</h1>

      <Suspense fallback={<CommentsSkeleton />}>
        <Await resolve={loaderData.slowData}>
          {(data) => <Comments data={data} />}
        </Await>
      </Suspense>
    </div>
  );
}
```

## Disabling During Submission

Prevent double submissions:

```tsx
function ContactForm() {
  const navigation = useNavigation();
  const isSubmitting = navigation.state === "submitting";

  return (
    <Form method="post">
      <input type="text" name="message" disabled={isSubmitting} />
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? "Sending..." : "Send"}
      </button>
    </Form>
  );
}
```

## Busy Indicators with CSS

Use CSS for simple loading indicators:

```css
.loading {
  opacity: 0.5;
  pointer-events: none;
}
```

```tsx
<body className={isNavigating ? "loading" : ""}>
```
