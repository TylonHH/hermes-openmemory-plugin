# OpenMemory Plugin for Hermes Agent

Self-hosted Mem0 memory provider for Hermes Agent.

OpenMemory is the official self-hosted version of Mem0, providing semantic memory with vector search, LLM-powered fact extraction, and persistence via PostgreSQL + Qdrant. Unlike the cloud Mem0 Platform API, OpenMemory runs on your own infrastructure (NAS, VPS, homelab).

## Installation

### Option 1: Manual Installation (Recommended)

Clone or download this repository and copy the `openmemory` directory to your Hermes plugins:

```bash
git clone https://github.com/tylonhh/hermes-openmemory-plugin.git
cp -r hermes-openmemory-plugin/openmemory ~/.hermes/plugins/memory/openmemory
```

Or directly:

```bash
mkdir -p ~/.hermes/plugins/memory/openmemory
curl -fsSL https://raw.githubusercontent.com/tylonhh/hermes-openmemory-plugin/main/__init__.py -o ~/.hermes/plugins/memory/openmemory/__init__.py
```

### Option 2: Hermes Plugins Install

If the plugin is available via the Hermes plugin registry:

```bash
hermes plugins install https://github.com/TylonHH/hermes-openmemory-plugin
```

## Setup

After installation:

1. Deploy OpenMemory (see docker-compose files)
2. Configure Hermes (see Configuration section below)

## Configuration

Add to `~/.hermes/.env`:

```bash
OPENMEMORY_API_URL=http://localhost:8765
OPENMEMORY_APP_ID=hermes
OPENMEMORY_USER_ID=hermes-user
```

## Testing

Run the test suite:

```bash
cd hermes-openmemory-plugin
pytest tests/ -v
```

## Links

- [Hermes Agent](https://github.com/NousResearch/hermes-agent)
- [OpenMemory](https://github.com/mem0ai/mem0/tree/main/openmemory)
