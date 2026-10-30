---
name: "model-consumption"
description: "Display the current model and live consumption figures from session_status, on demand or as the reply footer; identical for Barakat and Jimmy."
---

# Model & Consumption Display

This skill has one job only: showing the current model and live consumption numbers. Use it whenever Mohamed asks about the model, thinking level, tokens, context, cache, or consumption — and to build the footer line at the end of every reply.

## Source of truth

`session_status`, read fresh at display time. Never reuse numbers from an earlier reply.

Field mapping (strings exactly as shown):

- `Model` field → model (full string, never shortened).
- `Modes` line (`think <level>`) → thinking level (high, medium, or low as shown).
- `Tokens` line (`X in / Y out`) → tokens in/out (same units as shown, e.g. `110k` / `3.1k`).
- `Context` line (`A/B (P%)`) → context used/total and percent (same units, e.g. `110k/1.0m (10%)`).
- `Cache` line (`N% hit`) → cache percent (number plus `%` only, e.g. `50%`).

## Display format (exact — 5 fields, never a cost field)

```
🤖 <model> | 🧠 <thinking> | 📥 <in> / 📤 <out> | 📚 <ctx>/<total> (<pct>) | 💾 <cache%>
```

## Steps

1. Call `session_status`.
2. Extract the five values per the mapping above.
3. On demand (Mohamed asks about model/consumption): answer with the line, plus at most one short sentence of context. Nothing else.
4. As reply footer: place the line AFTER any verse tail block, as plain text outside all quotes — never inside `<aside>`, `<blockquote>`, or markdown quotes.
5. Verify every value came from this display's `session_status` call.

## Pitfalls

- Numbers go stale within minutes — fresh-read on every display, never copy an old line.
- Model string exactly as shown; thinking level live from `Modes`; units as shown.
- Used on every reply footer and on any direct question about model/consumption, by Barakat and Jimmy alike — removed only by an explicit order from Mohamed.
