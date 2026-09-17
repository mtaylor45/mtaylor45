<div align="center">

<a href="https://github.com/mtaylor45">
  <img src="/lcars-profile.svg" alt="Mike Taylor — LCARS Profile Console" width="100%">
</a>

<br>

<p>
  <a href="https://github.com/mtaylor45/worldmonitor">
    <img src="https://img.shields.io/badge/WORLD%20MONITOR-Active-FF9900?style=for-the-badge&labelColor=0b0b0b" alt="World Monitor">
  </a>
  <a href="https://github.com/mtaylor45/claude-hud-lcars">
    <img src="https://img.shields.io/badge/LCARS%20TOOLS-Active-9999FF?style=for-the-badge&labelColor=0b0b0b" alt="LCARS Tools">
  </a>
  <a href="https://github.com/mtaylor45/-SpellSight">
    <img src="https://img.shields.io/badge/WAND%20PORTAL-Active-CC99CC?style=for-the-badge&labelColor=0b0b0b" alt="Wand Portal">
  </a>
  <a href="https://github.com/mtaylor45/tauri-plugin-mpv">
    <img src="https://img.shields.io/badge/MPV%20SURFACE-Published-66CCCC?style=for-the-badge&labelColor=0b0b0b" alt="tauri-plugin-mpv-surface">
  </a>
</p>

</div>

---

## 👋 Hello, I'm Mike

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

A self-hosted, always-on situational-awareness dashboard with an LCARS interface, built for a
**two-panel rack kiosk** and controlled by a **fully local voice assistant**. It's a personal fork of
[koala73/worldmonitor](https://github.com/koala73/worldmonitor), extending the upstream dashboard with a
theme architecture, the LCARS visual system, a two-display deployment, and local voice interaction.

The deployment target is two displays driven by one machine: a **2U 1280×400 panel** carrying the data
and a **1U 1424×280 console** carrying navigation, actions and voice. Splitting them is what makes the
data panel readable — the 2U panel is for viewing, the 1U console is for control.

```text
┌─────────────────────────────────────────────────────┐
│               Chromium — one profile                │
│                                                     │
│   ┌──────────────────┐        ┌─────────────────┐   │
│   │  2U dashboard    │        │  1U nav console │   │
│   │  1280×400        │◀──────▶│  1424×280       │   │
│   │                  │ Broad- │                 │   │
│   │  Theme + chrome  │  cast  │  Pages, actions │   │
│   │  Globe / panels  │Channel │  Voice, status  │   │
│   └──────────────────┘        └─────────────────┘   │
│        ▲         │                                  │
│        │         ▼                                  │
│        │   Context Snapshot                         │
└────────┼─────────┼──────────────────────────────────┘
         │WebSocket│ HTTP
┌────────┴─────────┴──────────────────────────────────┐
│                Local Voice Sidecar                  │
│    Wake Word → STT → Local LLM → Actions → TTS      │
└─────────────────────────────────────────────────────┘
```

Three decisions carry most of the design: the **browser doesn't own the audio pipeline** (capture,
wake word, STT and TTS live in the sidecar), the **two displays talk to each other rather than through
the sidecar** (`BroadcastChannel`, so the console survives the voice backend being down), and the
**LLM never reads the DOM** — it consumes a structured context snapshot, so a merge with upstream can't
break the voice layer.

Everything is built. What's left is measurement: sub-3s voice latency on CPU, a trained "Computer"
wake model, alert thresholds against a week of real readings, and a palette legibility test at 2.5 m
on the actual panel.

→ **[Explore World Monitor](https://github.com/mtaylor45/worldmonitor)**

---

### 🖖 Claude HUD LCARS

A Star Trek-inspired operations dashboard for **Claude Code** — skills, agents, hooks, MCP servers,
plugins, memory, sessions and configuration, all searchable and actionable from one LCARS terminal.
A fork of [polyxmedia/claude-hud-lcars](https://github.com/polyxmedia/claude-hud-lcars).

The current line of work is **workspace awareness**: most skills, MCP servers and agent instructions
don't live in `~/.claude/` — they live in your repositories. Register some roots and the dashboard
surfaces project-level config alongside the global set, collapsing per-task worktrees so four active
worktrees don't report the same skills four times.

→ **[Explore Claude HUD LCARS](https://github.com/mtaylor45/claude-hud-lcars)**

---

### 🪄 Wand Portal

IR wand gesture recognition that turns spells into **Home Assistant entities over MQTT**.

Point an IR-illuminated camera at a room, wave a wand with a retroreflective tip, and every recognized
spell pulses a `binary_sensor`. What that sensor does is entirely up to your automations.

```text
IR camera ──► threshold ──► centroid track ──► gesture path
                                                    │
                                    resample 64pts, normalize
                                                    │
                                        cosine match vs templates
                                                    │
                                   MQTT ──► Home Assistant discovery
```

No model to fit: it's a **$1 Recognizer variant with rotation normalization deliberately disabled**,
so an up-stroke and a down-stroke stay different spells. Adding a spell costs one training session and
zero retraining of the others — about five waves, recorded from your phone while standing in the room.

It's an appliance, so recent work has been about surviving being an appliance: CI and a frozen MQTT
contract test, a missing or flapping USB camera that no longer turns `Restart=always` into a boot loop,
bounded tuning parameters, startup config validation that reports every problem at once, and a backup
ring for trained templates.

→ **[Explore Wand Portal](https://github.com/mtaylor45/-SpellSight)**

---

### 🎬 tauri-plugin-mpv-surface

Composite a real **mpv** video surface *beneath* a transparent Tauri v2 webview, so an HTML UI draws
over live, hardware-decoded video with alpha.

Web-shell desktop apps can only play what the embedded browser engine supports — hand it an MKV with
H.265 and it often just refuses. The usual fixes (transcode on the fly, or re-encode the whole library)
both cost a CPU core or a second copy of everything. This hands playback to mpv instead, while the
interface stays ordinary HTML.

Unlike the existing Tauri mpv plugins, it uses libmpv's **render API** rather than `--wid` window
embedding, so **Linux works identically on X11 and Wayland**; libmpv is resolved at runtime with
`dlopen`, so there's no build-time native dependency; and `MpvVideo` implements the `HTMLMediaElement`
surface, so adopting a native decoder is close to a one-line change.

Linux is verified with a headless pixel test that asserts compositing order, geometry, colour and
vertical orientation. Windows and macOS are written and type-checked but **never run on real hardware** —
reports welcome.

→ **[Explore tauri-plugin-mpv-surface](https://github.com/mtaylor45/tauri-plugin-mpv)**

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
![FastAPI](https://img.shields.io/badge/FastAPI-111111?style=flat-square&logo=fastapi&logoColor=009688)
![Tauri](https://img.shields.io/badge/Tauri-111111?style=flat-square&logo=tauri&logoColor=FFC131)
![Vite](https://img.shields.io/badge/Vite-111111?style=flat-square&logo=vite&logoColor=646CFF)
![Playwright](https://img.shields.io/badge/Playwright-111111?style=flat-square&logo=playwright&logoColor=2EAD33)

### AI & Data

![Ollama](https://img.shields.io/badge/Ollama-111111?style=flat-square&logo=ollama&logoColor=white)
![OpenAI](https://img.shields.io/badge/LLMs-111111?style=flat-square&logo=openai&logoColor=white)
![llama.cpp](https://img.shields.io/badge/llama.cpp-111111?style=flat-square&logo=meta&logoColor=0467DF)
![Whisper](https://img.shields.io/badge/faster--whisper-111111?style=flat-square&logo=openai&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-111111?style=flat-square&logo=opencv&logoColor=5C3EE8)
![Three.js](https://img.shields.io/badge/Three.js-111111?style=flat-square&logo=threedotjs&logoColor=white)
![MapLibre](https://img.shields.io/badge/MapLibre-111111?style=flat-square&logo=maplibre&logoColor=white)

### Infrastructure

![Proxmox](https://img.shields.io/badge/Proxmox-111111?style=flat-square&logo=proxmox&logoColor=E57000)
![Docker](https://img.shields.io/badge/Docker-111111?style=flat-square&logo=docker&logoColor=2496ED)
![Linux](https://img.shields.io/badge/Linux-111111?style=flat-square&logo=linux&logoColor=FCC624)
![Home Assistant](https://img.shields.io/badge/Home%20Assistant-111111?style=flat-square&logo=homeassistant&logoColor=18BCF2)
![MQTT](https://img.shields.io/badge/MQTT-111111?style=flat-square&logo=mqtt&logoColor=660066)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-111111?style=flat-square&logo=raspberrypi&logoColor=A22846)
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

The two kiosk panels, the wand camera and the media clients all hang off this — which is why most of
these projects eventually grow a `doctor` command. Every dependency an appliance has fails silently.

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

In practice that's openWakeWord → faster-whisper → an 8B on llama.cpp → a validated action → TTS,
with a deterministic boundary in the middle: the model emits a *claim* about what you wanted, and the
action registry — the same one the buttons use — decides whether it's a thing that exists.

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
HARDWARE / KIOSK      ██████████████████░░
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
| 🌎 **[World Monitor](https://github.com/mtaylor45/worldmonitor)** | LCARS situational-awareness kiosk — two panels, local voice, proactive alerts |
| 🖖 **[Claude HUD LCARS](https://github.com/mtaylor45/claude-hud-lcars)** | LCARS operations dashboard for Claude Code, now workspace-aware |
| 🪄 **[Wand Portal](https://github.com/mtaylor45/-SpellSight)** | IR wand gesture recognition → Home Assistant over MQTT |
| 🎬 **[tauri-plugin-mpv-surface](https://github.com/mtaylor45/tauri-plugin-mpv)** | Real mpv video compositing beneath a transparent Tauri v2 webview |
| 🏠 **Nearby Things** | Family-oriented local discovery / field-guide application |
| 🤖 **Local AI Infrastructure** | Small local models, voice interfaces, and dedicated AI hardware |

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

A few rules that keep showing up across these projects:

- **Fail loudly, or don't fail.** A device with no screen can't tell you it's broken, so it has to say
  so on the way up — and everything downstream of it has to keep running when it doesn't.
- **Keep the fork cheap.** Two upstream projects, three insertions and one deletion between them.
  New behaviour lives in new directories; upstream files get integration seams, not edits.
- **Measure the thing, don't assert it.** The interesting numbers — legibility at 2.5 m, false wakes
  over 24 hours, whether an audio device cancels or merely ducks — only exist on the hardware.

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
