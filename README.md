# Agent Skills

[Agent Skills](https://agentskills.io) for building web applications.

These skills teach AI coding agents how to use frameworks and libraries correctly. Instead of relying on potentially outdated training data, agents can reference these skills for accurate, up-to-date patterns.

## Installation

```sh
npx skills add remix-run/agent-skills
```

This installs the skills into your project so your AI agent can reference them.

## Available Skills

### [`react-router-framework-mode`](./skills/react-router-framework-mode/)

Full-stack React apps using [React Router's](https://reactrouter.com) framework mode. Covers routing, loaders, actions, forms, sessions, middleware, error handling, and rendering strategies.

```sh
npx skills add remix-run/agent-skills --skill react-router-framework-mode
```

### [`react-router-data-mode`](./skills/react-router-data-mode/)

React apps using React Router's data mode with `createBrowserRouter` and `RouterProvider`. Covers route objects, loaders, actions, forms, pending UI, and optimistic updates without the Vite plugin.

```sh
npx skills add remix-run/agent-skills --skill react-router-data-mode
```

### [`react-router-declarative-mode`](./skills/react-router-declarative-mode/)

React apps using React Router's declarative mode with `BrowserRouter`. Covers JSX-based routing, navigation with Link/NavLink, and reading URL params without data loading features.

```sh
npx skills add remix-run/agent-skills --skill react-router-declarative-mode
```

## How It Works

Each skill contains:

- **`SKILL.md`** - Main entry point with quick patterns and a reference table
- **`references/`** - Detailed documentation for specific topics

When your AI agent encounters a relevant task, it loads the skill and references to generate accurate code using current APIs and best practices.

## License

MIT
