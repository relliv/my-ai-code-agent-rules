# Level 1 Notes

## Context Tracking

Context window lengths are still limited. To track current content usage run `/statusline` command in cc cli. It will configure status line to current cli setup.

- [Always show available context percentage](https://github.com/anthropics/claude-code/issues/516)
- [Status line configuration](https://code.claude.com/docs/en/statusline)

This is important to avoid losing context or starting a new conversation. We need to use [/clear](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=f.%20Use%20/clear%20to%20keep%20context%20focused) command to clear context when we are done with a conversation.
