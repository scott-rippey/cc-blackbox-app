# CC-Blackbox

A local macOS IDE and flight recorder for Claude Code. Every session is
recorded like a black box: what Claude did, what it touched, what it cost.
Watch it live, replay it later, and run scheduled headless agents on your
repos.

**[Download the latest release](https://github.com/scott-rippey/cc-blackbox-app/releases/latest)** — the `.dmg` file is the installer; the other files on the release are the app's auto-update machinery. See what changed in each version: [CHANGELOG](CHANGELOG.md) or the [release notes](https://github.com/scott-rippey/cc-blackbox-app/releases).

## Why

Claude Code is powerful and opaque. It edits files, runs commands, spawns
subagents, and spends tokens, and when the terminal scrolls past, that history
is gone. CC-Blackbox keeps it. Every prompt, tool call, file touch, subagent,
failure, and dollar is captured to a local database on your Mac, permanently
reviewable.

## What you get

**A flight recorder.** Claude Code hooks stream every event of every session
(interactive or headless) into a local SQLite database: prompts, tool calls,
files read and edited, subagent activity, compaction, session end. Tokens and
cost are computed per session against an editable price table, including cache
pricing. Full-text search across everything any session ever did.

**A live cockpit.** The Ops Board is a real-time node scene: the orchestrator
with its current action, one card per active subagent showing what that agent
is doing right now, and the working set of files. The Activity EKG draws the
session as a waveform: reads, edits, commands, failures, compaction. File
activity lights up directly in the file tree as Claude works.

**Flight playback.** Replay any recorded session as an animated timeline at
1x to 100x with a scrubber: the same Ops Board and feed views, plus a per-tool
swimlane. Watch how a two-hour agentic session actually unfolded in two
minutes.

**An agent harness.** Define headless agents in a GUI: prompt with launch-time
slots, model and reasoning effort, plain-English permission presets, budget
and turn caps, daily or weekly schedules. Give an agent a subagent crew, with
its own model and tools per member, and the orchestrator delegates
automatically. Agents can run in throwaway git worktrees so your working files
are never touched, and every run's report can be picked up in an interactive
session that resumes the agent's own conversation.

**A workspace.** Multi-root workspaces with an editor (CodeMirror, markdown
and JSON preview modes, PDF and image viewers), plain shell and recorded
Claude terminal tabs, fuzzy quick open, and ripgrep-powered workspace search.

**Reports.** Cost and usage grouped by model, project, and client, over time.
Per-client Vercel tokens live in the macOS Keychain and inject only into that
client's sessions.

## Your data stays yours

Everything lives in a local SQLite database on your Mac
(`~/Library/Application Support/CC-Blackbox/`). No telemetry, no accounts, no
cloud component. The app talks to exactly two things: your Claude Code CLI
and, if you connect them, your own Vercel tokens.

## Install

1. Download the `.dmg` from the latest release above.
2. Open it and drag **CC-Blackbox** to Applications.
3. First launch: macOS shows the standard "downloaded from the internet"
   confirmation. Click Open.
4. In the app: Settings → Install hooks (required for session recording; the
   hooks are observe-only and leave your existing settings untouched).

Install once. The app keeps itself current from this repository's releases
automatically; a fresh download is only needed for a new machine.

## Requirements

- Apple Silicon Mac on a recent macOS
- [Claude Code CLI](https://code.claude.com) installed and authenticated

## About

The application is signed and notarized. The source code is not published;
this repository hosts installers and release notes. Version history:
[CHANGELOG](CHANGELOG.md).
© Scott Rippey. All rights reserved.
