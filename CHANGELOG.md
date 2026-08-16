# Changelog

All CC-Blackbox releases, newest first. Each entry mirrors that version's
[release notes](https://github.com/scott-rippey/cc-blackbox-app/releases).

<!-- Template for each release:

## v1.1.0 — 2026-09-01

### New
- ...

### Improved
- ...

### Fixed
- ...
-->

## v1.0.1 — 2026-08-16

### New
- Add clients directly in Settings: the Client Credentials card gains
  "+ Add client…", so a Vercel account can be connected before any project
  carries that client's tag.

### Improved
- The Client Credentials explainer leads with the browser-based Connect
  flow; typed client names commit (never silently discard) on focus loss.

## v1.0.0 — 2026-08-16

The first public release: a local macOS IDE and flight recorder for Claude
Code. Workspace, editor, and terminals with every session recorded locally
(events, tokens, cost); a live cockpit (Ops Board + Activity EKG); Flight
Playback with the full conversation; the Agent Harness for scheduled
headless agents with budgets, permission envelopes, and worktree isolation;
Reports with per-model cost, cache savings, agent economics, drill-down,
and CSV/PDF export; browser-based per-client Vercel connect; signed,
notarized, auto-updating. Everything stays on your Mac.
