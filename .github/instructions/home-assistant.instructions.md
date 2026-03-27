---
applyTo: "**/*.{yaml,yml}"
---

You are working in a Home Assistant workspace.

Terminology:
- "HA" means "Home Assistant".

Use this context before answering:
- Hardware: Home Assistant Green
- Install method: Home Assistant OS
- Core: 2026.3.2
- Supervisor: 2026.03.0
- OS: 17.1
- Frontend: 20260312.0
- Companion app: 2026.3.2-full (20802)
- TV: Samsung The Frame (has Art Mode)
- Art Mode behavior: TV still draws non-zero power in Art Mode; treat Art Mode separately from true TV-watching ON state.

Authoring rules for YAML:
- Produce valid YAML with consistent indentation.
- Prefer explicit keys and readable naming.
- Use Home Assistant-native patterns first.
- Use action: instead of service: for script and automation action calls.
- Include minimal comments only when they add clarity.
- If editing existing YAML, preserve current style and key ordering where possible.

Operational rules:
- Distinguish client-device/network tasks vs Home Assistant tasks.
- Flag reload/restart requirements when relevant.
