# Graphify — knowledge graph of the codebase (Graphify-Labs/graphify)

Purpose: map the whole project (code via local tree-sitter AST — nothing leaves the machine; docs/PDFs/images optionally via a semantic pass) into a graph you query instead of grepping file after file. Saves context and finds non-obvious connections.

## When

- First contact with an unfamiliar or large repo.
- Before planning a change: what already exists, what depends on what.
- Debugging: how does A reach B?
- Review: blast radius of changed symbols.

## Build / refresh

```bash
graphify .                 # build graph for current folder (or /graphify . inside the assistant)
graphify . --update        # re-extract only changed files (after edits / git pull)
graphify . --no-viz        # report + JSON only, skip the HTML
graphify hook install      # optional: auto-rebuild on commit and branch checkout
```

Outputs in `graphify-out/`:
- `GRAPH_REPORT.md` — read this first: god nodes (most-connected concepts), communities (subsystems), surprising links, suggested questions.
- `graph.json` — the full graph, queryable without re-reading files.
- `graph.html` — interactive view for the user (open in a browser).

Honors `.gitignore`, `.graphifyignore`, `--exclude`. Add `graphify-out/` to `.gitignore` unless the team wants it committed.

## Query

```bash
graphify query "what connects auth to the database?"   # scoped subgraph for a question
graphify explain "UserService"                          # node: source location, community, all edges
graphify path "LoginScreen" "ApiClient"                 # shortest path between two things
graphify export callflow-html                            # Mermaid architecture / call-flow page
```

Edges are tagged `EXTRACTED` (explicit in source) or `INFERRED` (resolved by graphify). Treat INFERRED edges as leads to confirm by reading the file at the given line, not as facts. A path's confidence is its weakest hop.

## Rules

- Query the graph first, then open only the files it points to.
- Re-run `--update` after significant edits so later queries aren't stale.
- If graphify isn't installed, fall back to Grep/Glob — don't block.
