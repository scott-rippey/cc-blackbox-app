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

## v4.2.1 — 2026-08-30

Two fixes from the first day of phone testing.

### Fixed
- A freshly started session now shows as awaiting input on the phone right away, so you can send the first prompt from the phone without touching the Mac. Before, a new tab stayed "Working" until something happened on the Mac side.
- The phone never types into a session that has not reached Claude's real prompt yet (for example the folder-trust question a brand-new folder shows first).

If you use the iPhone companion, also update it: the chat no longer goes blank after locking and unlocking the phone.

## v4.2.0 — 2026-08-30

Your iPhone can now watch and steer live sessions.

### New
- Mobile remote. Pair the CC-Blackbox iPhone companion from Settings, Mobile (a QR code or a link). The phone lists every live session on this Mac, shows what Claude is doing, and lets you send the next prompt, answer a permission ask, interrupt the turn, or stop the session. Shell tabs where you typed `claude` are included.
- Permission answers from the phone travel through Claude Code's own decision channel, never as typed keystrokes, so an answer can only apply to the exact prompt that asked. If no phone is connected, or nobody answers within 50 seconds, the terminal prompt appears exactly as before.
- Prompts and interrupts from the phone are accepted only while a Claude process is in front in that tab, so phone text can never land in a shell.
- Push notifications reach the phone when a session waits for input, waits for a permission, or ends. Everything is end-to-end encrypted through a relay that only routes ciphertext and keeps no pairing state.
- A small "phone" pill appears on a terminal tab while a paired phone is deciding a permission ask.

### Changed
- The Claude Code hook script gained one branch for permission asks; every other event keeps its instant, silent behavior. Settings, Hooks will show "Reinstall" once: the app repairs the PermissionRequest registration (its timeout becomes 60 seconds) and keeps your other hooks untouched.

## v4.1.5 — 2026-08-30

A fix for a false "live work" warning.

### Fixed
- Machine Sync no longer counts looking for an offline Mac as live work. With a paired Mac switched off, the app re-checked for it every 30 seconds and treated each check as an active sync, so Check for Updates could warn that a session or sync was running on an idle app, and automatic update checks and database maintenance could be held back. Only an actual transfer counts now, and an unreachable Mac is retried every 5 minutes instead.

## v4.1.4 — 2026-08-30

A quick polish of the Settings page.

### Changed
- Settings, Appearance: every on/off setting is now a sliding switch (green when on) instead of a checkbox. Show walkthrough is a switch too: turn it on to run the guided tour, turn it off or press Esc to end it. Reset Cockpit layout is now a proper button.
- The Appearance card moved to the bottom of the right-hand column, under System Health, so the left column is shorter.

## v4.1.3 — 2026-08-30

A small fix for a crash that hit laptops.

### Fixed
- The app no longer quits when an automatic update check runs without a network connection. The common case was opening the lid somewhere new: the check fired before Wi-Fi reconnected, and the app treated the failed request as a fatal error. Launching with no network could do the same. Both are now handled quietly; the check simply retries later.

## v4.1.2 — 2026-08-30

### Fixed
- Automatic update checks now wait until the app is idle: no recording session, no agent run, no machine sync, no database maintenance. The update unpacks inside the app, and on rare machines that step can crash it; a due check now waits for the first quiet moment instead of running over live work. Check for Updates in the app menu asks first when something is running.

### New
- Settings, Appearance: two toggles for desktop notifications. Show notifications turns every notice off (finished agents, sessions awaiting input, updates, credential and access notices). Play notification sound keeps the notices but makes them silent.

## v4.1.1 — 2026-08-28

### Fixed
- A pane dragged out of a group could not be dropped back onto that group's tab strip; every drop on the strip read as a new column or row. Dropping on a tab strip now always joins that group as a tab.

### Changed
- The Browser and the Simulator are open by default, as tabs beside the Editor. They stay closed only when you close them, and each workspace remembers its own choice. Reset Cockpit Layout brings them back.

## v4.1.0 — 2026-08-28

### New
- Drag any Cockpit pane by its title to split, tab, or dock it anywhere: Explorer, Editor, Visualizer, Terminal, the Terminals list, Browser, and Simulator. Drop on another pane's edge to split, on its middle to stack as tabs, or on the Cockpit's outer edge for a new column or row. The default arrangement is unchanged.
- The Browser (Cmd+Shift+B) and the iOS Simulator (Cmd+Shift+I) are panes of their own. They start hidden and appear as tabs beside the Editor.
- File tabs drag too: drop one on a pane's edge for a second editor side by side. The extra editor closes itself when its last file does.
- The terminal viewport and the Terminals list are separate panes, so the list can sit anywhere.
- Nothing is rebuilt by a move: terminals keep their scrollback, editors keep unsaved edits, the browser keeps its page, the simulator keeps streaming.
- The arrangement, divider sizes, and open files are remembered per workspace, including across restarts; earlier workspaces carry their sizes and collapsed visualizer over.
- View menu: show or hide each pane, and Reset Cockpit Layout (also in Settings, Appearance).

### Changed
- The Editor, Browser, Simulator switch at the top of the editor is gone; each is a pane with its own title tab.
- The Visualizer's collapse button and restore pill are replaced by the x on its title and the View menu.
- The walkthrough's editor stop explains moving panes; the tour reveals the Browser and Simulator as it goes and restores your layout when it ends.

## v4.0.0 — 2026-08-27

### New
- The iOS Simulator inside the app: a third face of the editor panel next to Editor and Browser (Cmd+Shift+I). Pick an iPhone or iPad and its screen streams live into the pane with no Simulator window; tap, swipe, scroll, and type on it; Home, Lock, volume, Desktop screenshot, stream controls, and maximize on the toolbar. Each workspace remembers its device; devices the app boots are shut down when it quits.
- An always-visible setup check above the device showing what the feature runs on (Xcode, an iOS runtime, Homebrew, the free open-source baguette driver), turning into a checklist with install commands when something is missing.
- Claude can drive the simulator: boot, install and launch a build, open URLs, read the screen, tap, swipe, type, press buttons, wait for text, and take screenshots, with a separate per-tab permission from the browser.
- Settings: an iOS Simulator card. Walkthrough: two Simulator stops.

### Improved
- The recording glow on terminal tabs is smaller, and the active tab gets a light fill and an accent outline so it stands out when several tabs are live.
- The walkthrough card moves out of the way on the three permission steps, Extra abilities, and System Health.

### Fixed
- Quitting with a browser page attached could end in a crash report instead of a clean quit.

## v3.5.0 — 2026-08-27

### New
- A walkthrough that drives the app: it switches the Browser on, opens the agent editor with an example agent filled in and walks every section in its real state, and visits every Settings card. Forty stops, one spotlight at a time, and the card moves out of the way when it would cover what it describes. Leaving the tour puts everything back.
- Settings, Appearance: a Show walkthrough button.

### Fixed
- The user guide's Settings reference documents the Browser Control (MCP) card.

## v3.4.0 — 2026-08-27

### New
- Decision logging for agents: one checkbox makes every process in a run (the orchestrator and each crew member) record its significant decisions and why, as it works. The log is private, so it never changes how the run behaves. Read it from the agent's Decisions button (a scrolling list of runs) or from a run's own Decisions button.

### Changed
- The app's built-in tools for Claude are grouped under one name, `ccbb`; the old entry is removed automatically on first launch.

## v3.3.2 — 2026-08-26

### Fixed
- Launching Claude in a folder whose name ends with a space no longer fails with "cwd is outside project"; the project path is stored exactly, and an already-registered project with the stripped path is repaired in place.
- Folders reached through a symlink register under the path you picked, so the workspace, the launch check, and the project list agree on the same folder; launches also compare resolved paths on both sides.

## v3.3.1 — 2026-08-26

### Added

- **Clear button in the browser toolbar.** The new ○ button blanks the page back to an empty tab, so you are never stuck displaying whatever you last opened. A cleared page stays cleared when you reopen the workspace.

### Fixed

- Resuming the last offered session now closes the "Resume Claude sessions?" dialog; it no longer waits for an extra "Not now" click.
- The What's New popup no longer shows install instructions to people who already updated; those stay on the release page for first-time downloads.

## v3.3.0 — 2026-08-26

### Added

- **Agentic Browser: your Claude sessions can drive the built-in browser.** Any Claude session in the app, a Claude tab or a `claude` typed into a plain shell, can control the embedded browser: navigate, read the page, click, type, scroll, run JavaScript, take screenshots, and read the console and network capture. Claude can build a page and then actually check it in the same browser you are watching.
- **You stay in charge.** The first browser action from each terminal asks Allow or Deny once. Allow lasts for that terminal; Deny sticks until you close it. A "Claude driving" badge shows in the browser toolbar while Claude works. Only the Claude process you approved can use its permission; any other process in that terminal is refused, and you get a notification about the attempt.
- **Automatic setup.** The app registers one entry in Claude Code's configuration and keeps it current. Outside CC-Blackbox that entry stays dormant and never connects. Settings has a Browser Control (MCP) card with an Uninstall that sticks across restarts.
- **Fully recorded.** Every action Claude takes lands in the session timeline and shows on the Ops Board as browsing activity, like any other tool use.

### Fixed

- URL bar: pasting text whose first word ends in a colon (like an error message) now searches instead of doing nothing.

## v3.2.0 — 2026-08-25

### Added

- **A real browser inside the Cockpit.** Click **Browser** next to Editor at the top of the editor panel (or press **⌘⇧B**) and the editor area becomes a full Chromium browser. It is not a preview frame, so sites that refuse to be embedded elsewhere, like GitHub and deployed apps, load normally, and plain-http dev servers work too. The address bar takes shorthand: `localhost:3000` gets http, `example.com` gets https. The ⤢ button maximizes the page to the whole window and Esc brings it back, without reloading anything. Each workspace keeps its own cookies and logins, and the page you were on returns when you reopen the workspace.
- **Chrome DevTools, docked.** The DevTools button opens the full Chrome DevTools right next to the page. Drag the divider to resize; the split is remembered per workspace.
- **Console and network capture.** The Console button opens a panel under the page with console output, uncaught errors, and every network request with status, duration, and size. Capture runs the whole time, even while DevTools is open, so the history from before you opened the panel is already there. The badge on the button counts errors.
- **Downloads, sign-ins, local certificates.** Downloads always ask where to save and never write anywhere until you choose. Sites behind basic auth get a sign-in dialog. A local dev server with a self-signed certificate is trusted only after you confirm, and only until the app quits.

### Notes

- The browser is locked down by design: every web permission (camera, microphone, location, notifications, USB, and the rest) is denied automatically, with two ordinary-browsing exceptions, fullscreen and copying to the clipboard. Popups open in the same page rather than new windows. If a page crashes, you get a Reload button and the rest of the app is unaffected.

## v3.1.5 — 2026-08-25

### Added

- **Deleting a file now asks first.** Right-click, Delete in the Explorer opens a confirm with **Move to Trash** and **Cancel**, so a stray click on the menu no longer removes a file on the spot. Deletes have always gone to the Trash and still do, so anything removed can be put back from Finder. Deleting a folder says it takes everything inside with it. If you would rather it stopped asking, tick **Don't ask again** in that dialog, and **Settings, Appearance, Ask before deleting a file** turns it back on. Ticking the box and then cancelling changes nothing.

### Changed

- **Folder arrows turn.** A folder in the Explorer now shows a single arrow that rotates as it opens, the way VS Code does, in place of the two separate characters it used before. Those sat at slightly different weights and heights, so the arrow appeared to shift as you expanded a folder.

## v3.1.4 — 2026-08-25

### Added

- **Remove a project.** Settings, Projects now has Remove… on every row. It deletes the project's recorded sessions (events, token usage, cost history), any transcript copies synced from another Mac, and its agents and runs, with surviving worktrees removed from disk. The confirm shows the exact counts and asks you to type the project name. It is refused while a session in that folder is recording or one of its agents is running. The folder and your workspace are untouched. Removing is per Mac: sessions synced from another Mac return on the next sync unless you remove the project there too.
- **Delete sessions.** Each row in Sessions has a ✕, and the session page has Delete…, to remove that session's history after a confirm. Agent run records stay, with their session link cleared. When sessions exist where Claude was opened and closed without a prompt, a **Delete N blank sessions** button clears them all at once. A recording session cannot be deleted.

### Changed

- **The title bar keeps its size.** The font size setting no longer scales the title bar, so the recording dot and view tabs stay clear of the window buttons at every size; the font size applies to the rest of the app.
- **Vercel: no more `VERCEL_ORG_ID` in Claude sessions.** With a lone `VERCEL_ORG_ID` the Vercel CLI refuses project commands (`env pull`, `link`, `deploy`), and the integration token already lands on the right team by itself, so the app stops setting it. The client and team now travel as `CCB_VERCEL_CLIENT` and `CCB_VERCEL_TEAM_ID` next to the token, so a session that inspects its environment can see the token came from CC-Blackbox for that client rather than from a stray shell export.

### Fixed

- A client tag edited from the Explorer's project row is trimmed like the Settings editor, so a trailing space can no longer detach a project from its client's credentials.

## v3.1.3 — 2026-08-24

### Fixed

- **Machine Sync no longer leaves a client's Vercel credentials switched off on your other Mac.** When a Mac pulled history for a folder it had never opened itself, that folder counted as already tracked but never got the local go-ahead, so Claude sessions in it ran without the client's Vercel token while Settings showed the credential connected. Opening the folder in a workspace now registers it properly on that Mac (the client tag carries over, no reconnect needed), the Track this folder ask recognizes synced folders and pre-fills their client, and Settings, Projects marks any that are still waiting as "synced only, track here".

### Changed

- **Agents only run in folders this Mac has opened.** Folders that exist only as synced history from another Mac are left out of the agent's project picker until you open them here. An agent saved earlier that still points at one stops with a message saying so, instead of running without the client's credentials.

## v3.1.2 — 2026-08-24

### Fixed

- **Machine Sync discovery now uses your Mac's built-in Bonjour engine.** Discovery previously ran on its own networking code, which some Wi-Fi setups pass in one direction but not the other; machines could see each other inconsistently or not at all. The app now advertises and searches through the same system service AirDrop and printers use, which handles unreliable Wi-Fi correctly. If Add machine could not find your other Mac before, update both Macs and try again.

### Added

- **Machine Sync joined the walkthrough.** The tour now has fifteen stops; Machine Sync is Step 14, right after the Database card, and the System Health card is Step 15. Replay it any time from Help, Show Walkthrough.

## v3.1.1 — 2026-08-24

### Fixed

- **Machine Sync discovery no longer flickers.** In 3.1.0, a Mac could silently stop announcing itself on the network after renaming it or toggling sharing, so it would vanish from the other Mac's Add machine list until the next toggle brought it back. The announcement teardown now completes cleanly before a new one starts, and if an announcement ever fails it retries by itself within seconds instead of staying dark. Update both Macs to 3.1.1 before pairing.

## v3.1.0 — 2026-08-24

### Added

- **Machine Sync: your session history on every Mac you use.** Settings has a new Machine Sync card. Turn on **Share this Mac** on one machine, then on another use **Add machine**, pick it from the list, and type the six-digit code that appears on the first Mac's screen. From then on the second Mac pulls everything it is missing automatically: at launch and hourly while both apps are open, or on **Sync now**. Only finished sessions move, and they arrive complete: history, costs, search, and Flight Playback all work on the synced copies. Sharing is one way and read only; nothing is ever written to or changed on the Mac you pull from, and you can revoke a machine at any time. Set up the other direction on the other Mac if you want both. Everything travels encrypted over your own network; no accounts, no cloud, no extra software. The first sync asks macOS's one-time Local Network permission; allow it, or the Macs cannot see each other.

### Changed

- **The Health card is now System Health**, so its purpose reads clearly among the other Settings cards.

### Fixed

- **The manual Check for Updates result no longer wraps mid-sentence.** The "You are up to date" dialog now fits the macOS alert width.

## v3.0.5 — 2026-08-24

### Fixed

- **Update staging happens once per version, not every four hours.** When a new version downloads, the app stages it in the background so the next restart can install it instantly. Previously, every periodic update check re-ran that staging on the same already-downloaded version. On most machines that was merely wasted work; on a machine where the staging step itself fails, each re-run was another chance for macOS to terminate the app mid-session (seen in the field: an overnight termination at exactly the four-hour check mark). Now a staged version simply waits for your next restart, and checks resume after it installs. If a newer release is published while one is staged, it is picked up on the following launch.

## v3.0.4 — 2026-08-23

### Improved

- **The Vercel switch is now named everywhere it matters.** The real difference between a Claude tab and a plain shell was hiding behind the phrase "client credentials": a Claude tab injects the project's client **Vercel account**, so work on client A's project reads deploys and logs as client A, and work on client B's as client B. A plain shell only ever has your own Vercel login; client accounts never reach shells. The walkthrough, every tab and button tooltip, and the user guide now say exactly that.
- **Sessions with no prompt say so.** A session where Claude was opened and closed before anything was typed used to show as "(untitled)". It now reads "(no prompt typed)", with a tooltip explaining that titles come from the first prompt.
- **Crash-session durations are honest now.** A session that died with the app used to show its duration as "until the next time you opened the app" (a seconds-long session could read as 22 hours). Durations now end at the last recorded activity, and existing sessions with clearly inflated durations are corrected automatically on first launch.

## v3.0.3 — 2026-08-23

### Improved

- **The walkthrough now covers everything.** Fourteen stops in working order: the Cockpit piece by piece (Explorer, editor, visualizer, terminals, and the Claude button), then Sessions, the Agent Harness, Reports, and every Settings card, including the previously untoured Model prices, Database, and Health. Every step's text was rewritten for clarity, and the app stays fully clickable while the tour runs. It still plays automatically on a fresh install, and you can replay it any time from Help, Show Walkthrough.

## v3.0.2 — 2026-08-23

### New

- **Crash recovery for Claude sessions**: if Claude sessions were running when the app closed (a crash, a force quit, a Mac restart, or a normal quit with sessions still running), the next launch offers to resume them. One click per session, or Resume all. Claude keeps every conversation on disk, so each session reopens in a terminal and picks up exactly where it left off.

### Improved

- **The Session button is now ▶ Claude.** Both tab types have recorded your sessions since 2.0.5, so the old name stopped meaning anything. What the dedicated Claude tab actually gives you: a guaranteed clean launch (no shell in between, so nothing in a shell profile can interfere) and the project's client credentials, which plain shells never receive. New tooltips throughout tell this story where you need it.
- **A running terminal always has its folder in Explorer.** Launching any project from the Launch Claude modal (or resuming a session, or acting on an agent run) now adds that project's folder to the workspace if it was not already there, with its Explorer section and shell appearing like any other folder. No more terminals whose files you cannot see.
- **The walkthrough got a redesign**: the tour card sits in one fixed spot instead of jumping around, the heavy screen dimming is gone in favor of a light glow ring on each highlighted area, and the app stays fully clickable during the tour.

### Notes

- The "+ Add directory" button in the launch dialog is now "+ Track a folder", which is what it always did: register a folder as a project without adding it to the current workspace.

## v3.0.1 — 2026-08-23

### Fixed

- The post-update notification's "Click to see what changed" did nothing; the click was silently lost the moment the notice appeared. Replaced with something better: after every update, the first launch now opens a **What's New** window showing that version's actual release notes, pulled straight from the release page. Close it and it stays gone until the next update. If you're offline at that moment it shows a link instead and fetches the notes next time the window opens.

## v3.0.0 — 2026-08-23

### New

- **Health card** (Settings): the app's own vital signs. Live memory across every process, main-process responsiveness, and a record of both into the flight recorder every 5 minutes (kept 30 days) so a slow leak shows up as a trend instead of a surprise. On every launch the app also checks whether macOS wrote any crash reports for it since the last check and surfaces them with a Reveal button; found reports stay listed until you look at them. Everything stays on your machine.
- **Crash recovery**: failures are contained instead of silent. A crashed or force-killed window is recreated on the spot; terminals, recordings, and agent runs live outside the window and keep going. A window that stops responding offers Keep Waiting or Reload Window. A fatal app error writes its evidence locally and relaunches the app, at most twice an hour, so a real problem can never turn into a relaunch loop.

### Improved

- **Major platform upgrade**: Electron 43 and an updated terminal engine, which fixes two macOS resource leaks that specifically affected long-running use with many terminals.
- **Heavy sessions**: very large session transcripts now import without ever freezing the app, and a terminal flooding output (think `yes`) is flow-controlled the way a real terminal behaves, so the app stays responsive and recording never stalls.
- **Long dock-mode stretches**: memory is now bounded across days of running with the window closed, and the database keeps its write-ahead log trimmed in quiet moments.

### Notes

- The first launch after this update migrates the database. A timestamped backup copy is written automatically beforehand, as always.
- The update installs over the air like any other version; nothing manual is needed.

## v2.0.6 — 2026-08-22

### Fixed
- A shell tab's recording lamp now tracks the claude process itself: the
  glow turns off when claude actually exits, even without a clean goodbye
  (force-killed, or exited immediately after /clear). Glowing means exactly
  one thing: a claude session is being recorded in that tab right now.
  /clear never interrupts the glow; the session keeps recording through it.
- Quitting the app could warn about a live session that had already ended
  in a shell tab. The quit confirmation now reflects reality.

## v2.0.5 — 2026-08-21

### New
- Typing `claude` in a shell tab now records it like a Session tab: the tab
  shows the recording lamp, the visualizer follows along live, and the
  session lands in history with full token and cost tracking. Each claude
  run in the same shell is its own recorded session.
- Recording applies in tracked project folders; in an untracked folder the
  app offers "Track this folder?" once, and runs after tracking are
  recorded. Shells stay yours otherwise: your commands and other tools are
  never recorded, and client credentials are never injected into plain
  shells. No hook reinstall needed.

### Fixed
- A rare event-ordering case could bind a recorded session to the wrong
  conversation after /clear, which then silently stopped recording. The
  session lifecycle now handles that ordering; existing history is
  unaffected.

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
