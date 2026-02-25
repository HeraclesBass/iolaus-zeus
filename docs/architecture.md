# Architecture

## System Overview

Iolaus + Zeus is a dual-interface platform where a CLI-based AI agent (Iolaus) and a web-based command center (Zeus) share a common backend infrastructure of 44+ microservices.

## Design Principles

1. **Interface agnostic** — Both CLI and web interfaces connect to the same service mesh
2. **Session persistence** — tmux-backed terminal sessions survive disconnects
3. **Event-driven** — WebSocket event bus for real-time state synchronization
4. **Progressive enhancement** — Core functionality works without JavaScript; rich features layer on top

## Component Architecture

### Zeus Terminal (Client)

```
src/
  components/
    App.tsx              # Root with SplitView, window management
    TerminalWindow.tsx   # xterm.js wrapper with fit addon
    CanvasPanel.tsx      # HTML/Mermaid/code rendering
    MusicDock.tsx        # Floating YouTube player
    TodoPanel.tsx        # Task tracking with progress bars
    WelcomePage.tsx      # Animated landing page
    TemplateToolbar.tsx  # 88 icon templates in 9 categories
  hooks/
    useWebSocket.ts      # Connection management, reconnect logic
    useTerminal.ts       # xterm.js lifecycle, resize handling
    useTheme.ts          # Theme persistence
  context/
    WindowContext.tsx     # Multi-window state management
    MusicContext.tsx      # Player state, playlist sync
```

### Zeus Terminal (Server)

```
server/
  index.ts               # Express + WebSocket server
  terminal.ts            # node-pty process management
  session.ts             # tmux session lifecycle
  database.ts            # SQLite for window state, history
  migrations/            # Schema versioning (6 migrations)
```

### Iolaus Brain

```
app/
  main.py               # FastAPI application
  services/
    cognition.py         # Natural language understanding
    context.py           # Conversation state management
    platform.py          # Service discovery, health checks
    agents.py            # Agent delegation and routing
  models/
    conversation.py      # Chat history models
    intent.py            # Command intent classification
```

## Data Flow

```
User Input (Terminal or Chat)
  │
  ├─ Zeus Terminal ──→ WebSocket ──→ node-pty ──→ tmux ──→ shell
  │                                                         │
  │                                                    ┌────┴────┐
  │                                                    │ Platform │
  │                                                    │ Services │
  │                                                    └────┬────┘
  │                                                         │
  └─ Iolaus Chat ──→ FastAPI ──→ Agent Router ──→ Specialist Agent
                                      │
                                 89 agents available
                                 (orchestrator, security,
                                  database, frontend, etc.)
```

## Deployment

Both services run on the same host, communicating over localhost. Zeus Terminal is process-based (not containerized) for direct tmux access. Iolaus Brain runs in a Docker container with host network mode for service discovery.

```yaml
# Iolaus Brain
services:
  iolaus-brain:
    build: ./iolaus-brain
    network_mode: host
    ports:
      - "8083:8083"

# Zeus Terminal runs as a systemd service
# for direct tmux process management
```

## Database

Zeus Terminal uses SQLite for local state:

| Table | Purpose |
|-------|---------|
| windows | Window positions, types, sizes |
| sessions | tmux session mapping |
| artifacts | Canvas content history |
| starred_videos | Music player favorites |
| settings | User preferences |

Six migrations manage schema evolution, from initial window tracking through media window support.
