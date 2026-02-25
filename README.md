# Iolaus + Zeus

Dual-interface AI platform combining a CLI agent (Iolaus) with a web-based command center (Zeus Terminal). Event-driven microservice architecture with real-time WebSocket communication across 44+ services.

## Architecture

```
                    ┌─────────────────────────────┐
                    │      Zeus Terminal (Web)     │
                    │   React + xterm.js + Vite    │
                    │    terminal.herakles.dev     │
                    └─────────────┬───────────────┘
                                  │ WebSocket
                    ┌─────────────┴───────────────┐
                    │     Express + node-pty       │
                    │   tmux session persistence   │
                    │   WebSocket + REST API        │
                    └─────────────┬───────────────┘
                                  │
        ┌─────────────────────────┼─────────────────────────┐
        │                         │                         │
┌───────┴───────┐   ┌────────────┴────────────┐   ┌────────┴────────┐
│  Iolaus Brain │   │   Platform Services     │   │  Observability  │
│  (FastAPI)    │   │   44 containers         │   │  Stack          │
│  AI cognitive │   │   12 frameworks         │   │  Loki/Grafana   │
│  AI cognitive │   │   Multi-port mesh       │   │  Prometheus     │
└───────────────┘   └─────────────────────────┘   └─────────────────┘
```

## Iolaus (CLI Agent)

The AI cognitive backend powering intelligent command-line interactions.

**Stack:** Python, FastAPI, Uvicorn

**Capabilities:**
- Natural language command interpretation
- Context-aware assistance across the platform
- Service health analysis and troubleshooting
- Multi-step task decomposition and execution
- Integration with 89 specialized agents

## Zeus Terminal (Web Interface)

Full-featured web terminal with tmux persistence, multi-window management, and integrated development tools.

**Stack:** React 18, TypeScript, Vite, xterm.js, WebSocket, node-pty

**Features:**

- **Multi-window terminal** — Split views, tabs, drag-and-drop window management
- **tmux persistence** — Sessions survive disconnects, reconnect seamlessly
- **Canvas panel** — Render HTML, Mermaid diagrams, code previews inline
- **Artifact history** — 50-item cache with thumbnails and auto-tagging
- **YouTube music dock** — Dockable player with 4 snap positions, multi-device sync
- **Token tracking** — Per-window context usage with threshold warnings (90/95/98%)
- **Icon template toolbar** — 88 templates across 9 categories, accessible in one hover
- **Mobile-first** — Touch-optimized UI, responsive layouts

**Quality:** 345 tests, TypeScript strict mode, zero regressions across releases

## Event Bus

Services communicate through a WebSocket event bus with typed message protocols:

```
music:subscribe    → Player state sync across windows
dock:update        → Floating player position management
context:warning    → Token usage threshold alerts
artifact:subscribe → Canvas content updates
service:health     → Platform-wide health events
```

## Service Map

| Service | Framework | Purpose |
|---------|-----------|---------|
| Zeus Terminal | Express + xterm.js | Web terminal interface |
| Iolaus Brain | FastAPI | AI cognitive backend |
| Iolaus Frontend | Nginx | Chat interface |
| Command Center | Flask + React | Platform monitoring |
| Portfolio Gateway | Node.js | Routing + auth |

## Related Projects

- [claude-orchestrator-showcase](https://github.com/HeraclesBass/claude-orchestrator-showcase) — The V11 agent framework that powers Iolaus
- [tos-analyzer](https://github.com/HeraclesBass/tos-analyzer) — Example service in the platform
- [portfolio](https://github.com/HeraclesBass/portfolio) — Full platform architecture overview

## Tech Stack

TypeScript, React, Python, FastAPI, Node.js, Express, WebSocket, xterm.js, tmux, Docker, Nginx
