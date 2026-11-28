# ADR 0003: Share long-term agent knowledge through Basic Memory

**Status:** Accepted

## Context

Pi already persists conversation and session history. We additionally want agents working on
different repositories to accumulate and reuse durable engineering knowledge: architectural
decisions, conventions, proven solutions, and lessons learned. The memory must survive disposable
agent containers and be available to all agents through the existing agentgateway MCP integration.

We do not require isolation between agents or projects, separate memory credentials, or automatic
storage of conversations. We prefer a lightweight, self-hosted solution with inspectable data and
minimal infrastructure.

## Decision

Use [Basic Memory](https://github.com/basicmachines-co/basic-memory) as a single shared, long-term
knowledge MCP service.

- Run one Basic Memory instance on the MCP backend network, accessible to agents only through
  **agentgateway**. Do not publish its MCP port on the host.
- Persist its Markdown knowledge files and SQLite index in a Podman volume. Treat Markdown as the
  portable knowledge source and the index as supporting infrastructure.
- Use one logical Basic Memory project (`shared`). Organize notes by purpose, for example
  `projects/<project-id>/`, `patterns/`, and `preferences/`. Cross-project retrieval is intentional.
- Do not require authentication between agentgateway and Basic Memory on the trusted local backend
  network. All agents can read and update all notes; project names are organizational labels, not
  authorization boundaries.
- Expose read, search, write, and edit tools through agentgateway. Restrict destructive and
  administrative operations where practical.
- Define memory usage guidelines in the global Pi `AGENTS.md`: search before substantial work, save
  only reusable and verified knowledge, look for existing notes before writing, and update stale
  information instead of creating duplicates. Optional Basic Memory skills may guide capture and
  curation, but must not be required for ordinary retrieval.
- Keep Pi session history separate. Never store credentials, secrets, or raw conversations as
  memory.

## Alternatives considered

- **[Engram](https://github.com/Gentleman-Programming/engram):** A particularly lightweight Go
  binary with SQLite, structured observations, and project-aware retrieval. Its primary MCP
  interface is stdio; using it as a central remote MCP service would require additional integration.
  Its emphasis on coding-agent continuity overlaps more with Pi's existing session management.
- **[memcp](https://github.com/Jartan-LLC/memcp):** A simple HTTP MCP memory service with SQLite and
  logical scopes. Its default SQLite backend relies on lexical retrieval, and individual memory
  records are less convenient to inspect and maintain as a shared technical knowledge base. Its
  static key management solution also restricts highly dynamic setups such as this one.
- **[MCP reference memory server](https://github.com/modelcontextprotocol/servers/tree/main/src/memory):**
  Offers a compact entity/relation graph persisted as JSONL, but requires more structure around
  writing, finding, and maintaining longer technical knowledge.
- **Repository-local Markdown or Pi extensions:** Minimal infrastructure, but knowledge is scattered
  across repositories or tied to Pi, making central cross-project discovery and reuse harder.

## Rationale and consequences

- Markdown notes are readable, editable, portable, and easy to back up. Basic Memory adds search and
  linked context without requiring a separate database service; semantic retrieval can be enabled if
  lexical search proves insufficient.
- A dedicated MCP service keeps memory independent of Pi and reusable by other agent
  implementations.
- All agents share the same writable knowledge base. This deliberately provides **no confidentiality
  or integrity isolation between projects**; agents can also introduce incorrect or malicious
  content. Retrieved memory must be treated as untrusted, potentially outdated reference material.
- Knowledge quality depends on agent behavior and occasional curation. Basic Memory has more runtime
  dependencies than a single-binary alternative, and concurrent edits and backup/restore behavior
  should be tested.
- Reconsider this decision if strict project isolation, automatic fact extraction, or substantially
  more sophisticated retrieval becomes necessary.
