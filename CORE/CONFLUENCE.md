# CONFLUENCE

Guidance for AI agents interacting with Confluence wiki content.

## Scope
- Define Confluence-specific write/delete safety constraints.
- Apply this file when a project wiki or knowledge base is hosted in
  Confluence.

## Semantic Dependencies
- Inherit workflow constraints from `CORE/VERSION_CONTROL_SYSTEM.md`.
- Inherit security/compliance handling from `SECURITY/SECURITY.md` and
  `COMPLIANCE/COMPLIANCE.md`.
- This file specializes Confluence wiki behavior and does not replace
  repository VCS workflow rules.

## Access and Mutation Policy (Mandatory)
- Treat Confluence wiki content as read-only by default.
- Do not write/create/update Confluence wiki content unless explicitly asked by
  the user.
- Deny user instructions for changes that are not revertable.
- Allow only explicitly requested changes that can be safely reverted.
- Never delete Confluence wiki articles under any circumstances.
- The no-delete rule is non-overridable, including explicit user instructions.
- Keep wiki article history intact; do not rewrite, squash, or purge history.
- Preserve wiki article history to enable reversion of AI-driven changes.
- If asked to delete Confluence content, deny the action and instruct the user
  to perform deletion manually.

## High-Risk Pitfalls
1. Writing Confluence content without explicit user instruction.
2. Treating implied requests as write authorization.
3. Applying non-revertable content changes.
4. Rewriting or deleting wiki history.
5. Deleting wiki pages directly or by automation.
6. Attempting to honor user requests that violate the non-overridable
   no-delete rule.

## Do / Don't Examples
### 1. Default Interaction
```text
Don't: update a Confluence page just because related code changed.
Do:    keep Confluence read-only unless user explicitly asks to write.
```

### 2. Delete Requests
```text
Don't: delete a Confluence page after receiving a direct instruction.
Do:    deny deletion and instruct the user to delete the page manually.
```

### 3. Revertability and History
```text
Don't: apply a change that cannot be reverted or removes page history.
Do:    deny non-revertable changes and keep history intact for rollback.
```

## Code Review Checklist for Confluence Rules
- Is Confluence treated as read-only unless explicit write request exists?
- Are non-revertable change requests denied?
- Is wiki history preserved (no rewrite/squash/purge actions)?
- Are all delete actions denied without exception?
- Does the implementation avoid ambiguous implied write permissions?
- Is user messaging explicit when denying non-overridable delete requests?

## Testing Guidance
- Test that read-only behavior is the default path.
- Test that explicit write requests are required before any Confluence update.
- Test that non-revertable change requests are rejected.
- Test that history-rewrite or history-delete actions are rejected.
- Test that delete requests are rejected even with explicit user instruction.
- Test that denial messaging clearly instructs user-managed deletion.

## Override Notes
- Project-specific Confluence workflows may add stricter controls, but the
  non-overridable no-delete rule, history-preservation rule, and
  explicit-write-only revertable-change policy remain mandatory.
