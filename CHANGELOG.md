# Changelog

## 0.11.0

- Run summary: when an agent ends a turn, the pane keeps a summary of it: time, prompts, files changed, whether tests passed, and any loop or failure warning. It holds counts only, never file names, prompts or commands, and stays in memory.
- A **Summary** link on the pane's row in the sidebar saves its last run as a PNG or SVG card, or as a README badge (the badge's Markdown is copied to the clipboard). Free and Pro. Two commands do the same for the newest run: *Save Last Run Summary Card* and *Save Last Run Badge*.
- Files changed and test results come from Claude Code's tool hooks, so other agents' summaries show time and prompts only. Nothing is sent anywhere.
- Sessions: filter saved sessions by project. Tick one or more folders in the new dropdown; it works together with the agent filter and the search box, and your choice is remembered.

## 0.10.5

- Custom local agents: configure up to three local CLI agents with a path, label and extra arguments; configured agents appear in the New Session picker.

## 0.10.4

- Usage: refreshes Claude Code, Codex and Copilot usage even when agents are started outside Paneherd.
- Usage: the sidebar shows one selected agent at a time; choose it from the Usage dropdown.
- Sessions: filter saved sessions by agent.
- Forecasts: adds pace-based forecasts for reported usage windows.

## 0.10.3

- Usage limits: when a pane reports Claude Code's limits, Paneherd also writes them to the Usage Meter's file (`~/.paneherd-meter/status.json`). The status-bar meter was staying on an old figure (for example 5%) while the sidebar already showed the live one from the pane (71%), because only sessions outside a pane used to update that file.

## 0.10.2

- Phone remote: a pane that has a question (an MCP server asking for your input, an agent needing a choice) now shows on the phone with the question's text, and Accept and Cancel buttons. Accept asks twice (*Confirm*) and then presses Enter in the terminal, on the option the dialog highlights; Cancel presses Esc. Nothing is pressed in a pane that is not asking. Pro, like answering permission prompts; the phone page needs this version of the extension for the buttons. Phone notifications for a question now carry its text. The phone cannot see the terminal, so read the question on the card first; a long tool call above it is shortened to 200 characters.
- Sidebar: a pane's agent badge, project and status stay on one line when the sidebar is narrow (the project name is shortened), so a pane takes two lines instead of four.
- Sidebar: with the search box still focused, the list no longer jumps back to the top on every update (every few seconds while panes work).

## 0.10.1

- Usage limits: the sidebar now also takes Claude Code's latest report from the Usage Meter's file (`~/.paneherd-meter/status.json`) when it is newer than the last one a pane sent. Claude Code sessions outside a pane report only there, so the five-hour bar could sit hours behind (5% while the real figure was 71%) next to a pace forecast that already counted the newer spend. A report older than 30 minutes now says *Updated 3 h ago* under the session bar.

## 0.10.0

- Sound: set `paneherd.sound` to *finished* (a pane ended its turn and waits for your next prompt) or *needsYou* (also a question or a permission prompt) and the computer plays its own short notification sound when that happens while you are looking elsewhere. Several panes at once make one sound. Free, off by default, and it uses only the operating system's stock sounds (Windows' default beep, macOS's Glass, a freedesktop sound on Linux if a player such as `canberra-gtk-play`, `paplay` or `aplay` is installed). Silent over Remote-SSH, WSL and dev containers, where it would play on another machine.
- A one-time review request: after you have used Paneherd on at least 4 different days and 20 finished turns, and while you are looking at a pane whose turn just ended, VS Code asks whether Paneherd is saving you time, with *Rate Paneherd* (opens the Marketplace reviews), *Later* and *Don't ask again*. It asks at most twice, 3 weeks apart, and never again after *Rate Paneherd* or *Don't ask again*. Only in Visual Studio Code itself, not in Cursor or other forks. The counts are kept on this computer only; nothing is sent.
- When the Pro trial ends, Paneherd says what Pro did during it: how many permission prompts you answered from the inbox, how many conflicting edits the conflict guard stopped, how many queued prompts were sent, and the most panes you ran at once past the free plan's 4. The counts are kept on this computer only; nothing is sent. A trial that ran out while the editor was closed is now noticed when it starts again.
- One-click activation: the key page after checkout has *Activate in VS Code* and *Activate in Cursor*. They open the editor, which checks the key offline and asks you to confirm before turning Pro on. Copying the key into *Enter License Key…* still works.
- A phone button in the sidebar's title bar opens *Pair a Phone…*, so pairing no longer needs the Command Palette.
- Phone remote: tap a pane to read its conversation, with your prompts, the agent's replies and one line per tool call (`Bash: npm test`, `Edit: src/x.ts`), newest at the bottom and *Load earlier* for more. A prompt box there sends to that pane only, and the conversation follows along while it is open. It works for Claude Code and Codex panes, read from the session files they save on this computer; other agents' panes say it is not available yet. Free, like the rest of the remote. The conversation crosses the relay end-to-end encrypted, as everything else does, and only while the phone shows it. The phone page needs this version of the extension for it.
- Phone remote: attach images to a prompt (the paperclip next to the prompt box), up to 4, such as a screenshot of a bug. The phone scales photos down first; the editor saves each image in the pane's folder under `.paneherd/phone` (git ignores that folder, and images older than 7 days are deleted) and adds its path to the prompt, which is how Claude Code, Codex and the other agents read an image. Free, like the rest of the remote, and end-to-end encrypted on the way.
- Phone notifications (Pro): turn on *Notify me on this phone* on the phone page and it gets a notification when a pane has needed you for 20 seconds (a permission prompt, a question, a finished turn), with the command for a permission prompt. Answer at the computer within that time and the phone stays quiet. A pane that VS Code has in front of you waits 2 minutes instead, because an open window does not mean you are at the computer; typing in the pane or answering cancels it. VS Code sends it straight to the phone's push service (Google's, Apple's, Mozilla's or Microsoft's), encrypted for that phone, so neither the push service nor Paneherd's relay can read it. On an iPhone or iPad it works from the Home Screen: add paneherd.com/remote to the Home Screen and pair it there, with the new *Scan the QR code* button or a pasted pairing link (*Copy link* in VS Code), since a Home Screen web app keeps its own storage. Needs iOS 16.4 or later.
- *Upgrade* links now say which button opened them (for example `?from=trial-end`), so the site can tell which ones lead to purchases. It is part of the link you open; the extension still sends nothing.

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
