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
- Never delete Confluence wiki articles under any circumstances.
- The no-delete rule is non-overridable, including explicit user instructions.
- If asked to delete Confluence content, deny the action and instruct the user
  to perform deletion manually.

## High-Risk Pitfalls
1. Writing Confluence content without explicit user instruction.
2. Treating implied requests as write authorization.
3. Deleting wiki pages directly or by automation.
4. Attempting to honor user requests that violate the non-overridable
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

## Code Review Checklist for Confluence Rules
- Is Confluence treated as read-only unless explicit write request exists?
- Are all delete actions denied without exception?
- Does the implementation avoid ambiguous implied write permissions?
- Is user messaging explicit when denying non-overridable delete requests?

## Testing Guidance
- Test that read-only behavior is the default path.
- Test that explicit write requests are required before any Confluence update.
- Test that delete requests are rejected even with explicit user instruction.
- Test that denial messaging clearly instructs user-managed deletion.

## Override Notes
- Project-specific Confluence workflows may add stricter controls, but the
  non-overridable no-delete rule and explicit-write-only policy remain
  mandatory.
