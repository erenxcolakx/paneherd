# Changelog

## 0.9.0

- 7-day Pro trial: *Try Pro free* in the sidebar, in every Pro prompt, or **Paneherd: Start 7-Day Pro Trial** unlocks every Pro feature for 7 days, with no card or account. It can be started once; Settings Sync carries it to your other machines. The sidebar shows the days left. When it ends, open panes keep running and Paneherd says once what now needs Pro.
- Pro now costs $2.99, once.
- Codex and Copilot usage limits: the sidebar's usage block shows Codex's limits (5-hour and weekly, or your plan's windows) under Claude Code's, read from the session logs Codex writes on this computer (`~/.codex/sessions`, or `$CODEX_HOME`). They update after each Codex reply and show while a Codex pane is open or a window has not reset yet. Copilot's monthly premium requests show there too once you click *Connect GitHub* and allow it: Paneherd then asks GitHub (`api.github.com/copilot_internal/user`) with your VS Code GitHub sign-in every few minutes while the sidebar is open. Gemini and Cursor keep no usage Paneherd can read. `paneherd.usageLimits` turns all of them off.
- New Marketplace name: Paneherd: Multi-Agent Console for Claude Code & Codex.

## 0.8.0

- Phone remote: *Pair a Phone…* shows a QR code; scan it and allow the phone in VS Code. On paneherd.com/remote the phone then shows every window's panes and their status, sends prompts to the panes you pick and, with Pro, answers permission prompts, from anywhere. Messages go through Paneherd's relay (relay.paneherd.com) encrypted end to end; the relay cannot read them and keeps nothing. The QR code works once, for 5 minutes, and each phone can be disconnected on its own (*Disconnect Phones…*). A phone icon in the status bar shows while a phone is connected. Setting: `paneherd.remoteRelay`.
- Terminal tabs are named after the pane: a new pane's tab showed only the folder name until it restarted; it now takes the first prompt (or the name set with the pencil) as soon as it is the active terminal. A tab you renamed yourself in VS Code keeps your name.

## 0.7.0

- GitHub Copilot CLI, Cursor CLI and Gemini CLI panes: choose them next to *New session*, or run *New Copilot Session*, *New Cursor Session* or *New Gemini Session*. Copilot panes show their status through hooks Paneherd passes in a plugin folder; Cursor and Gemini panes run without status for now (their CLIs do not report the end of a turn). Broadcast, arrange and restart (resuming the session) work for all three. Settings: `paneherd.copilotPath`, `paneherd.geminiPath`, `paneherd.cursorPath` and their `…ExtraArgs`; `paneherd.defaultAgent` takes the new agents.
- Panes survive *Reload Window* and extension updates: the running panes are picked up again in place, with their status, name and color, instead of being offered for reopening. After you quit the editor, terminals VS Code starts again on its own are closed and their sessions offered for reopening as before.

## 0.6.0

- Pro is on sale: *Upgrade* opens https://paneherd.com/#pricing ($4.99 one-time, perpetual key, 14-day refund).
- Fixed: a pane whose terminal had ended without VS Code saying so (seen when a CLI exits by itself) stayed in the sidebar, counted against the free plan's 4 sessions, and did nothing when you clicked it. Paneherd now notices that the process is gone, when you click a pane or session, when you open a session and every 30 seconds, and removes the pane.
- Codex panes: choose *Codex* next to *New session* (or run *New Codex Session*) to run the Codex CLI in the same grid. Broadcast, arrange, restart and reopening after a reload work. Status appears from the first prompt on, once you choose *Trust all and continue* when Codex asks about Paneherd's hooks (asked once); until then the pane shows `—`. Settings: `paneherd.codexPath`, `paneherd.codexExtraArgs`, `paneherd.defaultAgent`.
- Pane colors and icons: new panes get the next free color on their terminal tab and sidebar row (`paneherd.autoColor`). The pencil on a pane sets its name, color and icon; the terminal tab takes them when the pane restarts.
- Sidebar personalization: `paneherd.density`, `paneherd.fontScale`, `paneherd.paneDetails`, `paneherd.sessionPrompt`, `paneherd.statusPalette` (with a color-blind-safe palette) and `paneherd.statusColors`.

## 0.5.0

- Chrome companion: pair the new Paneherd Companion browser extension with *Connect the Chrome Companion…* and it shows on its toolbar badge how many panes need you, lists every window's panes, and opens the editor at a pane. With Pro it also notifies you when a pane needs you and answers permission prompts (Allow, Always, Deny). It talks only to 127.0.0.1; *Disconnect Chrome Companions* forgets every paired browser.

## 0.4.1

- Sidebar: the per-pane usage share (≈N%) now sits in its own column next to the refresh button instead of crowding the status line, and the locked forecast hint flows with the layout.

## 0.4.0

- Per-pane usage: each pane shows roughly how much of the 5-hour session limit it has used, subagents included.
- Limit forecast (Pro): under the session meter, when you hit the limit at the pace of the last 30 minutes, or where you will be when it resets.
- Both are estimated from your local Claude Code transcripts, with tokens weighted by API prices.

## 0.3.2

- Link to the new website, [paneherd.com](https://paneherd.com).

## 0.3.1

- Screenshots on the Marketplace page.

## 0.3.0

- Task queue (Pro): queue a prompt instead of sending it (*Queue*, or Ctrl+Enter). The first selected pane to end its turn takes it, one prompt at a time. The queue can be paused, and a prompt that has not started can be removed. Command: *Queue a Prompt for Open Panes…*.
- *Arrange Panes in a Grid* no longer breaks after you close a pane in the middle of the grid.
- New Marketplace name: Paneherd: Multi-Agent Console for Claude Code.
- *Upgrade* says Pro licenses are not on sale yet and offers to enter a key you already have.

## 0.2.0

- New name: Paneherd (was Agent Grid). Settings and commands moved from `agentGrid.*` to `paneherd.*`, and license keys now start with `PH1`.
- Usage limits at the top of the sidebar: how much of the current session (5 hours) and the week you have used, and when each resets. Read from Claude Code's status line input; a pane gets Paneherd's status line only when you and the project have none.
- Permission inbox (Pro): every pane's permission prompts in the sidebar, with Allow, Always, Deny and Terminal, and in notifications. Claude's own dialog stays up in the pane; the first answer wins, and a prompt answered in the pane leaves the inbox. A pane answered from the sidebar shows *Working* right away, not *Needs permission* until the tool finishes.
- Conflict guard (Pro): Claude asks before a pane edits a file another open pane changed in the last 10 minutes.
- Loop and stall alarms: repeated identical tool calls, failing tool calls in a row, and working panes silent for `paneherd.stallMinutes`.
- Broadcast is part of the free plan.
- Broadcast waits for a pane that is still starting, and presses Enter again if Claude did not take the prompt. Claude Code reports a session as started before its prompt accepts typing.
- New settings: `paneherd.usageLimits`, `paneherd.permissionInbox`, `paneherd.conflictGuard`, `paneherd.stallMinutes`.

## 0.1.2

- A new Activity Bar icon (a split terminal window) that no longer looks like the Extensions icon.
- The Paneherd sidebar opens once after install so its icon shows up in the Activity Bar.

## 0.1.1

- Search finds text inside conversations, not only titles: each matching session shows the matching messages, who wrote them, and how many more there are. Turkish letters match with or without diacritics (izgara finds ızgara).

## 0.1.0

First preview.

- Open new and saved Claude Code sessions as editor-area terminals in a grid.
- Status for each pane comes from Claude Code hook events.
- Sidebar with open panes, a session browser, and a broadcast box.
- Offline license keys; a free plan with up to 4 concurrent sessions.
- English and Turkish.
