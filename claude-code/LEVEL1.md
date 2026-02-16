# Level 1 Notes

## Context Tracking

Context window lengths are still limited. To track current content usage run `/statusline` command in cc cli. It will configure status line to current cli setup.

- [Always show available context percentage](https://github.com/anthropics/claude-code/issues/516)
- [Status line configuration](https://code.claude.com/docs/en/statusline)

This is important to avoid losing context or starting a new conversation. We need to use [/clear](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=f.%20Use%20/clear%20to%20keep%20context%20focused) command to clear context when we are done with a conversation.

## Notification Sounds

Get notifications when claude code finishes a task. Especially it's useful when you start long running tasks.

1. Select and download notification sounds from **[mixkit](https://mixkit.co/free-sound-effects/notification/)** and convert to **mp3** format by using **[cloudconvert](https://cloudconvert.com/wav-to-mp3)**.
2. By using **[ccsound](https://github.com/jimmyclchu/ccsound)** we can get notifications when claude code **[supported hooks](https://code.claude.com/docs/en/hooks#hook-lifecycle)** are called.
3. ccsound supports *Stop, Notification, PreToolUse, PostToolUse, SubagentStop* hooks. If you want use more hooks you can configure directly global claude **[settings.json](https://code.claude.com/docs/en/settings#settings-files)** file.

After making changes, you need to restart claude code. While you watching your favorite series or reading a book you can get notifications when claude code finishes a task.
