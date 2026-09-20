# Scenario B deployment contract

## Required Nao project

```text
Project: tesis-condition-b
Repository: https://github.com/cfocoder/cube_nao_semantic_layer_b.git
Branch: main
```

## Required runtime variables

```text
DIRECT_PG_HOST
DIRECT_PG_PORT=5432
DIRECT_PG_DATABASE
DIRECT_PG_USER
DIRECT_PG_PASSWORD
OPENAI_API_KEY / OPENAI_BASE_URL
OPENROUTER_API_KEY / OPENROUTER_BASE_URL
```

The PostgreSQL user must be read-only and limited to the `retail` schema. Do not configure `CUBE_API_URL`, `CUBE_API_SECRET`, or a Cube MCP in this project.
