# ADR-0001: Monorepo vs Multi-Repo

**Status:** Accepted

**Context:** QuickHire is a full-stack platform with a NestJS backend, Next.js frontend, shared validation schemas, and supporting packages. We need to decide how to organize the codebase to maximize developer experience, code sharing, and CI efficiency.

## Options Considered

### Option 1: Monorepo (Turborepo)

All code lives in a single repository with a build system (Turborepo) managing dependency graphs and caching.

**Pros:**
- Shared types, validation schemas (Zod), and utilities live in packages consumed by both frontend and backend — no need to publish to npm
- Atomic commits across apps/packages — a schema change in the shared package and its consumers can be in one PR
- Cross-cutting refactors (e.g., renaming a shared type) are safe: tools like TypeScript project references + Turborepo catch all usages
- Single CI pipeline with Turborepo's caching and dependency graph — only rebuild what changed
- Standardized tooling (ESLint, Prettier, tsconfig) enforced at root level
- Simplified onboarding: one `git clone`, one `npm install`, one `npm run dev`

**Cons:**
- Git history grows large (mono-repo scale)
- CI pipeline can become a bottleneck without proper caching (Turborepo solves this)
- Requires team discipline around ownership boundaries and code review scoping
- Harder to enforce per-service deployment permissions (mitigated by CI path filters)

### Option 2: Multi-Repo

Each deployable unit (backend API, frontend, shared libs) lives in its own repository.

**Pros:**
- Independent CI/CD per service — deploy backend without rebuilding frontend
- Clean ownership boundaries — each team owns their repo
- Smaller `git clone` per service — faster initial checkout for specific work
- Per-repo access control in GitHub (not relevant for a solo/pair project)

**Cons:**
- Shared types must be published to npm (internal registry) — adds versioning overhead and publish steps
- Cross-repo changes require synchronized PRs — a breaking schema change becomes a coordination problem
- No atomic commits across app boundaries — rollbacks are harder
- Higher setup overhead: multiple repos, CI configs, package publishing
- Developers context-switch between repos for even simple cross-cutting changes

## Decision

Adopt a **monorepo managed by Turborepo**.

## Rationale

1. **Shared schemas are core to the architecture.** Zod schemas define the contract between frontend and backend. In a multi-repo setup, every schema change would require a publish + version bump + update across repos. In a monorepo, it's a single PR with the change and all consumers updated atomically.

2. **Scale of the project.** QuickHire has a small, focused set of applications (NestJS API, Next.js frontend) and a handful of internal packages (shared schemas, utilities, configs). This is the sweet spot for a monorepo — the coordination overhead of multi-repo outweighs its benefits at this scale.

3. **Single-developer velocity.** As a solo/portfolio project, the ability to make cross-cutting changes in one PR without publishing packages is a significant productivity multiplier.

4. **Turborepo mitigates monorepo pain points.** Cached builds, dependency graph awareness, and parallel task execution keep CI fast. Path-based CI triggers prevent unnecessary pipeline runs.

## Consequences

- All code lives under a single GitHub repository with the Turborepo convention: `apps/` for deployable applications, `packages/` for shared libraries
- CI must use Turborepo's `--filter` and path-based triggers to avoid running all tasks on every PR
- Git history will grow over time — standard `.gitignore` and `.gitattributes` hygiene applies
- Migration to multi-repo is possible later if team scales or ownership boundaries demand it, but carries a one-time cost
