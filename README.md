# OpenMemory Plugin for Hermes Agent

<p align="center">
  <b>🏠 Self-hosted semantic memory for AI Agents</b><br>
  Run Mem0 on your NAS/VPS — no cloud dependency
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Plugin-Memory-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Docker-Ready-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/NAS-Compatible-green?style=for-the-badge">
</p>

## ✨ Features

| Feature | OpenMemory Cloud | OpenMemory Plugin |
|---------|-----------------|-------------------|
| Kosten | Pro API-Call | **Nur Infrastruktur** |
| Datenschutz | Cloud | **100% Self-Hosted** |
| NAS Support | ❌ | **✅** |
| Free Models | ❌ | **✅** (OpenRouter) |
| Web UI | ✅ | **✅** |

## 🚀 Quick Start (3 Minuten)

### 1. Plugin installieren

```bash
hermes plugins install https://github.com/TylonHH/hermes-openmemory-plugin
```

### 2. OpenMemory starten

```bash
docker compose -f docker-compose.example.yml up -d
```

### 3. Hermes konfigurieren

```bash
hermes config set memory.provider openmemory
echo "OPENMEMORY_API_URL=http://localhost:8765" >> ~/.hermes/.env
hermes /restart
```

**Done!** ✅

## 💾 NAS Deployment

Für Synology/QNAP/TrueNAS:

```bash
docker compose -f docker-compose-nas.yml up -d
```

Passe `/volume1/` in der Datei an dein NAS an.

## 🌐 Web UI

Nach dem Start erreichbar unter:
**http://your-server:3001**

## 💰 Kosten optimieren

**OpenRouter Free Tier:**

- `meta-llama/llama-3.1-8b-instruct:free`
- `google/gemini-flash-1.5:free`
- **Komplett kostenlos!**

## 🛠️ Troubleshooting

**Server antwortet nicht?**

```bash
curl http://localhost:8765/docs
docker ps | grep openmemory
```

**Plugin nicht erkannt?**

```bash
hermes memory status
ls ~/.hermes/plugins/memory/openmemory/
```

## 🔗 Links

- [Hermes Agent](https://github.com/NousResearch/hermes-agent)
- [OpenMemory Projekt](https://github.com/mem0ai/mem0/tree/main/openmemory)
- [Issues & Feedback](https://github.com/TylonHH/hermes-openmemory-plugin/issues)

## 🤝 Contributing

PRs welcome! Besonders:

- Bug Fixes
- Neue Docker Compose Varianten
- Docs Verbesserungen
- Übersetzungen
