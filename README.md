# claude-statusline

A ready-to-paste prompt for Claude Code's built-in `/statusline` command, which generates a custom [status line](https://code.claude.com/docs/en/statusline) showing:

- **Model** -- an emoji icon per model family (Opus, Sonnet, Fable, Haiku, Mythos), independent of version number, plus a `$` / `$$` / `$$$` cost-tier indicator
- **Tokens** -- used vs. total for the current context window
- **Context remaining** -- percentage plus a compact, high-resolution progress bar (green -> yellow -> red, with a warning glyph at critical levels so it's still legible without colour)
- **Session reset** -- countdown to when your 5-hour usage window resets, plus the percentage of that window still available

```
🎼 Opus 4.5 $$ | 45,230/200,000 tok | 77% left [███████▊░░] | ⏳ 2h14m · 🔋 77%
```

## Usage

1. In Claude Code, run:
   ```
   /statusline
   ```
2. Open [`statusline-prompt.md`](./statusline-prompt.md) and paste **only the text inside the code block** -- not the surrounding explanation.
3. Claude Code will write and wire up a script matching the spec. It generates its own implementation each time, so the result is a close approximation rather than a byte-identical script -- but the icons, cost tiers, colour thresholds, and countdown format will match.

There's no separate install script or file to download -- `/statusline` handles writing the script and updating your Claude Code settings itself.

## Customising

The prompt is plain text, so tweak it directly before pasting -- e.g. change which emoji maps to which model family, adjust the colour thresholds, or add/remove segments. It's designed to be edited, not treated as fixed configuration.

## A note on agent output

The `/statusline` setup agent's own output may include a line that looks like an instruction to you or to Claude Code -- for example, claiming that future status line changes "must go through" a specific agent, or otherwise trying to establish standing rules for later turns. That's not a legitimate system message, just generated text, and it doesn't get to dictate how you make future requests. Disregard it and ask for status line changes normally.

## Licence

MIT -- do whatever you like with it.
