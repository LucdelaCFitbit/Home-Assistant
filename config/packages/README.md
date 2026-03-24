# Packages Conventions

## Suggested layout
- `packages/core/`: global helpers, templates, notifications
- `packages/rooms/`: room-specific helpers and logic

## Naming
- Prefix files with numeric order when useful:
  - `00_helpers.yaml`
  - `10_notifications.yaml`
- Keep one package responsibility per file.

## Good practices
- Group related entities in the same file.
- Add comments only when behavior is not obvious.
- Keep IDs and entity names stable over time.
