# Runbook: Adding an MCP Server

## Overview

This guide explains how to add a new MCP (Model Context Protocol) server to the Agentic Factory
infrastructure.

## Prerequisites

- A running `agentgateway` container (from `podman compose up -d`).
- The `mcp/` directory containing the target service's configuration.
- Access to the backend network where the MCP server runs (e.g., Kubernetes for K8s, Jira, etc.).

## Steps

1. **Add MCP server entry to `compose.yaml`:** place your MCP configuration under
   `mcp/<name>/config.yaml`, and add it as a read-only volume mount in the compose file. If
   credentials are needed then add them to `secrets/<name>.env`.

   Under the `services:` section of `compose.yaml`, add a new service block pointing to your MCP
   host URL:

   ```yaml
   mcp-<name>:
     image: <your-mcp-image>
     command: [--config, /etc/config.toml]
     env_file:
       - ./secrets/<name>.env
     volumes:
       - ./mcp/<name>/config.toml:/etc/config.toml:ro
     networks: [pi-backend]
   ```

2. **Register the MCP target in `gateway/config.yaml`:** add a new target under `mcp.targets`:

   ```yaml
   targets:
     - name: kubernetes
       mcp:
         host: http://mcp-kubernetes:8080/mcp
     - name: jira
       mcp:
         host: http://jira-mcp:8080/mcp
   ```

   This makes the gateway aware of the new MCP endpoint.

3. **Restart / re-compose:**
   ```bash
   podman compose down
   podman compose up -d
   ```

## Verification

After the changes, confirm that the new MCP server is reachable via the gateway UI at
`http://localhost:4001/ui/` and that the agent can invoke the tool through the standard MCP flow.
