# Changelog

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
