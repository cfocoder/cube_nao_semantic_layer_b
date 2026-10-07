# Scenario B rules

This project tests direct access to the physical PostgreSQL schema.

## Required behavior

1. Use only the `direct_postgres` connection.
2. Generate read-only queries only: `SELECT` and `WITH` statements.
3. Use the synchronized `retail` schema and available columns; do not invent tables or fields.
4. Do not use Cube, `cube_semantic`, `cube_query`, or any data-access MCP. The only permitted MCP is `decimal_calculator.calculate`, for arithmetic over values already retrieved from `direct_postgres`; it cannot provide or change data.
5. Do not use any Contoso business skill or undocumented business-policy rule.
6. Do not invent values when the query fails or the schema is insufficient.
7. Report the data route and relevant assumptions in the answer.
8. When asked to produce long lists of results, show the results in one monospace plain-text code block using triple backticks, with one result item on each line. If the complete list won't fit in a single response, provide it as a downloadable text file intsead of leaving entries out

The technical schema context is allowed. Business-policy context is deliberately absent.

## Monetary total rounding

For any question that asks for a final monetary total:

- Use the `direct_postgres` connection and the verified `retail` schema.
- Aggregate the source values at full precision, then round the final `SUM` to two decimal places. For PostgreSQL floating-point amount columns, use the equivalent of `ROUND(SUM(amount_column)::numeric, 2)`.
- Never round each source row before summing (for example, do not use `SUM(ROUND(amount_column::numeric, 2))`).
- Keep full-precision values for ranking, filtering, thresholds, and other calculations that depend on exact values; this rounding rule is for final monetary totals only.
- Do not apply this rule to counts, quantities, rates, or percentages. Preserve frozen prompts, IDs, and gold values unchanged.

This rule applies to monetary totals generally, not only Q006.

## First-turn response requirement
Always answer the user's question in the current response and provide every requested field; if data is unavailable, state that explicitly without inventing values or deferring the answer to a follow-up.
