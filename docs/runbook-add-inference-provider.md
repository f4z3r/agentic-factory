# Runbook: Adding an Inference Provider

## Overview

This guide explains how to add a new inference provider to the Agentic Factory gateway.

## Prerequisites

- A running `agentgateway` container (from `podman compose up -d`).
- An API key from the desired inference provider (e.g., OpenRouter, vLLM, etc.).

## Steps

1. **Add the provider configuration to `gateway/config.yaml`:** under `llm.models`, add a new model
   entry pointing at the desired provider:

   ```yaml
   llm:
     models:
       - name: "<model-name>"
         provider: "<provider>"
         params:
           apiKey: $OPENROUTER_API_KEY
   ```

2. **Set the provider API key in `secrets/gateway.env`:** add or update the provider key:

   ```dotenv
   OPENROUTER_API_KEY=your-api-key
   ```

   The key can be referenced in `gateway/config.yaml` via `$<VAR_NAME>` placeholders.

3. **Verify:** the gateway should pick up the configuration changes very rapidly. Confirm via the
   gateway UI at `http://localhost:4001/ui/`.
