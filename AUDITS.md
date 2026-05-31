# Public Audit Samples

Short examples of the kind of focused repo audit I can do before a narrow docs, onboarding, or frontend quality pass.

## 2026-05-31: AI Tools Directory Audit Samples

### Hyraze/collective-ai-tools

Repository: https://github.com/Hyraze/collective-ai-tools

What I checked:

- Dependency install with `pnpm install --frozen-lockfile`
- TypeScript with `pnpm run type-check`
- Tests with `pnpm run test -- --run`
- Lint with `pnpm run lint:check`
- Production build with `pnpm run build`

Result:

- Dependencies installed successfully from the existing lockfile.
- TypeScript check passed.
- Vitest passed: 5 test files, 27 tests.
- Lint failed with 60 errors and 205 warnings.
- Production build fails on Windows because `prebuild` uses the Unix-only command `cp README.md public/`.

High-value follow-up slices:

- Cross-platform build fix: replace `cp README.md public/` with a Node script or package such as `shx`.
- Small lint cleanup PR: focus only on `@ts-ignore` to `@ts-expect-error`, unused `node` callback parameters, and unescaped JSX text.
- One accessibility cleanup pass for click handlers on non-interactive elements.

Suggested fixed scope:

- USD 80-120 for a build/onboarding PR that makes the local setup work cross-platform and documents verified commands.
- USD 120-200 for a targeted lint cleanup PR covering one error family without broad refactors.

### ShaikhWarsi/free-ai-tools

Repository: https://github.com/ShaikhWarsi/free-ai-tools

What I checked:

- Frozen install in `website/` with `pnpm install --frozen-lockfile`
- Non-frozen install with `pnpm install --no-frozen-lockfile`
- Lint with `pnpm run lint`
- Production build with `pnpm run build`

Result:

- Frozen install fails because `website/pnpm-lock.yaml` is absent.
- Non-frozen install succeeds, but generates an untracked lockfile.
- Lint fails with 6 errors and 20 warnings.
- Production build fails because `@vercel/analytics/next` is imported in `src/app/layout.tsx` but `@vercel/analytics` is not listed in `website/package.json`.

High-value follow-up slices:

- Add the missing package and commit a lockfile, or remove the analytics import if not intended.
- Fix the conditional `useMemo` call in `src/app/categories/[slug]/page.tsx`.
- Fix the JSX quote escaping issues in `submit` and `terms` pages.
- Add a short `website/README.md` setup section that states the package manager and verified commands.

Suggested fixed scope:

- USD 80-150 for a build-ready onboarding cleanup PR.
- USD 120-200 for a build plus lint cleanup pass that keeps product behavior unchanged.

