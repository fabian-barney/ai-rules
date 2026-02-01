# VERSION_CONTROL_SYSTEM

Guidance for version control system usage (Git and others).

## Commit Messages
- Always reference the ticket or issue identifier in commit messages.
- Add optional context about what changed and why when it improves clarity.
- Mark breaking changes explicitly.

## Ignore File
- Maintain a VCS ignore file in the repository root (for Git: `.gitignore`).
- Keep the ignore list minimal but practical to prevent VCS pollution and protect sensitive files.
- Start from a baseline set, then add project- or tool-specific entries as needed.
- Remove or override entries if the repository intentionally versions those files.

### Minimal Must-Have Ignores
Secrets
- `.env`, `.env.*` (except `.env.example`)
- `.envrc`
- `*.key`, `*.pem`, `*.p12`, `*.pfx`
- `*.jks`, `*.keystore`

OS and editor noise
- `.DS_Store`, `Thumbs.db`, `Desktop.ini`
- `*.swp`, `*.swo`, `*~`
- `.idea/`, `.vscode/`

Build output, dependencies, caches, logs
- `target/`, `build/`, `dist/`, `out/`
- `node_modules/`
- `.gradle/`
- `__pycache__/`, `*.pyc`, `.pytest_cache/`
- `*.log`, `logs/`
- `tmp/`, `*.tmp`, `*.cache`

## Safeguards
- Do not commit secrets; rotate and remove them from history if exposed.
- Review ignore rules when adding new tools to avoid accidental leaks.
