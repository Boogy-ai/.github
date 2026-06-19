<h1 align="center">Boogy</h1>

<p align="center">
  <b>Vibe whole apps into production.</b><br/>
  Frontend, API, and data from one Rust crate compiled to <code>wasm32-wasip2</code> —
  on a shared runtime with an isolated transactional store, capability security,
  and transactions that span services.
</p>

<p align="center">
  <a href="https://boogy-ai.github.io/"><b>Developer hub</b></a>
  ·
  <a href="https://boogy-ai.github.io/boogy-sdk/">API reference</a>
  ·
  <a href="https://boogy.ai">boogy.ai</a>
</p>

---

## Repositories

| Repo | What it is |
|------|------------|
| **[boogy-sdk](https://github.com/Boogy-ai/boogy-sdk)** | The SDK + `boogy` CLI — `Router`, typed handlers, `wit_glue!`, store helpers, `McpServer`. Full [architecture overview](https://github.com/Boogy-ai/boogy-sdk/blob/main/ARCHITECTURE.md) and [API reference](https://boogy-ai.github.io/boogy-sdk/). |
| **[boogy-superpowers](https://github.com/Boogy-ai/boogy-superpowers)** | Agent skills that teach a coding agent to build Boogy services well. |
| **[boogy-catalog](https://github.com/Boogy-ai/boogy-catalog)** | Provisionable, best-practice services — email, payments, governance — you deploy into your own tenant with your own keys. |

## How it fits together

```mermaid
flowchart TB
  client(["Client · browser · agent"])

  subgraph host["Boogy runtime"]
    direction TB
    edge["HTTP edge<br/>auth · ingress · rate limit · CORS"]
    sched["Fair scheduler<br/>per-owner budgets · backpressure"]
    inst["Your service<br/>Rust to wasm32-wasip2"]
    edge --> sched --> inst
  end

  store[("Per-service<br/>transactional store")]
  peer["Other services<br/>(the mesh)"]
  ext["Third-party APIs"]

  client -->|"request"| edge
  inst -->|"REST · JSON-RPC · MCP"| client
  inst -->|"store"| store
  inst -->|"peer::fetch"| peer
  inst -->|"outbound_http"| ext
```

You write handlers; the runtime gives each service a route subtree, an isolated
**relational, ACID** store, a set of capabilities it must declare, and REST /
JSON-RPC / MCP surfaces. Services call each other in-process — and a single
transaction can span that whole call tree.

## Start here

→ **[Developer hub](https://boogy-ai.github.io/)** routes you to the docs, skills,
and catalog. Building with an agent? `boogy skills install` vendors the skills
into your project.

---

<p align="center"><sub>MIT OR Apache-2.0</sub></p>
