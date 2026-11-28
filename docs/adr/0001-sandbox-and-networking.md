# ADR 0001: Rootless sandbox with direct internet access

**Status:** Accepted

## Decision

Run Pi as a non-root user in an ephemeral, rootless Podman container. Keep passwordless `sudo`
inside the container for installing system packages. Mount only the selected project workspace and
necessary persistent caches (notably `mise`); do not mount host credentials or the Podman socket.

Allow outbound internet access for `mise install`, language package managers, Git, and build
scripts. Do not require MCP or an HTTP proxy for ordinary development traffic.

## Rationale and consequences

- Rootless Podman limits host privileges, while in-container `sudo` keeps the development
  environment flexible.
- `mise` and repository build scripts download from multiple upstream registries; MCP cannot
  transparently replace their network access.
- Direct egress means Pi can reach public endpoints and potentially host-reachable private
  endpoints. Separate Podman networks alone do **not** enforce a public-internet-only policy.
- The workspace is not a security boundary: anything mounted there can be read or changed by the
  agent. If stronger isolation becomes necessary, add enforceable egress filtering and review
  mount/UID mappings.
