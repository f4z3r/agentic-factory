---
name: adr
description: |
  Architectural decision record (ADR) guidelines. Use when editing ADRs or when a code change
  warants an architectural change that needs to be documented.
---

# ADR Skill for MADR Standards

## Overview

This skill provides guidelines for writing Architectural Decision Records (ADRs) according to the
Markdown Architectural Decision Record (MADR) format.

## Structure

An ADR document should contain the following sections:

1. **Title**: A brief title that summarizes the decision.
2. **Status**: The status of the decision (e.g., proposed, accepted, deprecated, rejected).
3. **Context**: A short description of the circumstances surrounding the decision.
4. **Decision**: The actual decision made.
5. **Consequences**: What will happen as a result of this decision.
6. **Date**: The date when the decision is made.
7. **Participants**: The names of those involved in the decision.

## Example Template

    # Title of the Decision

    ## Status
    proposed / accepted / deprecated / rejected

    ## Context

    Briefly describe the context of the decision.

    ## Decision

    Clearly state the decision that was made.

    ## Consequences

    Describe the consequences of the decision.

    ## Date

    YYYY-MM-DD

    ## Participants

    - Name 1
    - Name 2

## Writing Guidelines

- Use clear and concise language.
- Focus on one decision per record.
- Keep it up to date as changes happen.

## Additional Resources

- [MADR Documentation](https://adr.github.io/madr)
