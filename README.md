<div align="center">

<a href="https://github.com/mtaylor45">
  <img src="./assets/profile-header.svg" alt="Michael Taylor — Software, Systems & Intelligence" width="100%">
</a>

<br>

<p>
  <a href="https://github.com/mtaylor45/worldmonitor">
    <img src="https://img.shields.io/badge/WORLD%20MONITOR-Active-FF9900?style=for-the-badge&labelColor=0b0b0b" alt="World Monitor">
  </a>
  <a href="https://github.com/mtaylor45/claude-hud-lcars">
    <img src="https://img.shields.io/badge/LCARS%20TOOLS-Active-9999FF?style=for-the-badge&labelColor=0b0b0b" alt="LCARS Tools">
  </a>
</p>

</div>

---

## 👋 Hello, I'm Michael

I'm a software builder who likes putting **software, data, AI, and physical infrastructure together into useful systems**.

My projects tend to live at the intersection of:

- 🧠 **Local AI & LLMs** — practical, private AI running on hardware I control
- 🌍 **Situational awareness** — geospatial data, news, infrastructure, signals, and visualization
- 🖥️ **Interfaces** — information-dense dashboards designed to be understood at a glance
- 🏠 **Self-hosted infrastructure** — Proxmox, Docker, networking, observability, and home-lab automation
- 🛠️ **Developer tooling** — tools that make complex development environments easier to see and operate
- 🎨 **Design systems** — especially interfaces that have a strong visual language rather than looking like generic dashboards

> **My favorite kind of project is one where the software eventually becomes a system.**

---

## 🚀 What I'm Building

### 🌎 World Monitor — LCARS Edition

A self-hosted situational-awareness dashboard built around the World Monitor codebase and redesigned as an **LCARS operations console**.

It is being developed toward an always-on display with a future **local voice interface**, using a dedicated Intel NUC and local AI.

**Current architecture:**

```text
┌─────────────────────────────────────────────────────────────┐
│                    LCARS WORLD MONITOR                      │
│                                                             │
│   Geospatial Data ──┐                                       │
│   News & Signals ───┼──► Dashboard ──► Kiosk Display        │
│   Infrastructure ───┘          │                            │
│                                ▼                            │
│                       Context Snapshot                      │
│                                │                            │
└────────────────────────────────┼────────────────────────────┘
                                 │
                                 ▼
                    Local Voice / AI Sidecar
                                 │
              Wake Word → STT → LLM → Actions → TTS
```

The goal isn't simply a dashboard that *looks* like LCARS. The architecture treats the interface as an **instrumentation system**: structured state, semantic actions, voice-accessible controls, and a visual language designed for glanceable information.

→ **[Explore World Monitor](https://github.com/mtaylor45/worldmonitor)**

---

### 🖖 Claude HUD LCARS

A Star Trek-inspired operations dashboard for **Claude Code**.

It turns the Claude Code environment into a searchable LCARS terminal covering skills, agents, hooks, MCP servers, plugins, memory, sessions, configuration, and more.

→ **[Explore Claude HUD LCARS](https://github.com/mtaylor45/claude-hud-lcars)**

---

## 🧰 Things I Like Working With

<div align="center">

### Software

![TypeScript](https://img.shields.io/badge/TypeScript-111111?style=flat-square&logo=typescript&logoColor=3178C6)
![JavaScript](https://img.shields.io/badge/JavaScript-111111?style=flat-square&logo=javascript&logoColor=F7DF1E)
![React](https://img.shields.io/badge/React-111111?style=flat-square&logo=react&logoColor=61DAFB)
![Node.js](https://img.shields.io/badge/Node.js-111111?style=flat-square&logo=node.js&logoColor=5FA04E)
![Python](https://img.shields.io/badge/Python-111111?style=flat-square&logo=python&logoColor=3776AB)
![Rust](https://img.shields.io/badge/Rust-111111?style=flat-square&logo=rust&logoColor=DEA584)

### AI & Data

![Ollama](https://img.shields.io/badge/Ollama-111111?style=flat-square&logo=ollama&logoColor=white)
![OpenAI](https://img.shields.io/badge/LLMs-111111?style=flat-square&logo=openai&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-111111?style=flat-square&logo=threedotjs&logoColor=white)
![MapLibre](https://img.shields.io/badge/MapLibre-111111?style=flat-square&logo=maplibre&logoColor=white)

### Infrastructure

![Proxmox](https://img.shields.io/badge/Proxmox-111111?style=flat-square&logo=proxmox&logoColor=E57000)
![Docker](https://img.shields.io/badge/Docker-111111?style=flat-square&logo=docker&logoColor=2496ED)
![Linux](https://img.shields.io/badge/Linux-111111?style=flat-square&logo=linux&logoColor=FCC624)
![GitHub](https://img.shields.io/badge/GitHub-111111?style=flat-square&logo=github&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-111111?style=flat-square&logo=grafana&logoColor=F46800)

</div>

---

## 🛰️ The Home Lab

A significant part of my development environment is also a small self-hosted infrastructure platform.

```text
                         ┌──────────────────┐
                         │     INTERNET     │
                         └────────┬─────────┘
                                  │
                         ┌────────▼─────────┐
                         │     UniFi        │
                         │  Gateway / LAN   │
                         └────────┬─────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
              ┌─────▼─────┐               ┌────▼─────┐
              │  Proxmox  │               │   NAS    │
              │  Cluster  │               │  Media   │
              └─────┬─────┘               └──────────┘
                    │
        ┌───────────┼───────────┐
        │           │           │
   ┌────▼────┐ ┌────▼────┐ ┌────▼────┐
   │ Compute │ │ Compute │ │ Compute │
   │  Node   │ │  Node   │ │  Node   │
   └─────────┘ └─────────┘ └─────────┘
                    │
              ┌─────▼─────┐
              │   Docker  │
              │   Swarm   │
              └───────────┘
```

I'm particularly interested in making infrastructure **observable, reproducible, and useful** rather than treating the home lab as a collection of machines.

---

## 🤖 Local AI Is the Direction

I'm especially interested in AI that can run **locally and continuously**.

That means exploring architectures where small, purpose-built models handle specific jobs rather than sending every interaction to a cloud API.

A typical local interaction might eventually look like:

```text
             ┌──────────────┐
             │    Person    │
             └──────┬───────┘
                    │ voice
                    ▼
             ┌──────────────┐
             │  Wake Word   │
             └──────┬───────┘
                    ▼
             ┌──────────────┐
             │     STT      │
             └──────┬───────┘
                    ▼
             ┌──────────────┐
             │ Local Small  │
             │     LLM      │
             └──────┬───────┘
                    │
             ┌──────▼───────┐
             │ Action /     │
             │ Context Bus  │
             └───┬─────┬────┘
                 │     │
          ┌──────▼─┐ ┌─▼──────┐
          │ Query  │ │ Action │
          │ System │ │ System │
          └────────┘ └────────┘
```

The interesting part isn't making an AI chatbot.

It's making **an AI that understands the system it lives inside**.

---

## 🎯 Current Interests

```text
LOCAL AI              ████████████████████
SYSTEMS / INFRA       ███████████████████░
DATA VISUALIZATION    ██████████████████░░
DEVELOPER TOOLS       █████████████████░░░
UI / DESIGN SYSTEMS   ████████████████░░░░
SELF-HOSTING          ███████████████████░
```

I'm particularly interested in:

- Local-first AI architectures
- Agentic developer tooling
- Geospatial visualization
- OSINT and situational-awareness systems
- Self-hosted infrastructure
- Home automation
- Observability
- Kiosk and ambient interfaces
- Data-rich UI/UX
- Hardware/software integration

---

## 📌 Featured Projects

| Project | Description |
|---|---|
| 🌎 **[World Monitor](https://github.com/mtaylor45/worldmonitor)** | LCARS-themed situational-awareness dashboard with a path toward local voice interaction |
| 🖖 **[Claude HUD LCARS](https://github.com/mtaylor45/claude-hud-lcars)** | LCARS operations dashboard for Claude Code |
| 🏠 **Nearby Things** | Family-oriented local discovery / field-guide application |
| 🤖 **Local AI Infrastructure** | Experiments with small local models, voice interfaces, and dedicated AI hardware |

---

## 🧭 Philosophy

I like software that **earns its complexity**.

A good system should be:

```text
Useful
  ↓
Observable
  ↓
Understandable
  ↓
Automatable
  ↓
Eventually… conversational
```

And I have a soft spot for software that makes you feel like you're operating a starship.

---

<div align="center">

### ✦ BUILD SYSTEMS. MAKE THEM VISIBLE. KEEP THEM YOURS. ✦

<br>

<a href="https://github.com/mtaylor45">
  <img src="https://img.shields.io/github/followers/mtaylor45?label=FOLLOW&style=for-the-badge&labelColor=0b0b0b&color=9999FF" alt="GitHub followers">
</a>
<a href="https://github.com/mtaylor45?tab=repositories">
  <img src="https://img.shields.io/badge/REPOSITORIES-VIEW-FF9900?style=for-the-badge&labelColor=0b0b0b" alt="Repositories">
</a>

</div>
