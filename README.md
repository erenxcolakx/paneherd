# Paneherd

A multi-agent console for coding agents. Run Claude Code, Codex, Copilot CLI, Gemini CLI, Cursor CLI, OpenCode, Qwen Code and Droid side by side in split panes in your editor. Answer Claude Code permission prompts in one place, see usage limits, and catch agents that loop or step on each other's files.

Paneherd opens each session as a real terminal in the editor area and lays them out as a grid. A sidebar lists your open panes with live status and every saved Claude Code conversation, so you can resume any of them in one click.

Website: [paneherd.com](https://paneherd.com/?utm_source=marketplace&utm_medium=readme). Guides: [run several Claude Code sessions at once](https://paneherd.com/guides/multiple-claude-code-sessions), [Claude Code worktrees](https://paneherd.com/guides/claude-code-worktrees), [run Codex and Claude Code together](https://paneherd.com/guides/run-codex-and-claude-code-together), [Claude Code usage limits](https://paneherd.com/claude-code-usage-limit) and [how Paneherd compares](https://paneherd.com/compare) with Claude Code's own tools, VS Code, Superset, Conductor, Claude Squad and cmux.

![Four Claude Code sessions in a grid, with the Paneherd sidebar: usage limits, three permission requests, open panes and a queued prompt](https://raw.githubusercontent.com/erenxcolakx/paneherd/main/images/grid.png)

![Three Claude Code sessions working side by side while permission requests are answered from the Paneherd sidebar](https://raw.githubusercontent.com/erenxcolakx/paneherd/main/images/demo.gif)

## Features

- **Grid of sessions**: new sessions and resumed conversations open as editor-area terminals arranged in 1–4 columns. *Arrange Panes in a Grid* tidies them up at any time.
- **Live status from Claude Code itself**: each pane shows *Working*, *Needs permission*, *Has a question*, *Your turn* or *Ended*. The status comes from Claude Code hook events, not from guessing at terminal output.
- **Know who is waiting**: panes that need you are highlighted, counted on the sidebar badge and in the status bar. *Go to Next Pane That Needs You* jumps straight to them.
- **Every saved conversation**: the sidebar lists your Claude Code sessions with their titles and last prompts, searchable by title, prompt or folder, and by what was said in the conversation. Matching messages appear under each session. It warns before you open a session that may still be running in another window. Filter the list by agent and by one or more project folders with the dropdowns above it.
- **Usage limits at the top**: the sidebar shows how much of your Claude plan's current session (5 hours) and week you have used, and when each resets. Claude Code reports these to the status line on Pro and Max plans, after a pane's first reply. Codex's limits show under them, read from the session logs Codex keeps on this computer, and Copilot's monthly premium requests once you click *Connect GitHub* (see [How it works](#how-it-works)).
- **Which pane uses the limit, and when it runs out (forecast: Pro)**: each pane shows roughly how much of the 5-hour session limit it has used. With Pro, a forecast under the session meter says when you hit the limit at the pace of the last 30 minutes, or where you will be when it resets. Both are estimated from the tokens in your local Claude Code transcripts, weighted by API prices. Usage that leaves no transcript on this computer (claude.ai, other machines) is counted against these sessions.
- **Permission inbox (Pro)**: every pane's permission prompts collect in the sidebar, with the command, file or URL. Allow, Always (saves the "don't ask again" rule Claude offers, where Claude's own dialog would: usually the project's `.claude/settings.local.json`), Deny, or answer in the pane. Claude's dialog stays open in the pane, and whichever answers first wins. A notification offers Allow and Deny too.
- **Conflict guard (Pro)**: before a pane edits a file that another open pane changed in the last 10 minutes, Claude asks you first and says which pane changed it.
- **Pane budget (Pro)**: set `paneherd.paneBudget` to the share of your 5-hour Claude limit one pane may use, and Claude asks before that pane's next tool call once it passes it, then again after every further step. Off by default.
- **Prompt cache clock**: a pane's row shows when its conversation's prompt cache runs out, and once it has, roughly what the next reply costs in points of your limit.
- **Loop and stall alarms**: a pane that repeats the same tool call, fails several tool calls in a row, or shows no activity for 10 minutes while working is flagged in the sidebar (and notified on Pro).
- **Own git worktree**: tick *Own git worktree* next to *New session* (or run *New Session in Own Git Worktree*) and the pane runs in a fresh worktree of the repository, on its own `paneherd/` branch, under `.claude/worktrees/` (kept out of `git status` through the repository's local exclude file). Agents working in the same repository can no longer overwrite each other's files. When you close the pane, a worktree that never took a prompt and has no changes or commits of its own is removed with its branch; any other is kept, so no work is lost and the conversation can still be resumed from its folder. Merge or delete the branches yourself. A folder that is not in a git repository opens as it is. Claude Code may ask to trust each new worktree folder.
- **Lead sessions (Pro)**: give one Claude Code pane a goal, and it plans how to split it across worker panes, each in its own git worktree and branch. You approve the plan first; then the lead starts the workers, waits for them and reads their reports. Workers run at the model and effort of the tier the lead picks, so mechanical work goes to a cheaper model. See [Lead sessions](#lead-sessions).
- **Broadcast**: type once and send it to any set of panes. Multi-line text arrives as a single paste. A pane that is still starting gets the text once its prompt is up.
- **Task queue (Pro)**: queue a prompt instead of sending it (*Queue*, or Ctrl+Enter). The first selected pane to end its turn takes it, so prompts queued for one pane run one after another without interrupting the current task, and prompts queued for several panes are shared out among them. The queue can be paused, and a prompt that has not started can be removed.
- **Notifications (Pro)**: a notification appears when a pane needs you and you are looking at something else. A paired phone can get them too (see *Phone remote*).
- **Sound**: set `paneherd.sound` to *finished* or *needsYou* and the computer plays its own short notification sound when a pane ends its turn (or, with *needsYou*, asks a question or needs permission) while you are looking elsewhere. Free and off by default; it uses only the system's stock sounds and is silent over Remote-SSH, WSL and dev containers.
- **Phone remote**: click the phone icon at the top of the sidebar (or run *Pair a Phone…*), scan the QR code with your phone and click *Allow*. Your phone opens [paneherd.com/remote](https://paneherd.com/remote), with no app to install, and shows every window's panes and their status and sends prompts to the panes you pick. Tap a pane to read its recent conversation (your prompts, the agent's replies and a line per tool call, for Claude Code and Codex panes) and to prompt just that pane. The paperclip next to the prompt box attaches images, such as a screenshot of a bug: each is saved in the pane's folder under `.paneherd/phone` (ignored by git, deleted after 7 days) and its path goes into the prompt for the agent to open. With Pro it also answers permission prompts (Allow, Always, Deny), answers a pane's question with Accept (asks twice, then presses Enter in the terminal) or Cancel (Esc), and, once you tap *Notify me on this phone*, gets a notification when a pane has needed you for 20 seconds (2 minutes while VS Code has that pane in front of you, since an open window does not mean you are at the computer); on an iPhone, add the page to the Home Screen and pair it there (it has a *Scan the QR code* button) for that. Messages go through Paneherd's relay encrypted end to end, see [How it works](#how-it-works). A phone icon in the status bar shows while a phone is connected, and *Disconnect Phones…* removes any phone.
- **Chrome companion**: the Paneherd Companion extension for Chrome (coming to the Chrome Web Store) counts the panes that need you on its toolbar badge and lists every window's panes, so you can see who is waiting without leaving the browser. With Pro it also notifies you and lets you answer permission prompts from Chrome. Pair it once with a code from *Connect the Chrome Companion…*.
- **Codex, Copilot, Gemini, Cursor, OpenCode, Qwen Code and Droid next to Claude Code**: pick the agent beside *New session* (or run *New Codex Session*, *New Copilot Session*, *New Gemini Session*, *New Cursor Session*, *New OpenCode Session*, *New Qwen Code Session* or *New Droid Session*) and the pane runs that CLI in the same grid. Broadcast, arrange and restart work for every agent, and status for all but Gemini and Cursor, see [Other agents](#other-agents). Any other CLI, such as Aider or a local model, runs as a [local agent](#local-agents). The permission inbox, conflict guard and conversation list are Claude Code's only.
- **Make panes easy to tell apart**: each new pane gets the next free color on its terminal tab and sidebar row. The pencil on a pane sets its name, color and icon. The sidebar changes at once; the terminal tab takes the name as soon as it is the active terminal, and the color and icon when the pane restarts with ⟳. A pane you have not named is named after its first prompt.
- **Make the sidebar yours**: `paneherd.density` (comfortable or compact), `paneherd.fontScale`, `paneherd.paneDetails` and `paneherd.sessionPrompt` choose what each row shows, and `paneherd.statusPalette` (a color-blind-safe `colorblind` palette) and `paneherd.statusColors` set the status colors. The sidebar follows your color theme; the terminal font is the editor's `terminal.integrated.fontFamily` and `fontSize`.
- **Tour**: the first time the sidebar opens, a short tour at its top shows what each part does and where it is. *Show the Tour* in the sidebar's ⋯ menu brings it back.
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
| Pane budget | | ✓ |
| Task queue | | ✓ |
| Lead sessions | | ✓ |
| Limit forecast | | ✓ |
| Notifications when a pane needs you | | ✓ |
| Phone remote: status, conversation history and sending prompts | ✓ | ✓ |
| Phone remote: answering permission prompts and questions | | ✓ |
| Phone remote: notifications when a pane needs you | | ✓ |
| Chrome companion: badge and pane list | ✓ | ✓ |
| Chrome companion: notifications and answering prompts | | ✓ |

Try every Pro feature free for 7 days first: click *Try Pro free* in the sidebar or run **Paneherd: Start 7-Day Pro Trial**. No card or account; the trial can be started once (it follows Settings Sync), and when it ends, open panes keep running.

Pro is a one-time $2.99 purchase with a perpetual license key, sold at [paneherd.com](https://paneherd.com/#pricing) with a 14-day refund. Activate the key from the page that shows it after checkout (*Activate in VS Code* or *Activate in Cursor*, then confirm in the editor), or enter it with **Paneherd: Enter License Key…**. Keys are checked offline on your machine.

## Requirements

- The [Claude Code CLI](https://code.claude.com/docs), signed in. Paneherd finds `claude` on `PATH`, in the standard install locations, or inside the Claude Code editor extension. You can also set `paneherd.claudePath`.
- Optional, for Codex panes: the [Codex CLI](https://developers.openai.com/codex/cli). Paneherd finds `codex` on `PATH`, in the standard install locations, or inside the OpenAI editor extension. You can also set `paneherd.codexPath`.
- Optional, for other panes: the [GitHub Copilot CLI](https://docs.github.com/copilot/how-tos/copilot-cli) (`copilot`), the [Gemini CLI](https://geminicli.com/docs/get-started/installation/) (`gemini`), the [Cursor CLI](https://cursor.com/docs/cli/installation) (`cursor-agent`), [OpenCode](https://opencode.ai/docs/) 1.x (`opencode`), [Qwen Code](https://github.com/QwenLM/qwen-code) (`qwen`) or [Factory Droid](https://docs.factory.ai/cli/getting-started/quickstart) (`droid`), signed in. Paneherd finds them on `PATH` or where their installers put them; `paneherd.copilotPath`, `paneherd.geminiPath` and `paneherd.cursorPath` override that.
- `curl`, which ships with Windows 10 and later, macOS, and most Linux distributions. Without it panes still work, but they cannot show status.
- A trusted workspace.

## How it works

Each pane runs `claude` with an extra `--settings` file that Paneherd writes for that pane. The file adds hooks for session start, prompt submit, tool use, permission requests, notifications, stop and session end. Your own hooks and settings stay in effect. The hooks run `curl` directly, without a shell, and post the event to a server inside the editor. That server listens only on `127.0.0.1` and checks a random per-window token.

Two hooks wait for the server's answer: the permission request hook (so the sidebar can decide while Claude's own dialog is up; with no decision within an hour it steps aside) and the pre-tool-use hook (so the conflict guard can ask first). The pane's status line, when Paneherd adds one, posts Claude's status line input, which carries the usage limits. Paneherd adds it only when neither you nor the project set a `statusLine`, so yours is never replaced; with your own status line the limits are not shown.

The Chrome companion connects to a second server in each editor window, which runs only after you pair a browser. It listens on `127.0.0.1`, on the first free port from 47470 to 47479, and accepts only browser extensions (by their origin) that paired with a one-time code shown in the editor, or that hold the token pairing gave them. *Disconnect Chrome Companions* revokes every browser.

The phone remote is off until you pair a phone. Then each editor window and the phone connect to Paneherd's relay (`wss://relay.paneherd.com`, a Cloudflare Worker; `paneherd.remoteRelay` points elsewhere). Everything between them is encrypted end to end with AES-256-GCM under a key made on the phone during pairing, which reaches the editor sealed with the one-time code in the QR code; that code sits in the part of the link browsers never send to a server. Through it go your panes' titles, folder names and status, your permission prompts, the prompts and images you send from the phone and, while you read a pane's conversation on the phone, that conversation as Claude Code or Codex saved it (`~/.claude/projects`, `~/.codex/sessions`); the editor reads those files only for a pane the phone asks about. Phone notifications skip the relay: the editor posts them to the push service the phone's browser uses (Google's, Apple's, Mozilla's or Microsoft's; no other host), encrypted for that phone with Web Push, so the push service delivers them without reading them. The relay forwards sealed messages and cannot read or keep them. A QR code works once, for 5 minutes, and only after you click *Allow* in VS Code; each phone has its own key, and *Disconnect Phones…* makes it useless. See the [privacy notes](https://paneherd.com/privacy#phone-remote).

Copilot's premium requests come from GitHub: after you click *Connect GitHub* in the sidebar and allow it in VS Code's sign-in, Paneherd sends your VS Code GitHub sign-in to `api.github.com/copilot_internal/user`, the endpoint the Copilot editor extensions read it from, every few minutes while the sidebar is open. That endpoint is not documented and may change.

Claude's usage figures normally come from Claude Code's status line, so they update with each reply. `paneherd.liveClaudeUsage` is off by default; when you turn it on, Paneherd reads the sign-in token Claude Code keeps in `~/.claude/.credentials.json` and, once a minute, sends it to `api.anthropic.com/api/oauth/usage`, the request Claude Code's own `/usage` makes, so the figures stay current between replies. It never refreshes or stores the token, and an expired one is not sent. That endpoint is not documented and may change or stop working.

Apart from that and a paired phone, nothing leaves your machine. Paneherd has no telemetry, and the license check needs no network access.

## Settings

| Setting | Default | |
|---|---|---|
| `paneherd.claudePath` | `""` | Path to `claude`. When empty, Paneherd looks for it automatically. |
| `paneherd.extraArgs` | `[]` | Extra arguments for every new session, for example `["--model", "opus"]`. |
| `paneherd.gridColumns` | `auto` | Columns in the grid (`auto`, `1`–`4`). |
| `paneherd.notifications` | `true` | Notify when a pane needs you (Pro). |
| `paneherd.usageLimits` | `true` | Show the plan usage limits; adds a status line to panes that have none. |
| `paneherd.liveClaudeUsage` | `false` | Keep Claude's usage figures current between replies by asking Anthropic once a minute with Claude Code's sign-in token (see Privacy). |
| `paneherd.permissionInbox` | `true` | Collect permission prompts in the sidebar (Pro). |
| `paneherd.conflictGuard` | `true` | Ask before editing a file another pane just changed (Pro). |
| `paneherd.paneBudget` | `0` | Ask once a pane has used more than this many percentage points of the 5-hour Claude limit; `0` is off (Pro). |
| `paneherd.paneBudgetStep` | `10` | Points added to a pane's budget each time it is asked. |
| `paneherd.stallMinutes` | `10` | Flag a working pane silent this long; `0` turns it off. |
| `paneherd.remoteRelay` | `""` | Relay for the phone remote (`wss://`). Empty uses Paneherd's own. |
| `paneherd.orchestration.tiers` | Haiku · Sonnet · Opus | The agent, model and effort a worker runs at each tier (`light`, `standard`, `heavy`), see [Lead sessions](#lead-sessions). |
| `paneherd.orchestration.maxWorkers` | `3` | Worker panes one lead runs at once. |
| `paneherd.orchestration.maxWorkersPerGoal` | `6` | Worker panes one lead may open in all. |
| `paneherd.defaultAgent` | `claude` | The agent a new session starts with (`claude`, `codex`, `copilot`, `gemini`, `cursor`, `opencode`, `qwen`, `droid`). |
| `paneherd.<agent>Path`, `paneherd.<agent>ExtraArgs` | | Path and extra arguments for `codex`, `copilot`, `gemini`, `cursor`, `opencode`, `qwen` and `droid` panes. |

## Codex panes

Codex has no per-session settings file, so Paneherd passes its hooks with `-c` options. Codex runs a hook only after you trust it, and it recognises a hook by its text, so every Codex pane gets exactly the same hook commands: each one runs `curl` with a small config file in the extension's storage, and `curl` reads the pane, port and token from `PANEHERD_*` environment variables of the pane's terminal. The token's name keeps Codex from passing it on to the commands it runs.

The first time, Codex asks "7 hooks are new or changed" when the pane starts: choose **Trust all and continue**. Codex remembers the approval in its `config.toml` (as hashes of the hook text under `[hooks.state]`), so later panes, other ports and other windows are not asked again. Until the hooks are trusted the pane shows `—` instead of a status, and broadcast still works with a short fixed wait for Codex to start. Codex reports its session with the first prompt, so a new Codex pane shows no status before you send one.

Hooks need `curl` 8.3 or newer and paths without spaces or quotes; otherwise the pane runs without status. Codex hooks only report status: permission prompts stay in Codex's own dialog. Paneherd's hooks are not removed by anything you trust in `config.toml`; to undo the approval delete the `[hooks.state]` entries whose key contains `<session-flags>`.

## Other agents

Every agent's hooks post to the same local server as Claude Code's, under Claude Code's event names, so the sidebar shows the same states. They only report status: permission prompts and questions stay in the agent's own dialog, and the permission inbox, conflict guard, conversation list and per-pane usage are Claude Code's only. The usage block shows Codex's limits and, once connected, Copilot's premium requests; Gemini and Cursor keep no usage Paneherd can read. A pane takes its title from its first prompt.

- **GitHub Copilot CLI**: Paneherd passes a small plugin folder with `--plugin-dir` and starts the session with `--session-id`, which a restart passes again to resume it. Copilot reports from the first prompt on, after you answer whether you trust the folder, including *Needs permission* and *Has a question*. Paneherd does not hook tool calls before they run: Copilot denies a tool when such a hook fails, for example while the editor reloads, so there are no loop alarms for Copilot panes.
- **Cursor CLI**: no status either. Cursor's plugin hooks (`--plugin-dir`) never report a submitted prompt or a finished turn, confirmed against the real CLI, so a Cursor pane would otherwise show *Working* forever; Paneherd shows nothing instead. The session-start and tool-use hooks it does send still carry the session id, so restart (`--resume`) and picking panes up after *Reload Window* work.
- **OpenCode** (1.x): Paneherd points `OPENCODE_CONFIG_DIR` at a folder of its own with a small plugin, which OpenCode loads after your own config, so your settings and providers stay. The plugin reports from the start, including *Needs permission* and *Has a question*, and posts straight to the editor with no `curl` or shell. It reads the pane from the terminal's environment and does nothing in an OpenCode started elsewhere, and it removes the token from the environment, so the commands OpenCode runs do not see it. Restart resumes the session with `--session`. OpenCode installs the plugin's packages into that folder the first time, which needs the network once. OpenCode 2 loads plugins differently and is not supported yet: its panes run without status. If you set `OPENCODE_CONFIG_DIR` yourself, Paneherd's folder takes its place in its panes.
- **Qwen Code**: Paneherd points `QWEN_CODE_SYSTEM_DEFAULTS_PATH` at a settings file of the pane's own that holds only hooks, in Claude Code's format. Qwen reads it under your own settings and runs its hooks next to yours, so your settings and hooks stay. The hooks report from the start, including *Needs permission*. The session starts with `--session-id`, and restart resumes it with `--resume`. If you or an administrator set `QWEN_CODE_SYSTEM_DEFAULTS_PATH` yourself, Paneherd's file takes its place in its panes.
- **Factory Droid**: Paneherd passes a settings file of the pane's own that holds only hooks with `--settings`, which Droid adds to your own settings for that session, so your settings and hooks stay. Droid reports once you have answered whether you trust the folder, including *Needs permission*. Droid lists each hook it runs in the conversation, so you see Paneherd's `curl` lines there. Restart resumes the session with `--resume`.
- **Gemini CLI**: no status yet. Gemini reads extra settings only from a file that an administrator owns, so Paneherd has no per-pane way to add its hooks, and it does not change your own Gemini settings. Broadcast waits a fixed few seconds. Without hooks Paneherd cannot tell whether a Gemini session took a prompt, so restarting a Gemini pane (⟳, also to apply a new color or icon) starts a new, empty session, and Gemini panes are not offered for reopening after you quit the editor; the old conversation stays in Gemini (`gemini --resume`). A window reload keeps them running.

The user's own hooks still run: Copilot and Cursor also run hooks from a project's `.claude/settings.json`, and Cursor from `~/.claude/settings.json`.

## Local agents

Any other command line agent can run in a pane too: Aider, Goose, Crush, Amp, a local model through `ollama run` or LM Studio's `lms chat`, or a script of your own.

1. In the sidebar, open the agent picker beside *New session* and choose **＋ Add local agent…** (or run **Paneherd: Add Local Agent…**).
2. Pick an agent Paneherd found on your `PATH`, or **Browse for an executable…** for anything else. For one that is not installed, Paneherd offers its install page.
3. Give it a name, and the arguments every new session starts with, for example `--model ollama_chat/qwen2.5-coder` for Aider. Use quotes for an argument with spaces.

The agent then shows up in the picker under that name. **⚙ Manage local agents…** in the same picker (or **Paneherd: Manage Local Agents…**) starts, renames, changes or removes one. Up to three local agents can be set up; they are stored in the `paneherd.custom1Path`, `custom1Label` and `custom1ExtraArgs` settings (and `custom2…`, `custom3…`), which you can also edit by hand.

A local agent runs in a pane as a plain terminal: broadcast, arrange, restart, colors and icons work, but Paneherd shows no status, usage or permission prompts for it, and a restart starts a new session.

## Lead sessions

A lead session is a Claude Code pane that hands parts of a goal to other panes, its workers, and keeps track of them. It is part of Pro.

1. Run **Paneherd: New Lead Session…**, pick a folder and type the goal. On the phone, **＋ New session** has a **Lead session** option too.
2. The lead starts in plan mode. It splits the goal only when the parts touch different files and do not need each other's results; a small goal it does itself. Its plan says how it splits the work and ends with a line listing the workers' tiers, such as `Paneherd workers: standard, light`.
3. The plan shows in the sidebar's permission requests and on the phone, with **Approve**, **Approve, accept edits** and **Revise…**. Its card gives a rough estimate of how much of your 5-hour window the plan may take, and says when that may be more than is left.
4. Once you approve, the lead sets up anything the workers share, then starts them. Each worker opens as a pane in a new git worktree on its own `paneherd/` branch (outside a git repository, in the lead's own folder) and gets a short brief: its task, the files to read first and the files it should change. It commits its work and ends with a short report.
5. The lead waits for the workers and reads each one's report and diff stat, never its whole conversation. When they are done, it checks the branches fit together and tells you what was done. It does not merge them unless you ask.

In the sidebar, the lead's pane is marked **Lead** and its workers are listed under it. Workers' permission prompts reach the inbox and the phone like any other pane's, and you can prompt a worker yourself at any time.

**Tiers.** The lead picks a tier per worker, and `paneherd.orchestration.tiers` says what each tier runs:

| Tier | Default | For |
|---|---|---|
| `light` | Claude Code, Haiku, effort low | Mechanical edits, docs and tests with a check to run |
| `standard` | Claude Code, Sonnet, effort medium | Most features and fixes |
| `heavy` | Claude Code, Opus, effort high | Hard design or debugging |

A tier can run Codex instead (`"agent": "codex"`, with a Codex model name or empty for its default). The lead may also ask for Codex or Claude Code for one worker; that worker keeps the tier's effort and runs the tier's default Claude model, or Codex's own default. The lead cannot name a model: the mapping is yours. Paneherd raises a light task to standard when it has no check (`done_when`), is a bug fix, an investigation or a design, or is to change more than 8 files. At most `maxWorkers` workers run at once and `maxWorkersPerGoal` open in all; past that, the lead is told to wait or reuse a worker.

**How it talks to Paneherd.** The lead pane starts with an MCP server of Paneherd's (`--mcp-config`), on the same `127.0.0.1` server as the hooks and with the same per-window token, which sits in a file in the extension's storage and is deleted when the lead closes. Its tools start a worker, list the workers, wait for them, read a result and send a worker more text; nothing else. Workers get no such server, so they cannot start workers of their own. Who leads whom is kept in the workspace, so a window reload keeps it.

## Known limitations

- A terminal tab keeps the color and icon it was created with: a change made in the sidebar reaches it when the pane restarts. VS Code renames only the active terminal, so a pane that got a new name or its first prompt while in the background takes the name when you switch to it.
- The phone remote needs a connection on both ends. It shows a window's panes only while that window is open and online, and a phone is notified only by a window that is open: with VS Code closed, nothing needs you anyway.
- A Codex pane can be reopened after a reload, or restarted with `resume`, only once its hooks have reported the session id (with its first prompt).
- Panes survive *Reload Window* and extension updates, not quitting the editor: Claude Code saves the conversation, and Paneherd offers to reopen the panes with `--resume`. When you reopen the editor, VS Code may start the panes' terminals again on its own (`terminal.integrated.persistentSessionReviveProcess`); Paneherd closes those and offers the sessions instead. Status needs the hook server's old port to be free after a reload; if another program took it, a picked-up pane shows `—` until you restart it (⟳).
- Panes that are picked up again keep their status but start their loop and stall tracking over, and a permission prompt that was waiting in the inbox is answered in the pane.
- Opening the same conversation in two places at once mixes up its history. Paneherd warns when a session was active in the last two minutes.
- If your organization's managed settings allow only managed hooks, status is unavailable.
- The conflict guard sees edits made with Claude's file tools, not files changed by shell commands.
- A stall alarm means silence, not a fault: a long build or test run looks the same.
- A queued prompt goes in as soon as its pane's turn ends, even when Claude ended it with a question. Pause the queue to answer first.
- Queued prompts survive a window reload for the panes that come back; a prompt that was being typed in when the window reloaded comes back as failed instead of being typed twice.
- Usage limits appear only on Claude Pro and Max plans, and only when Paneherd can add its status line.
- *Show* in the Chrome companion selects the pane in its own window, but the operating system brings forward the editor window that last had focus. Chrome asks once before it opens the editor.
- A lead session runs on Claude Code only, and its workers on Claude Code or Codex. Paneherd does not merge workers' branches, and a worker that closes keeps its worktree when it has commits or changes, as any pane in its own worktree does.
- The cost on a lead's plan card is a rough estimate from typical costs per tier, not from the plan's actual work, and it needs the usage display and a few replies in the current 5-hour window. A lead that leaves out its `Paneherd workers:` line gets no estimate.
- Per-pane usage and the forecast are estimates. Anthropic does not publish how the limits count tokens, so Paneherd weights them by relative API price.

---

Paneherd is an independent product and is not affiliated with or endorsed by Anthropic. Claude and Claude Code are trademarks of Anthropic, PBC. Codex and OpenAI are trademarks of OpenAI; GitHub and Copilot of GitHub, Inc. and Microsoft; Gemini of Google LLC; Cursor of Anysphere, Inc. Paneherd is not affiliated with or endorsed by any of them either.
