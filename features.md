# CC-Blackbox: Feature Overview

CC-Blackbox is a local macOS Electron app: a personal IDE and agent harness for
Claude Code. On the surface it is a workspace with an editor and terminals.
Underneath, every Claude Code session (interactive or headless) is recorded to
SQLite (events, tokens, cost), visualized live, and replayable as an animated
timeline. This is the feature list; step-by-step usage lives in
[`user-guide.md`](user-guide.md).

## Workspace & Cockpit

- **Workspaces** are named sets of root folders. The picker opens on launch;
  the last workspace reopens automatically. Hover a picker row to rename or
  remove a workspace; removing keeps its session history.
- **Cockpit layout**: three resizable panes: Explorer (left), Editor over
  Visualizer (middle), Terminals (right). Pane sizes, open tabs, and collapsed
  state persist per workspace, including through app restarts.
- **Quick open (⌘P)** fuzzy-finds any file in the workspace; **workspace search
  (⌘⇧F)** runs full-text search on the bundled ripgrep (honors `.gitignore`).
- **App-wide zoom**: one font-size slider in Settings scales the entire UI.
  The slider previews on the Appearance card only; Apply commits the zoom
  app-wide in one jump.

## Explorer & Editor

- File tree per root with live refresh, gitignored files dimmed, and full
  context menu: open, reveal in Finder, copy path, new file/folder, rename,
  delete, remove root from workspace. Delete asks first (Move to Trash /
  Cancel, Esc or Cancel only) and moves the item to the Trash, so it can be
  put back from Finder; the dialog's "Don't ask again" turns the prompt off
  (Settings -> Appearance turns it back on). Folder rows carry a VS Code
  style disclosure caret: one glyph that rotates as the folder opens. Drag a
  root folder's header up or down to reorder roots; the order persists with
  the workspace.
- **Live activity dots**: while a Claude session runs, files it reads (blue)
  and edits (pink) light up directly in the tree.
- Editor tabs with per-kind rendering: CodeMirror with syntax highlighting for
  code; Edit/Rendered toggle for Markdown; Edit/Tree toggle for JSON; native
  PDF viewer; image and DOCX previews; an info card for binaries.
- Safe editing: ⌘S save, optional autosave, and explicit conflict banners when
  a file is deleted or changed on disk while you have unsaved edits. Large
  files open read-only with a "load anyway" escape hatch.

## Embedded Browser

- A real Chromium browser inside the Cockpit: an **Editor | Browser** toggle
  at the top of the editor panel (**⌘⇧B**). It is not an iframe preview, so
  sites that refuse to be framed (GitHub, most production apps) load
  normally, service workers and cookies behave like a real browser, and dev
  servers over plain http work.
- **One live page per workspace**, with its own persistent cookie profile
  per workspace, so client A's logged-in staging session is never ambient
  while you work on client B. The last URL, panel layout, and Editor/Browser
  choice persist per workspace.
- Chrome: back/forward/reload, a URL bar that accepts shorthand
  (localhost:3000 gets http, bare domains get https) and searches Google
  for anything that is not an address, open in the system
  browser, a **Clear** button that blanks the page out (you are never stuck
  displaying the last thing you opened), and a **Maximize** mode where the same live page fills the whole
  window (Esc restores). Nothing reloads when you maximize, restore, or
  toggle away; the page keeps running.
- **Docked DevTools**: the full Chrome DevTools, side by side with the page
  inside the pane. The split drags and persists.
- **Console & Network capture**: console output, uncaught errors, and a
  request log (method, status, type, duration, size) collected continuously
  into a panel below the page, with level filters and click-to-expand
  stacks. Capture keeps working while DevTools is open.
- Locked down by default: the page gets no special access to the app, and
  every web permission (camera, mic, location, notifications, USB, and the
  rest) is auto-denied, with two ordinary-browsing exceptions: fullscreen
  and sanitized clipboard writes. Only http/https navigation is allowed,
  popups navigate in place, and downloads go through a save dialog. Self-signed
  certificates are accepted only for localhost, only after you confirm, and
  only until the app quits. A crashed page shows a Reload card and never
  takes the app down with it.

## Agentic Browser: your Claude sessions can drive it

- Any Claude session running in the app, a ▶ Claude tab or a `claude` you
  typed into a plain shell, can control the embedded browser: navigate, read
  the page, take a screenshot, click, type, scroll, run JavaScript, wait for
  something to appear, and read the console and network capture. So Claude
  can build a page and then actually check it in the same browser you are
  watching, instead of asking you to look.
- **You approve it per terminal tab.** The first time a session tries to use
  the browser, CC-Blackbox asks once: Allow or Deny. Allow lasts for that
  terminal tab; Deny sticks until you close and reopen the tab. While a
  session is driving, a **Claude driving** pill shows in the browser toolbar
  and pulses on each action. It acts inside that browser's logged-in
  sessions, which is why the consent is per tab and worth thinking about.
- It works through a small local server the app runs; nothing leaves your
  machine. Setup is automatic: the app adds one entry to Claude Code's config
  shortly after launch and re-adds it if Claude Code overwrites it. Outside
  CC-Blackbox that entry stays dormant and never connects. Settings, Browser
  Control (MCP), can remove it for good if you would rather not have it.
  Every action Claude takes is recorded in the session timeline like any
  other tool use, and shows up on the Ops Board.

## Terminals & Recorded Sessions

- **The terminal list is grouped by workspace folder**: a header per
  folder in Explorer order, that folder's shells and sessions underneath,
  and an Other section for anything outside the workspace folders.
- **The terminal list always matches the open workspace.** Tabs for other
  workspaces are tucked away with their shells and sessions running
  untouched, and return the moment you switch back; terminals whose folder
  belongs to no workspace stay visible everywhere.

- **Two kinds of tabs**: plain shells and Claude Code sessions (recorded;
  lamp dot shows live/finished state). A `claude` you type into a plain
  shell records too: every shell carries the recording token, so the moment
  a claude session starts inside it the tab grows the recording lamp, the
  visualizer follows it, and it lands in Sessions with full cost tracking.
  Run claude, exit, run it again and each run is its own recorded session.
  Recording needs the folder to be a tracked project; in an untracked
  folder the app offers **Track this folder?** once and records from the
  next run. Only the recording token is injected; a plain shell never
  receives client credentials. Drag tabs to reorder; the order survives
  window close-and-reopen.
- **▶ Claude** opens the Launch Claude modal: pick a workspace folder or
  any tracked project and launch a dedicated Claude tab. It is a clean
  direct launch, and it is the multi-client Vercel switch: the session
  carries the project's client Vercel account, so each client's work runs
  as that client (a plain shell's typed claude records too, but only ever
  has your own Vercel login). Launching a project outside the open
  workspace adds its folder to the workspace first, so a running terminal
  always has its files in Explorer.
- **Folder-permission preflight (macOS)**: picking a folder deliberately
  triggers the macOS Files-and-Folders permission prompt at pick time, and
  every session launch re-checks access first. A denied folder shows a
  recovery dialog (blocked path + an Open System Settings button to the
  Privacy & Security pane) instead of a session that dies at boot with no
  explanation.
- **Robust claude launch**: the app finds the claude binary itself (known
  install locations, then a login-shell probe). Every claude session
  (interactive tabs and headless Agent Harness runs alike) starts it
  directly, no shell in between, so tmux-exec dotfiles, alias installs,
  PATH gaps, and unusual login shells can't silently break anything. If
  claude isn't installed at all, a dialog says so with a link to get it.
- **Flight recorder**: a session that dies within seconds of launch keeps
  its final terminal output (exit code + ANSI-stripped tail) in the session
  record; Session detail shows it under "Session exited at launch", so a
  boot death is diagnosable after the tab is gone.
- **Track this folder?** Whenever a folder enters the app untracked (new
  workspace root, File → Add Folder, first session launch, agent editor,
  add-directory), one small dialog asks once: track it for reporting, with an
  optional client tag (suggestions prevent typo-minted clients). It shows the
  git repo root that will actually be registered, Enter tracks, Esc skips,
  and skipped folders are remembered so passive paths never nag. A client
  tagged at launch applies to that very session's credentials.
- **Client credentials (the Vercel tie-in)**: each client can hold a Vercel
  credential stored encrypted in the macOS Keychain. **Connect Vercel…**
  opens the browser: sign in (or be already signed in), click Allow, and the
  credential lands in the app assigned to that client: no dashboard, no
  token creation, no pasting. (Pasting a dashboard token remains available
  as the fallback path.) Launch a Claude session on that client's project
  and the token is injected as `VERCEL_TOKEN` (plus `CCB_VERCEL_CLIENT`,
  the client tag, and `CCB_VERCEL_TEAM_ID` for team installs, so a session
  that inspects its environment can tell the token came from CC-Blackbox
  for that client and team, not from a stray shell export; nothing else is
  set, because a lone `VERCEL_ORG_ID` makes the Vercel CLI refuse
  project commands), so Claude can operate Vercel for that client (read logs,
  inspect deployments), authenticated correctly from the first command.
  Scoping is strict: the token reaches only that client's Claude sessions;
  never plain shells (they carry the recording token, nothing else), never
  another client's sessions, never the database.
  The browser handoff is CSRF-bound: the app only accepts a credential for
  a Connect it started moments ago, with a matching one-time state.
  **Credential health**: the app quietly re-verifies stored credentials (at
  launch and before use). If Vercel definitively rejects one, its row turns
  red with an "invalid since" date, you get one notification (click lands
  in Settings), and sessions stop receiving it; they fall back to your own
  Vercel login instead of failing confusingly. Reconnect or paste a fresh
  token and everything resumes. Network blips never mark a credential bad.
- Tabs are folder-aware and titled by folder; an amber dot marks a background
  tab where Claude is waiting for input.

## Session Recording

- Claude Code hooks (installed from Settings with a reviewable diff of
  `~/.claude/settings.json`) stream every event (prompts, tool calls,
  failures, subagent activity, compaction, session end) to the app over a
  local authenticated listener, into SQLite.
- Tokens and cost are computed from the session transcript against a
  user-editable model price table (including split 5m/1h cache-write pricing).
- Hooks are observe-only and env-guarded: outside CC-Blackbox they exit
  instantly and silently, and existing hooks in settings.json are preserved.
- Sessions get auto-generated titles (editable), and crashed sessions are
  detected and marked on next launch.

## Live Visualization

- **Cockpit visualizer** follows the active Claude tab with three views:
  Live Feed (human-readable event stream), Files Touched (read/edit counts),
  and Heatmap (directory tree tinted by activity).
- **Ops Board**: a live node scene showing the orchestrator orb with its current
  verb (reading, editing, running, delegating…), one card per active subagent
  with its own current action, completed-agent chips, a working-set file
  strip, and the agent's last report.
- **Activity EKG**: a waveform of event density colored by category
  (read/edit/bash/other), with markers for prompts, failures, and compaction,
  plus a subagent band and live counters.

## Sessions & Flight Playback

- **Sessions view**: every recorded session with project, client, branch,
  duration, tokens, and cost. Search is instant full-text (SQLite FTS5) over
  everything the session did, plus titles.
- **Session detail**: stat strip, Activity EKG, Ops Board, full event feed,
  and per-file touch counts, all live-updating while the session runs.
- **↻ Resume** continues any finished session's conversation in a new
  recorded tab. Harness runs deep-link to their run report.
- **Delete**: the ✕ on any finished row (or **Delete…** in the detail
  header) removes that session's events, token usage and cost history after
  a confirm; agent run records stay, with their session link cleared. A
  **Delete N blank sessions** button appears whenever ended sessions exist
  where Claude was opened and closed with no prompt typed, and clears them
  in one go. A recording session cannot be deleted. Deleting is per Mac: a
  session synced from another Mac returns on the next sync unless deleted
  there too.
- **Flight Playback**: replay any session as an animated timeline at 1×–100×
  with a scrubber, in a FULL-SCREEN overlay above the app (the view behind is
  dimmed and untouched; ✕ Exit playback or Esc drops you back where you
  were). Views: Ops Board, Feed, Heatmap, and a per-tool swimlane Timeline.
  The Feed interleaves the actual conversation (your prompts and Claude's
  replies, read from the recorded transcript) with the tool events, synced
  to the playback clock, so a replay shows what was said, not just done.

## Agent Harness

- **Agents**: saved headless `claude -p` configurations per project: name,
  prompt template (with `{date}` `{time}` `{project}` `{branch}` slots),
  model and reasoning-effort pickers from a curated catalog, permission
  preset, budget/turn caps, structure, and schedule. Every run is recorded
  like any session.
- **Structure (Auto vs Orchestrator + crew)**: an explicit two-card choice in
  the editor. **Auto** is one prompt; Claude works alone or spins up built-in
  helpers (Explore, Plan, general-purpose) as it sees fit; that is headless
  Claude Code's default behavior. **Orchestrator + crew** keeps the same
  prompt as the orchestrator's task: crew members run as subagents, each
  reports its results back to the orchestrator (which writes the final
  report), and work is routed automatically by each member's description;
  no special coordinator prompt is needed.
- **Subagent crews**: an agent can carry named specialists (e.g. a Fable
  orchestrator delegating to Opus and Sonnet workers). Each subagent gets
  its own description, system prompt, model, effort, tools preset (all tools
  or read-only), and an optional per-subagent sandbox (its own temporary git
  worktree, so parallel edits can't collide), built entirely in the GUI.
  Subagents run in parallel, and the Ops Board attributes every tool call to
  the subagent that made it, by name.
- **First-class prompt editor**: a large CodeMirror editor with markdown
  highlighting, live `{slot}` highlighting, a character counter, and a
  "preview filled prompt" toggle that shows the prompt exactly as it would
  launch right now (today's date, current branch).
- **Visual permission builder**: three presets (Plan only / Edit files /
  Full auto) plus scoped extra abilities (git, npm, any shell, web search,
  web fetch), always summarized in plain English so you know exactly what an
  agent may do before it runs. Permissions are ONE envelope for the whole
  run: the mode and abilities govern the orchestrator and every crew member
  alike (Plan only means nobody edits, whatever a member's tools), and the
  crew cards reflect the active mode live.
- **Budget & turn caps**: an optional per-agent budget cap stops a run when
  the client-side cost estimate reaches it (an estimate, not billing; it works
  on a Claude plan), and max turns (default 50) caps agentic loops.
- **Worktree isolation**: an agent can run in a throwaway git worktree so your
  working files are never touched; you review and merge its changes yourself.
- **Decision logging**: one checkbox makes every process in a run (the
  orchestrator and each crew member) record its significant decisions and
  why, as it goes, through a private logging tool. Nothing about the log
  enters any agent's output or another agent's context, so it never changes
  how the run behaves; it is there for you afterwards. The agent card's
  Decisions button opens a scrolling list of its runs (newest first); each
  run opens into its decisions in the same window, and a run row's Decisions
  button opens straight into that run. Use it to tune a prompt against what
  the agent actually chose. Each logged decision costs one tool turn.
- **Schedules**: manual, daily, or weekly. They fire only while the app is
  open; missed occurrences are skipped. Guards: max 3 concurrent runs,
  30-minute wall-clock kill.
- **Run history with full visibility**: live status with a "what it's doing
  now" line, launch-config pills (model, effort, permission mode, crew size,
  caps; snapshotted at launch, immune to later agent edits), elapsed time,
  cost, the final report rendered as markdown, copy-as-markdown, and worktree
  reveal/remove. **▶ Watch live** jumps from a running row straight into the
  run's live Ops Board and Activity EKG; **View session** does the same after
  it finishes.
- **Robust failure handling**: timeouts, crashes, launch failures, and
  app-closed-mid-run are each reported in plain language; **↻ Re-run**
  relaunches any run with the exact same prompt and config, and an optional
  auto-retry relaunches a failed run once (never loops). A native
  notification fires when a run finishes or fails while the app is
  unfocused or closed to the dock.
- **⚡ Act on run**: resume the run's conversation in a new recorded
  interactive tab (in the worktree or the main repo) with a follow-up
  instruction pre-typed; nothing runs until you press Enter.

## Reports

Two tabs share one scope row and the rolling 5-hour usage chip (approximates
the plan's usage block, scope-independent). The scope row holds range presets
(this month → all time) plus **client and project filters** that govern every
panel below (defaults: global).

- **Cost report**: grouped by client, project, model, day, week, or month,
  with a stacked-by-model chart (stable per-model colors), a token/cost
  table with cache-efficiency (% of prompt tokens served from cache) and
  agent-spend columns, expandable per-model sub-rows showing what caching
  saved in dollars, and CSV export. Per-model rows are priced from
  per-message transcript data; live/unimported sessions appear only in the
  session totals (disclosed in the UI).
- **Drill-down**: click any cost row (or model sub-row) to open the Sessions
  view filtered to exactly the sessions behind that number, with a
  dismissible filter pill.
- **Operations**: agent economics over ALL recorded runs, with KPI tiles (runs,
  success rate, spend, average duration, timeouts, retries, session crashes,
  permission denials) and a per-agent table, plus a friction panel:
  permission requests/denials and compactions grouped by project or time,
  denials ranked by tool, and a session-health line (clean / crashed /
  closed mid-flight).
- **Churn**: the most re-touched files per project, a spec-leak detector
  showing where edits keep landing session after session. Obeys the shared
  range row.
- **Export PDF**: one button renders the whole scoped report (summary
  tiles, spend-over-time chart stacked by model, cost by project, model
  economics with cache savings, agent economics, friction, session health,
  and top churn files) as a paginated A4 PDF (light paper palette, vector
  charts, page numbers) via a hidden print window, then opens it. The
  document header names the scope, so a client- or project-filtered export
  reads as that client's report.
- **Send Feedback** (Help menu): opens a prefilled GitHub issue on the
  public releases repo with the app version and macOS build filled in.
- **Guided walkthrough that drives the app**: a tour over the live UI that
  operates it as it goes. The Cockpit piece by piece (Explorer, editor,
  the Editor|Browser toggle, then the Browser switched on with its toolbar
  explained and how Claude can drive it, the visualizer, terminals, the
  Claude button), Sessions, then the Agent Harness in depth: the tour opens
  the New Agent editor pre-filled with an example and walks every section
  in its real state (Auto vs Orchestrator + crew, the prompt and its slots,
  model and caps, a crew member expanded, Plan only vs Edit files, extra
  abilities, the plain-English summary, worktree and retry, Log decisions,
  schedule, Create), then Run history, Reports, and every Settings card
  (hooks, Browser Control, projects and clients, credentials, model prices,
  database, sync, health). The card sits in one fixed spot with a step
  counter, each stop gets a glow ring, and the app stays fully clickable.
  Fires once, when a fresh install opens its first workspace; existing
  installs never see it uninvited; **Help → Show Walkthrough** or
  **Settings → Appearance → Show walkthrough** replays it any time. Leaving
  the tour at any point puts everything back (the example agent closes
  unsaved, the pane returns to the Editor).

## Settings

- **Hooks**: install/reinstall/uninstall with status detail and a
  side-by-side diff of the settings.json change before anything is applied;
  timestamped backups are written first.
- **Projects**: every tracked folder in one list: client tag inline-editable
  with suggestions, current branch, path, and an add-folder button. The
  manual home for anything the auto-ask skipped; client changes re-attribute
  the project's recorded history retroactively. **Remove…** takes a project
  out of the app for good: its recorded sessions (events, token usage, cost
  history), any transcript copies synced from another Mac, and its agents
  and runs (surviving worktrees removed from disk). The confirm shows the
  exact counts and asks you to type the project name; it is refused while a
  session in that folder is recording or one of its agents is running. The
  folder and your workspace are untouched, and Remove is machine-local:
  sessions synced from another Mac come back on the next sync unless the
  project is removed there too.
- **Client credentials**: per-client Vercel access, with browser-based
  **Connect Vercel…** (or paste a token), Keychain storage, read-only
  verify, remove.
- **Model prices**: the editable $/MTok table (input, output, cache read,
  cache write 5m/1h) behind all cost math; add or remove models freely.
- **Appearance**: the app-wide font-size slider (previews on the card;
  Apply resizes the whole app), the autosave toggle, and
  **Glass background**: the window becomes genuinely transparent so the
  desktop shows through the app, sharp, at a subtle strength that never
  fights readability. Toggling it recreates the window in place: dirty
  editor tabs are saved first (the toggle refuses if a save fails or a file
  has an unresolved disk conflict), and terminals reattach.
- **Database**: observability for the flight recorder, with file + WAL size,
  reclaimable space, per-table row counts, schema version, and the migration
  backups beside the DB; an on-demand integrity check (verbatim
  `PRAGMA quick_check`); **Compact** (VACUUM, offered when there's real
  space to reclaim, refused while anything is recording, a run is queued or
  active, or a Claude session is mid-launch); Reveal in Finder;
  and an opt-in **retention window** (keep everything by default, or
  3/6/12/24 months). Retention sweeps only ended sessions older than the
  cutoff (live sessions and agent-run records are never touched), after a
  confirm that states the exact cutoff date and that cost-report history
  goes with it. Sweeps run daily; heavy operations (check, compact, sweep)
  run in a separate helper process so recording never stalls.
- **Machine Sync**: pull session history from your other Macs over the
  local network. No accounts, no cloud, no extra tooling. One-way per
  direction and pull-only: adding a machine on Mac B copies Mac A's
  finished sessions (history, costs, transcripts for playback) to B and
  never writes to A; set up the reverse on A for both ways. Pairing is a
  one-time 6-digit code shown on the source Mac's screen; after that syncs
  run automatically at launch and hourly while both apps are open, plus
  **Sync now**. Traffic is TLS-encrypted with pinned per-machine
  identities; revoke or remove a machine any time. Synced sessions appear
  everywhere (Sessions, Reports, search, Flight Playback), marked with
  their source machine; only Resume stays on the machine that owns the
  live conversation. A synced session's project folder shows up in
  Settings → Projects as "synced only" until you open that same folder in a
  workspace here (or pick it under Projects); from then on it is a normal
  tracked project on this Mac too, client credentials included.
- **System Health**: the app watching itself. Live memory across every process,
  main-process heap, and event-loop latency, sampled into the flight
  recorder every 5 minutes (kept 30 days) so a slow leak shows up as a
  trend, not a surprise. On every launch the app also checks for crash
  reports the OS wrote since the last check (crash dumps and macOS
  diagnostic reports; advisory non-fatal reports are filtered out) and
  surfaces any it finds with a Reveal button.

## Desktop Behavior

- Closing the window never kills flights: live sessions, schedules, and runs
  continue in the dock; reopening reattaches every terminal. ⌘Q is the real
  shutdown, and when a recording or an agent run is still active it asks
  once before ending them (closing the window instead keeps them going).
- **Auto-updates**: installed builds check this app's release feed at launch
  and every few hours, download in the background, and notify once when a
  version is ready. The update applies on a real quit (⌘Q) or the app menu's
  Restart to Update: never mid-flight, never by surprise. The app menu also
  has Check for Updates for an on-demand check, and About shows the version.
  If a downloaded update ever fails to install, the next launch says so and
  points at the manual download; nothing fails silently. After an update
  installs, the first launch shows a What's New pop-up with that version's
  actual release notes (pulled from the release page; offline shows a link
  instead), once per version. And an app launched
  from outside the Applications folder (Downloads, a build folder) offers to
  move itself there with one click, so updates always reach the copy you
  actually run.
- **Crash recovery**: failures are contained instead of silent. If the
  window's renderer crashes or is force-killed, the app recreates the
  window on the spot (terminals and recordings live outside it and
  survive); if the window keeps crashing it stops retrying and says so
  while sessions and runs continue in the background. If the window stops
  responding for a while, a dialog offers Keep Waiting or Reload Window
  (reload recovers the window; unsaved editor changes are lost, which the
  dialog says up front). If the app itself hits a fatal error, it saves
  what it can, relaunches itself once or twice, and otherwise stays down
  rather than looping; crash evidence is captured locally and surfaced on
  the Health card at the next launch. Claude sessions that were running
  when the app closed, whether by crash or a normal quit, are offered for
  resume on the next launch, one click each or all at once, with their
  conversations intact (Claude keeps them on disk continuously).
- Native notifications fire when Claude finishes or needs input while the
  window is unfocused; in-app, the tab gets an amber dot instead.
- Links in any rendered content (markdown, DOCX, agent reports) open in the
  system browser; the window itself never navigates.
- All data lives locally in `~/Library/Application Support/CC-Blackbox/ccb.db`.
  Nothing leaves the machine, and the database snapshots itself to a
  timestamped backup before any schema upgrade (newest 5 kept).
