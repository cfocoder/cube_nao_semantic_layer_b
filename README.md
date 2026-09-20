# Scenario B — Direct PostgreSQL with technical schema context

This repository is the Nao Multi-project context for formal scenario **B**.

## Boundary

- One read-only PostgreSQL connection named `direct_postgres`.
- Nao may synchronize table, column, schema, and preview metadata from `retail`.
- No Cube MCP or Cube semantic contract.
- No Contoso business skill or `effective_business_days` rule.
- The agent may use the physical schema and generate read-only SQL.

## Project mapping

- Nao project: `tesis-condition-b`
- Repository: `cfocoder/cube_nao_semantic_layer_b`
- Expected route: `direct_postgres`
