This release caps how many agents work at once instead of how many model requests they send, so local model servers keep each agent's prompt cache. Other extensions can now also follow child agents through lifecycle events.

## 💥 Breaking changes

### Cap working agents instead of model requests

Cap how many agents work at once with `piAgents.maxWorkingAgents`, which replaces `piAgents.maxConcurrentRequests`. The old setting capped single model requests, so agents still took turns at the model between tool calls, and a local model server with one prompt cache reprocessed every agent's context at each switch. Now an agent keeps its slot from its first request until its work ends, and agents over the cap wait their turn:

```json
{
  "piAgents": {
    "maxWorkingAgents": 1
  }
}
```

An agent that waits for its helpers gives its slot to them meanwhile. pi-agents no longer reads `maxConcurrentRequests`; rename it in your settings.

*By @mavam.*

## 🚀 Features

### Publish child-agent lifecycle state

Co-loaded extensions can observe child-agent activity on `pi-agents:state`. Versioned, immutable snapshots publish after session restoration and whenever lifecycle state changes; `pi-agents:state:request` lets late listeners request the current state. Graceful shutdown clears live ownership without claiming persisted jobs completed. Terminal integrations can use these events to stay working while child agents run, even after the parent Pi session settles, without parsing the panel or footer.

*By @wyattjoh.*
