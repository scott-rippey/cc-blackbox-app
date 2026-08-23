# CC-Blackbox User Guide

How to use the app, task by task. For the at-a-glance feature list see
[`features.md`](features.md).

## First-time setup

1. Launch the app. The **Workspace Picker** appears.
2. Click **New Workspace**, then **+ Add root folder** and pick a project
   folder (the first pick fills in the workspace name; edit it if you like).
   Add more roots if the workspace spans several folders, then
   **Create & Open**.
3. Go to **Settings** (title bar) and click **Install hooks…** in the
   *Claude Code Hooks* card. Review the side-by-side diff of
   `~/.claude/settings.json` and click **Apply**. Without hooks installed,
   sessions still run but nothing is recorded.
4. Check the *Model Prices* card. Cost math uses this table; edit any row
   (click it) or **+ Add model** if you use a model that isn't listed.

The app remembers your last workspace and reopens it on launch. The
`<workspace name> ▾` button at the top right returns to the picker. Hover a
workspace row in the picker for **✎ Rename** (the name defaults to the first
folder you added) and **✕ Remove**.

## The Cockpit

The **Cockpit** view is three panes: Explorer, Editor + Visualizer, Terminals.
Drag the dividers to resize; sizes persist per workspace. The visualizer
collapses with its `▼` button and comes back via the **▲ Visualizer** pill.

- **⌘P**: quick-open any file by name.
- **⌘⇧F**: full-text search across the workspace (ripgrep, honors
  `.gitignore`). Results show `path:line`; Enter or click opens the file.
- **⌘⇧O** (File → Add Folder to Workspace…): add another root; a shell tab
  opens in it automatically, and if the folder isn't tracked yet you're asked
  once whether to track it (with an optional client tag). The same ask
  appears for new workspace roots; Skip is remembered, and **Settings →
  Projects** can track anything later.
- **Settings → App font size** zooms the entire UI. The slider previews on
  the Appearance card; **Apply** resizes the whole app in one jump.

## Working with files

Click a file in the Explorer to open it. Right-click for the full menu:
open, reveal in Finder, copy (relative) path, new file/folder, rename,
delete (moves to Trash), and, on a root, remove it from the workspace.
With several roots, drag a root folder's header up or down to reorder them;
the order sticks with the workspace.

Editor behavior by file type:

- **Code**: syntax-highlighted editor. **⌘S** saves; enable
  **Autosave** in Settings to save 800 ms after you stop typing.
- **Markdown**: toggle **Edit** / **Rendered** at the top right.
- **JSON**: toggle **Edit** / **Tree** (collapsible tree view; JSON linting
  in the editor).
- **PDF / images / DOCX**: rendered previews. Binaries show an info card
  with **Reveal in Finder**.

If a file changes on disk while your tab is clean it reloads silently. If you
have unsaved edits, a banner offers **Reload from disk** or
**Keep mine (overwrite)**. Deleted-from-disk files offer
**Recreate with my content**. Files over 2 MB open read-only with a
**Load anyway** option.

## Terminals: shells and recorded sessions

The tab list is grouped by workspace folder: each folder gets a small
header (same order as the Explorer) with its terminals underneath, so the
terminal list mirrors your folder list. Anything outside the workspace
folders appears under Other. The tab list shows the terminals of the workspace you have open. Switch
workspaces and the list switches with you; the other workspace's shells and
sessions keep running in the background and their tabs return when you
switch back.

The Terminals pane (right) has two spawn buttons:

- **+ Shell**: a plain login shell. With multiple roots you pick which
  folder. Opening a workspace starts one shell per folder, so the list
  begins one-to-one with your folders. Typing `claude` in a shell records
  it like a ▶ Claude tab (the tab grows the recording lamp while it runs):
  recording starts when the folder is a tracked project; in an untracked
  folder the app asks **Track this folder?** once, and the *next* claude
  run there records. The shell itself (your commands, other tools) is
  never recorded, and no client credentials are injected into shells.
- **▶ Claude**: a dedicated Claude tab, via the **Launch Claude** modal.
  Both tab types record; what the dedicated tab adds is a guaranteed clean
  launch (no shell in between, so nothing in a shell profile can interfere)
  and the project's client credentials, which plain shells never receive.
  The modal:
  - **Workspace folders** lists your roots and any tracked projects in them
    (with the current git branch). **Launch** starts the session; on an
    untracked folder it first asks **Track this folder?** once: type an
    optional client and press Enter (or Skip; the session launches either
    way). Because the ask happens before the session spawns, a client tagged
    here means this very first session already carries that client's
    credentials.
  - Each project row keeps an inline **Client** field for quick edits.
  - **Other projects** lists every tracked project outside this workspace.
    Launching one adds its folder to the workspace at that moment, so the
    files appear in Explorer next to the new terminal; remove the folder
    from Explorer later like any other if you no longer want it.
  - **+ Track a folder** registers a folder anywhere on disk as a project
    (same ask). It then appears under Other projects, ready to launch.

A live Claude tab shows a pulsing lamp and a glow border. When Claude finishes
or asks for input on a background tab, that tab gets an amber dot; if the
window is unfocused you get a native notification instead. When the process
exits the tab flashes green and removes itself. Drag a tab up or down the
list to reorder; the order sticks, even across a window close-and-reopen.

**macOS folder permission.** Folders in Documents, Desktop, or Downloads are
gated per app by macOS. The first time you pick one, macOS shows its
permission prompt; click Allow so sessions can run there. If access was ever
denied, launching a session in that folder shows a dialog naming the blocked
path, with an **Open System Settings** button that lands on Privacy &
Security → Files and Folders. Flip CC-Blackbox on for the folder and launch
again.

**If a session dies right after launch**, open it in Sessions: the detail
view shows "Session exited at launch: captured output" with the exit code
and whatever the process printed before it died. If Claude Code itself isn't
installed, launching a session says so directly, with a button to the
install page. And if the Hooks card in Settings ever shows an amber
"outdated" script state after an update, click Reinstall hooks once to pick
up the current version.

### Vercel access per client

Client work usually means client Vercel accounts, and Claude is far more
useful when it can reach the right one: reading deploy logs, inspecting a
failed build, checking env vars, without you pasting a token into the chat
(where it would land in the recorded transcript) or keeping it in a file in
the repo.

The tie-in works through the client tag. Assign a client to a project (the
**Track this folder?** ask does this the moment a folder enters the app,
**Settings → Projects** edits it any time after, and **+ Add client…** in
the Client Credentials card creates a client directly, handy for connecting
a credential before any project wears the tag), then connect that client's
Vercel once: **Settings → Client Credentials → Connect Vercel…**. Your
browser opens Vercel's authorization page; sign in if needed, click
**Allow**, and the credential arrives back in the app automatically,
assigned to that client. (Prefer a manually created token? The small
**paste token** option next to the button still takes one.) From then on,
every Claude session launched on that client's projects starts with
`VERCEL_TOKEN` already in its environment; the `vercel` CLI and API are
authenticated as that client from the first command.

The scoping is deliberate: the token reaches only that client's Claude
sessions. Plain shells never see it, other clients' sessions never see it,
and it never enters the database; it lives only encrypted in the macOS
Keychain. **Verify** confirms a stored token works using a read-only check.

Credentials can die outside the app (revoked, rotated, expired). The app
checks for that on its own: if Vercel rejects a stored credential, the
client's row turns red with an "invalid since" date, you get a single
notification (clicking it opens Settings), and sessions on that client's
projects fall back to your own Vercel login until you **Reconnect…** or
paste a fresh token. A network hiccup never marks a credential invalid;
only a definitive rejection from Vercel does.

## Watching a live session

The Cockpit **Visualizer** follows your active Claude tab:

- **Live Feed**: every event in plain English (prompts, tool calls with
  timing, failures, subagents, compaction). Click a file path to open it.
- **Files Touched**: per-file read/edit counts.
- **Heatmap**: the directory tree tinted by activity.

The Explorer also lights up in real time: blue dots on files being read,
pink on files being edited.

For the full picture, open the session in the **Sessions** view while it
runs: the **Activity EKG** (event-density waveform with prompt, failure, and
compaction markers) and the **Ops Board** (orchestrator orb, one card per
live subagent, working-set files, last report) update live.

## Reviewing past sessions

**Sessions** lists every recorded session with project, client, branch, date,
duration, tokens, and cost. The search box is full-text over everything the
session did (every command, file, prompt) plus titles. Click a row to open it.

In a session's detail page:

- Rename it via the title field (saved on blur).
- The stat strip shows model, duration, token breakdown, and cost. If usage
  is missing, **re-import usage** retries the transcript import.
- **↻ Resume**: continue the conversation in a new recorded Claude tab.
- **⚡ View run report**: appears when the session was a harness run; jumps
  to that run in the Agent Harness.
- **▶ Flight Playback**: replay the session as an animated timeline in a
  full-screen overlay (the app stays put behind it). Transport controls:
  play/pause, back-to-start, speeds 1×–100× (default 10×), and a scrubber.
  Tabs: **Ops Board**, **Feed**, **Heatmap**, and **Timeline** (a per-tool
  swimlane of every tool call, failures in red). The **Feed** weaves the
  real conversation (your prompts, Claude's replies) between the tool events
  as the clock advances. **✕ Exit playback** or **Esc** returns you exactly
  where you were.

## The Agent Harness

**Agent Harness** runs headless Claude Code agents: saved prompts with
explicit permissions, run on demand or on a schedule, every run recorded like
any session.

### Creating an agent

Click **+ New Agent**:

1. **Name** and **Project** (the folder the agent runs in; **Folder…**
   registers a new one).
2. **Prompt**: click the preview box (or its **✎ Edit prompt** corner) for
   the full-screen editor: markdown highlighting, the `{date}` `{time}`
   `{project}` `{branch}` slots highlighted as you type, a character counter,
   and a **▶ Preview filled prompt** toggle that shows the prompt exactly as
   it would launch right now (today's values, current branch).
3. **Model**, **Effort**, **Max turns** (default 50), and **Budget cap ($)**.
   The effort list adapts to the chosen model, and the cap stops the run when
   the client-side cost estimate reaches it (an estimate, not billing, so it
   works on a Claude plan; empty = no cap).
4. **Structure**: two cards. **Auto** (default): one prompt, and Claude works
   alone or spins up built-in helpers (Explore, Plan, general-purpose) as it
   sees fit. **Orchestrator + crew**: your prompt stays the task and drives
   the orchestrator; crew members run as subagents, report their results back
   to it, and it writes the final report. Delegation is automatic, routed by
   each member's description. Switching back to Auto removes the crew (it
   asks first).
5. **Crew** (in Orchestrator + crew): **＋ Add subagent** to give the agent
   named specialists it can delegate to; they run in parallel. Per subagent:
   - **Name** (lowercase-and-hyphens, e.g. `code-reviewer`): this exact name
     shows up on the Ops Board while the run is live.
   - **Description** (required): the orchestrator decides when to delegate
     based on this, so write it like a job posting.
   - **System prompt** (optional), **model** (inherit or any catalog model,
     e.g. a Fable orchestrator with Opus and Sonnet workers), **effort**, a
     tools preset: **All tools** or **Read-only** (Read/Grep/Glob, for
     reviewers and researchers that must not change files), and
     **🏝 Sandbox: own worktree**: on top of wherever the run works, this
     member gets its OWN temporary git worktree so its edits can't collide
     with the orchestrator's or other members' (auto-cleaned if it changes
     nothing).
6. **Run permissions**: ONE envelope for the whole run: the mode and extra
   abilities govern the orchestrator and every crew member alike. Pick a
   preset:
   - **👁 Plan only**: the whole run reads and analyzes; nobody changes a
     file, even a member with all tools. Crew cards reflect this live. The
     member tools presets still matter here: they pick which tools EXIST for
     a member (All tools keeps shell/web reach for exploring; Read-only is
     pure file reading), while the mode governs what those tools may do.
   - **✎ Edit files** (recommended): file edits auto-approved run-wide;
     anything not allowed below is refused.
   - **⚡ Full auto**: never stops for anything. Only for agents you fully
     trust unattended.

   Then grant **extra abilities** as needed: git commands, npm commands, any
   shell command, web search, web fetch. They're granted run-wide too,
   though a Read-only member can't use them, since its toolset (Read/Grep/
   Glob) lacks the tools they need. The plain-English "What this run can do"
   summary always states exactly what the run may do. Esc closes the editor
   (with a discard warning if you have unsaved edits); ⌘↩ saves. Deleting an
   agent always asks first; it also deletes the agent's run history and
   removes its worktrees.
7. **Run in a git worktree**: the whole run (orchestrator and crew) works in
   a throwaway copy of the repo; your working files are never touched and you
   merge its changes yourself. **Auto-retry once on failure** relaunches a
   failed run once with the same prompt (the retry never retries again).
8. **Schedule**: Manual only, Daily, or Weekly at a set time. Schedules fire
   only while the app is open; missed times are skipped. At most 3 runs
   execute concurrently, and each run is killed after 30 minutes.

### Runs

**▶ Run** on an agent card queues a run. In **Run history** each run shows
its status, launch-config pills (model, effort, permission mode, crew size,
caps; snapshotted at launch, so editing the agent later never rewrites what
a past run actually used), a live "what it's doing now" line while running,
elapsed time, and cost. When it finishes, the report (the agent's final
message) renders as markdown; click to expand, **Copy report** for the
markdown source.

- **▶ Watch live** (while running) jumps straight to the run's live session
  detail: the Ops Board with one card per subagent, the Activity EKG, and the
  full event feed. After the run, **View session** opens the same recording.
- **Cancel** stops a queued or running run. **↻ Re-run** launches a new run
  with the exact same prompt and launch config.
- Failures are explained in plain language: a 30-minute timeout, a crash, a
  launch failure, and "App was closed mid-run" each say so. If a run finishes
  or fails while the app is unfocused (or closed to the dock), a native
  notification tells you which agent and how it ended.
- **Reveal worktree** / **Remove worktree** manage a finished run's worktree.
- **⚡ Act on run**: the follow-through move. It resumes the run's
  conversation in a new recorded interactive tab, with everything the agent
  learned intact. Choose where it acts (in the worktree, or fresh in the main
  repo), edit the pre-typed follow-up instruction, and click
  **Open terminal →**. The instruction is typed into the prompt for you;
  nothing runs until you press Enter.

## Reports

**Reports** has two tabs, **Cost** and **Operations**, sharing one scope
row: range presets (this month → all time) plus **client** and **project**
dropdowns. Every panel below follows that scope, so "acme, last month" is
two clicks and the whole view narrows to it. The rolling 5-hour usage chip
in the corner approximates the plan's usage block and ignores the scope.

- **Cost**: group by client, project, model, day, week, or month. The chart
  stacks cost by model (each model keeps its color everywhere); the table
  adds **Cache %** (how much of the prompt side came from cache) and
  **Agent $** (spend from harness runs). Click a row's ▸ to expand its
  per-model breakdown, including what caching saved you in dollars.
  **Export CSV** includes the new columns. One honest caveat, also shown in
  the UI: model rows are priced from per-message transcript data, so live or
  unimported sessions count in the session totals but not the model rows.
- **Drill-down**: click any row (or model sub-row) and the Sessions view
  opens filtered to exactly those sessions, with a pill showing the filter.
  Dismiss the pill (or just type in search) to get the full list back.
- **Operations**: the harness ledger: KPI tiles (runs, success rate, agent
  spend, average duration, timeouts, retries, session crashes, permission
  denials), a per-agent table over every recorded run, and a **Friction**
  panel: permission asks and denials (ranked by tool), compactions (with
  the auto count: those mean a session filled its context), and a
  session-health line.
- **Churn**: the most re-touched files per project (edits × sessions), a
  spec-leak detector: files that keep getting edited across sessions usually
  mark unclear specs or unstable design. Obeys the scope row.
- **Export PDF**: saves the whole scoped report as a paginated A4 document
  (summary tiles, charts, every table) and opens it. Scope it to one client
  first and the header reads as that client's report; handy for sharing.

To send feedback about the app: **Help → Send Feedback…** opens a GitHub
issue with your app version already filled in.

New here? The app gives first-time installs a short spotlight walkthrough
when the first workspace opens; replay it whenever you like from
**Help → Show Walkthrough**.

## Settings reference

- **Claude Code Hooks**: install status, the resolved settings.json and
  script paths, and **Install / Reinstall / Uninstall** with a reviewed diff.
  Hooks are observe-only and env-guarded: outside CC-Blackbox they exit
  instantly and silently. Your other hooks are preserved and a timestamped
  backup is written before any change.
- **Projects**: every tracked folder in one list: name, current branch,
  path, and the client tag (inline-editable with suggestions so a typo can't
  mint a second client). **+ Add project folder…** tracks anything the
  auto-ask skipped. Changing a client re-attributes that project's recorded
  history retroactively; renaming into an existing client merges histories.
- **Client Credentials**: per-client Vercel tokens: connect, verify
  (read-only), remove. Stored encrypted in the macOS Keychain, never in the
  database. Why this exists and how tokens reach sessions:
  [Vercel access per client](#vercel-access-per-client).
- **Appearance**: app-wide font size (10–18 px; the slider previews on the
  card, Apply resizes the whole app), the autosave toggle, and
  **Glass background**: the window turns genuinely transparent so your
  desktop shows through the app (subtle, still fully readable). The window
  blinks once as it recreates when you toggle; unsaved editor tabs are
  saved first (the toggle refuses if a save fails), and terminals reattach
  automatically.
- **Model Prices**: the $/MTok table behind all cost math: input, output,
  cache read, and cache-write at 5-minute and 1-hour TTLs. Click a row to
  edit; **+ Add model** for new models.
- **Database**: what the flight recorder's SQLite file is up to:
  - **Storage**: size on disk, WAL size, and reclaimable space, with a
    **Compact** button (VACUUM) that lights up when there's real space to
    reclaim. Compact is refused while a session or run is active, and also
    while a Claude session is still launching, with an honest message; it
    needs the database to itself for a moment.
  - **Row counts**: events, messages, sessions, agent runs.
  - **Check now**: a real integrity check (`PRAGMA quick_check`), reported
    verbatim: `ok` in green or the exact findings in red. No invented
    health scores.
  - **Schema + backups**: current schema version and the pre-migration
    backup copies kept beside the DB. **Reveal in Finder** opens the folder.
  - **Keep history**: "Keep everything" (the default) or a 3/6/12/24-month
    window. Setting a window asks once, showing the exact cutoff date: all
    sessions that ENDED before it are permanently deleted, and Reports lose
    those months too. Live sessions and agent-run records are never touched.
    The sweep repeats daily while the app is open; heavy work (check,
    compact, sweep) runs in a separate helper process, so recording never
    stalls.
- **Health**: the app's own vital signs.
  - **Memory**: total across all of the app's processes with a per-process
    breakdown, plus the main process heap. Numbers differ from Activity
    Monitor; macOS compresses idle memory, so trends matter more than the
    absolute figure.
  - **Event loop**: how responsive the main process has been (p99 and max
    latency). Sustained high numbers mean something is stalling recording.
  - **Samples**: the same numbers are recorded to the database every 5
    minutes and kept for 30 days, so a slow leak shows up as a trend.
  - **Crash reports**: on every launch the app checks whether the system
    wrote any crash reports for it since the last check (crash dumps plus
    macOS diagnostic reports; advisory non-fatal reports are filtered out).
    Anything found is shown with a **Reveal** button so the evidence is one
    click away instead of buried in a Library folder.

## Good to know

- **Closing the window doesn't quit.** Live sessions, schedules, and runs
  keep going in the dock; clicking the dock icon brings everything back,
  terminals reattached. **⌘Q** is the real shutdown: it ends PTYs, runs,
  and recording, and asks once first when something is still live. Runs
  interrupted by a restart are marked failed.
- **Updates apply on quit, never mid-flight.** The app checks for updates on
  its own (launch + every few hours) and notifies once when a new version
  has downloaded. It installs on your next real quit (⌘Q), or immediately
  via **CC-Blackbox → Restart to Update**. **CC-Blackbox → Check for
  Updates** forces a check any time; **About CC-Blackbox** shows your
  version. After an update, the first launch greets you with a What's New
  window listing exactly what changed in the version you just received;
  close it and it stays gone until the next update.
- **If something crashes, the app picks itself up.** A crashed window is
  recreated automatically; your terminals, recordings, and runs live
  outside the window and keep going. If the window hangs, you get a
  dialog with **Keep Waiting** (the default) and **Reload Window**;
  reload recovers the window but discards unsaved editor changes, so it
  warns you first. If the whole app hits a fatal error it relaunches
  itself, at most twice an hour so a real problem can't turn into a
  relaunch loop. Evidence of any crash is kept locally and shown on the
  Settings Health card the next time you launch, with a Reveal button to
  the report file. And if Claude sessions were running when the app
  closed (a crash, a force quit, a Mac restart, or a normal quit with
  sessions still running), the next launch offers to resume them: Claude keeps every conversation on disk, so each session
  reopens in a terminal and picks up exactly where it left off.
- Links in rendered markdown, DOCX, and agent reports open in your system
  browser, never inside the app.
- Folders become workspace roots or projects only through the native folder
  dialog; the app refuses paths that weren't explicitly picked.
- All data is local: `~/Library/Application Support/CC-Blackbox/ccb.db`.
  Before a new app version upgrades the database, a timestamped backup copy
  is written next to it automatically (the newest 5 are kept), so you can
  always roll back to an older build by restoring the copy. This is a hard
  guarantee: if the backup cannot be written (disk full, permissions), the
  app refuses to upgrade the database and tells you why instead of
  proceeding without a safety net.
