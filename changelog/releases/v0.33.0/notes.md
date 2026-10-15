This release gives every agent status its own colored circle glyph, including closed agents, and counts them in the footer next to the model. Switching sessions now tells you and Pi which agents it paused, and failed agent results show their error details.

## 🔧 Changes

### Show closed agents in the footer count

The pi-fancy-footer `agents` widget now counts closed agents with a new `◌` glyph, such as `2◉ 1● 3◌`. A closed agent is one whose answer reached Pi or that was stopped, and messaging it starts it again. The widget also drops its `✦` prefix, colors each glyph by its status, and now sits left of the model instead of at the right edge.

Closed agents and graphs show `◌` in `/agents`, in `/agent` completions, and in the attach view. An agent or graph whose result waits for Pi shows `◐` instead of an accent-colored `●`, and a failed one shows `⊗` instead of `✗`, so every status has its own circle glyph. Graph details and tool results still show how each agent's work ended. Closed helpers don't count in the footer, because nobody can message them.

*By @mavam.*

## 🐞 Bug fixes

### Error details in failed agent results

Failed agent result cards now show the available error message alongside the failure category, instead of only a label such as `model_error`. This makes provider refusals and other failures understandable without opening the agent's conversation. Long error messages collapse like answers and expand with Ctrl+O, including failures summarized in graph results. Pi still receives the full error text.

*By @mavam in #99.*

### Warn when switching sessions pauses agents

Switching away from a session while its agents work or have results waiting now names those agents in the new session, for you and for Pi, such as when you start a new session, resume another one, or move to another worktree. They stay with the original session: resume it to continue them and receive their results. Previously, they disappeared without a word.

*By @mavam.*
