When you interrupt an agent and steer it to finish, pi-agents now delivers its answer to Pi. Your request is no longer silently dropped.

## 🐞 Bug fixes

### Deliver results of agents you interrupt and resume

If you interrupt an agent while it works on a job or message from Pi and then steer it to finish from the attach view, its next answer now goes to Pi. Before, the interrupt silently withdrew Pi's request, so the finished agent sat idle in the panel and Pi never got its result. While the answer waits for Pi, the panel shows `result queued`. To withdraw the request, stop the agent instead. Interrupting a graph's agent still ends its part in the graph.

*By @mavam in #94.*
