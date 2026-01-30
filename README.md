# AI Rules (Baseline)

This repository contains shared, versioned AI guidance intended to be vendored
into other projects (e.g., via git subtree). It is the single source of truth
for baseline agent rules across repositories.

## Structure
- `AI/BASELINE.md` - global rules and expectations
- `AI/REVIEW.md` - review checklist and focus areas
- `AI/SECURITY.md` - security and privacy expectations
- `AI/JAVA.md` - Java-specific guidance (Effective Java-style rules)
- `AI/TESTING.md` - test expectations and verification notes

## Usage (git subtree)
Example:
```bash
git subtree add --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash
```

Update later:
```bash
git subtree pull --prefix docs/ai https://github.com/fabian-barney/ai-rules.git main --squash
```

## Versioning
Tag releases (e.g., `v1.0.0`) and pin subtree updates to a tag when stability
is required.
