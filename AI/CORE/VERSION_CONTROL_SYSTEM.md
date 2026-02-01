# VERSION_CONTROL_SYSTEM

Guidance for version control system usage (Git and others).

## Commit Messages
- Always reference the ticket or issue identifier in commit messages.
- Add optional context about what changed and why when it improves clarity.
- Mark breaking changes explicitly.

## Ignore File
- Maintain a VCS ignore file in the repository root (for Git: `.gitignore`).
- Keep the ignore list minimal and security-focused; add project-specific entries as needed.

### Minimal Must-Have Ignores
- `.env`, `.env.*` (except `.env.example`)
- `.envrc`
- `*.key`, `*.pem`, `*.p12`, `*.pfx`
- `*.jks`, `*.keystore`
- `.DS_Store`, `Thumbs.db`, `Desktop.ini`
- `*.swp`, `*.swo`, `*~`
- `.idea/`, `.vscode/`

## Safeguards
- Do not commit secrets; rotate and remove them from history if exposed.
- Review ignore rules when adding new tools to avoid accidental leaks.