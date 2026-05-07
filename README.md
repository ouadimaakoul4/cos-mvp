# cos-mvp

# COS – Cognitive Operating System

[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue)]()
[![Rust](https://img.shields.io/badge/rust-stable-orange)]()
[![Status](https://img.shields.io/badge/status-MVP--v1.0-green)]()

**A formally grounded, capability-secure operating system for persistent multi‑agent cognition.**

COS is not an agent framework or an IDE plugin. It is a **new class of operating system** that combines memory safety, strong isolation, temporal knowledge, and controlled self‑evolution to host trustworthy, long‑running AI agents.

---

## Table of Contents

- [Why COS?](#why-cos)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Quick Start (MVP)](#quick-start-mvp)
- [Technology Stack](#technology-stack)
- [Roadmap](#roadmap)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

---

## Why COS?

Current AI platforms trust monolithic, unverifiable runtimes with no built‑in notion of **memory safety**, **temporal context**, or **secure agent isolation**.  
Agents leak state, suffer from prompt injection, and cannot safely modify even their own behaviour.

COS solves this by applying five decades of operating systems research – capability‑based security, microkernels, formal verification – directly to the AI stack:

- **Immutable verified kernel** → no memory corruption, no privilege escalation.
- **Temporal knowledge graph** → agents remember facts with timelines, not flat vector stores.
- **WASM sandboxes with deny‑by‑default capabilities** → untrusted code stays fully contained.
- **Bounded self‑evolution** → the system can improve itself without breaking its own invariants.

---

## Key Features

- **Rust‑based capability kernel** – Verified core with cryptographic tokens for every resource.
- **Temporal graph memory (CozoDB)** – Relational + graph + vector, with native time‑travel and soft erasure.
- **WASM agent sandboxes** – Each agent is compiled to WebAssembly and runs with minimal, explicitly granted powers.
- **MCP & A2A first‑class** – Interact through the Model Context Protocol (tools, prompts) and Google’s Agent‑to‑Agent protocol for collaboration.
- **Immutable audit trail** – Every state change is logged and replayable for forensic analysis.
- **Formal verification** – Critical safety properties are machine‑proved (Kani/Verus) rather than just tested.
- **Governance layer** – Real‑time monitoring of memory coherence, safety violations, and agent trust.

---

## Architecture

```
 MCP Client (Claude Desktop, Inspector, ...)
          │
          ▼
 ┌──────────────────────┐
 │   MCP Gateway (rmcp) │
 └──────────┬───────────┘
            │
 ┌──────────▼──────────┐
 │  Governance Layer   │  reflexion, trust, safety monitor
 └──────────┬──────────┘
            │
 ┌──────────▼──────────┐
 │   Cognitive Layer   │  orchestrator, planner, consensus, evolution vetting
 └──────────┬──────────┘
            │
 ┌──────────▼──────────┐
 │   Memory Subsystem  │  CozoDB temporal graph, vector index, Ψ compressor
 └──────────┬──────────┘
            │
 ┌──────────▼──────────┐
 │  Agent Sandboxes    │  architect, backend, security, verification (WASM)
 └──────────┬──────────┘
            │
 ┌──────────▼──────────┐
 │  Verified Micro‑Kernel│ capability system, permissions, scheduler, event bus
 └─────────────────────┘
```

**Invariants**:
- Kernel is immutable at runtime.
- All cross‑domain communication is capability‑checked.
- Agents start with zero authority.

---

## Quick Start (MVP)

> The MVP is a single Rust binary that boots a capability kernel, spins up a sandboxed agent, and exposes a temporal graph via MCP.

### Prerequisites

- Rust stable (1.80+)
- Wasmtime 25 (automatically fetched by Cargo)
- MCP Inspector (`npx @modelcontextprotocol/inspector`) for testing

### Build & Run

```bash
git clone https://github.com/cos-org/cos-mvp.git
cd cos-mvp
cargo run --release
```

You should see `COS kernel booted` followed by an MCP server ready on stdio.

### Test with the MCP Inspector

```bash
npx @modelcontextprotocol/inspector stdio -- cargo run --release
```

The inspector will list two tools: `create_entity` and `query_entities`.  
Try creating an entity and then querying it – the data lands in CozoDB and appears in the audit log.

---

## Technology Stack

| Component            | Technology                                      |
|----------------------|-------------------------------------------------|
| **Language**         | Rust (stable)                                   |
| **Kernel verification** | Verus + Kani model checker                     |
| **Agent sandbox**    | Wasmtime 25 + WASI Preview 2                    |
| **Temporal graph**   | CozoDB (embedded, Datalog + graph-algo)         |
| **MCP**              | `rmcp` (official Rust SDK)                      |
| **A2A**              | Planned `a2a-rs` (gRPC/QUIC)                    |
| **Inference**        | Burn (WASM backend) / Candle (lightweight)      |
| **Observability**    | `tracing` + append‑only signed audit log        |
| **Frontend**         | Tauri (Rust + TypeScript) – future IDE plugin   |

---

## Roadmap

- **Phase 0 (MVP)** ✅  
  Trusted skeleton: capability kernel, one sandboxed agent, CozoDB temporal graph, MCP gateway, Kani‑verified capability check.

- **Phase 1 – Verified Core & Governance**  
  Multi‑agent sandboxes, full Verus verification of core, reflexion/trust module, stable A2A.

- **Phase 2 – Cognitive Amplification**  
  Specialized agents, consensus engine, memory compressor with provenance, digital twin for safe evolution.

- **Phase 3 – Autonomous Planning & IDE**  
  Planner with safety‑constrained MCTS, Tauri IDE plugin, distributed cognition, bounded self‑evolution loop.

---

## Documentation

- [COS Final Blueprint v1.0](docs/blueprint-v1.0.md) – Full mathematical, architectural, and safety specification.
- [MVP Specification](docs/mvp-v1.0.md) – Build instructions, feature list, acceptance criteria.
- [Whitepaper (arXiv preprint)](https://arxiv.org/...) (coming soon)

---

## Contributing

COS is an open community project. We welcome contributions around **formal verification**, **WASM sandboxing**, **graph databases**, and **safe AI**.

1. Read the [Blueprint](docs/blueprint-v1.0.md) to understand the design.
2. Check the [MVP issues](https://github.com/cos-org/cos-mvp/issues) for starter tasks.
3. Join the discussion on GitHub Discussions or Discord (link coming).

All contributions must respect the project’s **security invariants** (no raw `unsafe` without proof, sandbox‑first design). By contributing, you agree to license your work under the same MIT/Apache‑2.0 terms.

---

## License

COS is dual-licensed under

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE))
- MIT license ([LICENSE-MIT](LICENSE-MIT))

at your option.

---

*Cosmos is meant to be explored. COS makes sure that exploration is safe, durable, and verifiable.*
