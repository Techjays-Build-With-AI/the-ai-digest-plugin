---
description: Generate this week's Techjays AI Dispatch issue from the Leads Sync transcript
argument-hint: "[YYYY-MM-DD meeting date | \"path/to/transcript\"]"
allowed-tools:
  - Read
  - Write
  - Glob
  - Bash(ls:*)
  - Bash(open:*)
  - Bash(date:*)
  - mcp__claude_ai_Google_Calendar__authenticate
  - mcp__claude_ai_Google_Calendar__complete_authentication
  - mcp__claude_ai_Google_Drive__authenticate
  - mcp__claude_ai_Google_Drive__complete_authentication
---

# /dispatch — build this week's AI Dispatch issue

You are producing the next issue of **The AI Dispatch**, Techjays' weekly Leads Sync digest.
Anyone invited to the meeting can run this; your job is to make an issue that is indistinguishable
in look and structure from the last one. Work through the steps in order. Do not skip the auth or the
self-check. **Do not fabricate meeting content** — if you cannot get a real transcript, stop and say so.

Argument (`$1`, optional): a meeting date `YYYY-MM-DD`, OR a path/URL to a transcript to use directly.
If empty, target the most recent past occurrence of the meeting.

## Step 0 — Preflight
- Output directory is the current working directory. Note it.
- Read the design assets now so you know the contract before fetching anything:
  - `${CLAUDE_PLUGIN_ROOT}/assets/style-contract.md` — the rulebook. Follow it exactly.
  - `${CLAUDE_PLUGIN_ROOT}/assets/template.html` — the frozen shell you will fill.
  - `${CLAUDE_PLUGIN_ROOT}/assets/example-issue-01.html` — the reference standard to match.
- If `$1` is a readable file path or a transcript URL, skip Steps 1-3, load that transcript, and go to Step 4.

## Step 1 — Connect Google (per-user, one time per machine)
The transcript lives on the runner's own Google Calendar, so each person authenticates with their own account.
- Try to use the Google Calendar MCP tools. If the only tools available are `authenticate`/`complete_authentication`,
  the connector is not yet authorized:
  1. Call `mcp__claude_ai_Google_Calendar__authenticate`, show the user the returned Google authorization URL,
     and ask them to approve it in their browser.
  2. When they return with the `http://localhost:.../callback?...` URL, pass it to
     `mcp__claude_ai_Google_Calendar__complete_authentication`.
- Repeat the same flow for **Google Drive** (`mcp__claude_ai_Google_Drive__authenticate`). You need both:
  Calendar to find the event, Drive to read the attached transcript.
- If a connector is not installed at all (no `mcp__claude_ai_Google_Calendar__*` tools exist), tell the user to
  enable the **Google Calendar** and **Google Drive** connectors once, then re-run — point them at the README.

## Step 2 — Find the meeting
- Search the calendar for the event titled **"Techjays - Weekly Leads Sync"** (match loosely; it is recurring).
- Pick the occurrence for `$1` if given, else the most recent one on or before today.
- Capture its **date**, **attendees/host**, and its **attachments** (name, mimeType, fileId/fileUrl).
- If you find no matching event, stop and report what you searched.

## Step 3 — Get the transcript
- The transcript is a doc attached to the event (often a Google Doc such as a Gemini notes / transcript file).
  Prefer the attachment whose name looks like notes/transcript/recording.
- Use the Google Drive MCP tools to read/export that document as text (export Google Docs as plain text or markdown).
- If the event has no attachment, check the event description for a Drive link and read that.
- If you still have no transcript, stop and report — do not invent content.

## Step 4 — Number the issue
- In the output directory, list `techjays-ai-dispatch-issue-*.html`. The new issue number is the highest existing
  number + 1, zero-padded to two digits (`01`, `02`, ...). If none exist, start at `01`.
- Target filename: `techjays-ai-dispatch-issue-<NN>.html`.

## Step 5 — Assemble the issue (follow the style contract)
- Distil the transcript into the department spine from `style-contract.md` §3: **Model Watch** first, a **Cover Story**
  for the week's biggest theme, thematic departments for the rest, **Action Board** last. Number them sequentially.
- Build every block ONLY from the component library in `style-contract.md` §5, using the exact class names.
- Fill each `{{PLACEHOLDER}}` in `template.html`:
  `{{ISSUE_NO}}` (x3, identical), `{{COVER_KICKER}}`, `{{COVER_LINE}}` (the week's thesis, one `<em>`),
  `{{TEASER_ITEMS}}`, `{{DATELINE}}` (real filed date/host/counts), `{{EDITORS_HEADLINE}}` (different words from the
  cover line), `{{EDITORS_NOTE}}` (2 paragraphs), `{{BYLINE}}`, `{{CONTENTS}}` (numbers match the departments),
  `{{SECTIONS}}`, `{{COMPILED_FROM}}` (`Compiled from the <DD Mon YYYY> leads weekly sync`).
- Obey the golden rules: hyphens never em-dashes; exact model names (Opus/Sonnet are both Claude — never
  "Claude vs Sonnet"); coral = problem/emphasis, teal = fix/forward; correct contrast on dark panels.

## Step 6 — Write and open
- Write the filled HTML to `techjays-ai-dispatch-issue-<NN>.html` in the output directory.
- Run the self-check in `style-contract.md` §6 against your output. Fix anything that fails (in particular,
  grep your file for `—` and remove any).
- Open it: `open techjays-ai-dispatch-issue-<NN>.html`.

## Step 7 — Report
Tell the user: the issue number, the meeting date it was built from, the departments included, and the file path.
Note anything you had to infer or leave out because the transcript was thin.
