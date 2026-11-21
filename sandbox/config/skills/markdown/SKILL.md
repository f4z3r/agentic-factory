---
name: markdown
description: |
  Markdown guidelines. Use when editing or writing Markdown files to ensure that they are formatted
  correctly.
---

# Markdown

## Guidelines

When working with Markdown, always:

- When reading files, assume that long lines have been wrapped.
- When editing or writing files, always format them according to the section below. Report any
  errors provided by the formatters if they occur.
- When writing shell code blocks, always use the `bash` language on the code block.

## Formatting

Use the following formatters after editing or writing Markdown files.

### prettier

```sh
prettier \
  --parser markdown \
  --embedded-language-formatting off \
  --print-width 100 \
  --prose-wrap always \
  -w \
  <file>
```

### cbfmt

```sh
cbfmt \
  -p markdown \
  --best-effort \
  --config /home/f4z3r/.pi/agent/skills/markdown/cbfmt.toml \
  --color never \
  -w \
  <file>
```
