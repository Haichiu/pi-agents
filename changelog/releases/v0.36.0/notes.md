Pi agents can now spawn coordinated groups recursively through a single tool, with durable lifecycle and messaging behavior. This release also removes obsolete graph-spawning interfaces and improves how agent tool calls and replies appear.

## 💥 Breaking changes

### One recursive agent_spawn for every way to start agents

`agent_spawn` starts one group of 1 to 12 agents for Pi or for an agent permitted to spawn. Use `after` to pass results between agents within the group; the group reports one result to whoever started it. A group of one shows as its agent but retains a separate group ID:

```json
{
  "agents": [
    { "name": "scout", "job": "Map the API surface." },
    { "name": "review", "job": "Review the map.", "after": ["scout"], "spawn": true }
  ]
}
```

Agents permitted to spawn can use `agent_spawn`, `agent_send`, `agent_status`, `agent_wait`, and `agent_stop`. Spawning returns at once; the group result arrives as `[results]` input to the spawning agent or from a direct `agent_wait`, never both. Pi receives its groups' results as messages unless it calls `agent_wait` directly. Intermediate answers don't finish an agent's job while it is still owed results. `agent_wait` is model-only in both Pi and agents: codemode scripts cannot safely acknowledge results they might discard or truncate. Peer messages don't wake a blocked wait.

An interrupted agent keeps its children working and its job open for a corrective answer. Stopping it stops its entire subtree; a run that fails withdraws only the groups it spawned that still owe results. Agents can message their spawner, peers in their group, and their own children by name or reachable ID; Pi's agents can message each other. Spawning is bounded to three agent levels, 24 children per agent over its lifetime (including completed or cancelled children), and 16 unfinished initial spawned-agent jobs across a session. Later messages to finished agents don't use that fixed 16-agent allowance; the optional working-agent cap still applies.

This replaces earlier ways to start agents without compatibility shims. `agent_spawn_graph` and the agents' blocking `delegate_graph` are gone, `agent_spawn` takes only `agents` with a `job` for each agent, and profiles say `spawn: true` instead of `delegate: true`. Agents that earlier versions started can't spawn.

*By @mavam in #115.*

## 🚀 Features

### Hold agents' model calls from extensions

Co-loaded extensions can now hold agents' model calls, for example during peak pricing, with an optional `beforeAgentRequest` hook:

```ts
discovery.register({
  namespace: "peak-pricing",
  beforeAgentRequest: async ({ model, signal }) => {
    while (isPeak(model.provider, Date.now())) await sleep(60_000, signal);
  },
});
```

Each generation request and compaction summary waits until the callback returns. Held agents keep working: they receive messages, and tools they already called finish. Stopping, interrupting, and shutting down end a hold. The extension owns its policy, including manual holds and status; pi-agents only waits.

*By @mavam in #112.*

### Optional agent lifecycle hooks

Co-loaded extensions can now capture persistent state when agents are created and customize their model requests through optional lifecycle hooks:

```ts
import type { AgentHooksDiscovery } from "pi-agents/hooks";

pi.events.on("pi-agents:hooks:discover", (value) => {
  const hooks = value as AgentHooksDiscovery | undefined;
  if (hooks?.protocol !== 1) return;
  hooks.register({
    namespace: "my-extension",
    onAgentCreate: () => ({ version: 1, enabled: true }),
    onAgentRequest: ({ state, model }, payload) => undefined,
  });
});
```

Each spawn captures one snapshot, which a graph's agents share. Each agent keeps it across follow-ups, compaction, and resumed sessions, and helpers inherit their creator's. A failing hook names its extension and error in the agent's error. Malformed or conflicting registrations are ignored with a notice, so a faulty extension never disables agents. Extensions without this integration and users without consumer extensions keep their existing behavior.

*By @mavam in #110.*

## 🔧 Changes

### Tool calls and messages without the leading mark

Agent tool calls and messages between agents now render without the leading `✦` mark. Tool calls show their name in bold, the way Pi shows its own tools:

```text
spawn lister profile=explorer thinking=low tools=[read,ls]
  List the files in src and summarize them.
```

*By @mavam in #114.*

## 🐞 Bug fixes

### Clickable agent and graph replies

Long agent and graph replies can now be expanded by clicking their cards in Pi's fullscreen mode. Click again to collapse a card. Ctrl+O still expands or collapses all reply cards and tool output, including in regular terminal mode. Replies and error messages remain complete; only their previews are collapsed.

*By @mavam.*
