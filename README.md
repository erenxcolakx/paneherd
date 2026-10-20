# Paneherd

A multi-agent console for Claude Code. Run several Claude Code sessions side by side in split panes in your editor, answer all their permission prompts in one place, see your plan's usage limits, and catch agents that loop or step on each other's files.

Paneherd opens each session as a real terminal in the editor area and lays them out as a grid. A sidebar lists your open panes with live status and every saved Claude Code conversation, so you can resume any of them in one click.

Website: [paneherd.com](https://paneherd.com). It has a guide to running several Claude Code sessions at once and a comparison with Claude Code's own tools.

![Four Claude Code sessions in a grid, with the Paneherd sidebar: usage limits, three permission requests, open panes and a queued prompt](https://raw.githubusercontent.com/erenxcolakx/paneherd/main/images/grid.png)

## Features

- **Grid of sessions**: new sessions and resumed conversations open as editor-area terminals arranged in 1–4 columns. *Arrange Panes in a Grid* tidies them up at any time.
- **Live status from Claude Code itself**: each pane shows *Working*, *Needs permission*, *Has a question*, *Your turn* or *Ended*. The status comes from Claude Code hook events, not from guessing at terminal output.
- **Know who is waiting**: panes that need you are highlighted, counted on the sidebar badge and in the status bar. *Go to Next Pane That Needs You* jumps straight to them.
- **Every saved conversation**: the sidebar lists your Claude Code sessions with their titles and last prompts, searchable by title, prompt or folder, and by what was said in the conversation. Matching messages appear under each session. It warns before you open a session that may still be running in another window.
- **Usage limits at the top**: the sidebar shows how much of your Claude plan's current session (5 hours) and week you have used, and when each resets. Claude Code reports these to the status line on Pro and Max plans, after a pane's first reply.
- **Which pane uses the limit, and when it runs out (forecast: Pro)**: each pane shows roughly how much of the 5-hour session limit it has used. With Pro, a forecast under the session meter says when you hit the limit at the pace of the last 30 minutes, or where you will be when it resets. Both are estimated from the tokens in your local Claude Code transcripts, weighted by API prices. Usage that leaves no transcript on this computer (claude.ai, other machines) is counted against these sessions.
- **Permission inbox (Pro)**: every pane's permission prompts collect in the sidebar, with the command, file or URL. Allow, Always (saves the "don't ask again" rule Claude offers, where Claude's own dialog would: usually the project's `.claude/settings.local.json`), Deny, or answer in the pane. Claude's dialog stays open in the pane, and whichever answers first wins. A notification offers Allow and Deny too.
- **Conflict guard (Pro)**: before a pane edits a file that another open pane changed in the last 10 minutes, Claude asks you first and says which pane changed it.
- **Loop and stall alarms**: a pane that repeats the same tool call, fails several tool calls in a row, or shows no activity for 10 minutes while working is flagged in the sidebar (and notified on Pro).
- **Broadcast**: type once and send it to any set of panes. Multi-line text arrives as a single paste. A pane that is still starting gets the text once its prompt is up.
- **Task queue (Pro)**: queue a prompt instead of sending it (*Queue*, or Ctrl+Enter). The first selected pane to end its turn takes it, so prompts queued for one pane run one after another without interrupting the current task, and prompts queued for several panes are shared out among them. The queue can be paused, and a prompt that has not started can be removed.
- **Notifications (Pro)**: a notification appears when a pane needs you and you are looking at something else.
- **Phone remote**: run *Pair a Phone…*, scan the QR code with your phone and click *Allow*. Your phone opens [paneherd.com/remote](https://paneherd.com/remote), with no app to install, and shows every window's panes and their status and sends prompts to the panes you pick; with Pro it also answers permission prompts (Allow, Always, Deny). Messages go through Paneherd's relay encrypted end to end, see [How it works](#how-it-works). A phone icon in the status bar shows while a phone is connected, and *Disconnect Phones…* removes any phone.
- **Chrome companion**: the Paneherd Companion extension for Chrome (coming to the Chrome Web Store) counts the panes that need you on its toolbar badge and lists every window's panes, so you can see who is waiting without leaving the browser. With Pro it also notifies you and lets you answer permission prompts from Chrome. Pair it once with a code from *Connect the Chrome Companion…*.
- **Codex, Copilot, Gemini and Cursor next to Claude Code**: pick the agent beside *New session* (or run *New Codex Session*, *New Copilot Session*, *New Gemini Session* or *New Cursor Session*) and the pane runs that CLI in the same grid. Broadcast, arrange and restart work for every agent, and status for all but Gemini and Cursor, see [Other agents](#other-agents). The permission inbox, conflict guard, conversation list and usage limits are Claude Code's only.
- **Make panes easy to tell apart**: each new pane gets the next free color on its terminal tab and sidebar row. The pencil on a pane sets its name, color and icon. The sidebar changes at once; the terminal tab takes the name as soon as it is the active terminal, and the color and icon when the pane restarts with ⟳. A pane you have not named is named after its first prompt.
- **Make the sidebar yours**: `paneherd.density` (comfortable or compact), `paneherd.fontScale`, `paneherd.paneDetails` and `paneherd.sessionPrompt` choose what each row shows, and `paneherd.statusPalette` (a color-blind-safe `colorblind` palette) and `paneherd.statusColors` set the status colors. The sidebar follows your color theme; the terminal font is the editor's `terminal.integrated.fontFamily` and `fontSize`.
- **Panes survive a reload**: after *Reload Window* or an extension update, the panes that were running are picked up again where they are, with their status, name and color. After you quit the editor, the panes you had open are offered for reopening with `--resume`.

<img src="https://raw.githubusercontent.com/erenxcolakx/paneherd/main/images/sidebar.png" alt="The Paneherd sidebar: usage limits, the permission inbox with Allow, Always and Deny, open panes with their status, and the task queue" width="300">

## Free and Pro

| | Free | Pro |
|---|---|---|
| Sessions open at once | 4 | Unlimited |
| Live status, sidebar, session browser | ✓ | ✓ |
| Broadcast to several panes | ✓ | ✓ |
| Usage limits, per-pane usage, loop and stall alarms | ✓ | ✓ |
| Permission inbox | | ✓ |
| Conflict guard | | ✓ |
| Task queue | | ✓ |
| Limit forecast | | ✓ |
| Notifications when a pane needs you | | ✓ |
| Phone remote: status and sending prompts | ✓ | ✓ |
| Phone remote: answering permission prompts | | ✓ |
| Chrome companion: badge and pane list | ✓ | ✓ |
| Chrome companion: notifications and answering prompts | | ✓ |

Pro is a one-time $4.99 purchase with a perpetual license key, sold at [paneherd.com](https://paneherd.com/#pricing) with a 14-day refund. Enter the key with **Paneherd: Enter License Key…**. Keys are checked offline on your machine.

## Requirements

- The [Claude Code CLI](https://code.claude.com/docs), signed in. Paneherd finds `claude` on `PATH`, in the standard install locations, or inside the Claude Code editor extension. You can also set `paneherd.claudePath`.
- Optional, for Codex panes: the [Codex CLI](https://developers.openai.com/codex/cli). Paneherd finds `codex` on `PATH`, in the standard install locations, or inside the OpenAI editor extension. You can also set `paneherd.codexPath`.
- Optional, for other panes: the [GitHub Copilot CLI](https://docs.github.com/copilot/how-tos/copilot-cli) (`copilot`), the [Gemini CLI](https://geminicli.com/docs/get-started/installation/) (`gemini`) or the [Cursor CLI](https://cursor.com/docs/cli/installation) (`cursor-agent`), signed in. Paneherd finds them on `PATH` or where their installers put them; `paneherd.copilotPath`, `paneherd.geminiPath` and `paneherd.cursorPath` override that.
- `curl`, which ships with Windows 10 and later, macOS, and most Linux distributions. Without it panes still work, but they cannot show status.
- A trusted workspace.

## How it works

Each pane runs `claude` with an extra `--settings` file that Paneherd writes for that pane. The file adds hooks for session start, prompt submit, tool use, permission requests, notifications, stop and session end. Your own hooks and settings stay in effect. The hooks run `curl` directly, without a shell, and post the event to a server inside the editor. That server listens only on `127.0.0.1` and checks a random per-window token.

Two hooks wait for the server's answer: the permission request hook (so the sidebar can decide while Claude's own dialog is up; with no decision within an hour it steps aside) and the pre-tool-use hook (so the conflict guard can ask first). The pane's status line, when Paneherd adds one, posts Claude's status line input, which carries the usage limits. Paneherd adds it only when neither you nor the project set a `statusLine`, so yours is never replaced; with your own status line the limits are not shown.

The Chrome companion connects to a second server in each editor window, which runs only after you pair a browser. It listens on `127.0.0.1`, on the first free port from 47470 to 47479, and accepts only browser extensions (by their origin) that paired with a one-time code shown in the editor, or that hold the token pairing gave them. *Disconnect Chrome Companions* revokes every browser.

The phone remote is off until you pair a phone. Then each editor window and the phone connect to Paneherd's relay (`wss://relay.paneherd.com`, a Cloudflare Worker; `paneherd.remoteRelay` points elsewhere). Everything between them is encrypted end to end with AES-256-GCM under a key made on the phone during pairing, which reaches the editor sealed with the one-time code in the QR code; that code sits in the part of the link browsers never send to a server. The relay forwards sealed messages and cannot read or keep them. A QR code works once, for 5 minutes, and only after you click *Allow* in VS Code; each phone has its own key, and *Disconnect Phones…* makes it useless. See the [privacy notes](https://paneherd.com/privacy#phone-remote).

Without a paired phone nothing leaves your machine. Paneherd has no telemetry, and the license check needs no network access.

## Settings

| Setting | Default | |
|---|---|---|
| `paneherd.claudePath` | `""` | Path to `claude`. When empty, Paneherd looks for it automatically. |
| `paneherd.extraArgs` | `[]` | Extra arguments for every new session, for example `["--model", "opus"]`. |
| `paneherd.gridColumns` | `auto` | Columns in the grid (`auto`, `1`–`4`). |
| `paneherd.notifications` | `true` | Notify when a pane needs you (Pro). |
| `paneherd.usageLimits` | `true` | Show the plan usage limits; adds a status line to panes that have none. |
| `paneherd.permissionInbox` | `true` | Collect permission prompts in the sidebar (Pro). |
| `paneherd.conflictGuard` | `true` | Ask before editing a file another pane just changed (Pro). |
| `paneherd.stallMinutes` | `10` | Flag a working pane silent this long; `0` turns it off. |
| `paneherd.remoteRelay` | `""` | Relay for the phone remote (`wss://`). Empty uses Paneherd's own. |
| `paneherd.defaultAgent` | `claude` | The agent a new session starts with (`claude`, `codex`, `copilot`, `gemini`, `cursor`). |
| `paneherd.<agent>Path`, `paneherd.<agent>ExtraArgs` | | Path and extra arguments for `codex`, `copilot`, `gemini` and `cursor` panes. |

## Codex panes

Codex has no per-session settings file, so Paneherd passes its hooks with `-c` options. Codex runs a hook only after you trust it, and it recognises a hook by its text, so every Codex pane gets exactly the same hook commands: each one runs `curl` with a small config file in the extension's storage, and `curl` reads the pane, port and token from `PANEHERD_*` environment variables of the pane's terminal. The token's name keeps Codex from passing it on to the commands it runs.

The first time, Codex asks "7 hooks are new or changed" when the pane starts: choose **Trust all and continue**. Codex remembers the approval in its `config.toml` (as hashes of the hook text under `[hooks.state]`), so later panes, other ports and other windows are not asked again. Until the hooks are trusted the pane shows `—` instead of a status, and broadcast still works with a short fixed wait for Codex to start. Codex reports its session with the first prompt, so a new Codex pane shows no status before you send one.

Hooks need `curl` 8.3 or newer and paths without spaces or quotes; otherwise the pane runs without status. Codex hooks only report status: permission prompts stay in Codex's own dialog. Paneherd's hooks are not removed by anything you trust in `config.toml`; to undo the approval delete the `[hooks.state]` entries whose key contains `<session-flags>`.

## Other agents

Every agent's hooks post to the same local server as Claude Code's, under Claude Code's event names, so the sidebar shows the same states. They only report status: permission prompts and questions stay in the agent's own dialog, and the permission inbox, conflict guard, conversation list, per-pane usage and usage limits are Claude Code's only. A pane takes its title from its first prompt.

- **GitHub Copilot CLI**: Paneherd passes a small plugin folder with `--plugin-dir` and starts the session with `--session-id`, which a restart passes again to resume it. Copilot reports from the first prompt on, after you answer whether you trust the folder, including *Needs permission* and *Has a question*. Paneherd does not hook tool calls before they run: Copilot denies a tool when such a hook fails, for example while the editor reloads, so there are no loop alarms for Copilot panes.
- **Cursor CLI**: no status either. Cursor's plugin hooks (`--plugin-dir`) never report a submitted prompt or a finished turn, confirmed against the real CLI, so a Cursor pane would otherwise show *Working* forever; Paneherd shows nothing instead. The session-start and tool-use hooks it does send still carry the session id, so restart (`--resume`) and picking panes up after *Reload Window* work.
- **Gemini CLI**: no status yet. Gemini reads extra settings only from a file that an administrator owns, so Paneherd has no per-pane way to add its hooks, and it does not change your own Gemini settings. Broadcast waits a fixed few seconds. Without hooks Paneherd cannot tell whether a Gemini session took a prompt, so restarting a Gemini pane (⟳, also to apply a new color or icon) starts a new, empty session, and Gemini panes are not offered for reopening after you quit the editor; the old conversation stays in Gemini (`gemini --resume`). A window reload keeps them running.

The user's own hooks still run: Copilot and Cursor also run hooks from a project's `.claude/settings.json`, and Cursor from `~/.claude/settings.json`.

## Known limitations

- A terminal tab keeps the color and icon it was created with: a change made in the sidebar reaches it when the pane restarts. VS Code renames only the active terminal, so a pane that got a new name or its first prompt while in the background takes the name when you switch to it.
- The phone remote needs a connection on both ends. It shows a window's panes only while that window is open and online, and it does not notify the phone yet.
- A Codex pane can be reopened after a reload, or restarted with `resume`, only once its hooks have reported the session id (with its first prompt).
- Panes survive *Reload Window* and extension updates, not quitting the editor: Claude Code saves the conversation, and Paneherd offers to reopen the panes with `--resume`. When you reopen the editor, VS Code may start the panes' terminals again on its own (`terminal.integrated.persistentSessionReviveProcess`); Paneherd closes those and offers the sessions instead. Status needs the hook server's old port to be free after a reload; if another program took it, a picked-up pane shows `—` until you restart it (⟳).
- Panes that are picked up again keep their status but start their loop and stall tracking over, and a permission prompt that was waiting in the inbox is answered in the pane.
- Opening the same conversation in two places at once mixes up its history. Paneherd warns when a session was active in the last two minutes.
- If your organization's managed settings allow only managed hooks, status is unavailable.
- The conflict guard sees edits made with Claude's file tools, not files changed by shell commands.
- A stall alarm means silence, not a fault: a long build or test run looks the same.
- A queued prompt goes in as soon as its pane's turn ends, even when Claude ended it with a question. Pause the queue to answer first.
- Queued prompts do not survive a window reload.
- Usage limits appear only on Claude Pro and Max plans, and only when Paneherd can add its status line.
- *Show* in the Chrome companion selects the pane in its own window, but the operating system brings forward the editor window that last had focus. Chrome asks once before it opens the editor.
- Per-pane usage and the forecast are estimates. Anthropic does not publish how the limits count tokens, so Paneherd weights them by relative API price.

---

Paneherd is an independent product and is not affiliated with or endorsed by Anthropic. Claude and Claude Code are trademarks of Anthropic, PBC. Codex and OpenAI are trademarks of OpenAI; GitHub and Copilot of GitHub, Inc. and Microsoft; Gemini of Google LLC; Cursor of Anysphere, Inc. Paneherd is not affiliated with or endorsed by any of them either.
