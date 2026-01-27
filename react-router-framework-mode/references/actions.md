# Actions and Form Handling

Actions handle data mutations (create, update, delete). After an action completes, all loaders on the page automatically revalidate.

## Server Action

Runs on the server:

```tsx
import { redirect, data } from "react-router";

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const title = formData.get("title");
  
  await db.createProject({ title });
  
  return redirect("/projects");
}
```

## Client Action

Runs in the browser:

```tsx
export async function clientAction({ request, serverAction }: Route.ClientActionArgs) {
  const formData = await request.formData();
  
  // Can call server action
  const result = await serverAction();
  
  // Or handle entirely on client
  await clientApi.update(formData);
  
  return result;
}
```

## Form Component

The `<Form>` component submits to actions:

```tsx
import { Form } from "react-router";

export default function NewProject() {
  return (
    <Form method="post">
      <input type="text" name="title" required />
      <button type="submit">Create</button>
    </Form>
  );
}
```

### Form Props

- `method` - HTTP method: `"get"`, `"post"`, `"put"`, `"delete"`, `"patch"`
- `action` - URL to submit to (defaults to current route)
- `navigate` - Set to `false` to prevent navigation after submission

```tsx
<Form method="post" action="/projects/new">
  {/* ... */}
</Form>
```

## useSubmit

Submit forms programmatically:

```tsx
import { useSubmit } from "react-router";

function AutoSave({ data }) {
  const submit = useSubmit();
  
  useEffect(() => {
    const timeout = setTimeout(() => {
      submit(data, { method: "post" });
    }, 1000);
    
    return () => clearTimeout(timeout);
  }, [data, submit]);
  
  return null;
}
```

## useFetcher

For mutations that don't navigate:

```tsx
import { useFetcher } from "react-router";

function LikeButton({ postId }) {
  const fetcher = useFetcher();
  const isLiking = fetcher.state === "submitting";
  
  return (
    <fetcher.Form method="post" action={`/posts/${postId}/like`}>
      <button disabled={isLiking}>
        {isLiking ? "Liking..." : "Like"}
      </button>
    </fetcher.Form>
  );
}
```

### Fetcher Properties

- `fetcher.state` - `"idle"`, `"submitting"`, or `"loading"`
- `fetcher.data` - Data returned from the action
- `fetcher.Form` - Form component scoped to this fetcher
- `fetcher.submit()` - Submit programmatically

## Returning Data from Actions

Return data for the UI to use:

```tsx
import { data } from "react-router";

export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const email = formData.get("email");
  
  const errors: Record<string, string> = {};
  
  if (!email?.toString().includes("@")) {
    errors.email = "Invalid email address";
  }
  
  if (Object.keys(errors).length > 0) {
    return data({ errors }, { status: 400 });
  }
  
  await createUser({ email });
  return redirect("/dashboard");
}
```

Access action data with `fetcher.data`:

```tsx
function SignupForm() {
  const fetcher = useFetcher();
  const errors = fetcher.data?.errors;
  
  return (
    <fetcher.Form method="post">
      <input type="email" name="email" />
      {errors?.email && <span>{errors.email}</span>}
      <button type="submit">Sign Up</button>
    </fetcher.Form>
  );
}
```

## Form Validation Pattern

```tsx
export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const email = String(formData.get("email"));
  const password = String(formData.get("password"));

  const errors: Record<string, string> = {};

  if (!email.includes("@")) {
    errors.email = "Invalid email address";
  }

  if (password.length < 12) {
    errors.password = "Password must be at least 12 characters";
  }

  if (Object.keys(errors).length > 0) {
    return data({ errors }, { status: 400 });
  }

  // Validation passed
  await createAccount({ email, password });
  return redirect("/dashboard");
}
```

## Multiple Actions per Route

Use a hidden input or button name to distinguish intents:

```tsx
export async function action({ request }: Route.ActionArgs) {
  const formData = await request.formData();
  const intent = formData.get("intent");
  
  switch (intent) {
    case "update":
      return updateProject(formData);
    case "delete":
      return deleteProject(formData);
    default:
      throw new Response("Invalid intent", { status: 400 });
  }
}

// In component
<Form method="post">
  <button name="intent" value="update">Save</button>
  <button name="intent" value="delete">Delete</button>
</Form>
```
