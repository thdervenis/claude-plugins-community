# next-steps

After each turn, suggests up to three next prompts above the input box.

```
✦ next steps                                   [dismiss]
1  [run the tests you just wrote]
2  [do the same for the settings page]
3  [/code-review high]
1-3 draft it · Tab takes 1 · 0 dismisses
```

The suggestions sit in a rounded card in your theme's accent color. Press `1`, `2` or `3` from an empty prompt box (or click a row) and that prompt is written into the box as a draft. Edit it, then press Enter yourself. `0` or the `dismiss` button closes the card. The top suggestion also shows as the box's dim ghost text, so Tab takes it.

The plugin never submits a prompt on its own.

## How it works

It is a function-hooks plugin (`hooks/register.tsx`):

- `turn.complete`: forks the session with `$.model.fork` to ask for likely next prompts. The fork shares the session's prompt cache, so it costs about one short reply.
- `$.command.list`: the session's skills and slash commands (plugin, user and MCP ones with their descriptions) go into the fork's question, so a suggestion can be `/skill arguments`. A suggestion that names a command the session does not have is dropped.
- `ui.render` on `AbovePrompt`: draws the card: a header, one button per suggestion with its number in a fixed-width column, and a key hint. The number keys are bound through hidden buttons, so the visible rows carry no hotkey badge.
- A press calls `$.prompt.fill`; the top suggestion goes to `$.prompt.suggest`.
- `turn.start`: hides the suggestions.

Suggestions draw in the terminal. Other surfaces show nothing.

## Options

| Option | Default | What it does |
| --- | --- | --- |
| `minAnswerChars` | `80` | Skip suggestions after answers shorter than this |
| `suggestSkills` | `true` | Tell the suggester which skills and slash commands the session has |
