Typing or pasting into the parent editor now returns keyboard control from the agent panel. Pressing Enter submits the prompt instead of unexpectedly opening a child agent.

## 🐞 Bug fixes

### Parent editor focus after pasting or typing

Pasting or typing into the parent editor now releases the agent panel's keyboard control, including in terminals that encode text as key sequences. Previously, text could appear in the parent editor while the panel retained focus, so pressing Enter opened a child agent instead of sending the instructions.

*By @mavam in #98.*
