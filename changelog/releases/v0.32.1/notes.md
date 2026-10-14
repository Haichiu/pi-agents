This release fixes a crash that ended Pi when a session was replaced right after it started, as RPC clients do when they send a new session request shortly after spawning Pi.

## 🐞 Bug fixes

### Replacing a session right after start no longer crashes Pi

Pi no longer exits when a session is replaced right after it starts, for example when an RPC client sends `new_session` shortly after spawning `pi --mode rpc`. Notices from pi-agents work that outlives its session, such as pruning old agent storage, are now discarded instead of failing on the replaced session's context.

*By @mavam.*
