---
name: norg
description: |
  Norg/Neorg guidelines. Use when editing or writing Norg files (with .norg extension) to ensure
  that they are formatted correctly.
---

# Norg/Neorg Skill

## Overview

This skill provides guidelines for working with Norg/Neorg files, a markup language designed for
note-taking and organization within Neovim. Below is a cheatsheet to help you understand and use the
Norg syntax effectively.

## Structure

### Headings

Headings in Norg are created using asterisks (`*`). The number of asterisks determines the heading
level.

```norg
* Heading 1
** Heading 2
*** Heading 3
**** Heading 4
```

Norg supports infinite heading levels, but Neorg caps out at six. Higher levels are treated as level
six headings.

### Reverse Headings

To return to a previous heading level, use `---` to go back one level or `===` to close all
headings.

```norg
* Heading 1
** Heading 2
   Part of heading 2.
   ---
  Part of heading 1.
```

### Lists

#### Unordered Lists

Unordered lists are created using hyphens (`-`).

```norg
- List item.
-- Nested list item
-- Also nested.
- Next list item.
```

#### Ordered Lists

Ordered lists are created using tildes (`~`).

```norg
~ List item.
~~ Nested list item.
~~ Also nested.
~ Next list item.
```

### Code Blocks

Code blocks are enclosed between `@code` and `@end`. You can specify a language for syntax
highlighting.

```norg
@code
This is literally an empty code block.
@end

@code lua
print("If you have the lua treesitter parser installed, this will be highlighted as lua.")
print("If not, run `:TSInstall lua`.")
@end
```

## Inline Markup

### Styling

Norg supports various inline styling options:

```norg
This is *bold*.
This is /italic/.
This is _underline_.
This is -strikethrough-.
This is a spoiler: !org-mode is a bozo!.
This is a `verbatim object, you can't put *markup* in here!`.
%This is a comment, and you never see it in your exported output.%
```

You can mix and match styles, but avoid putting markup inside a verbatim object.

### Mathematical Notation

Norg supports superscript, subscript, and inline math:

```norg
This is ^superscript^.
This is ,subscript,.
This is inline maths: $f(x) = supports \\LaTeX$.
```

For more complex expressions, use the "rigid" syntax:

```norg
$|\int_0^t d(\ln{S}) = ...|$
```

### Styling with a Pipe

Use the pipe (`|`) to allow arbitrary whitespace within markup:

```norg
/|  but this is fine!!   |/
```

## Links

### Links to URLs

Links to URLs are enclosed in curly braces:

```norg
{https://github.com/nvim-neorg/neorg}
{file:///home/vhyrro/top-secret.txt}
```

### Links to Objects

To link to an object, use its prefix and name:

```norg
{*** styling, but extended}
{*** styling,butextended}
```

### Links to Non-Norg Files

Links to non-Norg files can include line numbers:

```norg
{/ myfile.txt}
{/ myfile.txt:43}
```

### Links with Descriptions

Add a description to a link using square brackets:

```norg
{* some heading}[my custom description]
```

### Links to Anything

Use the `#` character to link to any object type:

```norg
{# anything with this title}
```

### Links Across Files

Prefix the link with `:filename:` to link across files:

```norg
{:dir/file:* Heading in that file}
{:$/index.norg:* Heading in that file}
{:$notes/index.norg:* Heading in that file}
```

### Anchors

Anchors allow you to reference links multiple times:

```norg
This is a link to [my site]{https://very.long.link.to.my.boring.site}.
Be sure to reference [my site] if you're interested!
```

## Tasks

Tasks in Norg are created by adding a task status to any item:

```norg
- ( ) Here's a classical TODO item.
* (x) Here's a heading with a done TODO state.
```

Supported task states:

```norg
( ) Undone -> not done yet
(x) Done -> done with that
(?) Needs further input
(!) Urgent -> high priority task
(+) Recurring task with children
(-) Pending -> currently in progress
(=) Task put on hold
(_) Task cancelled (put down)
```
