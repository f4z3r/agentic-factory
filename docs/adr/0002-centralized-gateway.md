# ADR 0002: Centralize authenticated capabilities behind agentgateway

**Status:** Proposed

## Decision

Use agentgateway as Pi's single endpoint for model inference and MCP tools. Run agentgateway and MCP
servers as separate local Podman containers, configured in this repository. Pi has no
inference-provider credentials, kubeconfigs, or privileged service tokens.

Agentgateway holds inference-provider secrets and controls MCP tool exposure. Each MCP server holds
its own minimum required backend credentials. For Kubernetes, use a dedicated read-only identity
restricted by RBAC. Keep agentgateway's administrative interface inaccessible to Pi.

## Rationale and consequences

- Provider and infrastructure credentials remain outside the agent's executable environment.
- MCP registrations and access policies can be changed without rebuilding the Pi image; additional
  agents can reuse the gateway.
- MCP allowlists are defense in depth, **not** a replacement for backend authorization (e.g.
  Kubernetes RBAC).
- The Pi-to-gateway connection is unauthenticated because it is confined to a trusted local Podman
  network. This network must not be exposed to untrusted containers or users.
- Direct internet access remains available to Pi; the gateway is the exclusive path for **configured
  authenticated capabilities**, not a universal network-enforcement point.
