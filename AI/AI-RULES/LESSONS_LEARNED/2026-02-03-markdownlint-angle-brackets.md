# 2026-02-03-markdownlint-angle-brackets

## Scope
- Applies only to this repository.
- Do not copy these rules into consuming projects.

## Issue
Markdownlint failed because placeholder tokens like `<ref>` were written in plain
text and treated as inline HTML (MD033), breaking CI.

## Prevention
- Wrap placeholder commands in backticks or fenced code blocks.
- Avoid raw angle-bracket tokens in prose; use code formatting instead.
- Run markdownlint (or scan for `<...>` tokens) before pushing docs changes.
