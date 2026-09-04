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

## v4.6.1 — 2026-09-04

## Open files from Finder

- **CC Blackbox shows up under Recommended.** Right-click a text, configuration, or source file in Finder, choose Open With, and the app is listed with the other editors instead of hiding under All Applications. It never claims to be the default for a type; it just offers.
- **Opening a file actually opens it.** Choosing the app from Open With, double-clicking a file you assigned to it, or dropping one on the Dock icon now lands the file as an editor tab. Before, the app came forward and nothing happened. A file inside your open workspace opens in place; one from anywhere else opens as a standalone tab for the rest of the session. If no workspace is open yet, the file waits and opens the moment you pick one.

## v4.6.0 — 2026-09-04

## The Claude tab, fullscreen

- **Claude tabs always run Claude Code's fullscreen renderer.** The prompt stays pinned to the bottom of the terminal from the first frame and the conversation scrolls above it, on every Mac, regardless of how Claude Code's own rollout would have started you. The mouse works inside Claude: click in the prompt to place the cursor, drag to select (it copies on release), scroll with the wheel at a speed tuned for this terminal. Plain shells are untouched. To get the classic renderer back, put `"CLAUDE_CODE_DISABLE_ALTERNATE_SCREEN": "1"` in the `env` block of your `~/.claude/settings.json`.
- **The Claude button explains itself.** Its tooltip, its walkthrough stop, and the empty-terminal hint list what a Claude tab gives you over a plain shell: a clean direct launch, the project's client Vercel account, and the fullscreen renderer.

## Right-click, at last

- **Right-click in any terminal or editor.** Copy, Paste, and Select All in a native menu (Cut too in the editor). In a terminal, right-click also selects the word under the pointer, so right-click a path and Copy is one motion. Inside a Claude tab, hold Option (or Shift) while dragging to make a native terminal selection instead of Claude's.

## Workspaces

- **Leaving and re-entering a workspace no longer wipes your terminals.** Switching workspaces, or stepping out to the Workspace Picker and back, used to dispose every terminal: you came back to empty tabs that Claude redrew piece by piece, with no history. Terminals now keep their scrollback across the round trip, and you land on the tab you left.
- **Clearer first-run wording.** The New Workspace form now says "Open your first project" and explains that a workspace is a set of project folders (one git repo each), not the folder that holds all of them.

## Sessions

- **A clear button in the search box.** The ✕ inside the Sessions search (or Esc while typing in it) empties the search and brings the full list back.

## Walkthrough

- **Reports, panel by panel.** The guided tour now walks the Cost tab (totals, grouped spend, churn) and the Operations tab (how the work went, the activity calendar, the hour strip, agents, friction) one panel at a time, switching tabs in front of you and explaining what each number means.

## v4.5.4 — 2026-09-03

## Sort your sessions

- **Every column in Sessions sorts.** Click Title, Project, Client, Branch, Date, Duration, Tokens, or Cost to sort by it; click again to flip. Dates and numbers start biggest or newest first, text starts A to Z, and the arrow marks the active sort.
- **The top really is the top.** Sorting runs in the database, not on the rows already on screen, so "most expensive" is the most expensive of all your sessions. The list loads two hundred at a time; a Load more button at the bottom appends the next two hundred in the same order, and search results sort the same way.
- **Nothing gets lost on a live update.** When a session ends while you are looking at the list, the rows you had paged in stay put, and a failed load says so in the toolbar instead of showing an empty list.

## Operations

- **The busiest-day label no longer gets clipped.** On the 90 day and All time views the peak label sits inside the chart with room above it, and swings to the side when the busiest day is at either edge of the range.

## v4.5.3 — 2026-09-03

## A calendar for your work

The Operations tab's Activity section is rebuilt around the way you actually look at a month.

- **A calendar instead of a bar chart.** For ranges up to about five weeks (this month, last month, 30 days) each day is a cell tinted by how much Claude actually worked, with the total in the cell, a thin bar showing which projects, and week totals down the right. Hover a day for the project breakdown; click it to open exactly those sessions. The Daily/Weekly/Monthly toggle is gone because all three are visible at once.
- **One line for long ranges.** 90 days and All time show active time over the range as a single line (per day up to three months, per week beyond) with the busiest point marked, plus one bar of your projects and their totals.
- **Explore any month.** A new Explore button opens a full-window calendar you can page through month by month, with project chips to narrow it. Esc closes it.
- **When you work, with real hours.** Hovering the hour strip now reads like a clock ("2pm to 3pm, 2.8h active") instead of a 24-hour timestamp.
- **Day totals never double count.** A block where two projects ran at the same time counts once, in the cell and in the week.

## Claude in the browser: fewer wasted turns

- **Clicks that open a page now wait for it.** A link that would open a new tab opens in place here, and the click reports the page it landed on instead of the old one, so Claude no longer has to navigate there by hand afterwards.
- **Console and network reads cover the current page.** After visiting two pages, asking for the console returns only the page on screen; the whole history is one option away.

## v4.5.2 — 2026-09-03

## Stays connected, wherever you are

Two fixes from a morning at a coffee shop.

- **The Mac now notices dead relay connections and heals itself.** Public Wi-Fi and network switches can silently kill the connection your paired iPhone relies on, without the Mac ever being told. The app previously waited for a close that never came, so the phone showed a session that would not update. The Mac now checks its own connection every 20 seconds and automatically reconnects within about 45 seconds of it going quiet: no more toggling Mobile off and on after changing networks, and working from a coffee shop behaves like working from home.
- **No more question-mark cursor.** Hovering the Reports info icons, tiles, and column headers kept the regular pointer instead of switching to the help cursor. The hover explanations are unchanged.

## v4.5.1 — 2026-09-02

## Operations, rebuilt

The Reports view finished growing up. Cost got its overhaul in 4.5.0; this release rebuilds Operations around your actual work.

- **Activity, honestly measured.** Active time counts 10-minute blocks where Claude actually produced work, so a tab left open all day inflates nothing. You get totals, an average per working day, and a daily chart stacked by project.
- **When you work.** A 24-hour profile shows which hours of the day your work really happens, in your local time.
- **Friction, explained.** Permission asks and denials (ranked by tool, so you can see where an allowlist rule would help), compactions with the auto count, and a session-health line. Project rows click through to their sessions.
- **Agents in their own place.** Run economics and each run's logged decisions (what was decided, why, and what was considered instead, with crew attribution) live in a section that stays out of the way when no agents ran.
- **Hover anything.** Every section header carries a small i icon explaining what the numbers mean, and tiles and column headers explain themselves on hover.
- **Cost polish.** Totals rows are highlighted across every table, By Model gets a real distinct-sessions total, and the guided walkthrough now tours Reports properly: scope, Cost, then Operations, switching tabs as it goes.

## v4.5.0 — 2026-09-02

## Reports, rebuilt

The Cost report grew up in this release.

- **Accurate pricing, including Fable 5.1.** The app now knows Claude Fable 5.1 and its pricing (including its unusual cache-read rate), and Sonnet 5's price is corrected to the current $2/$10 rate (the announced increase never happened). Cost lookups also fall back within a model family, so a new model variant can never silently count as $0 again.
- **Your history gets re-priced once.** On first launch after this update, the app re-prices all recorded sessions with the corrected table. If your totals shift, that is the report getting more accurate: Fable 5.1 usage used to count as $0.
- **One row per model.** The By Model view collapses id variants into their model family, so Fable 5.1 is one line and internal error records no longer appear as a model.
- **Totals everywhere.** A totals row on every report table, and a summary strip at the top of the Cost tab: total spend, sessions, tokens in and out, cache hit rate, and agent spend for whatever range and filters you have set.
- **Charts you can read.** By Client, By Project, and By Model draw horizontal bars: every name gets its own labeled row, long lists scroll instead of squeezing, and clicking a bar (or any table row) opens the sessions behind it. Time views keep their columns, now also clickable.
- **Price edits re-price history.** Change a model price in Settings and all recorded history follows, safely in the background, even across restarts.

## v4.4.13 — 2026-09-01

### Session names roll everywhere now

- 4.4.12 introduced names that follow the last thing you typed, but a safety rule froze the names of sessions that existed before the update, and resuming a session carried the freeze forward. This release lifts the freeze: every session's name now rolls with your latest message.
- Renames you make yourself stay permanent, exactly as before: from the session list in the app, or by holding a session card on your phone and tapping Rename. If you renamed something during the few hours on 4.4.12, redo that rename once and it sticks for good.

### New here? Installing for the first time

Download `cc-blackbox-4.4.13.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.12 — 2026-09-01

### Session names that actually name things

- A session's name now follows the last real thing you typed, so the list always tells you what each session is currently about. Slash commands like /catchup never become a name, and text injected behind the scenes cannot either.
- Rename a session yourself and that name is permanent for its lifetime: from the session list in the app, or by holding down a session card on your phone (companion app build 10, on TestFlight now) and tapping Rename.
- /clear starts a session fresh and unnamed; resuming in the tab keeps the name, since that continues the same conversation.
- Names you set before this version are left exactly as they are.

### New here? Installing for the first time

Download `cc-blackbox-4.4.12.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.11 — 2026-09-01

### Toggles everywhere

- Every on/off control in the app is now the same sliding toggle you know from Settings: the agent editor's "Run in a git worktree", "Auto-retry once on failure", and "Log decisions", plus the delete confirmation's "Don't ask again". No more checkboxes. The Extra abilities rows stay as selection chips on purpose; they are a pick-list, not settings.

### The browser follows Claude's driving

- When a Claude session drives the embedded browser, the Cockpit now flips to the Browser pane automatically, the same way the Simulator pane already did. You see what Claude is doing instead of it happening behind another tab, and the very first action waits for the pane to be on screen so nothing is lost while it appears.

### New here? Installing for the first time

Download `cc-blackbox-4.4.11.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.10 — 2026-09-01

### A continued conversation keeps its name

- Resuming a session (in the tab with /resume, or reopening one with Resume) no longer renames it. Before this, the next thing you said after resuming became the session's name on the phone and in the session list. A continued conversation now carries its original name for good.

This is a small companion release to 4.4.9; phone-side fixes from today (markdown tables, the stop button, always opening at the newest message) ride the companion app on TestFlight.

### New here? Installing for the first time

Download `cc-blackbox-4.4.10.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.9 — 2026-09-01

### Update problems now tell you what actually happened

- When an update cannot be prepared, the message now names the real reason and the fix: a failed download points at your connection, a corrupted file says it was never installed and retries fresh, a signature refusal says the file was rejected to protect you, low disk space says how much to free, and a permissions problem names the Applications folder. Before this, every failure showed the same generic "download it manually" note with an internal error code in parentheses.
- The Check for Updates window matches: its headline and detail now describe the same problem, and it only blames the network when the update service really was unreachable.

### Session names stay yours

- A session's name comes from the first thing you typed into it. Background Claude runs that share a terminal (like a pre-push code review) could previously get their text stamped as the session's name on the phone and in the session list before you typed anything. That door is closed; names only ever come from your own session.

### New here? Installing for the first time

Download `cc-blackbox-4.4.9.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.8 — 2026-09-01

### Your phone now sees the whole conversation

- What Claude says between tool calls now reaches the phone, and it is kept for the next time you open the session. Before this, those messages could vanish if the phone was locked or sitting on the sessions list; a decoding bug also silently dropped them even while you watched. Both are fixed.
- Headless Claude runs spawned inside a tab (a pre-push code review, a scripted `claude -p`) no longer appear inside the chat you are watching. They stay in the Mac's records; your phone chat shows only your own conversation.

### Tap a tool call for detail

- Tool rows on the phone now expand on tap to show the full tool input, in a monospaced card. A file edit shows a small red and green diff. Tap again to collapse. Rows without extra detail show no chevron.
- Anything that looks like a secret (API keys, tokens, passwords, private keys) is masked on the Mac before it is sent to the phone.

### A stop button on the phone

- While Claude is working (or waiting on a permission), the send arrow becomes a red stop button. Tap it to interrupt the turn, exactly like pressing Escape in the terminal. When the session is ready for you, the arrow returns.
- Opening a session now always lands on the newest message, even when nothing is running.

The phone-side half of these features needs companion app build 7 (1.0.3), rolling out on TestFlight now. The Mac-side fixes work with any phone build.

### New here? Installing for the first time

Download `cc-blackbox-4.4.8.dmg`, open it, and drag CC Blackbox into Applications. The other files on this release are for the app's built-in auto-updater; you never need them.

## v4.4.7 — 2026-09-01

### Restart to Update actually comes back

This fixes the update relaunch for good. Restarting to update swapped the app correctly, but the copy the updater relaunched was starting in a headless mode with no window, so it bounced in the Dock and vanished, and you had to click the icon to open it. It now relaunches as a normal app and comes to the front on its own.

Because this fix lives in the part of the app that performs the swap, you will see the smooth relaunch starting with the update AFTER this one. Updating into this version may still need one click to reopen; every update after that comes back by itself.

### Installing

Already on 4.4.4 or newer? Do nothing; it updates itself. If the relaunch leaves it in the Dock without opening, click the icon once. Fresh install or stuck updater? Download `cc-blackbox-4.4.7.dmg` below, quit CC Blackbox, and drag it into Applications. The other files on this release are for the built-in auto-updater.

## v4.4.6 — 2026-08-31

### Fixes "Could not check for updates"

If Check for Updates started reporting that the update service was not reachable, this fixes it. The cause was a leftover download folder from an earlier version that the updater could not clean up on Apple's filesystem; that cleanup failure was being misreported as a network problem, so the check failed instantly every time. The updater now retries the cleanup, and a single stubborn leftover can no longer fail the whole check. Machines that hit this heal themselves on the next check once they are on this version.

### Installing

Already on 4.4.4 or newer with a working updater? Do nothing; it updates itself and applies on your next quit. If your updater is stuck on the error above, download the `.dmg` from this release (being re-attached shortly; if it is not present yet, use the previous release's DMG or wait for auto-update), quit CC Blackbox, and drag it into Applications this once. The other files on this release are for the built-in auto-updater.

## v4.4.5 — 2026-08-31

### An honest Xcode checklist

The iOS Simulator setup strip's Xcode row now tells the truth on every machine:

- The "Select Xcode" button appears only when a real Xcode is actually installed, and its command points at the copy that was found (wherever it lives, spaces and all). On a Mac with no Xcode, the row sends you to the App Store instead of offering a command that cannot work.
- The raw command no longer crowds the row and truncates the explanation. Hover the Copy or action button to see exactly what will run.

### Installing

Already on 4.4.4? Do nothing; the app updates itself and applies the new version the next time you quit. On 4.4.3? That build's updater cannot apply updates; download `cc-blackbox-4.4.5.dmg` below, quit CC Blackbox, and drag it into Applications (one time; the app also tells you this on its own). New here? Same DMG steps. The other files on this release are for the built-in auto-updater.

## v4.4.4 — 2026-08-31

### Updates that finish the job

Two updater fixes from today's field testing:

- The silent non-update is gone. A leftover hidden backup from an earlier update could block every later one: the swap quietly failed and the app stayed on its old version while claiming an update was ready. Backup names are now collision-proof, the startup cleanup treats old backups as removable no matter what, and any cleanup failure is written to the install log instead of vanishing.
- Together with 4.4.3's verified relaunch, Restart to Update now does the whole job: quit, swap, come back.

### /clear follows through to your phone

When you run /clear in a terminal session, the phone app now clears that chat and continues in the fresh session, exactly like the terminal does. No more dead "Session ended (clear)" chat next to a mysterious new session. (Requires the phone app 1.0.1, arriving via TestFlight; older phone builds keep the previous behavior.) This also fixed a false "session ended" push notification that fired on every /clear.

### The very last hyphens

The window title bar and the workspace picker still spelled the old hyphenated name. Every user-facing surface now says CC Blackbox.

### Installing

On 4.4.3? That build's updater helper shipped with a defect that stops it from applying updates, so this one time, update by hand: download `cc-blackbox-4.4.4.dmg` below, quit CC Blackbox, and drag it into Applications. (If you wait instead, the app notices the failed update on its own after a couple of tries and shows you the same instruction.) On 4.4.0 through 4.4.2? Do nothing; the app updates itself and applies the new version the next time you quit. Otherwise download `cc-blackbox-4.4.4.dmg` below, quit CC Blackbox if it is running, and drag the app into Applications. The other files on this release are for the built-in auto-updater.

## v4.4.3 — 2026-08-31

### Restart to Update actually restarts

Clicking Restart to Update used to quit the app and then, sometimes, nothing came back. The update itself always applied; macOS just quietly ignored the relaunch request for the freshly swapped app. The updater's helper now verifies that the app really launched and retries with increasing waits until it does, logging every attempt. If macOS refuses repeatedly, the log says so plainly and you can open the app yourself; your update is already installed either way.

### The name, everywhere this time

A handful of remaining places still said "CC-Blackbox" with the dash: notification banners, a certificate dialog, a sync error message, the hook uninstall preview, and a few messages Claude relays. They all say CC Blackbox now, matching the menu bar rename from 4.4.1.

### Installing

Already on 4.4.0 or newer? Do nothing; the app updates itself and applies the new version the next time you quit. Otherwise download `cc-blackbox-4.4.3.dmg` below, quit CC Blackbox if it is running, and drag the app into Applications. The other files on this release are for the built-in auto-updater.

## v4.4.2 — 2026-08-31

### A much cleaner phone experience

This release fixes everything the phone companion surfaced in today's field testing:

- No more floods of machine text. The Mac's internal plumbing (background task notices, injected review prompts, system reminders) never appears in your phone chat anymore; you see the same conversation the terminal shows.
- Your first message no longer appears twice. Opening a session on the phone used to duplicate the first prompt (usually a slash command) because the transcript and the live event stream both contributed it at the boundary. One of them now yields.
- Very long messages are capped at a readable length instead of taking over the screen, and subagent side-conversations stay out of the main chat.

The matching phone app update (TestFlight build 4) also gives the model and context readout some breathing room above the composer.

### Installing

Already on 4.4.0 or newer? Do nothing; the app updates itself and applies the new version the next time you quit. Otherwise download `cc-blackbox-4.4.2.dmg` below, quit CC Blackbox if it is running, and drag the app into Applications. The other files on this release are for the built-in auto-updater.

## v4.4.1 — 2026-08-31

### The app is finally named right everywhere

The bold name in the macOS menu bar now reads CC Blackbox, matching the rest of the app. The About, Hide, and Quit menu items say CC Blackbox too. (For the technically curious: the display name is localized on top of the internal bundle identifiers, which keep their original spelling so updates, permissions, and stored credentials are untouched.)

### Updater housekeeping

When an update applies, the copy of the old version kept next to the app for safety is now named after the version it actually contains. Purely cosmetic, but honest names matter when you are poking around after the fact.

This release is also the first delivered end to end by the new updater that shipped in 4.4.0. If it arrived automatically and applied when you quit: that was it, working as designed.

### Installing

Already on 4.4.0? Do nothing. The app updates itself; the new version applies the next time you quit. New here, or still on 4.3.x or older? Download `cc-blackbox-4.4.1.dmg` below, quit CC Blackbox if it is running, and drag the app into Applications. The other files on this release are for the built-in auto-updater.

## v4.4.0 — 2026-08-31

### A brand new auto-updater

CC Blackbox 4.4.0 replaces the old update machinery from the ground up. Updates now download, verify, and install completely outside the running app, which eliminates the update crash a few of you hit where the app died mid-update.

What the new pipeline does for you:

- Downloads are checksum-verified end to end, and the update's code signature is verified against Apple's chain before anything is swapped in.
- The swap itself happens only when you quit, handled by a tiny helper that waits for the app to fully exit. Your terminals, sessions, and runs are never interrupted.
- If anything goes wrong at any step, the app tells you plainly and points you at the manual download; it never retries forever or leaves you half-updated.
- Update checks still wait for an idle moment, so they never compete with live recording or running agents.

### Finds the right Claude install

If your Mac has more than one Claude Code install (for example, an old npm install left behind next to the native one), CC Blackbox now prefers Claude's own installs and verifies that a candidate actually runs before using it. A dead leftover shim can no longer win over your working install.

### Simulator setup fixes

The iOS Simulator setup checklist no longer mistakes Command Line Tools for a full Xcode, and when Xcode needs attention it now points you at the specific remedy (select it, finish first launch, or accept the license) instead of a generic message.

### Installing this update

Because this release replaces the updater itself, install it once by hand: download `cc-blackbox-4.4.0.dmg` below, quit CC Blackbox fully (Cmd-Q), and drag the app into Applications. The other files on this release are for the app's built-in auto-updater. After this one manual install, updates apply automatically again.

## v4.3.1 — 2026-08-31

Fixes 4.3.0, which could not start and was pulled within the hour. If you caught 4.3.0, install this version's DMG manually (drag CC Blackbox into Applications over the broken copy; your data and settings are untouched). This release carries everything 4.3.0 had: the phone status strip and the CC Blackbox name.

## v4.3.0 — 2026-08-31

Pulled: this build crashed at launch and was removed from the releases page. Use v4.3.1.

A status readout on the phone, and the app is now called CC Blackbox.

### New
- The iPhone companion shows a status strip above the chat: the session's model and how full its context window is, with a bar that goes green, yellow at 60 percent, and red at 85. It works for every session automatically; nothing to install or configure. Update the phone app together with the Mac.

### Changed
- The app's name is now CC Blackbox (two words) everywhere you read it: menu bar, dock, About window, dialogs, and docs. Nothing moves on disk; updates, settings, and your database are untouched.

## v4.2.5 — 2026-08-31

Clearer statuses on the phone, and the update warning tells you what it sees.

### Improved
- The phone now tells "Ready" apart from "Awaiting your input". A session just sitting at its prompt shows a quiet grey "Ready"; the green "Awaiting your input" (and its push and "needs you" count) appears only when Claude has actually finished a turn for you. Update the iPhone companion together with the Mac; older phone builds hide Ready sessions until they change state.
- Session cards on the phone no longer use a slash command as the session name. A session whose first prompt was a command (for example `/catchup`) shows just its project and status.
- The "Something is running right now" warning under Check for Updates now names what it sees (for example which project has a recording session, or a queued agent run), so you can tell a real trigger from a stale tab at a glance.

## v4.2.4 — 2026-08-31

The phone's chat now mirrors the terminal.

### Fixed
- Slash commands in the phone's history show as the command you typed (for example `/catchup`) instead of the command's internal instructions. Injected helper text the terminal never displays is now hidden on the phone too.

## v4.2.3 — 2026-08-30

Resumed sessions now show their history on the phone.

### Fixed
- Opening a resumed session in the iPhone companion shows the conversation right away. Before, a session opened with `claude --resume` could appear empty on the phone until you typed something on the Mac.

## v4.2.2 — 2026-08-30

Opening a session on your phone now shows the conversation.

### Fixed
- The phone's chat opens with the session's history instead of starting empty. Resumed sessions especially: the terminal shows the whole resumed conversation, and now the phone does too (the most recent messages, with new activity streaming in live as before).

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
