# Scenario B rules

This project tests direct access to the physical PostgreSQL schema.

## Required behavior

1. Use only the `direct_postgres` connection for data access. `decimal_calculator.calculate` is the only permitted MCP, solely for arithmetic as specified below; it is not a data-access route.
2. Generate read-only queries only: `SELECT` and `WITH` statements.
3. Use the synchronized `retail` schema and available columns; do not invent tables or fields.
4. Do not use Cube, `cube_semantic`, `cube_query`, or any other data-access MCP. Use `decimal_calculator.calculate` as required by the section below, only for arithmetic over values already retrieved from `direct_postgres`; it cannot provide or change data.
5. Do not use any Contoso business skill or undocumented business-policy rule.
6. Do not invent values when the query fails or the schema is insufficient.
7. Report the data route and relevant assumptions in the answer.
8. When asked to produce long lists of results, show the results in one monospace plain-text code block using triple backticks, with one result item on each line. If the complete list won't fit in a single response, provide it as a downloadable text file intsead of leaving entries out

The technical schema context is allowed. Business-policy context is deliberately absent.

## Required arithmetic: `decimal_calculator.calculate`

For every user-facing result that requires arithmetic over observed or explicitly provided numeric inputs, you **MUST** call the shared MCP `decimal_calculator.calculate` before presenting the calculated result—even when the calculation is simple. This includes averages/division, ratios, differences, percentages, and multi-step or weighted denominators. For an average, retrieve the authoritative numerator and denominator through the scenario's approved data route, then calculate the division with the MCP; do not perform the final arithmetic mentally or substitute a SQL/Cube expression that returns the derived average.

Keep data semantics separate from arithmetic:

- Use only this project's approved data route, specified above, to select, filter, and aggregate source rows and retrieve observed totals/counts. Do not send raw table rows to the calculator for database aggregation.
- Apply only business rules defined by this project's `RULES.md` or a skill explicitly referenced by it before forming the arithmetic expression. The calculator does not retrieve data, select filters, infer missing values, or decide business rules.
- Pass an explicit expression using the exact numeric inputs returned by the data tool, without currency symbols or thousands separators; use only `+`, `-`, `*`, `/`, `^`, and parentheses. Prefer one expression with parentheses for a multi-step result so the trace records the full formula.
- Use the calculator's returned value; do not round intermediate inputs. Round only the final displayed value to the requested precision, and follow any existing source-total rounding rule for source aggregates.
- If the MCP call fails or is unavailable, do not silently fall back to mental/model arithmetic or a SQL/Cube-derived substitute. Preserve the observed inputs, state that the derived result could not be verified, and retry only the identical expression if the failure is transient.

Example: for an ordinary weekday average, pass the actual observed amount and day count as a plain expression such as `12345.67 / 22`. For weighted denominators, encode only the factor and formula explicitly defined for that scenario; never introduce a policy from another scenario.

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
