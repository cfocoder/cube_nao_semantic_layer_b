# Scenario B rules

This project tests direct access to the physical PostgreSQL schema.

## Required behavior

1. Use only the `direct_postgres` connection.
2. Generate read-only queries only: `SELECT` and `WITH` statements.
3. Use the synchronized `retail` schema and available columns; do not invent tables or fields.
4. Do not use Cube, `cube_semantic`, `cube_query`, or any MCP.
5. Do not use any Contoso business skill or undocumented business-policy rule.
6. Do not invent values when the query fails or the schema is insufficient.
7. Report the data route and relevant assumptions in the answer.

The technical schema context is allowed. Business-policy context is deliberately absent.

## Scoped Q006 rounding trial

For the manual Q006 question, “What were the total online sales in 2009?”, only:

- Use the `direct_postgres` connection and the available `retail.factonlinesales` schema.
- Apply the year filter to `datekey` using the verified schema.
- Round only the final aggregate to two decimal places: `ROUND(SUM(salesamount)::numeric, 2)`.
- Do not round individual rows before summing; do not use `SUM(ROUND(salesamount::numeric, 2))`.
- Do not apply this temporary trial rule to other questions, or change benchmark prompts, IDs, or golds.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
