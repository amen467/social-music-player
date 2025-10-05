<!-- .github/copilot-instructions.md - guidance for AI coding agents -->
# Copilot instructions — social-music-player

Short, actionable guidance to help an AI coding agent work productively in this repository.

- Project type: Next.js (App Router) + TypeScript + Tailwind + Supabase + NextAuth.
- Key files:
  - `src/app/layout.tsx` — global layout, fonts, and `globals.css` import.
  - `src/app/page.tsx` — home page (edit point for UI changes).
  - `src/lib/supabaseClient.ts` — Supabase client creation; uses env vars `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`.
  - `src/app/api/auth/[...nextauth]/route.ts` — NextAuth handler exported as `GET`/`POST`.
  - `package.json` — dev commands: `npm run dev` (uses `next dev --turbopack`), `npm run build`, `npm start`, `npm run lint`.

How the project is structured (big picture):

- Uses the Next.js App Router under `src/app`. Components placed here are server components by default (no `use client` directive).
- Global styling is in `src/app/globals.css` and the app uses Tailwind utility classes throughout.
- Authentication is handled via NextAuth in the API route at `src/app/api/auth/[...nextauth]/route.ts` — keep the exported handler signature (`export { handler as GET, handler as POST }`).
- Supabase is initialized once in `src/lib/supabaseClient.ts` and intended to be imported where needed. Do not hardcode keys; prefer environment variables.

Developer workflows and useful commands:

- Local dev: `npm run dev` (starts Next with turbopack). If turbopack causes issues, remove `--turbopack` in the script temporarily to fall back to default.
- Build: `npm run build` then `npm start` for production preview.
- Lint: `npm run lint` (project uses ESLint + `eslint-config-next`).
- Environment: Set `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` for Supabase integration. Client-side code reads `NEXT_PUBLIC_` vars — do not add secrets to source.

Project-specific patterns and conventions:

- App dir server components: Files in `src/app` are server components unless they start with `"use client"`. Add `"use client"` at the top only when you need client-only features (state/hooks, browser-only APIs).
- Path alias: `@/*` maps to `./src/*` (see `tsconfig.json`). Prefer imports like `@/lib/supabaseClient` for clarity.
- Keep Tailwind utility classes in JSX — this project uses utility-first styling rather than component CSS-in-JS.
- Prefer functional React components and TypeScript strict typing — `tsconfig.json` enables `strict`.

Integration points and external dependencies:

- Supabase: `@supabase/supabase-js` — initialized at `src/lib/supabaseClient.ts`.
- NextAuth: `next-auth` — provider configuration lives in `src/app/api/auth/[...nextauth]/route.ts` (the file shows the handler shape; look for provider config elsewhere if added).
- Hosting: README recommends Vercel; Next features (e.g., edge runtime) may be used later — be conservative with changes that affect serverless/runtime behavior.

Editing guidance for agents (concise rules):

1. Preserve server vs client boundary: don't add browser-only APIs to server components. If adding hooks or event handlers, mark the file `"use client"` and keep the component small.
2. Do not commit secrets. Use `process.env` and document required env vars in PR descriptions.
3. When changing auth routes, preserve the exported handler exports (`GET`, `POST`).
4. Run `npm run lint` and `npm run build` locally when changing TypeScript or config to catch errors early.
5. Use the path alias `@/` for imports from `src/`.

Where to look for examples in the repo:

- `src/lib/supabaseClient.ts` — canonical supabase setup.
- `src/app/layout.tsx` — global layout and font loading.
- `src/app/page.tsx` — typical component structure and Tailwind usage.

If you need clarification or more context (missing files, envs, or provider configs), ask the human maintainer. After making edits, run lint and build and include the failing output if any errors occur.
