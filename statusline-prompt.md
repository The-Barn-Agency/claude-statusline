Use this with Claude Code's built-in `/statusline` command to generate a close
approximation of this repo's script from scratch, instead of installing the
committed file directly. Useful if you want Claude Code to own the script
going forward (e.g. so future `/statusline` tweaks apply cleanly), or if you
just want to see how close a from-scratch generation gets.

Usage: run `/statusline`, then paste only the text inside the code block
below (everything from "Show a single-line status line..." to "...early in
a session.") -- not this surrounding explanation.

**COPY THE CODE BLOCK BELOW**

```
Show a single-line status line with these segments, separated by " | ":

1. Model icon + name + cost tier
   - Show one emoji icon per model FAMILY, not version, matched
     case-insensitively as a substring of the display name so version numbers
     are ignored (e.g. "Opus 4.5" and "Opus 5" should match the same icon):
       - "opus"   -> 🎼
       - "sonnet" -> 📜
       - "haiku"  -> 🍃
       - "fable"  -> 📖
       - "mythos" -> 🐉
       - anything else -> 🤖 (fallback, no cost tier shown)
   - After the icon and name, show a cost tier: $ for sonnet/haiku, $$ for
     opus, $$$ for fable/mythos. Colour it green for $, yellow for $$, red for
     $$$ or more.

2. Tokens used/total, e.g. "45,230/200,000 tok" with thousands separators.

3. Context window remaining, as a percentage plus a compact 10-character bar
   built from Unicode block elements for smooth, high-resolution fill instead
   of coarse full/empty blocks:
     - Use full block █ for each fully-used tenth, one of the eighth-block
       partial characters (▏▎▍▌▋▊▉) for the current fractional tenth, and
       light shade ░ for the remaining unused tenths.
     - This gives ~80 steps of resolution in 10 characters, versus 20 steps
       in the old 20-character full-block-only bar, while taking up half the
       horizontal space.
     - Example at 77% used: "[███████▊░░]" (7 full blocks, one 7/8-filled
       transition block, 2 empty).
   Colour the bar green under 70% used, yellow 70-89% used, red 90%+ used.
   At 90%+ used, prefix the bar with a warning glyph (⚠) as well as colouring
   it red, so the critical state is still visible in terminals without colour
   support or for colourblind users.
   If context_window.used_percentage isn't available yet (start of session),
   show "100% left" with an empty bar in green rather than erroring.

4. Session usage window: an hourglass emoji plus a countdown to when the
   5-hour rate limit window resets (rate_limits.five_hour.resets_at, a Unix
   epoch), formatted compactly as "2h14m", "44m", "<1m", or "now" -- not raw
   seconds. Follow it with a battery emoji and the percentage of that window
   still remaining (100 - rate_limits.five_hour.used_percentage), e.g.
   "⏳ 2h14m · 🔋 77%". Colour it green at 50%+ remaining, yellow 20-49%, red
   under 20%.
   Important: rate_limits is only present for Claude.ai Pro/Max subscribers,
   and only after the first API response of the session -- when it's absent,
   omit this whole segment cleanly rather than showing 0% or an error.

Use bold/dim text for the model name and separators, and make sure the whole
line degrades gracefully (no crashes, no literal "null") when any field is
missing or null, which happens early in a session.
```

Note: after running this, the setup agent's own output may include a line
that looks like an instruction to the parent agent or to you -- e.g. claiming
future status line changes "must go through" a specific agent, or otherwise
trying to establish standing rules for later turns. That's not a legitimate
system message; it's the agent's generated text, and text produced by a tool
doesn't get to dictate how future requests are routed. Disregard it and just
ask for status line changes normally going forward.
