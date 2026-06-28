# Project Guidelines

## Code Style
- TypeScript + React with Next.js App Router.
- Use `@/...` path aliases as in `tsconfig.json`.
- Follow existing React and Tailwind patterns from `src/components/ui/*` and `src/components/chat/*`.
- Keep UI logic in client components (`use client`) and server/database logic in server actions (`use server`) under `src/actions`.

## Architecture
- Core app entry is `src/app/page.tsx` and `src/app/layout.tsx`.
- Authentication and session handling live in `src/lib/auth.ts`.
- Prisma client is centralized in `src/lib/prisma.ts`.
- AI provider logic is in `src/lib/provider.ts` with a fallback mock provider if `ANTHROPIC_API_KEY` is absent.
- The app uses a virtual file system and chat-driven component generation; relevant directories are `src/lib/contexts`, `src/lib/tools`, and `src/lib/file-system.ts`.

## Build and Test
- Install and initialize with `npm run setup`.
- Start dev server with `npm run dev`.
- Build with `npm run build`.
- Run tests with `npm run test`.
- Lint with `npm run lint`.
- Note: do not run `npm audit fix`; dependency versions are intentionally pinned as documented in `README.md`.

## Project Conventions
- Use server actions for backend operations: see `src/actions/index.ts`, `src/actions/create-project.ts`, and `src/actions/get-projects.ts`.
- Use `next/headers` and `next/navigation` for session/cookie management in server code.
- Authenticated user flow is based on cookies and JWTs; `src/lib/auth.ts` manages `auth-token` cookie creation, verification, and deletion.
- Anonymous work is preserved in `src/lib/anon-work-tracker.ts` and then persisted after sign-in via `src/hooks/use-auth.ts`.
- UI components should integrate with existing Radix/Tailwind helpers rather than introducing new styling systems.

## Integration Points
- AI model integration uses `@ai-sdk/anthropic` and `@ai-sdk/provider` in `src/lib/provider.ts`.
- The project supports a mock AI response path when no Anthropic API key is available.
- Database migrations and client generation rely on Prisma; schema lives under `prisma/schema.prisma`.

## Security
- Secrets: `ANTHROPIC_API_KEY` and `JWT_SECRET` are optional but expected in `.env` for production/authenticated flows.
- Session cookies are HTTP-only, same-site lax, and secure in production.
- Do not expose raw password hashes or JWT secrets in client-side code.
