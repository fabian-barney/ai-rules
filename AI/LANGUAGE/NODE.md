# NODE

Guidance for Node.js projects.

## General
- Use an LTS Node version and document it (for example via `engines` or `.nvmrc`).
- Use a single package manager consistently and follow repository policy on lockfiles.

## VCS Ignore Additions
Add these when using Node tooling (if not already covered by the baseline ignore list):
- `node_modules/`
- `npm-debug.log*`, `yarn-debug.log*`, `yarn-error.log*`, `pnpm-debug.log*`
- `.npm/`, `.yarn/`, `.pnpm-store/`
- `.pnp.*` (Yarn Plug'n'Play)