# OpenMemory Plugin for Hermes Agent

<p align="center">
  <b>🏠 Self-hosted semantic memory for AI Agents</b><br>
  Run Mem0 on your NAS/VPS — no cloud dependency, full data control
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Plugin-Memory-orange?style=for-the-badge&logo=openai">
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

> **[Screenshot einfügen: OpenMemory Web UI]**
> *Das Dashboard zeigt alle gespeicherten Memories, Kategorien und Statistiken*

*(Ersetze dies mit einem Screenshot von `http://your-nas:3001`)*

### Terminal Output

```
╭─ ⚕ Hermes ───────────────────────────────────────────────────────────────╮
│  Hallo René! Es freut mich zu hören, dass du OpenMemory auf deinem       │
│  NAS verwendest.                                                         │
╰──────────────────────────────────────────────────────────────────────────╯
  ┊ 🧠 memory +user: "User's name is René. Uses OpenMemory on their NAS."
```

---

## ✨ Warum OpenMemory?

| Feature | Mem0 Cloud Plugin | OpenMemory Plugin (dieses Repo) |
|---------|-------------------|----------------------------------|
| **Datenhaltung** | Externe Cloud | **100% Self-Hosted** |
| **Kosten** | Pro API-Call | **Nur Infrastruktur (optional free)** |
| **NAS Support** | ❌ Nein | **✅ Optimiert** (Synology, QNAP, TrueNAS) |
| **Free Models** | ❌ Nein | **✅ Ja** (OpenRouter Free Tier) |
| **Web UI** | ❌ Kein Zugang | **✅ Integriert** |
| **Datenschutz** | Daten auf Servern | **Complett lokal** |
| **Offline** | ❌ Benötigt Internet | **✅ Optional** |

### Für wen ist dieses Plugin?

- 🏠 **Homelab-User** die volle Kontrolle über ihre Daten wollen
- 💰 **Kostenbewusste** User die keine Cloud-Gebühren zahlen wollen
- 🔒 **Privacy-Fokus** User die keine Daten an Dritte senden wollen
- 🌐 **NAS-Besitzer** (Synology, QNAP, TrueNAS) die Docker nutzen

---

## 🚀 Quick Start (3 Minuten)

### Voraussetzungen

- Docker + Docker Compose installiert
- Hermes Agent installiert ([hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com))

### Schritt 1: Plugin installieren

```bash
hermes plugins install https://github.com/TylonHH/hermes-openmemory-plugin
```

**Manuelle Installation:**

```bash
mkdir -p ~/.hermes/plugins/memory/openmemory
git clone https://github.com/TylonHH/hermes-openmemory-plugin.git
cp -r hermes-openmemory-plugin/plugins/memory/openmemory/* ~/.hermes/plugins/memory/openmemory/
rm -rf hermes-openmemory-plugin
```

### Schritt 2: OpenMemory Server starten

**Standard Setup:**

```bash
cd ~/.hermes/plugins/memory/openmemory
docker compose -f docker-compose.example.yml up -d
```

**NAS Setup** (Synology, QNAP, etc.):

```bash
# Anpassen: /volume1/ zu deinem NAS Volume
nano docker-compose-nas.yml
docker compose -f docker-compose-nas.yml up -d
```

### Schritt 3: Hermes konfigurieren

```bash
echo 'OPENMEMORY_API_URL=http://localhost:8765' >> ~/.hermes/.env

# Für NAS:
# echo 'OPENMEMORY_API_URL=http://diskstation:8765' >> ~/.hermes/.env

hermes config set memory.provider openmemory
hermes /restart
```

### ✅ Verify Installation

```bash
hermes memory status
```

**Erwartete Ausgabe:**

```
Memory status
────────────────────────────────────
  Built-in:  always active
  Provider:  openmemory

  Plugin:    installed ✓
  Status:    available ✓
```

---

## 🌐 Web UI Dashboard

Nach dem Start ist das Dashboard erreichbar unter:

```
http://dein-server:3001
```

**Features der Web UI:**
- 📋 Alle gespeicherten Memories ansehen
- 🔍 Memories durchsuchen und filtern
- 📊 Statistiken und Nutzungsdaten
- ⚙️ Einstellungen verwalten

---

## 💾 NAS Deployment

Das Plugin ist speziell für NAS-Systeme optimiert:

### Synology DSM

1. Öffne **Container Station**
2. Erstelle ein neues Projekt
3. Füge den Inhalt von `docker-compose-nas.yml` ein
4. Passe die Pfade an:
   ```yaml
   volumes:
     - /volume1/docker/openmemory/qdrant:/qdrant/storage
   ```

> 💡 **Tipp:** Stelle sicher, dass der Docker Benutzer Schreibrechte auf `/volume1/docker/` hat.

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

## 💰 Kosten optimieren

OpenMemory benötigt ein LLM für die Memory-Verarbeitung. Mit **OpenRouter** kannst du das komplett kostenlos betreiben:

### Free Models (OpenRouter)

| Model | Provider | Limit |
|-------|----------|-------|
| `meta-llama/llama-3.1-8b-instruct:free` | Meta | 8K tokens |
| `google/gemini-flash-1.5:free` | Google | 1M tokens |
| `mistralai/mistral-7b-instruct:free` | Mistral | 8K tokens |

### Konfiguration

In `docker-compose.yml`:

```yaml
environment:
  - OPENAI_API_BASE=https://openrouter.ai/api/v1
  - OPENAI_API_KEY=sk-or-v1-***
  - LLM_MODEL=meta-llama/llama-3.1-8b-instruct:free
```

**Ergebnis:** $0 monatliche Kosten für Memory-Operationen! 🎉

---

## 🏗️ Architektur

```
┌─────────────────────────────────────────────────────┐
│                    Hermes Agent                      │
│                                                      │
│  ┌─────────────────────────────────────────────────┐ │
│  │         OpenMemory Plugin                       │ │
│  │                                                  │ │
│  │  • openmemory_profile (Alle Memories laden)     │ │
│  │  • openmemory_search (Semantische Suche)        │ │
│  │  • openmemory_conclude (Neue Memories speichern)│ │
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

### Server antwortet nicht

**Problem:** `Connection failed. Is OpenMemory running?`

**Lösung:**

```bash
# Prüfen ob Container laufen
docker ps | grep openmemory

# API testen
curl http://localhost:8765/docs

# Logs checken
docker logs openmemory-api
docker logs openmemory-qdrant
```

### Plugin wird nicht erkannt

**Problem:** `hermes memory status` zeigt openmemory nicht

**Lösung:**

```bash
# Prüfen ob Plugin installiert ist
ls -la ~/.hermes/plugins/memory/openmemory/__init__.py

# Falls nicht, manuell installieren
mkdir -p ~/.hermes/plugins/memory/openmemory
git clone https://github.com/TylonHH/hermes-openmemory-plugin.git
cp -r hermes-openmemory-plugin/plugins/memory/openmemory/* ~/.hermes/plugins/memory/openmemory/
```

### Memories werden nicht gespeichert

**Problem:** `openmemory_conclude` speichert keine Memories

**Lösung:**

1. Prüfe ob `OPENMEMORY_API_URL` in `~/.hermes/.env` gesetzt ist
2. Prüfe ob `user_id` und `app_id` in der `docker-compose.yml` korrekt sind
3. Schau in die Qdrant UI unter `http://localhost:6333/dashboard`

### Circuit Breaker aktiv

**Problem:** `Circuit breaker open. API temporarily unavailable.`

**Erklärung:** Nach 5 fehlgeschlagenen API Calls pausiert das Plugin für 2 Minuten.

**Lösung:** Warte 2 Minuten oder starte Hermes neu:

```bash
hermes /restart
```

---

## 📊 Vergleich mit anderen Memory Providern

| Feature | OpenMemory | Honcho | Mem0 Cloud | Holographic | Built-in |
|---------|-----------|--------|------------|-------------|----------|
| **Self-Hosted** | ✅ | ✅ | ❌ | ✅ | ✅ |
| **Semantische Suche** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **LLM Extraktion** | ✅ | ❌ | ✅ | ❌ | ❌ |
| **Vector DB** | Qdrant | pgvector | Qdrant | - | - |
| **Web UI** | ✅ | ❌ | ❌ | ❌ | ❌ |
| **Setup** | Mittel | Einfach | Einfach | Einfach | Keins |
| **Kosten** | Infrastruktur | Infrastruktur | Pro-Call | Frei | Frei |
| **NAS Optimiert** | ✅ | ❌ | ❌ | ❌ | ❌ |

---

## 🔗 Links

- **[Hermes Agent](https://github.com/NousResearch/hermes-agent)** - Das Hauptprojekt
- **[OpenMemory Projekt](https://github.com/mem0ai/mem0/tree/main/openmemory)** - Mem0's Self-Hosted Version
- **[Mem0 Dokumentation](https://docs.mem0.ai/open-source/overview)** - Offizielle Mem0 Docs
- **[Issues & Feedback](https://github.com/TylonHH/hermes-openmemory-plugin/issues)** - Bugs melden, Features vorschlagen

## 🤝 Contributing

PRs sind herzlich willkommen! Besonders gesucht:

- 🐛 **Bug Fixes** - Wenn etwas nicht funktioniert
- 🐳 **Neue Docker Compose Varianten** - z.B. mit PostgreSQL statt nur Qdrant
- 📝 **Docs Verbesserungen** - Mehr Beispiele, bessere Erklärungen
- 🌍 **Übersetzungen** - README in anderen Sprachen
- 🧪 **Tests** - Mehr Testabdeckung

**Wie du beitragen kannst:**

1. Fork das Repository
2. Erstelle einen Feature Branch (`git checkout -b feature/mein-feature`)
3. Committe deine Änderungen (`git commit -m 'feat: add something cool'`)
4. Pushe den Branch (`git push origin feature/mein-feature`)
5. Öffne einen Pull Request

## 📜 License

Dieses Plugin ist unter der **MIT License** lizenziert - siehe [LICENSE](LICENSE) Datei.

---

<p align="center">
  <b>Mit ❤️ gebaut für die Open-Source AI Community</b><br>
  <a href="https://github.com/TylonHH">TylonHH</a> · 
  <a href="https://github.com/NousResearch/hermes-agent">Hermes Agent</a> · 
  <a href="https://github.com/mem0ai/mem0">Mem0</a>
</p>
