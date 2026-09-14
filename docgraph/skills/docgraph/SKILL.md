---
name: docgraph
description: Drive the user's DocGraph vault from Claude Code — draw ERDs, architecture and flow diagrams on a whiteboard, or read/write notes — via the DocGraph MCP server. Use for any request that mentions DocGraph, a whiteboard, an ERD or a diagram of "my database"/"my system", or /docgraph.
---

# DocGraph

You are driving the user's DocGraph vault through the `docgraph` MCP server
(tools named `mcp__docgraph__*`). Act on the request in `$ARGUMENTS`; if it is
empty, ask what to draw or write.

## 0. Preflight

If no `mcp__docgraph__*` tool is available, stop and tell the user, in three
lines:

1. In DocGraph: **Settings → AI → MCP → Enable**, then **Copy** the token.
2. `export DOCGRAPH_MCP_TOKEN="<token>"` in the shell profile (the plugin's
   server config reads that variable), then restart Claude Code.
3. Alternatively register the server directly:
   `claude mcp add --scope user --transport http docgraph http://127.0.0.1:8765/ --header "Authorization: Bearer <token>"`

If a call fails with "No vault is open", ask the user to open a vault in
DocGraph and retry. Every write pops a confirmation modal inside DocGraph; tell
the user to approve it there (it times out after 120 s).

## 1. Work out what is being asked

- **ERD of "my database" / "this project" / a path** → derive the schema from
  source (section 2), then draw (section 3).
- **ERD from a description** ("users, orders, order_items…") → draw directly.
- **Architecture / system / infra diagram** → read the codebase (services,
  datastores, queues, external APIs, deploy config) and draw with `infra` nodes.
- **Flowchart / process** → `shape` nodes (`diamond` for decisions,
  `round-rectangle` for steps, `cylinder` for storage), labeled edges.
- **Extend / change an existing board** → `read_board` first, then
  `edit_board` ops (`add_node`, `connect`, `relabel`, `recolor`, `remove_node`,
  `disconnect`, `add_frame`). Untouched nodes keep their positions; pass
  `relayout: true` only if asked to tidy up.
- Anything else about the vault (notes, tasks, tables, search) → use the
  matching `mcp__docgraph__*` tool; `search_vault` / `read_file` for reading,
  `create_file` / `edit_file` / `append_to_file` for writing.

Two different paths appear in these requests. A **project path** ("to this
path /Users/me/proj", "for my database") is the code to read the schema from;
it is NOT where the board goes. The **board path** is vault-relative and must
end in `.wb`. If the user gave no board path, use `Diagrams/<project>-erd.wb`
(or `-architecture.wb`, `-flow.wb`) and say so.

## 2. Deriving a schema from a project

Search the project path (default: the current working directory) in this
order and use the first rich source; merge if several exist:

| Source | Files to read |
|---|---|
| SQL | `*.sql`, `schema.sql`, `structure.sql`, `migrations/**`, `db/migrate/**` |
| Prisma | `schema.prisma` |
| Drizzle | `**/schema.ts`, `drizzle/**` |
| TypeORM / MikroORM / Sequelize | `@Entity` / `@Table` classes, `models/**` |
| Django | `**/models.py` |
| Rails | `db/schema.rb` |
| SQLAlchemy | `Base` subclasses, `alembic/versions/**` |
| Laravel | `database/migrations/**`, `app/Models/**` |
| Go / Rust | `gorm` structs, `sqlx`/`diesel` `schema.rs`, `migrations/**` |
| Supabase | `supabase/migrations/**` |

For each table collect: name, columns with a short type string (`uuid`,
`varchar(255)`, `int`, `timestamptz`, `bool`, `jsonb`, …), the primary key,
and every foreign key with its target table and column. Migrations must be
replayed in order (later `ALTER TABLE` wins). Skip framework bookkeeping tables
(`schema_migrations`, `_prisma_migrations`, `django_migrations`, sessions,
cache) unless asked. Cap at 60 columns per table; if the schema has more than
~40 tables, ask whether to draw all of it or a named subset, or frame it by
domain.

If nothing is found, say what you looked for and ask the user to point at the
schema or describe it.

## 3. Drawing the board

Call `mcp__docgraph__create_board` once with:

- `path`: vault-relative, ending in `.wb`.
- `nodes`: one `{ "id", "type": "table", "label": "<table name>", "columns": [...] }`
  per table. Each column is `{ "name", "type", "key" }` where `key` is `"pk"`,
  `"fk"`, or omitted. Keep column order as in the source.
- `edges`: one per foreign key, `{ "source": "<child table id>",
  "target": "<parent table id>", "sourceColumn": "<fk column>",
  "targetColumn": "<pk column>", "sourceCard": "many", "targetCard": "one" }`.
  Use `zero-or-one` / `one-or-many` / `zero-or-many` when the source makes the
  nullability or uniqueness explicit (a unique FK is one-to-one). Add a short
  `label` only when the relationship name is not obvious from the column.
- `frames`: optional `{ "label", "around": [ids] }` to group tables by domain
  or schema when there are more than ~8 tables.

Never send coordinates; layout is automatic. Node ids must be unique; using
the table name as the id is fine.

For non-ERD boards the same shape applies with `type: "shape"` (`shape`:
`rectangle`, `round-rectangle`, `diamond`, `circle`, `cylinder`, `hexagon`,
`arrow-rectangle`, `plus`, `triangle`) or `type: "infra"` (`kind`: `server`,
`service`, `container`, `function`, `worker`, `database`, `cache`, `store`,
`warehouse`, `search`, `lb`, `gateway`, `cdn`, `firewall`, `dns`, `proxy`,
`boundary`, `queue`, `topic`, `eventbus`, `stream`, `auth`, `idp`, `secrets`,
`user`, `users`, `mobile`, `webclient`, `external`, `task`, `milestone`,
`approval`, `timer`, `document`, `report`, `email`, `presentation`, `payment`,
`budget`, `growth`, `target`, `order`, `campaign`, `idea`), plus `text`,
`label`, and `noteLink` (`link`: a vault path). Freehand ink cannot be
authored.

## 4. Report

After the write is approved, tell the user the board path, the table and
relationship counts, and anything you skipped or guessed (an FK inferred from
a `_id` naming convention, a table left out). Offer to embed the board in a
note: `append_to_file` on a note with a link to the `.wb` path.
