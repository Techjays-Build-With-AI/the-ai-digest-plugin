# Techjays Dispatch

A Claude Code plugin that turns the **Techjays - Weekly Leads Sync** meeting into an issue of
**The AI Dispatch** — the team's magazine-style AI digest. Any attendee can run it, so the digest
never depends on one person, and the design is frozen so every issue looks identical.

```
/dispatch            # build this week's issue from the latest sync
/dispatch 2026-07-16 # build the issue for a specific past sync
```

It reads the transcript doc attached to the calendar event (via your own Google Calendar + Drive),
fills a frozen template, writes `techjays-ai-dispatch-issue-NN.html` into the current folder, and opens it.

---

## One-time setup (per person)

**1. Enable the Google connectors** (once per machine). In Claude Code:

```
/mcp
```

Connect **Google Calendar** and **Google Drive**. (Both are standard claude.ai connectors. The first
`/dispatch` run will also start the OAuth flow for you and hand you the authorization link if they aren't
connected yet.) Each person authorizes with their **own** Google account — there are no shared credentials.

**2. Install the plugin.**

```
/plugin marketplace add techjays/techjays-dispatch   # or the internal git URL / local path
/plugin install techjays-dispatch
```

Then restart Claude Code if prompted. `/dispatch` is now available anywhere.

> Installing from a local clone instead:
> `/plugin marketplace add /path/to/techjays-dispatch` then `/plugin install techjays-dispatch`.

---

## How it works

1. **Auth** — connects your Google Calendar + Drive (per-user OAuth; prompts only the first time).
2. **Find** — locates the latest "Techjays - Weekly Leads Sync" event (or the date you pass).
3. **Read** — opens the transcript doc attached to that event via Google Drive.
4. **Number** — scans existing `techjays-ai-dispatch-issue-*.html` and takes the next number.
5. **Assemble** — maps the transcript into the fixed department structure using the frozen design.
6. **Emit** — writes `techjays-ai-dispatch-issue-NN.html` and opens it in your browser.

If it can't get a real transcript, it stops and tells you — it never invents meeting content.
You can also feed a transcript directly: `/dispatch ./some-transcript.md`.

---

## What's in here

```
.claude-plugin/marketplace.json         # makes this repo installable as a marketplace
plugins/techjays-dispatch/
  .claude-plugin/plugin.json            # plugin manifest
  commands/dispatch.md                  # the /dispatch workflow
  assets/
    template.html                       # FROZEN design shell (head+CSS, tj-logo, cover, footer)
    style-contract.md                   # component library + house rules + section recipe
    example-issue-01.html               # the reference standard every issue must match
```

## Uniformity: how issues stay identical

The look is **not** regenerated each week. `assets/template.html` ships the entire design system (tokens,
fonts, the Techjays logo, the masthead, cover, and footer) and `assets/style-contract.md` is the rulebook —
the component library, the color semantics (coral = a problem, teal = the fix), and the house rules
(plain hyphens, exact model names, sentence-case voice). `/dispatch` only pours each week's content into
those slots. To evolve the design, edit the template/contract once here and everyone's next issue picks it up.

## Requirements

- Claude Code with the Google Calendar + Google Drive connectors available.
- You must be an attendee of the meeting (so the transcript doc is shared with your Google account).
- macOS `open` is used to preview the file; on Linux swap for `xdg-open` if needed.
