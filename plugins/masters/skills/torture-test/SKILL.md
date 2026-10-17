---
name: torture-test
description: Exhaustive adversarial QA of one or more of the user's projects. Fans out parallel background tester agents that fuzz, simulate failures and drive the real apps, verifies the worst findings, runs a hardware/desktop gap round with the user's consent, then delivers ranked bug tables and PDF reports. Report only; never fixes. Run via /torture-test.
argument-hint: "[project folder names, or blank to choose]"
disable-model-invocation: true
---

# Torture test

Find every real bug in the user's projects by simulating unexpected situations in every nook and cranny. **Report only. Never edit project source.** Fix nothing until the user picks bug numbers from the final report.

Time is no object. Rigor beats speed.

## 0. Targets

- `$ARGUMENTS` names project folders under the working directory. If it is blank, list the folders and ask with AskUserQuestion (multiSelect).
- Check memory first. Excluded projects stay out: PowerPipe (`pipeline/`) and the web equalizer are abandoned, and Cadence is the only music player.
- Survey each target with one `ls`/`find` (depth 2, skipping node_modules, .venv, .git, bin and obj). Read its README and the memory notes about it.

## 1. Round 1: parallel deep testers

Spawn one `general-purpose` Agent per scope, all in ONE message, `run_in_background: true`.

Split big apps into two testers:
- Electron: main process/IPC/scanning in one, renderer/UI/logic in the other.
- Web with a backend: frontend in one, sync/rules/tools in the other.

Small things (a plugin, a single HTML file) get one tester each.

Every brief must contain:

**Hard rules (paste verbatim):**
- REPORT ONLY. Never edit files under the project. Harnesses, fixtures and copies go in `<scratchpad>\<scope>\`.
- Never use real user data. Use an isolated `--user-data-dir`, or redirect `APPDATA` to scratch. Real media libraries are READ-ONLY. Don't touch `release/` or `dist/` outputs.
- Never deploy, git commit/push, or read or write production Firebase or other cloud backends. Block prod hosts in test browsers and use emulators.
- Scripts that hit external APIs run on a scratch copy, so they cannot overwrite project outputs. No request loops. Never print API keys.
- Audio at volume 0 / `--mute-audio`. No adb/phone. Never type passwords, PINs or credentials. Never change Windows system, device or power settings.
- Kill every process you start.
- Electron apps: launch `node_modules/electron/dist/electron.exe` directly, because `npx electron` fetches the wrong version. Drive the app over CDP (`--remote-debugging-port`).

**Attack surface (adapt per scope, and tell them to go beyond it):**
- **Inputs:** empty, 0-byte, truncated or corrupt files, wrong extensions, huge files, unicode/Gurmukhi/RTL/emoji names, paths over 260 chars, quotes, `& % ^ ! [] ;`, and leading dashes (argument injection). Also UNC paths, path traversal, and symlink/junction loops.
- **Persisted state:** invalid JSON, plus valid JSON of the wrong type or shape (null, array, string, a wrong field type). Old schemas, partial writes, crash mid-write, a full or blocked quota. Two tabs or instances writing at once.
- **State and races:** rapid click/skip/seek storms, actions mid-animation or mid-load, double events, a queue edited while playing, and the same item processed twice.
- **Errors:** missing binaries, network down, 429/5xx/timeouts, a file deleted mid-use, a device removed, a child process killed.
- **Boundaries:** 0, 1, max, negative, NaN, Infinity, 1e308. Time zones (Kiritimati, Adak, Kolkata), DST, midnight rollover, far-future clocks.
- **Security:** XSS via metadata, hash, storage or remote data (use a `window.__xss` canary). IPC channel exposure, navigation and new-window guards, CSP, contextIsolation/sandbox, and file operations reachable from the renderer. Firestore/DB rules as an allow/deny matrix on the emulator.
- **Sync:** offline, first-sync failure, clock skew, last-write-wins loss, concurrent devices, account switch, delete races, malformed remote docs.
- **DSP/audio:** compare filter response numerically with the RBJ cookbook at 44.1, 48, 96 and 192 kHz. Also measure limiter ceiling and transparency, clicks on parameter or track change, and gapless transitions.
- **UI:** every view and control. Viewports from 320 px to 4K, zoom 50–400%, DPI 1–2×, reduced motion, light/dark, forced-colors, keyboard-only use, focus traps, axe-core, contrast. Check horizontal overflow with scrollWidth vs clientWidth.
- **Leaks and performance:** hundreds of transitions or ops, with heap, listener, DOM-node, handle and child-process counts kept flat. Also CPU use while paused or hidden, and 10k-item lists.
- **Lifecycle:** second instance, quit or crash mid-operation, orphan processes, window state restore with off-screen coordinates.
- **Claude plugins/skills:** manifest and frontmatter validity, `claude plugin validate --strict`, name collisions, referenced paths and tools existing, unbounded loops, edits without approval, prompt-injection exposure, trigger breadth.

**Output contract:** every finding gets severity (critical/high/med/low), file:line, an exact repro, observed vs expected, and CONFIRMED (reproduced) or SUSPECTED (code-read only; try hard to confirm). The final reply holds the ranked findings, then areas tested clean, then harness paths. No fixes, no praise.

While they run:
- Answer the user briefly.
- Never predict results.
- When a tester reports, check its top 1–2 findings yourself at the cited file:line before you trust them.

## 2. Round 2: gap tests (needs consent)

Collect every "not tested / skipped" item from round 1. Ask the user with AskUserQuestion before any of these:
- **Downloads or installs:** name each package, source and size. For example JDK 21 via winget for the Firestore emulator, or firebase-tools and rules-unit-testing installed locally in scratch.
- **Hardware actions:** live exclusive audio, unplug/replug, real sleep.
- **Running the Claude CLI.**

Then spawn:
- **ONE desktop/hardware agent.** Computer-use, the DAC and sleep must never run in two agents at once.
- **Separate headless agents** for emulator, zoom, two-tab and CLI work.

Desktop-agent protocol (paste into its brief):
- Talk to the user only through VBScript message boxes. Run `wscript.exe x.vbs` with `WScript.Quit MsgBox("…", vbOKCancel + vbSystemModal, "QA test")` and read the exit code. Cancel means skip that test. Log every prompt and answer to `prompts.log`.
- Before any exclusive-mode or DAC test, the box says: headphones off, IEM earpieces detached.
- The user's DAC (KM-HIFI-384KHZ) uses **jack detection**: pulling the headphone cable out of the DAC jack removes the Windows endpoint. "Unplug" = pull the cable out of the DAC jack. "Replug" = put it back, earpieces still detached. Never ask for USB removal. Word boxes so they can't be misread.
- **Real sleep:** first simulate (electron main `--inspect`, emit powerMonitor `suspend`/`resume`). Then show a box that states the exact duration (30–90 s) so the user can set a phone timer. Then arm a waitable timer (`CreateWaitableTimer` + `SetWaitableTimer` fResume=true) and call `SetSuspendState(false,false,false)` in the same PowerShell process. No scheduled tasks, no power-setting changes. CDP sockets die across sleep, so reconnect before judging the app.
- Computer-use typing goes through the clipboard and overwrites it, so tell the user afterwards. Never close the user's own apps without a box asking first.

Claude CLI facts:
- The desktop app is MSIX. The CLI is at `%APPDATA%\Claude\claude-code\<ver>\<hash>\claude.exe` from inside the app, but outside the app (user's terminal) it is at `%LOCALAPPDATA%\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Roaming\Claude\claude-code\<ver>\<hash>\claude.exe`.
- If `claude auth status` says loggedIn:false, give the user the PowerShell command `& "<outside path>" auth login` and wait. Never log in for them.
- In Git Bash set `MSYS_NO_PATHCONV=1`, or `/skill` arguments get mangled into Windows paths.
- Run CLI sessions one at a time: parallel runs coincided with a desktop-app crash once. Use `-p`, `--max-turns 40`, a 900 s timeout, `acceptEdits` and a fresh copy of the target per run. Never `--dangerously-skip-permissions`.

If the desktop app crashes, the agents stop:
1. List leftover processes by command line (`Get-CimInstance Win32_Process`). Kill only the ones from your agents; leave the user's own processes.
2. Resume each agent with SendMessage, telling it where it stopped.
3. Re-show a consent box before any hardware step.

## 3. Report

1. Merge every tester's findings per project. Dedupe across testers (combine their IDs) and rank worst to least: Critical = data loss, won't start, security or loud audio; then High, Medium, Low. Put suspected/code-only items in a separate note.
2. Give the user one table per project in chat: `# | Bug | Where | ID` in plain language, plus a line naming the root cause where several bugs share one.
3. Build a PDF per project. Write an HTML file linking `report.css` from this skill folder (copy the css next to the HTML). Use this structure: a header (title, subtitle, date/scope/method meta row), `.summary` count cards (`.crit/.high/.med/.low`), then per severity an `h2` with `<span class="pill p-crit|p-high|p-med|p-low">` and a table using `td.num`, `td.where` and `td.id`. End with `.note` boxes for root cause, suspected items and clean areas, then a footer legend for the IDs.
4. Print with headless Edge: `msedge --headless=new --disable-gpu --user-data-dir=<scratch>\edge-ud --no-pdf-header-footer --print-to-pdf=<Testing>\<Project>-Bug-Report.pdf file:///<html>`. Take a `--screenshot` at `--window-size=794,1400` and look at it before sending.
5. Send the PDFs with SendUserFile and open them in the user's browser (`Start-Process <pdf>`).
6. Ask which bug numbers to fix. Do not start fixing on your own.
