# VERSION_CONTROL_SYSTEM

Guidance for version control system usage (Git and others).

## Commit Messages

### Required
- **Always reference ticket/issue identifier** in commit messages (e.g., `[PROJ-123]`, `#456`, `fixes #789`).
- Use clear, descriptive commit messages that explain what and why, not just how.

### Encouraged
- Additional context about the change (motivation, trade-offs, side effects).
- Breaking changes should be clearly marked.
- Co-authors when pair programming (use `Co-authored-by: Name <email>`).

### Format Example
```
[PROJ-123] Add user authentication feature

Implements OAuth2 login flow for external identity providers.
This allows users to authenticate using their corporate credentials.

- Added OAuth2 client configuration
- Implemented token validation
- Added user session management
```

## VCS Ignore File

### Maintenance
- **Maintain a VCS ignore file** (e.g., `.gitignore` for Git) in the repository root.
- Keep the ignore file up-to-date as the project evolves.
- Review and update ignore patterns when adding new tools or frameworks.

### Minimal Must-Have Ignore Patterns

For security and quality reasons, always ignore:

#### Secrets and Credentials
- `.env` - Environment variables with secrets
- `.env.*` - Environment-specific files (except `.env.example`)
- `*.key`, `*.pem`, `*.p12`, `*.pfx` - Private keys and certificates
- `*.keystore`, `*.jks` - Java keystores
- `**/secrets/`, `**/credentials/` - Secret directories
- `config/**/secret*` - Secret configuration files

#### IDE and Editor Files
- `.idea/` - IntelliJ IDEA
- `.vscode/` - Visual Studio Code (except shared settings)
- `*.swp`, `*.swo`, `*~` - Vim swap files
- `.DS_Store` - macOS Finder
- `Thumbs.db` - Windows thumbnails

#### Build Artifacts and Dependencies
- `target/`, `build/`, `dist/`, `out/` - Build output directories
- `node_modules/`, `bower_components/` - JavaScript dependencies
- `vendor/` - Vendored dependencies (unless explicitly vendored)
- `*.class`, `*.jar`, `*.war`, `*.ear` - Java compiled artifacts
- `*.pyc`, `__pycache__/`, `.pytest_cache/` - Python compiled files
- `.gradle/`, `.mvn/` - Build tool caches

#### Logs and Temporary Files
- `*.log` - Log files
- `logs/` - Log directories
- `*.tmp`, `*.temp` - Temporary files
- `*.cache` - Cache files

#### OS and System Files
- `.DS_Store`, `.AppleDouble`, `.LSOverride` - macOS
- `Thumbs.db`, `Desktop.ini` - Windows
- `*~` - Linux backup files

#### Package Manager Lock Files (Context-Dependent)
- Consider project policy on whether to commit lock files
- `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml` (commit for applications, ignore for libraries)
- `Gemfile.lock`, `composer.lock` (typically commit)
- `poetry.lock`, `Pipfile.lock` (typically commit)

## Additional Best Practices

### Branching
- Use descriptive branch names that reference the issue (e.g., `feature/PROJ-123-user-auth`).
- Delete branches after merging to keep the repository clean.
- Protect main/master branches from direct commits.

### Code Review
- Use pull/merge requests for all changes to main branches.
- Reference related issues in PR descriptions.
- Keep PRs focused and reviewable (avoid large, mixed-purpose PRs).

### History Hygiene
- Avoid force-pushing to shared branches unless coordinated with the team.
- Squash commits when merging if the project uses a clean history model.
- Write commits as logical units of work.

### Sensitive Data
- Never commit secrets, credentials, or sensitive data.
- If secrets are accidentally committed, rotate them immediately and use tools like `git-filter-repo` or
  BFG Repo-Cleaner to remove them from history.
- Use secret scanning tools in CI/CD pipelines.

### Repository Metadata
- Maintain a comprehensive README.md with setup instructions.
- Keep a CHANGELOG.md for tracking notable changes.
- Use .gitattributes for line ending normalization and merge strategies.

### Submodules and Subtrees
- Document submodule/subtree usage in the README.
- Pin submodules to specific commits, not branches.
- Update subtrees intentionally with clear commit messages.
