---
description: Review staged git changes.
---

Review the staged changes (`git diff --cached`). Focus on:
- Bugs and logic errors
- Security issues
- Error handling gaps
- Test coverage for new, complex logic
- The staged changes should introduce a single logical change
- If code can be made simpler somewhere, propose an alternative, simpler implementation.

Avoid running external commands unless they are defined in an activated skill.

Return your review in a concise form. If no changes are required and no issues are detected, just
reply with "LGTM" and a single sentence summary of the changes.
