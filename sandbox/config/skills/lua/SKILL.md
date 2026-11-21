---
name: lua
description: |
  Lua guidelines. Use when reading, editing, or writing Lua files to ensure they are formatted
  properly, and my personal coding preferences are followed.
---

# Lua

## Guidelines

When working with Lua source code, always:

- Unless you have reason to believe otherwise, assume the Lua code is written for LuaJIT.
- When editing or writing files, always format them according to the section below. Report any
  errors provided by the formatters if they occur.

### Use small functions

Put logical code into functions, avoid writing code outside of function declarations.

Do not use code such as the following, even if used in a script:

```lua
local io = require("io")
local os = require("os")

-- ...
local url = "https://example.com"
local req = request.new_from_uri(url)
req.headers:upsert(":method", "GET")
req.headers:upsert("accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8")
local resp_headers, stream = assert(req:go(TIMEOUT))
local response = stream:get_body_as_string()
if resp_headers:get(":status") ~= "200" then
  io.stderr:write("Error: Page fetch call failed -", response)
  os.exit(1)
end
local ph = assert(io.popen("html-to-markdown > /tmp/html-to-markdown.md", "w"))
ph:write(content)
ph:close()
local fh = assert(io.open("/tmp/html-to-markdown.md", "r"))
local out = fh:read("*a")
fh:close()
os.remove("/tmp/html-to-markdown.md")
print(out)
```

Instead, put the code into functions and call them:

```lua
local io = require("io")
local os = require("os")

-- ...

local function convert_html_to_md(content)
  local ph = assert(io.popen("html-to-markdown > /tmp/html-to-markdown.md", "w"))
  ph:write(content)
  ph:close()
  local fh = assert(io.open("/tmp/html-to-markdown.md", "r"))
  local out = fh:read("*a")
  fh:close()
  os.remove("/tmp/html-to-markdown.md")
  return out
end

local function main()
  local url = "https://example.com"
  local req = request.new_from_uri(url)
  req.headers:upsert(":method", "GET")
  req.headers:upsert("accept", "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8")
  local resp_headers, stream = assert(req:go(TIMEOUT))
  local response = stream:get_body_as_string()
  if resp_headers:get(":status") ~= "200" then
    io.stderr:write("Error: Page fetch call failed -", response)
    os.exit(1)
  end
  return convert_html_to_md(response)
end

main()
```

### Explicitely import everything

Do not call code such as `io.stderr:write("...")` without explicitely importing `io`:

```lua
local io = require("io")
```

### Use defensive programming

Assert return values where they can be null but you assume that they will contain a value. For
instance:

```lua
local fh = assert(io.open("myfile", "r"), "myfile could not be opened for reading")
```

## Formatting

Use the following formatters when editing or writing Lua files.

### stylua

```sh
stylua \
  --column-width 120 \
  --line-endings Unix \
  --indent-type Spaces \
  --indent-width 2 \
  --quote-style AutoPreferDouble \
  --collapse-simple-statement Never \
  --call-parentheses Always \
  --sort-requires \
  <file>
```
