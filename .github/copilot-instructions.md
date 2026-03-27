# Workspace Context: Home Assistant Lab

Always consider this workspace as a Home Assistant environment running on Home Assistant Green.

## Terminology
- "HA" means "Home Assistant".

## Fixed Environment Facts
- Hardware: Home Assistant Green
- Installation method: Home Assistant OS
- Home Assistant Core: 2026.3.2
- Supervisor: 2026.03.0
- Operating System: 17.1
- Frontend: 20260312.0
- Companion app: 2026.3.2-full (20802)
- TV: Samsung The Frame (Art Mode in use)
- Art Mode behavior: non-zero power draw is expected in Art Mode; distinguish Art Mode from true TV-watching ON state.

## Response Rules For This Workspace
- Prioritize Home Assistant best practices for YAML scripts, automations, helpers, and dashboard configuration.
- Use action: instead of service: for script and automation action calls.
- Prefer solutions compatible with Home Assistant OS and Supervisor-managed workflows.
- When giving commands, clearly separate client-device/network actions from Home Assistant-level actions.
- If a suggestion depends on version-specific behavior, validate against the fixed versions above.
- Keep answers practical, step-by-step, and ready to paste when possible.

## Safety / Accuracy
- Do not invent entities, integrations, or file paths if they are unknown.
- If required data is missing, ask only the minimal clarifying questions.
