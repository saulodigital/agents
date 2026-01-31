# CLAUDE.md

Global preferences and conventions for Claude Code.

## Tech Stack

- **Language**: TypeScript/JavaScript
- **Frontend**: React, Next.js
- **Backend**: Node.js, Express
- **Package Manager**: npm/yarn/pnpm (follow project conventions)

## Code Style Priorities (highest to lowest)

### 1. Correct & Secure
- Validate inputs, handle edge cases, fail safely
- Security is never sacrificed for brevity

### 2. Clear & Readable
- Prefer the smallest correct solution
- Avoid speculative abstractions
- Optimize for readability per line, not line count at all costs

### 3. Type-Safe
- Strong typing where the language supports it
- Explicit interfaces at boundaries; inferred types internally
- Types used as design constraints, not decoration

### 4. Well-Documented (when it pays rent)
- Docstrings for public APIs, modules, and non-obvious logic
- Comments explain *why*, not *what*
- Inline explanations only when intent isn't self-evident

### 5. Functional (selectively)
- Immutability by default
- Pure functions where practical
- Composition over inheritance
- Not dogmatic—stateful or imperative code is fine when it simplifies the model

## Testing

- Unit tests for individual functions/components
- Integration tests for systems working together
- Focus on meaningful coverage, not metrics
- Test file naming: `*.test.ts` or `*.spec.ts` colocated with source
- Prioritize: happy path → error states → edge cases

## Golden Rules

- **Clarity over cleverness** — small pure functions, descriptive names, minimal surface area
- **Type-first** — validate at boundaries, strict TypeScript (no `any`, no implicit `any`)
- **Single source of truth** — types live with the domain, shared across layers
- **Accessible by default** — semantic HTML, keyboard support, color contrast
- **Security-in-depth** — validate inputs, server-side auth checks, least privilege
- **Incremental delivery** — small, test-covered, shippable slices

## Error Handling

- Handle errors at system boundaries only (user input, external APIs, network)
- Use try/catch for async operations; never swallow errors silently
- Return typed error responses from APIs; throw only for unexpected failures
- Use Error Boundaries in React for graceful UI recovery

## Naming Conventions

**Files & Directories**
- Directories: `kebab-case` (e.g., `auth-wizard`, `user-settings`)
- Files: `kebab-case` (e.g., `user-profile.tsx`, `api-client.ts`)
- Hooks: `kebab-case` with `use-` prefix (e.g., `use-auth.ts`)

**Code**
- Components: `PascalCase` (e.g., `UserProfile`, `RichTextEditor`)
- Types/Interfaces: `PascalCase` (e.g., `UserProfileProps`, `ApiResponse`)
- Variables: `camelCase` (e.g., `userData`, `isLoading`)
- Constants: `SCREAMING_SNAKE_CASE` (e.g., `API_BASE_URL`, `MAX_FILE_SIZE`)
- Functions: `camelCase` with descriptive verbs (e.g., `fetchUserData`, `validateInput`)
- Event handlers: `handle` prefix (e.g., `handleClick`, `handleSubmit`)
- Booleans: verb prefixes (e.g., `isLoading`, `hasError`, `canSubmit`)
- Custom hooks: `use` prefix (e.g., `useAuth`, `useForm`)
- State setters: `set` + PascalCase (e.g., `setUser`, `setLoading`)

**Database (Prisma)**
- Models: `PascalCase` (e.g., `User`, `Post`, `UserProfile`)
- Fields: `camelCase` (e.g., `firstName`, `createdAt`, `isActive`)

**API Routes (file-based)**
- Verb-based: `/api/<resource>/<verb>` (e.g., `/api/users/create`, `/api/tags/list`)
- Max 3 path segments after `/api`
- No IDs in URL paths — pass in body (POST) or query params (GET)

**Allowed Abbreviations**
- Standard: `err`, `req`, `res`, `props`, `ref`, `config`, `auth`
- Time: `min`, `max`, `prev`, `curr`

## Imports

- Order: React/Next → external packages → internal (`@/`) → relative
- Group with blank lines between sections

## Code Style

- 2 space indentation
- Single quotes (except to avoid escaping)
- Semicolons
- Strict equality (`===`) over loose (`==`)
- Trailing commas in multiline objects/arrays
- Space after keywords, commas, and around infix operators
- Curly braces for multi-line `if` statements
- `else` on same line as closing brace
- Always handle error parameters in callbacks

## TypeScript

- Enable `strict` mode in `tsconfig.json`
- Prefer `interface` for extendable object shapes; use `type` for unions/intersections/primitives
- Use type guards to narrow `unknown` safely (avoid `as` casts)
- Use utility types (`Partial`, `Pick`, `Omit`, `Record`, `Readonly`) to compose shapes
- No implicit `any`; annotate public function signatures
- Prefer inferred types internally; explicit types at module boundaries

## React

- Server Components by default; add `'use client'` only when needed (state, events, browser APIs)
- **Pages are Server Components by default** — extract interactive parts to `src/components` as Client Components
- Keep components small and composable; avoid prop drilling
- No untyped props; export prop interfaces
- Use stable keys in lists (never array index)
- Clean up side effects in `useEffect` (abort fetches, remove listeners, clear timers)
- Prefer `useCallback`/`useMemo` for expensive operations
- Use Suspense for streaming expensive sections

## UI & Styling

- **Tailwind CSS v4+** — always use Tailwind; never plain CSS files
- Define CSS variables in `globals.css` for theme colors/spacing; prefer tokens over hard-coded values
- Use `@layer components` for reusable utility compositions (e.g., `.card`, `.btn-primary`)
- Dark mode via CSS variables (not Tailwind `dark:` variant)
- Avoid inline `style={}`; use only for truly dynamic values (runtime calculations)
- Mobile-first responsive design
- Use `focus-visible` for accessible focus states
- Use `motion-safe:` for animations; respect `prefers-reduced-motion`

## General Preferences

- No unnecessary abstractions or over-engineering
- Direct solutions over clever ones
- Avoid backwards-compatibility hacks—delete unused code completely

## AI Assistant Behavior

- If requirements are clear, proceed directly — don't ask unnecessary questions
- Generate minimal diffs; don't refactor unrelated code
- If requirements conflict: Security > data integrity > UX > performance > DX
- When uncertain, implement the smallest secure version and note assumptions
