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

## v2.0.4 — 2026-08-19

### New
- Opening a workspace starts one shell per folder, in explorer order; the
  terminal list begins one-to-one with the folder list, on cold start and on
  workspace switches, without duplicating folders that already have one.

### Changed
- Removed the reopen-last-shells behavior; the one-per-folder rule replaces
  it.

## v2.0.3 — 2026-08-19

### New
- The terminal list is grouped by folder: a section per workspace folder in
  explorer order, with an Other section for terminals outside them.

### Fixed
- Terminal tab drags stay within their folder section; no more drop targets
  that do nothing.

## v2.0.2 — 2026-08-19

### Fixed
- Launching a session for a project outside the current workspace shows its
  terminal immediately (Other projects, Resume, Act on run).
- Switching back to a workspace always lands on a terminal instead of a
  blank panel.
- Each workspace remembers only its own shells for reopen.
- The Dock icon reopens the window during a PDF export.
- Fixed a crash that could appear on quit with terminals open.

## v2.0.1 — 2026-08-19

### New
- The app keeps itself in the right place: launched from Downloads or a disk
  image, it offers a one-click move to Applications and relaunches from
  there, so updates always reach the copy you actually use.
- A failed update says so on the next launch, with a manual download link.
- Cmd-Q asks once before stopping a live recording or agent run; closing
  the window still keeps everything running in the dock.
- Terminal tabs follow the open workspace; other workspaces' terminals keep
  running in the background and return when you switch back.

## v2.0.0 — 2026-08-18

### Reliability (hardening audit of macOS behavior: quit, crash, power loss, updates, sandbox)
- The update crash-loop breaker now actually works: repeated install-time
  crashes pause automatic updates and point at a manual download (the
  detection previously cleared a moment too early and could never trigger).
  Quitting mid-download no longer miscounts toward it; the "update ready"
  notice fires once per version.
- The database is durable across power loss and kernel panics, not just app
  crashes (synced at every commit).
- The last half-second of layout/window changes survives an immediate quit.
- Reinstalling the recording hook can no longer garble a hook a live session
  is executing (atomic swap).
- A recreated login keychain (password reset, migrated Mac) no longer wedges
  token storage: Settings detects it and offers a one-click reset that keeps
  the old file as a backup.
- Both windows run inside the macOS renderer sandbox; an unneeded
  hardened-runtime entitlement that weakened library validation was removed.
- Files with # or ? in their names preview correctly.
- Agent Harness: a hung worktree checkout is killed cleanly with every
  process it started; a partial checkout that cannot be deleted safely right
  then is cleaned up automatically later instead of blocking future runs.
- Clicking the Vercel connect link during app launch no longer risks a
  startup error.

## v1.0.10 — 2026-08-18

### Fixed
- Typing /resume inside a tab no longer stops the recording: it rolls into
  a fresh session exactly like /clear does (was: everything after went
  unrecorded and unbilled).
- /clear can no longer misfile the previous conversation onto the new
  session's transcript; each session is bound to its own transcript and a
  late-arriving end-of-session event can never overwrite its replacement.
- Agent runs queued during database maintenance no longer stay queued
  forever (and no longer block that agent's schedule); a run caught
  mid-launch is requeued rather than failed.
- Agent runs launch the same direct way interactive tabs do, so tmux-on-
  login, nushell, and pwsh setups no longer produce runs that hang thirty
  minutes and time out with no explanation.
- Update safety: quitting mid-download is no longer miscounted as a crash;
  a network hiccup during a manual check can no longer un-pause updates
  paused for a real crash.
- Compact is refused while a Claude session is still launching, not only
  while one is recording.
- A reconnected Vercel token can no longer be marked invalid by a check that
  started before you connected it.
- Agent Harness display: "View run report" always lands on the run; the
  delete-agent confirmation reports the real run count; the agent card shows
  "Launching" instead of a dead "Watch live" button while a run starts.

## v1.0.9 — 2026-08-18

### Fixed
- Session detail settles when a session ends (no more permanent "live"
  header until you leave and return).
- Editor tab names are never clipped; the tab strip hides its scrollbar and
  keeps the active tab in view.
- Session launches are never silently dead: moved or deleted folders,
  database maintenance, and other failures all show a clear message.
- Notifications fire in dock-only mode (window closed).
- Reports bucket days, weeks, and months in your local timezone; the
  permission-denials count is no longer undercounted.
- The walkthrough scrolls its target into view; the track-folder question
  can no longer open invisibly behind the tour.
- Agent Harness: Re-run honors the original run's worktree setting, empty
  schedule times are rejected at save, failed worktree checkouts clean up,
  and scheduled runs wait while database maintenance holds the lock.
- Model price edits can't leak between rows, quick-open search can't show
  stale results, PDF export failures say why, and running from the disk
  image warns you to move the app to Applications.

## v1.0.8 — 2026-08-18

### Fixed
- Using /clear no longer stops recording: it cleanly closes the current
  recorded session and starts a fresh one in the same tab.
- Long sessions no longer freeze the live session detail view.
- Unsaved editor changes and terminal scrollback survive switching views.
- Settings hook and credential actions show their errors instead of doing
  nothing visibly.
- Resumed sessions (Resume, Act on run) no longer double-count the original
  conversation's cost in Reports.
- Update crash protection: if the built-in updater crashes the app twice in
  a row, the app stops trying, tells you, and points at the manual
  download. A failed Restart to Update also says so.
- Quick sessions no longer show an "exited at launch" banner; agent runs
  that fail at launch notify and honor auto-retry; a session's final events
  can no longer be lost to a race when the tab closes.

## v1.0.7 — 2026-08-17

### New
- Robust claude launch: the app finds the claude binary itself and starts
  session tabs directly, no shell in between, so tmux auto-attach dotfiles,
  alias installs, PATH gaps, and unusual login shells can't silently break a
  session. A missing Claude Code install gets a clear dialog with a link.
- Flight recorder: a session that dies within seconds of launch keeps its
  final terminal output and exit code, shown in the session's detail view.
- Proxy-proof recording: the hook script bypasses proxy settings for its
  local delivery. If Settings shows an amber "outdated" hook script state
  after updating, click Reinstall hooks once.

### Fixed
- Agent Harness scheduling: a failed launch no longer stalls the remaining
  queue, and canceling a run during its launch moment is honored and
  recorded as a cancel.

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
