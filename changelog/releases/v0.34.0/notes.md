Codemode scripts of agents can now run classifiers and generate images through models, as in Pi. This release requires Pi 1.3.0 or newer.

## 💥 Breaking changes

### Requires Pi 1.3.0

pi-agents now requires Pi 1.3.0 or newer. Update Pi with `pi update self` before updating pi-agents.

*By @mavam in #102.*

## 🚀 Features

### Classifier and image models in agent scripts

Codemode scripts of agents now get `models`, as in Pi: they can list the models you can use, run classifiers with `models.classify()`, and generate images with `models.generateImages()`. These calls count toward the agent's usage, and a restart doesn't repeat the ones that finished.

Only agents with `codemode` get them, and helpers only when they get `codemode` from an agent that has them. Agents created by earlier versions get them the next time you open their session.

*By @mavam in #102.*
