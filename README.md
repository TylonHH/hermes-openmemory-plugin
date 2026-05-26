# OpenMemory Plugin for Hermes Agent

<p align="center">
  <b>🏠 Self-hosted semantic memory for AI Agents</b><br>
  Run Mem0 on your NAS/VPS — No cloud dependency, full data control.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Plugin-Memory-orange?style=for-the-badge&logo=memory">
  <img src="https://img.shields.io/badge/Docker-Ready-blue?style=for-the-badge&logo=docker">
  <img src="https://img.shields.io/badge/NAS-Compatible-green?style=for-the-badge&logo=synology">
  <img src="https://img.shields.io/badge/Synced_with_Hermes-✓-brightgreen?style=for-the-badge">
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/TylonHH/hermes-openmemory-plugin?style=social" alt="GitHub stars">
  <img src="https://img.shields.io/github/license/TylonHH/hermes-openmemory-plugin?style=social" alt="License">
</p>

---

## 📸 Preview

### Web UI Dashboard

![OpenMemory Web UI](https://via.placeholder.com/800x450.png?text=Dashboard+Screenshot+Placeholder)
*The Dashboard displays all stored memories, categories, and statistics.*

*(Replace this placeholder with a real screenshot from your installation)*

### Terminal Output

```text
╭─ ⚕ Hermes ───────────────────────────────────────────────────────────────╮
│  Hello René! It's great to hear you are using OpenMemory on your NAS.    │
╰──────────────────────────────────────────────────────────────────────────╯
  ┊ 🧠 memory +user: "User's name is René. Uses OpenMemory on their NAS."
```

---

## ✨ Why OpenMemory?

| Feature | Mem0 Cloud Plugin | OpenMemory Plugin (This Repo) |
|---------|-------------------|-------------------------------|
| **Data Residency** | External Cloud | **100% Self-Hosted** |
| **Cost** | Per API-Call | **Infrastructure Only (optional: $0)** |
| **NAS Support** | ❌ None | **✅ Optimized** (Synology, QNAP, TrueNAS) |
| **Free Models** | ❌ None | **✅ Yes** (OpenRouter Free Tier) |
| **Web UI** | ❌ No access | **✅ Integrated** |
| **Privacy** | Data on 3rd party servers | **Completely Local** |

### Who is this plugin for?

- 🏠 **Homelabers** who want full control over their data.
- 💰 **Cost-conscious** users avoiding recurring cloud fees.
- 🔒 **Privacy advocates** keeping everything on-premise.
- 🌐 **NAS owners** (Synology, QNAP, TrueNAS) running Docker.

---

## 🚀 Quick Start (3 Minutes)

### Prerequisites

- Docker + Docker Compose installed.
- Hermes Agent installed ([hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com)).

### Step 1: Install the Plugin

```bash
hermes plugins install https://github.com/TylonHH/hermes-openmemory-plugin
```

**Manual Installation:**

```bash
mkdir -p ~/.hermes/plugins/memory/openmemory
git clone https://github.com/TylonHH/hermes-openmemory-plugin.git
cp -r hermes-openmemory-plugin/plugins/memory/openmemory/* ~/.hermes/plugins/memory/openmemory/
rm -rf hermes-openmemory-plugin
```

### Step 2: Start the OpenMemory Server

**Standard Setup:**

```bash
cd ~/.hermes/plugins/memory/openmemory
docker compose -f docker-compose.example.yml up -d
```

**NAS Setup** (Synology, QNAP, etc.):

```bash
# Edit paths (e.g., /volume1/) to match your volume structure
nano docker-compose-nas.yml
docker compose -f docker-compose-nas.yml up -d
```

### Step 3: Configure Hermes

```bash
echo 'OPENMEMORY_API_URL=http://localhost:8765' >> ~/.hermes/.env

# For NAS installations (replace with your NAS IP or hostname):
# echo 'OPENMEMORY_API_URL=http://diskstation:8765' >> ~/.hermes/.env

hermes config set memory.provider openmemory
hermes /restart
```

### ✅ Verify Installation

```bash
hermes memory status
```

**Expected Output:**

```text
Memory status
────────────────────────────────────
  Built-in:  always active
  Provider:  openmemory

  Plugin:    installed ✓
  Status:    available ✓
```

---

## 🌐 Web UI Dashboard

After starting the containers, the dashboard is available at:

`http://your-server:3001`

**Dashboard Features:**
- 📋 View all stored memories.
- 🔍 Search and filter memories.
- 📊 Usage statistics.
- ⚙️ Manage settings.

---

## 💾 NAS Deployment

This plugin is optimized for NAS environments. Here are the recommended volume mappings for common systems:

### Synology DSM

1. Open **Container Station**.
2. Create a new project.
3. Paste the content of `docker-compose-nas.yml`.
4. Adjust paths:
   ```yaml
   volumes:
     - /volume1/docker/openmemory/qdrant:/qdrant/storage
   ```

### QNAP QTS

```yaml
volumes:
  - /share/CACHEDEV1_DATA/docker/openmemory/qdrant:/qdrant/storage
```

### TrueNAS Scale

```yaml
volumes:
  - /mnt/tank/docker/openmemory/qdrant:/qdrant/storage
```

---

## 💰 Cost Optimization

OpenMemory requires an LLM for memory processing. With **OpenRouter**, you can run it for **free** using their free tier models.

### Free Models via OpenRouter

| Model | Provider | Context Window |
|-------|----------|----------------|
| `meta-llama/llama-3.1-8b-instruct:free` | Meta | 8k tokens |
| `google/gemini-flash-1.5:free` | Google | 1M tokens |
| `mistralai/mistral-7b-instruct:free` | Mistral | 8k tokens |

### Configuration

In `docker-compose.yml`:

```yaml
environment:
  - OPENAI_API_BASE=https://openrouter.ai/api/v1
  - OPENAI_API_KEY=***  - LLM_MODEL=meta-llama/llama-3.1-8b-instruct:free
```

**Result:** $0 monthly costs for memory operations! 🎉

---

## 🏗️ Architecture

```text
┌─────────────────────────────────────────────────────┐
│                    Hermes Agent                      │
│                                                      │
│  ┌─────────────────────────────────────────────────┐ │
│  │         OpenMemory Plugin                       │ │
│  │                                                  │ │
│  │  • openmemory_profile (Load all memories)       │ │
│  │  • openmemory_search (Semantic search)          │ │
│  │  • openmemory_conclude (Store new memories)     │ │
│  └──────────────────┬──────────────────────────────┘ │
│                     │ HTTP API                        │
└─────────────────────┼─────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────┐
│               OpenMemory Server                      │
│                                                      │
│  ┌─────────────────┐     ┌──────────────────────┐   │
│  │    FastAPI       │────▶│   LLM Integration    │   │
│  │    (Port 8765)   │     │   (OpenRouter/etc.)  │   │
│  └────────┬─────────┘     └──────────────────────┘   │
│           │                                           │
│           ▼                                           │
│  ┌─────────────────┐     ┌──────────────────────┐   │
│  │    Qdrant        │     │   PostgreSQL          │   │
│  │  (Vector Store)  │     │   (Metadaten)        │   │
│  │   (Port 6333)    │     │   (Optional)         │   │
│  └─────────────────┘     └──────────────────────┘   │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## 🛠️ Troubleshooting

### Server not responding

**Error:** `Connection failed. Is OpenMemory running?`

**Solution:**

```bash
# Check if containers are running
docker ps | grep openmemory

# Test API
curl http://localhost:8765/docs

# Check logs
docker logs openmemory-api
docker logs openmemory-qdrant
```

### Plugin not found

**Error:** `hermes memory status` does not show openmemory.

**Solution:**

```bash
# Check if plugin is installed
ls -la ~/.hermes/plugins/memory/openmemory/__init__.py

# Re-install manually
mkdir -p ~/.hermes/plugins/memory/openmemory
git clone https://github.com/TylonHH/hermes-openmemory-plugin.git
cp -r hermes-openmemory-plugin/plugins/memory/openmemory/* ~/.hermes/plugins/memory/openmemory/

# Restart Hermes
hermes /restart
```

### Memory not saving

**Error:** `openmemory_conclude` does not store memories.

**Checklist:**
1. Ensure `OPENMEMORY_API_URL` is set in `~/.hermes/.env`.
2. Ensure `user_id` and `app_id` in `docker-compose.yml` match your settings.
3. Verify Qdrant status at `http://localhost:6333/dashboard`.

### Circuit Breaker active

**Warning:** `Circuit breaker open. API temporarily unavailable.`

**Reason:** After 5 failed API attempts, the plugin pauses calls for 2 minutes to prevent flooding the server.

**Solution:** Wait 2 minutes or restart Hermes.

---

## 📊 Comparison with other Memory Providers

| Feature | OpenMemory | Honcho | Mem0 Cloud | Holographic | Built-in |
|---------|-----------|--------|------------|-------------|----------|
| **Self-Hosted** | ✅ | ✅ | ❌ | ✅ | ✅ |
| **Semantic Search** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **LLM Extraction** | ✅ | ❌ | ✅ | ❌ | ❌ |
| **Vector DB** | Qdrant | pgvector | Qdrant | - | - |
| **Web UI** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Setup Effort** | Medium | Low | None | Low | None |
| **NAS Optimized** | ✅ | ❌ | ❌ | ❌ | ❌ |

---

## 🔗 Links

- **[Hermes Agent](https://github.com/NousResearch/hermes-agent)** - The main project.
- **[OpenMemory Project](https://github.com/mem0ai/mem0/tree/main/openmemory)** - Mem0's self-hosted backend.
- **[Mem0 Documentation](https://docs.mem0.ai/open-source/overview)** - Official docs.
- **[Issues & Feedback](https://github.com/TylonHH/hermes-openmemory-plugin/issues)** - Report bugs or request features.

## 🤝 Contributing

PRs are welcome! We are looking for:

- 🐛 **Bug Fixes**
- 🐳 **New Docker Compose variants** (e.g., with PostgreSQL instead of Qdrant)
- 📝 **Docs improvements**
- 🌍 **Translations**

**How to contribute:**

1. Fork the repo.
2. Create your feature branch (`git checkout -b feature/amazing-stuff`).
3. Commit your changes (`git commit -m 'Add amazing stuff'`).
4. Push to the branch (`git push origin feature/amazing-stuff`).
5. Open a Pull Request.

## 📜 License

This plugin is licensed under the **MIT License** - see [LICENSE](LICENSE) file.
