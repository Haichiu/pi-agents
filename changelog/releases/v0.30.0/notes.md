Pi-agents now closes resumed agents after Pi accepts their answers and can prune old session storage with an opt-in retention setting. These updates make agent lifecycle management more reliable while keeping cleanup under your control.

## 🚀 Features

### Prune old agent storage with piAgents.retentionDays

Pi-agents can now delete the agent storage of sessions you no longer use. Set `piAgents.retentionDays` in `~/.pi/agent/settings.json` or the project's `.pi/settings.json`, and at each Pi start pi-agents removes the stores in `~/.pi/agent/pi-agents/sessions/` that haven't changed for that many days. The current session's store and stores another running Pi holds stay. Without the setting, nothing is deleted.

*By @mavam.*

## 🔧 Changes

### Deliver agent results without truncation

Agent results now reach Pi whole. Graph result messages, `agent_wait` results, and codemode scripts' `result` and `error` fields are no longer cut at 40,000 characters, and the `truncated` field is gone. Agents likewise see every job in `agent_status` and the full graph result from `delegate_graph`.

*By @mavam.*

### Keep Pi usable while agents work

Pi no longer blocks on agents by default. After starting agents, Pi tells you what it started and ends its turn or keeps working, and each result arrives on its own as a message, so you can keep talking to Pi while agents work. Pi waits with `agent_wait` only when a turn must combine results, for example in a codemode script, and a running wait now says that sending a message stops it.

Single-shot runs such as `pi --print` still wait for the results they need, since the run ends with Pi's answer.

Pi also treats a result as an agent's report rather than your request, acknowledges results briefly while other agents still work, never polls for results, and prefers a graph when you want several results together.

*By @mavam.*

## 🐞 Bug fixes

### Close resumed agents after Pi takes their answer

If you interrupt an agent and resume it by chatting in the attach view, Pi's wait now closes it once it takes the new answer. The finished agent no longer stays idle in the panel indefinitely. Failed and interrupted agents still stay open, and saved conversations remain available.

*By @mavam.*
