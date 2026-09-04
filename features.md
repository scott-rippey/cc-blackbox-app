# CC Blackbox: Feature Overview

CC Blackbox is a local macOS Electron app: a personal IDE and agent harness for
Claude Code. On the surface it is a workspace with an editor and terminals.
Underneath, every Claude Code session (interactive or headless) is recorded to
SQLite (events, tokens, cost), visualized live, and replayable as an animated
timeline. This is the feature list; step-by-step usage lives in
[`user-guide.md`](user-guide.md).

## Workspace & Cockpit

- **Workspaces** are named sets of root folders. The picker opens on launch;
  the last workspace reopens automatically. Hover a picker row to rename or
  remove a workspace; removing keeps its session history.
- **Cockpit layout**: seven movable panes (Explorer, Editor, Visualizer,
  Terminal, Terminals list, Browser, Simulator) in a resizable dock. The
  default is Explorer (left), Editor with Browser and Simulator as its tabs
  over the Visualizer (middle), Terminal and its list (right). Drag any pane
  by its title tab: drop on the edge of another pane to split, onto its tab
  strip or its middle to stack them as tabs, or on the outer edge of the
  Cockpit for a new column or row.
  The ✕ on a title hides a pane; the **View** menu shows it again, and **View
  → Reset Cockpit Layout** (also in Settings → Appearance) restores the
  default. Editor file tabs drag too: drop one on another pane's edge for a
  side-by-side editor, and the extra editor closes itself when its last file
  does. Panes are never rebuilt by a move: terminal scrollback, unsaved
  editor buffers, and a loaded browser page all survive. The arrangement,
  divider sizes, and open tabs persist per workspace, including through app
  restarts.
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

- A real Chromium browser inside the Cockpit as a pane of its own: a tab
  beside the Editor by default (**⌘⇧B** brings it forward or hides it), and
  it can be dragged anywhere. It is not an iframe preview, so
  sites that refuse to be framed (GitHub, most production apps) load
  normally, service workers and cookies behave like a real browser, and dev
  servers over plain http work.
- **One live page per workspace**, with its own persistent cookie profile
  per workspace, so client A's logged-in staging session is never ambient
  while you work on client B. The last URL, panel layout, and pane
  arrangement persist per workspace.
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
  something to appear, and read the console and network capture. A click
  that opens a link (including one that would open a new tab) waits for the
  page it opens and says so, and console and network reads cover the page
  currently loaded unless Claude asks for the whole history. So Claude
  can build a page and then actually check it in the same browser you are
  watching, instead of asking you to look.
- **You approve it per terminal tab.** The first time a session tries to use
  the browser, CC Blackbox asks once: Allow or Deny. Allow lasts for that
  terminal tab; Deny sticks until you close and reopen the tab. The
  permission belongs to the Claude process that asked: if some other
  program in that tab later tries to use it, it is refused and you get a
  notification saying so. While a
  session is driving, a **Claude driving** pill shows in the browser toolbar
  and pulses on each action. It acts inside that browser's logged-in
  sessions, which is why the consent is per tab and worth thinking about.
- It works through a small local server the app runs; nothing leaves your
  machine. Setup is automatic: the app adds one entry to Claude Code's config
  shortly after launch and re-adds it if Claude Code overwrites it. Outside
  CC Blackbox that entry stays dormant and never connects. Settings, Browser
  Control (MCP), can remove it for good if you would rather not have it.
  Every action Claude takes is recorded in the session timeline like any
  other tool use, and shows up on the Ops Board.

## iOS Simulator

- Apple's iOS Simulator inside the Cockpit as a pane of its own: a tab
  beside the Editor by default (**⌘⇧I** brings it forward or hides it), and
  it can be dragged anywhere. The device screen streams
  live into the pane with no Simulator window anywhere; Apple's simulator
  runtime does the running, the app mirrors and drives it.
- **A setup strip that is always there.** Above the device the pane shows
  what the feature runs on: Xcode, an iOS runtime, Homebrew, and the free
  open-source baguette driver. When everything is present it is one green
  line; when something is missing it opens into a checklist with a link or
  a one-line install command per row. Install buttons open a terminal with
  the command typed in for you to run; nothing installs behind your back.
- **One device per workspace.** The device menu lists every iOS simulator
  on the Mac by runtime with its boot state; picking a shut-down one boots
  it. The attached device and the stream settings persist per workspace.
- **Tap it yourself**: click to tap, drag to swipe, scroll with the wheel,
  type when the screen has focus (Unicode pastes through the pasteboard).
  Toolbar buttons and Simulator.app's shortcuts for Home (⌘⇧H), Lock (⌘L),
  and volume (⌘↑/⌘↓); a Desktop screenshot (⌘S); frame rate and stream
  size controls with a live fps readout; and a **Maximize** mode (Esc
  restores).
- Honest about the device: the pane polls the real boot state, so a device
  shut down elsewhere (quitting Simulator.app shuts its devices down) shows
  as off with a Boot button instead of a frozen picture. Devices the app
  booted are shut down when it quits; devices you booted are left alone.
- Simulator.app can be open on the same device at the same time; both
  views stay live.

## Agentic Simulator: your Claude sessions can drive it

- Any Claude session in the app can control the attached device: list and
  boot simulators, install and launch a build, open URLs and deep links,
  read the screen as an accessibility outline, tap by label or coordinate,
  swipe, type, press keys and hardware buttons, wait for text to appear,
  switch light/dark, and take screenshots. So Claude can build an iOS app in
  the terminal, run it in the simulator, and check its own work while you
  watch.
- **Separate consent from the browser.** The first time a session tries to
  use the simulator, CC Blackbox asks once: Allow or Deny, per terminal tab,
  independent of the browser permission. While a session is driving, a
  **Claude driving** pill shows in the simulator toolbar; when Claude
  attaches a device, the Cockpit switches to the Simulator so you see it.
- Same plumbing as the browser: the app's local MCP entry, nothing leaving
  the Mac, every action recorded in the session timeline and shown on the
  Ops Board as SIMULATING.

## Terminals & Recorded Sessions

- **The terminal viewport and the Terminals list are separate panes**: the
  list (▶ Claude, + Shell, and the folder-grouped tabs) can sit anywhere in
  the Cockpit, on its own or stacked with another pane, while the viewport
  shows the active terminal wherever you put it.
- **The terminal list is grouped by workspace folder**: a header per
  folder in Explorer order, that folder's shells and sessions underneath,
  and an Other section for anything outside the workspace folders.
- **The terminal list always matches the open workspace.** Tabs for other
  workspaces are tucked away with their shells and sessions running
  untouched, and return the moment you switch back with their scrollback
  and the tab you left active (the cockpit stays mounted under the Workspace
  Picker); terminals whose folder belongs to no workspace stay visible
  everywhere.

- **Claude tabs run Claude Code's fullscreen renderer**, always: the
  prompt is pinned to the bottom of the terminal from the first frame, the
  mouse works inside Claude (click to place the cursor, drag to select),
  and wheel scrolling is tuned for the app's terminal. Shells are untouched;
  `CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN=1` in settings.json `env` opts out.
- **Open files from Finder.** The app is listed under Recommended in
  Finder's Open With for text, configuration, and source files, and a
  file opened that way (or dropped on the Dock icon) lands as an editor
  tab: inside the open workspace if it belongs there, otherwise as a
  standalone tab for this run.
- **Right-click edit menu** in every terminal and editor: Copy, Paste,
  Select All (Cut in the editor), native macOS menu; right-click selects
  the word under the pointer in a terminal, and Option-drag forces a native
  selection inside a Claude tab where Claude Code owns the mouse.
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
  any tracked project and launch a dedicated Claude tab. Three things a
  plain shell does not give you: a clean direct launch; the multi-client
  Vercel switch (the session carries the project's client Vercel account,
  so each client's work runs as that client, while a plain shell's typed
  claude records too but only ever has your own login); and Claude Code's
  fullscreen renderer, always on (prompt pinned to the bottom, click to
  place the cursor, drag to select, tuned scrolling). The button's tooltip
  and its tour stop list the same three. Launching a project outside the open
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
  that inspects its environment can tell the token came from CC Blackbox
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
- Hooks are observe-only and env-guarded: outside CC Blackbox they exit
  instantly and silently, and existing hooks in settings.json are preserved.
- Session titles follow the last prompt you typed (slash commands never
  become a title); rename one in Sessions or from the phone and the name
  sticks. Crashed sessions are detected and marked on next launch.

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
  duration, tokens, and cost. Click any column header to sort by it (click
  again to flip); the sort runs in the database, so the top of the list is
  the true top across all your sessions, not just the ones on screen. The
  list loads 200 at a time with a Load more button, in the same order.
  Search is instant full-text (SQLite FTS5) over everything the session
  did, plus titles, and sorts the same way.
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
- **Decision logging**: one switch makes every process in a run (the
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

- **Cost report**: grouped by client, project, model, day, week, or month.
  A KPI strip totals everything the scope admits: total spend, sessions,
  input and output tokens, cache hit rate, and agent spend. Client, project,
  and model groupings draw horizontal stacked-by-model bars (one labeled row
  per category, so long project names never collide; many categories scroll
  vertically), while time groupings keep vertical columns; every model keeps
  a stable color everywhere, and bars are clickable straight into the
  drill-down. Model id variants always collapse into their major family
  (a point release or dated snapshot is one row with its family, never a
  duplicate entry; Claude Code's synthetic error records are excluded), and
  pricing falls back along the same family, so a new id variant never
  silently prices at $0. The token/cost table adds cache-efficiency (% of
  prompt tokens served from cache) and agent-spend columns, a totals footer
  row on every table, expandable per-model sub-rows showing what caching
  saved in dollars, and CSV export. Per-model rows are priced from
  per-message transcript data; live/unimported sessions appear only in the
  session totals (disclosed in the UI). Editing a model price in Settings
  re-prices all recorded history, so past reports always reflect the
  current price table. The 4.5.0 update itself ships corrected prices
  (Fable 5.1 added; Sonnet 5's cancelled increase rolled back) and
  re-prices history once on first launch, so totals can shift after
  updating: that is the report getting more accurate, not a data change.
- **Drill-down**: click any cost row (or model sub-row) to open the Sessions
  view filtered to exactly the sessions behind that number, with a
  dismissible filter pill.
- **Operations**: how the work went (Cost says what it cost). Activity
  leads: KPI tiles for active time, average active time per working day,
  sessions, busiest project, permission denials, compactions, and session
  crashes; then the range itself. A range of about a month or less (this
  month, last month, 30 days) is a wall calendar: each day tinted by its
  active time with the total and a thin per-project ribbon, week totals down
  the right, and a hover card listing the day's projects largest first;
  click a day to open its sessions. A longer range (90 days, all time) is a
  single line of active time per day (per week past three months) with the
  busiest point marked and one project bar with totals. **Explore** opens
  any month as a full-window calendar with project filter chips and
  previous/next paging inside the range. Under both, a "when you work"
  profile shows which hours of the day the work happens in, local time,
  with an AM/PM hover. Active time is measured in 10-minute blocks where Claude
  actually produced work (from per-message timestamps), so a tab left open
  all day counts for nothing. A friction panel shows where the work hit
  resistance: permission requests and denials, compactions (auto ones mean
  a session filled its context), denials ranked by tool, and a
  session-health line; project rows click through to their sessions. Agent
  economics and logged decisions (what, why, alternatives, with crew
  attribution, expandable per run; the 30 most recent decision-logging
  runs) live in an Agents section that only
  grows when agents actually ran in the range. Every section header carries
  a hover info icon explaining what the numbers mean, and KPI tiles and
  cache columns explain themselves on hover too.
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
  operates it as it goes. The Cockpit piece by piece (Explorer, Editor,
  moving panes, the Browser brought forward with its toolbar explained and
  how Claude can drive it, the iOS Simulator the same way, the Visualizer,
  Terminals, the Claude button), Sessions, then the Agent Harness in depth:
  the tour opens the New Agent editor pre-filled with an example and walks
  every section in its real state (Auto vs Orchestrator + crew, the prompt
  and its slots, model and caps, a crew member expanded, Plan only vs Edit
  files vs Full auto, extra abilities, the plain-English summary, worktree
  and retry, Log decisions, schedule, Create), then Run history, Reports
  panel by panel (the scope row, then the Cost tab switched in front of
  you: totals, grouped spend, churn; then Operations: the how-the-work-went
  totals, the activity calendar, the hour strip, agents, friction), and
  every Settings card (hooks, Browser Control, projects and
  clients, credentials, appearance, model prices, database, sync, health).
  The card sits in one fixed spot with a step counter, each stop gets a
  glow ring, and the app stays fully clickable. Fires once, when a fresh
  install opens its first workspace; existing installs never see it
  uninvited; **Help → Show Walkthrough** or **Settings → Appearance → Show
  walkthrough** replays it any time. Leaving the tour at any point puts
  everything back (the example agent closes unsaved, the pane arrangement
  returns to what it was).

## Mobile remote (iPhone companion)

- A paired iPhone running the CC Blackbox companion app watches every live
  interactive session on this Mac and can steer it: send the next prompt,
  answer a permission ask, interrupt the turn, or stop the session. Shell
  tabs where you typed `claude` count too.
- Everything travels through a relay (relay.ccblackbox.app) that only
  routes ciphertext: pairing is a QR code shown on the Mac, every message is
  end-to-end encrypted between the Mac and the phone, and the relay holds
  no pairing state. Up to two phones per Mac.
- Permission asks from the phone are answered through Claude Code's own
  decision channel (the PermissionRequest hook), never by typing keystrokes,
  so an answer can only ever apply to the exact prompt it was asked for.
  With no phone connected, the terminal prompt appears exactly as before.
- Prompts and interrupts from the phone are honored only while a Claude
  process owns the tab's foreground; a tab whose claude just exited refuses
  them, so phone text can never land in a shell.
- A push notification reaches the phone when a session waits for input,
  waits for a permission, or ends; pushes are encrypted with a key set at
  pairing and the relay cannot read them.
- Settings → Mobile: turn it on, pair a phone (QR or link), see each
  phone's last-seen time and live dot, unpair. If the installed hooks
  predate phone answers, the card says so with a Reinstall button.
- The Mac keeps its own relay link honest: a connection that goes silent
  (a network switch, a captive portal) is noticed within about a minute
  and redialed on its own, so a paired phone never sits on a dead link.
- A status readout above the chat shows the session's model and how full its context window is (green, then yellow at 60 percent, red at 85). It appears automatically; nothing to configure.
- The chat mirrors the terminal's turn, not just its tool calls: what
  Claude says between tool calls reaches the phone and is kept for the
  next time the session is opened, tool rows expand on tap to show the
  full input (a file edit shows a small red/green diff), and headless
  claude runs spawned inside a tab (a pre-push review, a scripted
  `claude -p`) stay out of the phone's view of that chat.
- Session names follow the last thing you typed, so the list always
  says what each session is currently about; slash commands never
  become a name. Hold a session card on the phone and tap Rename to set
  a permanent name of your own (the app's session list can rename too);
  a rename holds everywhere for the session's lifetime. Ended sessions
  leave the phone list immediately.


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
  Apply resizes the whole app), the autosave toggle, **Show notifications**
  and **Play notification sound** (turn every desktop notification off, or
  keep them but silent), a **Reset Cockpit
  layout** button (every pane back to its default place; also under View),
  and
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
  and every few hours, waiting for an idle moment (no recording session,
  no agent run, no sync import) out of politeness to live work. The update
  downloads in the background, is checked against the feed's checksum,
  unpacked by a separate helper process (never inside the app), verified
  as a genuinely signed CC Blackbox build of the announced version, and
  only then staged; one notification says a version is ready. It applies
  on a real quit (⌘Q) or the app menu's Restart to Update, which swaps the
  app and relaunches it: never mid-flight, never by surprise. The app menu
  also has Check for Updates for an on-demand check (it warns if something
  is running and lets you check anyway), and About shows the version. If
  any step fails, the message names what happened (download, corrupted
  file, signature refused, disk space, unpack) and the one thing that
  fixes it; after repeated failures on the same version the app stops
  retrying and points at the manual download. Nothing fails silently. After an update
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
