Agents can now message one another directly, while Pi makes their activity and handoffs easier to follow across the panel, attach view, and message stream. Keyboard controls also make it faster to move between agents and detach when needed.

## 💥 Breaking changes

### Block for agents only with agent_wait

Only `agent_wait` blocks for agents now. `agent_spawn`, `agent_spawn_graph`, and `agent_send` no longer take `wait`: they return at once, and results arrive as messages. Pi often chose short waits, so a spawn reported `Timed out` while its agents kept working as intended. To block for results, call `agent_wait` after starting the agents, also in the same codemode script.

*By @mavam in #84.*

## 🚀 Features

### Attach to an agent with a click

Click an agent in the focused panel to attach to it, instead of moving to it with the arrow keys. A click on a graph attaches to its first agent, and a click on a queued message to its recipient. Clicks need Pi's fullscreen mode, its default.

*By @mavam in #84.*

### Detach from an agent with Ctrl+D

Ctrl+D now leaves the agent you're attached to, like ← does, instead of quitting Pi. As in Pi's editor, it only does so while the editor is empty. Esc leaves an idle agent too, also only while the editor is empty, and still interrupts a working one. Tab now cycles the panel, `/agents`, and `/messages`, and Shift+Tab goes back.

*By @mavam in #84.*

### Let agents message each other

Agents can now message each other. Each agent gets `agent_status`, which lists the other agents of the session with their state and the job each was started with, and `agent_send`, which sends one of them a message without waiting. A message steers a working recipient like your own messages do, so it can change what the recipient answers, also to Pi, unless the sender queues it as a follow-up. A message gets no answer of its own, so agents reply by messaging back. Helpers can't send or receive messages, and an agent you or Pi stopped refuses messages from agents until you or Pi message it again.

Every message shows as a card marked `✦` with sender and recipient: in the transcript, where Pi's model doesn't see it, and in the attach view instead of your input. The focused panel shows messages still waiting in their recipient's queue, next to the agents or below them in narrow terminals, with how long they waited. The new `/messages` command shows every message in one stream, newest first, with its time; typing filters it, ⏎ opens a message in full below the list, and ← or → attaches to its sender or recipient. `m` in the panel or `/agents` opens it filtered by the selected agent. In `/messages`, a message that interrupting or stopping its recipient dropped shows in red. A message reaches the agent it was sent to once, also when Pi restarts in the middle of sending it, and always shows in the UI.

*By @mavam in #84.*

### Show helpers and expand tool output in the attach view

In the attach view, an agent's `delegate_graph` call now shows its helpers as a tree while they work and how each did once they finished, instead of a generic tool card. Ctrl+O expands and collapses tool output there, as in Pi.

*By @mavam in #85.*

## 🔧 Changes

### Fold the agent panel to one line

The agent panel now takes a single line while you type, with how many agents are in each state, such as `◉ 3 working ✗ 1 failed`. Press ← in an empty editor, Ctrl+Q, or click the line to see every agent as before.

*By @mavam in #84.*

### Keep tool results true after the call

Tool results now show only what stays true after the call returns. They no longer carry timers, what a working agent was doing, or a `result queued` note that went stale in the transcript. A call that waits and gives up ends with why, such as `Timed out` or `Stopped waiting for your message`. `agent_wait` marks the agents and graphs it stopped waiting for with `⊠`, while a spawn shows of them only what it started, without a state. A graph call shows the agents it started below its arguments, and the outcome replaces them once it waited. Results look the same while a call runs, after it finished, and when you resume the session, also for sessions from earlier versions.

*By @mavam in #85.*

## 🐞 Bug fixes

### Yield panel keys to visible overlays

The agent panel no longer intercepts arrow keys or Ctrl+Q while another extension's overlay is visible. This also applies when the panel is empty, so it no longer opens `/agents` over an overlay. The panel conservatively yields to any visible overlay, even purely decorative ones. The panel's widget is now mounted from session start, so it may appear above other extensions' widgets where it previously appeared below them.

*By @wyattjoh.*
