Pi agents can now inspect images, run codemode scripts, and inherit the tools active in your Pi session. This release also improves recovery from storage errors and requires Pi 1.2.0 or newer.

## 💥 Breaking changes

### Requires Pi 1.2.0

pi-agents now requires Pi 1.2.0 or newer, whose pi-durable brings the nested tool calls, durable codemode, and image support that agents use. Update Pi with `pi update self` before updating pi-agents.

*By @mavam in #91.*

## 🚀 Features

### Agents read images

Agents can now look at images. `read` returns PNG, JPEG, GIF, WebP, and BMP files as images, turned upright and shrunk to fit the model's limits, instead of failing.

*By @mavam.*

### Agents run codemode scripts

Agents can now use `codemode`, Pi's tool for JavaScript scripts that call other tools, to batch, chain, or filter tool calls in one step. Agents have it whenever it's active in your session, and a profile can name it too.

Scripts call the agent's other tools, including `delegate_graph` and `agent_send`, and each call is tracked on its own, so several delegations in one script each get their own helpers. A script interrupted by a restart continues where it was, without running its finished calls again.

*By @mavam.*

## 🔧 Changes

### Agents inherit your tools

Agents now start with the tools you have active in Pi, instead of always `read`, `bash`, `edit`, and `write`. With `codemode` or `grep` active in your session, agents get them too, and their helpers inherit the agent's tools. Tools that agents can't run, such as MCP and other extensions' tools, are left out. A profile or a spawn that names tools still decides on its own.

*By @mavam in #91.*

### MIT license

pi-agents is now licensed under the MIT license, like the other Pi extensions by the same author, and the package ships the license text.

*By @mavam.*

## 🐞 Bug fixes

### Agents continue after a storage error

If writing to agent storage fails, for example because the disk is full, agents now pause, Pi tells you, and the storage reopens so agents continue as after a restart. In sessions started with `--no-session`, agents live in memory, so Pi tells you they are lost instead.

*By @mavam in #91.*

### Failed commands show their exit code

When you attach to an agent, a failed bash command now ends with "Command exited with code N", as in Pi, instead of a `<harness>` block meant for the model. Other tool notes, such as how read converted an image, show as plain lines too.

*By @mavam in #91.*
