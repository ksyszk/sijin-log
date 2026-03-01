# Agent Guidelines for morethan-log

This is a Next.js blog using Notion as CMS. Built with TypeScript, React, Emotion, and React Query.

## Build Commands

```bash
# Install dependencies
npm install

# Development server (port 3000)
npm run dev

# Production build
npm run build

# Start production server
npm start

# Lint (ESLint + Next.js rules)
npm run lint

# Format (Prettier - check only)
npx prettier --check .

# Format (Prettier - write changes)
npx prettier --write .
```

**Note:** This project has no test framework configured. Tests should be added if needed.

---

## Code Style Guidelines

### Formatting (Prettier)

- **Indent:** 2 spaces
- **Semicolons:** No (omit)
- **Quotes:** Double quotes
- **Trailing commas:** ES5 style
- **Bracket spacing:** true

### TypeScript

- **Strict mode:** Enabled
- **JSX:** Preserved (`jsx: "preserve"`)
- **JSX Import source:** `@emotion/react`
- Use explicit types for props and function return types.

### Components

- Use `React.FC<Props>` for functional components
- Define `Props` type explicitly for component props
- Example:

```tsx
type Props = {
  data: TPost
}

const PostCard: React.FC<Props> = ({ data }) => {
  // ...
}
```

### Imports

- Use path aliases for `src/` imports: `import { queryKey } from "src/constants/queryKey"`
- Use relative imports for same-level components: `import Tag from "../../../components/Tag"`
- Order: React imports → external libs → internal modules → components

```tsx
import Link from "next/link"
import { CONFIG } from "site.config"
import { formatDate } from "src/libs/utils"
import Tag from "../../../components/Tag"
import styled from "@emotion/styled"
```

### Naming Conventions

- **Files:** camelCase (e.g., `usePostQuery.ts`, `PostCard.tsx`)
- **Components:** PascalCase (e.g., `PostCard`, `RootLayout`)
- **Hooks:** camelCase with `use` prefix (e.g., `useScheme`, `usePostQuery`)
- **Types:** PascalCase with prefix (e.g., `TPost`, `Props`)

### Styling (Emotion)

- Use `@emotion/styled` for component styles
- Define styled components at the bottom of the file
- Use theme values via `${({ theme }) => theme.colors.gray10}` or `${({ theme }) => theme.scheme}`
- Use media queries with `@media (min-width: 768px)` pattern

```tsx
const StyledWrapper = styled(Link)`
  background-color: ${({ theme }) =>
    theme.scheme === "light" ? "white" : theme.colors.gray4};

  @media (min-width: 768px) {
    margin-bottom: 2rem;
  }
`
```

### Error Handling

- API routes: Use try/catch with appropriate HTTP status codes
- Return error responses: `res.status(401).json({ message: "Invalid token" })`
- Catch blocks: `res.status(500).send("Error message")`

```tsx
export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  try {
    // ...
    res.json({ revalidated: true })
  } catch (err) {
    return res.status(500).send("Error revalidating")
  }
}
```

### React Query

- Use `@tanstack/react-query` for data fetching
- Define query keys in `src/constants/queryKey.ts`
- Query key pattern: `queryKey.post(\`${slug}\`)`

### Next.js Conventions

- Pages in `src/pages/` directory
- API routes in `src/pages/api/`
- Components in `src/components/` and `src/routes/`
- Use `next/link` for internal navigation
- Use `next/image` for images with `fill` or explicit dimensions
- Access route params via `useRouter()`: `const { slug } = router.query`

### ESLint

- Extends `next/core-web-vitals`
- Run `npm run lint` before committing

---

## Project Structure

```
src/
├── apis/              # API client functions
├── assets/            # Fonts, static assets
├── components/        # Reusable UI components
├── constants/         # Query keys, app constants
├── hooks/             # Custom React hooks
├── layouts/           # Layout components
├── libs/              # Utility functions
├── routes/            # Page route components
├── styles/            # Theme, colors, media queries
├── types/             # TypeScript type definitions
└── pages/             # Next.js pages and API routes
```

---

## Configuration

- Site configuration: `site.config.js`
- Environment variables required: `NOTION_PAGE_ID`
- Optional: `NEXT_PUBLIC_GOOGLE_MEASUREMENT_ID`, `NEXT_PUBLIC_UTTERANCES_REPO`, etc.
