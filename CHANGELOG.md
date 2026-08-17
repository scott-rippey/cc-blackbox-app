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

## v1.0.6 — 2026-08-16

### New
- macOS folder permission handling: picking a folder now triggers the macOS
  permission prompt right away, and every session launch checks folder
  access first. A blocked folder shows a clear dialog naming the path, with
  an Open System Settings button to the exact privacy pane, instead of a
  session that dies instantly with nothing to go on.

### Fixed
- Editor tab names no longer get cut off; tabs fit the full file name and
  the tab strip scrolls.

## v1.0.5 — 2026-08-16

### New
- Credential health monitoring: stored Vercel credentials are verified in
  the background (at launch and before sessions use them). A credential
  Vercel definitively rejects turns red in Settings with an "invalid since"
  date and one notification; sessions stop using the dead token and fall
  back to your own Vercel CLI login. Reconnecting or pasting a fresh token
  restores everything.
- Expanded first-run walkthrough covering the full app: hooks, projects,
  Vercel credentials, the Cockpit and Ops Board, session playback, the
  Agent Harness, and Reports. Replay it anytime from Help, Show Walkthrough.

### Fixed
- A flaky network can no longer mark a good credential as broken; only a
  definitive rejection from Vercel does.

## v1.0.4 — 2026-08-16

### Fixed
- Stuck Connect attempts can be canceled; canceling (or removing the row)
  also invalidates the abandoned browser tab so a late authorization is
  refused.
- Locally added client rows without a stored credential can be removed.

## v1.0.3 — 2026-08-16

### New
- Update announcements: the first launch after an update shows a
  notification with the new version; clicking opens that release's notes.

## v1.0.2 — 2026-08-16

### New
- First-run walkthrough: a spotlight tour over the live interface, shown
  once when a fresh install opens its first workspace; replay from
  Help → Show Walkthrough.

### Fixed
- Per-client credential injection now always wins inside recorded sessions
  (shell-profile exports can no longer override it, and projects without a
  stored credential clear stale exports so the Vercel CLI falls back to
  your real login).
- The Connect flow's waiting hint explains that the account chosen on
  Vercel's page is the one that gets connected.

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
