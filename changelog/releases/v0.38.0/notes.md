You can now retry agents stalled by repeated crashes, inspect script tool calls in the attached view, and use structured agent results in codemode scripts. This release requires Pi 1.4.0 or later and adds per-model compaction settings and clearer failure notices.

## 💥 Breaking changes

### Pi 1.4.0 required

pi-agents now requires Pi 1.4.0 or later, so update Pi before updating pi-agents. Sessions keep their agents across the update. Going back to an earlier pi-agents version afterwards isn't supported: it may never resume an agent that was waiting to retry a failed request.

*By @mavam in #108.*

## 🚀 Features

### Agent tools return data to codemode scripts

Codemode scripts of agents now get structured results from `agent_status`, `agent_send`, and `agent_spawn` instead of their text. `agent_status` returns the agents a script can message with their state, model, and job; `agent_send` the recipient, the mode, whether it interrupted the recipient, and how many queued messages it dropped; and `agent_spawn` the group and its agents with their IDs and full names:

```js
const { group, agents } = await tools.agent_spawn({ agents: [{ name: "scout", job: "Map src/" }] });
```

A refused call rejects in the script with its reason. The model still reads the same text as before.

*By @mavam in #108.*

### Per-model compaction settings for agents

Agents now compact with Pi's per-model token settings in `compaction.modelOverrides`: an agent on a listed model uses that entry's `reserveTokens` and `keepRecentTokens`, so agents on small- and large-window models each compact at the right size. Before, agents used only the general compaction settings.

*By @mavam in #108.*

### Retry agents whose work crashes Pi

When Pi crashes or is killed several times in a row while an agent works, without the agent getting any further, the agent's work no longer resumes by itself on every restart. The agent shows `stalled after repeated crashes` instead, in the panel, `/agents`, the attach view, and `agent_status`. Select it in `/agents` and press `r` to retry it, or `s` to stop it. Agents whose unfinished work this version of pi-agents can't run show as stalled too.

*By @mavam in #108.*

### Script tool calls in the attached view

The attached view now shows an agent's `codemode` calls like Pi does: the script, then each tool call the script made with its arguments, status, and duration, as the calls run and after the script finished, and then the script's output. Press Ctrl+O to see the whole script, every call, and the errors of failed calls.

*By @mavam in #108.*

### Warnings for failed agent compactions

When an agent's context compaction fails, its line in the panel and `/agents` now shows `compaction failed`, and `/agents`, the attach view, and `agent_status` show the error. The agent keeps its state and goes on working. The warning stays until a later compaction succeeds, because the agent's context may overflow meanwhile.

*By @mavam in #108.*

## 🔧 Changes

### Agent failure notices name the agent

Notices about failures in agents' background work now name the agent or graph they are about and say what failed, such as `Agent api: its tool call failed: …`, instead of showing only the error. When an agent's work stalls after repeated crashes, the notice says how to retry or stop it. A storage failure now shows one notice instead of two.

*By @mavam in #108.*

## 🐞 Bug fixes

### Agent requests follow Pi's telemetry setting

Agents' model requests now identify themselves to providers as Pi's own requests do: they send Pi's `User-Agent`, and name Pi as the app to OpenRouter, NVIDIA NIM, and OpenCode only while Pi's install telemetry is on. Turning telemetry off with the `enableInstallTelemetry` setting or `PI_TELEMETRY=0` now covers agents too.

*By @mavam in #108.*

### Attach view tells messages from your input by their sender

The attach view now tells messages from other agents apart from what you typed by the sender stored with each message, not by the text. A message you send that starts like `[from <name>]` shows as your own input, and Esc returns it to the editor with the rest of your queued input. Agents still read the sender from the `[from <name>]` and `[user]` markers. Inputs that earlier versions stored carry no sender, so the attach view shows them as plain input with their markers.

*By @mavam in #108.*

### Click to expand cards in the attached view

Clicking a tool result or a message in the attached view now expands or collapses it, as in Pi's own transcript. Before, only Ctrl+O did, for all cards at once. Clicking the `… +N newer lines` note jumps back to new output. Like the mouse wheel, clicks need Pi's fullscreen mode.

*By @mavam in #108.*
