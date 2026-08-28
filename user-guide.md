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

The **Cockpit** view starts as Explorer (left), an Editor with Browser and
Simulator tabs over the Visualizer (middle), and the Terminal with its
Terminals list (right). Every pane has a title tab, and you can move it:
drag the title and drop it on the edge of another pane to split that pane,
onto the tab strip or the middle of another pane to stack the two as tabs, or
on the outer edge of the Cockpit for a new column or row.
The little ✕ on a title hides a pane; the **View** menu brings it back, and
**View → Reset Cockpit Layout** puts everything back where it started. The
Browser (**⌘⇧B**) and the iOS Simulator (**⌘⇧I**) are panes too, open as
tabs beside the Editor until you close them. File tabs move the same way:
drag one onto a pane's edge to get a second editor side by side (it closes
itself when its last file does). Nothing is lost by a move: terminals keep
their scrollback, editors keep unsaved edits, the browser keeps its page.
Drag the dividers to resize. The arrangement, sizes, and open files are
remembered per workspace.

- **⌘P**: quick-open any file by name.
- **⌘⇧F**: full-text search across the workspace (ripgrep, honors
  `.gitignore`). Results show `path:line`; Enter or click opens the file.
- **⌘⇧O** (File → Add Folder to Workspace…): add another root; a shell tab
  opens in it automatically, and if the folder isn't tracked yet you're asked
  once whether to track it (with an optional client tag). The same ask
  appears for new workspace roots; Skip is remembered, and **Settings →
  Projects** can track anything later.
- **Settings → App font size** zooms the entire UI below the title bar. The
  slider previews on the Appearance card; **Apply** resizes the app in one
  jump. The title bar (window buttons, recording dot, view tabs) keeps its
  normal size at every setting, like a native toolbar.
- **Settings → Projects → Remove…** takes a folder out of the app for good:
  its recorded sessions and cost history, any transcript copies synced from
  another Mac, and its agents and runs. The dialog shows exactly what goes
  and asks you to type the project name. The folder stays in your workspace;
  it just stops being tracked, and the app will not ask to track it again.
  Removing is per Mac: sessions synced from another Mac return on the next
  sync unless you remove the project there too.

## Working with files

Click a file in the Explorer to open it. Right-click for the full menu:
open, reveal in Finder, copy (relative) path, new file/folder, rename,
delete, and, on a root, remove it from the workspace. Delete asks first,
then moves the file or folder to the Trash, so you can put it back from
Finder if you change your mind. If you would rather it stopped asking, tick
"Don't ask again" in that dialog; **Settings -> Appearance -> Ask before
deleting a file** turns the prompt back on.
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

## The Browser

The Browser tab sits beside the Editor (**⌘⇧B** or View → Browser brings it
forward, and brings it back if you closed it): a real Chromium browser you
can drag anywhere in the Cockpit like any other pane. Type an address and press
Enter; shorthand works (`localhost:3000` becomes http, `example.com` becomes
https), and anything that is not an address becomes a Google search, so
typing `github` gets you there in two steps. This is not a preview iframe: sites that refuse to be embedded
elsewhere, like GitHub or your deployed apps, load normally here.

What the toolbar gives you:

- **← → ⟳** back, forward, reload (the reload button becomes ✕ while a page
  is loading).
- **↗** opens the current page in your system browser.
- **○** clears the page back to a blank tab, so you are never stuck
  displaying whatever you last opened. The cleared page also stays cleared
  when you reopen the workspace.
- **DevTools** docks the full Chrome DevTools next to the page. Drag the
  divider to resize; the split is remembered.
- **Console** opens a capture panel under the page: console output, uncaught
  errors, and every network request with status, duration, and size. The
  badge counts errors. Capture runs all the time, even with DevTools open,
  so the history from before you opened the panel is already there.
- **⤢** maximizes the page to the whole window; press **Esc** or click **⤡**
  to come back. The page never reloads when you switch around.

Each workspace gets its own browser profile (cookies and logins), and the
page you were on comes back when you reopen the workspace. Opening a file
from the Explorer or the ⌘P palette switches you back to the editor
automatically; the page keeps running behind it.

The browser is deliberately locked down: permission prompts (camera,
microphone, location, notifications, and so on) are denied automatically.
The only exceptions are fullscreen and copying to the clipboard, which
normal sites need. Popups open in the same page, and downloads always ask
where to save. For a
local dev server with a self-signed certificate, the app asks once and
trusts it until you quit. Sites that need basic auth (staging servers) get
a sign-in dialog. If a page crashes, you get a Reload button; the rest of
the app is unaffected.

### Letting Claude drive the browser

Your Claude sessions can control this browser themselves: navigate, read the
page, screenshot, click, type, run JavaScript, and read the console and
network capture. That lets Claude verify frontend work in the same browser
you are watching, rather than asking you to check.

This is set up for you: CC-Blackbox adds a single entry to Claude Code's
configuration shortly after launch, and re-adds it if Claude Code ever
overwrites it. Outside CC-Blackbox that entry stays dormant and never
connects, so it is safe to leave alone. If you would rather not have it at
all, Settings has a Browser Control (MCP) card with an Uninstall button, and
that choice sticks across restarts until you click Install again.

After that, the first time a Claude session in a terminal tries to use the
browser, you get a one-time Allow / Deny prompt for that tab. Allow lasts as
long as the tab is open; Deny sticks until you close and reopen it. This
applies to both ▶ Claude tabs and a `claude` you run in a plain shell.
While Claude is driving, a **Claude driving** badge appears in the browser
toolbar. Because the browser holds your logged-in sessions, only allow it for
work you trust. Everything Claude does is recorded in the session timeline
like any other tool.

## The Simulator

The Simulator tab sits beside the Editor (**⌘⇧I** or View → Simulator brings
it forward, and back if you closed it), showing an iPhone or iPad running in
Apple's iOS Simulator,
streamed live into the app. There is no Simulator window to manage: the
device runs headless and this pane is its screen.

The strip at the top is the setup check, and it never goes away. When
everything is in place it reads as one green line naming what the feature
runs on (your Xcode version, the iOS runtimes, the driver version). When
something is missing it opens into a checklist:

- **Xcode** with the iOS platform (free from the App Store).
- **iOS runtime**: at least one simulator runtime. The row gives you the
  `xcodebuild -downloadPlatform iOS` command if none is installed.
- **Homebrew**: how the driver installs. Advisory; the pane works without
  it once the driver is present.
- **Simulator driver**: baguette, a free open-source tool that streams the
  simulator screen and delivers taps. The row's **Install baguette** button
  opens a terminal with `brew install baguette` typed in; you press Return.
  Copy does the same without the terminal. Apple Silicon only.

Click the line to see the rows any time; ↻ re-checks.

What the toolbar gives you:

- **The device menu** lists every iOS simulator on this Mac, grouped by
  runtime, with a dot on the booted ones. Picking a shut-down device boots
  it (a few seconds). Each workspace remembers its device.
- **● live / ○ off** shows the real state; **Boot** and **Shut down** do what
  they say.
- **⌂ ⏻ + −** are Home, Lock, and volume; the shortcuts are Simulator.app's
  own (**⌘⇧H**, **⌘L**, **⌘↑ / ⌘↓**) while the screen has focus.
- **◔** saves a full-resolution PNG to your Desktop (**⌘S**).
- **fps** and **size** tune the stream. Half size at 30 fps is the default
  and is easy on the Mac; full size costs more CPU. A small fps readout
  sits in the corner of the screen.
- **⤢** maximizes the device to the whole window; **Esc** or **⤡** restores.

On the screen itself: click to tap, press and hold for a long press, drag
to swipe, scroll with the wheel, and type once you have clicked into it;
pasting works too, including emoji and non-Latin text.

Two things to know. Quitting Simulator.app shuts its devices down (Apple's
default), so if you open the same device there and quit, the pane shows it
as off with a Boot button; nothing is lost. And when CC-Blackbox quits it
shuts down only the devices it booted; anything you booted yourself is left
running.

### Letting Claude drive the simulator

Your Claude sessions can control the attached device: boot a simulator,
install and launch your build, open a URL or deep link, read the screen as
a list of labeled elements, tap by label or by coordinate, swipe, type,
press keys and hardware buttons, wait for text to appear, switch light and
dark, and take screenshots. Building the app is still Claude's job in the
terminal (`xcodebuild`); the simulator tools take over from there.

The first time a session in a terminal tries, you get a one-time Allow /
Deny prompt for that tab, separate from the browser one. Allow lasts as long
as the tab is open; Deny sticks until you close and reopen it. While Claude
is driving, a **Claude driving** badge appears in the simulator toolbar,
and when Claude attaches a device the Cockpit switches to the Simulator so
you can watch. It only ever touches the simulated device, never your Mac.
Everything Claude does is recorded in the session timeline like any other
tool.

## Terminals: shells and recorded sessions

The tab list is grouped by workspace folder: each folder gets a small
header (same order as the Explorer) with its terminals underneath, so the
terminal list mirrors your folder list. Anything outside the workspace
folders appears under Other. The tab list shows the terminals of the workspace you have open. Switch
workspaces and the list switches with you; the other workspace's shells and
sessions keep running in the background and their tabs return when you
switch back.

The Terminals list (a pane of its own, on the right by default; the terminal
viewport is a separate pane so the two can sit apart) has two spawn buttons:

- **+ Shell**: a plain login shell. With multiple roots you pick which
  folder. Opening a workspace starts one shell per folder, so the list
  begins one-to-one with your folders. Typing `claude` in a shell records
  it like a ▶ Claude tab (the tab grows the recording lamp while it runs):
  recording starts when the folder is a tracked project; in an untracked
  folder the app asks **Track this folder?** once, and the *next* claude
  run there records. The shell itself (your commands, other tools) is
  never recorded, and no client credentials are injected into shells.
- **▶ Claude**: a dedicated Claude tab, via the **Launch Claude** modal.
  Both tab types record; the dedicated tab adds two things. First, a
  guaranteed clean launch: no shell in between, so nothing in a shell
  profile can interfere. Second, and this is the multi-client feature:
  the session gets the project's client **Vercel account** injected
  automatically, so Claude working on client A's project reads deploys
  and logs as client A, on client B's project as client B. A plain shell
  only ever has your own Vercel login; client accounts never reach
  shells. If you manage one account for yourself, both tab types behave
  the same; the moment you manage clients, ▶ Claude is the switch.
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
authenticated as that client from the first command. A companion variable,
`CCB_VERCEL_CLIENT`, names the client next to it (and `CCB_VERCEL_TEAM_ID`
names the Vercel team for team installs), so if Claude inspects the
environment it can see the token was placed there by CC-Blackbox for that
client rather than left behind by a shell profile. Nothing else is set:
the token alone is what the Vercel CLI and API need to land on the right
team.

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

To delete a session, click the ✕ at the end of its row (or **Delete…** in
its detail page) and confirm; its events, token usage and cost history are
removed. Agent run records stay, with their session link cleared. Rows
marked "(no prompt typed)" are sessions where Claude was opened and closed
before you typed anything; when any exist, a **Delete N blank sessions**
button next to the count clears them all at once. A session that is still
recording cannot be deleted. Deleting is per Mac: a session synced from
another Mac comes back on the next sync unless you delete it there too.

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
   **Log decisions**: every process in the run (the orchestrator and each
   crew member) records its significant decisions and why as it works,
   through a private logging tool the app provides. Nothing about the log
   enters any output or any other agent, so it never changes what the run
   does. The agent card's **◇ Decisions** button opens a scrolling list of
   the runs that logged decisions (newest first); click a run to read its
   decisions in the same window, and use Back to return to the list. Use it
   to improve the prompt. Each logged decision costs one tool turn. The tool
   arrives through the same config entry as browser control (see Letting
   Claude drive the browser), so that entry must be installed. In a Plan
   only run, Claude Code refuses the tool call itself, but the decision is
   still recorded the moment it is made, so the log works in every mode.
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
- **◇ Decisions** (agents with Log decisions on): opens the agent's
  decisions window straight into this run. Each entry names who logged it
  (orchestrator or a crew member), what was decided, why, and what was
  rejected. It grows live while the run is going.
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

New here? The app gives first-time installs a guided walkthrough when the
first workspace opens. It drives the real app as it goes: it switches the
Browser on, opens the agent editor with an example agent filled in and walks
every section (structure, crew, permissions, abilities, decisions, schedule),
and visits every Settings card, with the app staying fully usable throughout.
Replay it whenever you like from **Help → Show Walkthrough** or
**Settings → Appearance → Show walkthrough**.

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
- **Browser Control (MCP)**: the one entry the app keeps in Claude Code's
  own config (`~/.claude.json`) so your Claude sessions can reach the app's
  tool server: browser driving and decision logging both go through it. It
  is installed automatically at every launch and re-added if Claude Code
  overwrites it; the card shows status, Reinstall, and Uninstall (which
  sticks until you click Install again). Outside CC-Blackbox the entry
  stays dormant and never connects.
- **iOS Simulator**: the same setup check the Simulator pane shows (Xcode,
  iOS runtime, Homebrew, driver) with a Re-check button, and how device
  shutdown and Claude's per-tab permission work.
- **Appearance**: app-wide font size (10–18 px; the slider previews on the
  card, Apply resizes the whole app; the title bar stays its normal size),
  the autosave toggle, a **Show walkthrough** button that replays the guided
  tour, a **Reset Cockpit layout** button (every pane back to its default
  place; also under View), and
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
- **Machine Sync**: session history across your Macs, over your own
  network.
  - **Share this Mac** makes it visible to your other Macs and lets the
    ones you approve pull its finished sessions. Approval is explicit: the
    first time another Mac connects, a 6-digit code appears on THIS Mac's
    screen and must be typed on the other one. Revoke a machine any time;
    nothing is ever pushed to or changed on a shared Mac.
  - **+ Add machine…** is the other direction: pick one of your Macs that
    is sharing, type the code from its screen, and from then on this Mac
    pulls whatever it is missing, automatically at launch and hourly while
    both apps are open, or on **Sync now**. Only finished sessions move;
    costs, search, and Flight Playback all work on the synced copies.
    Resume stays on the source machine (the live conversation lives in
    Claude Code's own storage there).
  - Project folders that arrived only through sync are listed under
    Settings → Projects as **synced only · track here…**. Opening that folder
    in a workspace on this Mac (or clicking the badge and picking it) makes
    it a normal tracked project here, so its client's Vercel credentials
    inject into sessions on this Mac too. Until then, the client's
    credential row can look connected while sessions in that folder get no
    token.
  - The first discovery or sync triggers macOS's one-time Local Network
    permission prompt; allow it or the Macs cannot see each other. If
    discovery keeps finding nothing, check System Settings → Privacy &
    Security → Local Network.
  - Everything is encrypted in transit. Two Macs syncing both ways just
    means doing the one-time add on each of them.
- **System Health**: the app's own vital signs.
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
