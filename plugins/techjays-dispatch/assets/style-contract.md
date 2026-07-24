# The AI Dispatch — Style Contract

This is the single source of truth for how every issue of **The AI Dispatch** is built.
`/dispatch` fills `template.html` with content assembled ONLY from the components below.
The goal is that issue №07 is instantly recognizable as the same publication as issue №01,
no matter who ran the command or which model generated it.

`example-issue-01.html` (in this folder) is the reference standard. When in doubt, match it.

---

## 1. Golden rules (do not break these)

1. **Never touch the frozen chrome.** The `<head>`/CSS, the `<style>` block, the `#tj-logo`
   symbol, the `.nameplate`, the masthead, and the footer come from `template.html` verbatim.
   Do not add fonts, change tokens, or restyle. Only fill `{{PLACEHOLDERS}}`.
2. **Hyphens, never em-dashes or en-dashes for punctuation.** Use `-` with spaces (`landed well - Ashish ran`).
   The only allowed `&ndash;` / `&times;` are inside numeric ranges already present in reference copy
   (e.g. `3&ndash;4&times;`, `15&ndash;30 min`). Do not introduce `—` (U+2014) anywhere, including comments.
3. **Name the exact model, never a family, in any model-vs-model comparison.** Opus, Sonnet, Fable,
   Haiku, Kimi K3, Qwen, GPT-5, Gemini. Opus and Sonnet are BOTH Claude — never write "Claude vs Sonnet."
   "Claude Code", "Claude Design", "Claude for Chrome" (product names) and "built with Claude" (the agent)
   are fine. Cross-vendor family comparisons ("Claude review vs GPT vs Gemini") are fine.
4. **Color is semantic. Keep the binding.**
   - **navy** `--navy #0B2545` = structure, authority, headings, ink.
   - **coral** `--coral #F26B5B` (text `--coral-ink #C7402F`) = a problem / what broke / emphasis / the `<em>`.
   - **teal** `--teal #1AC0C6` (text `--teal-ink #0A6E72`) = the fix / forward / solutions / eyebrows / links.
   Never flip coral and teal for variety.
5. **Contrast:** small text on white uses `--navy` / `--teal-ink` / `--coral-ink` (never bright teal/coral).
   Text on a navy panel uses `#EAF1F8` / `#DCE6F2`, with `--teal-300 #7FE0E3` labels. `<em>` on light = coral-ink.
6. **Voice:** sentence case, plain and concrete, builder-not-consultant. Paraphrase the transcript;
   attribute named quotes with a `<cite>`. Keep it faithful — this digest paraphrases a real meeting.
7. **Register the diagrams' palette** (SVG): fills/strokes use navy `#0B2545`, teal `#1AC0C6`,
   teal-300 `#7FE0E3`, teal-ink `#0A6E72`, coral `#F26B5B`, slate `#6B7280`/`#374151`, white `#FFFFFF`,
   paper `#F7F9FC`. SVG fonts: `'Bricolage Grotesque'` (bold labels), `'IBM Plex Mono'` (mono), `'Inter'` (body).

### Publication safety (these are golden rules too - the digest is meant to be shareable beyond the room)

8. **Employee and speaker names are fine.** Keep full names of Techjays people and meeting speakers, and attribute quotes to them.
9. **Never name a customer.** Strip every customer / client / project / product / codename. Refer to a customer as `a customer`, `one of our customers`, or - preferred - by the **nature the transcript gives** (`a coaching customer`, `a retail customer`, `a logistics client`). If a customer is referenced but the transcript does not reveal its nature, **STOP and ask the user** what to call it. Never guess a name or an industry.
10. **Customer problems are fine; identifiers are not.** Keep the problem and the lesson, but remove all PII (names / emails / handles of people at the customer) and company-identifying info (their name, their partners or vendors, headcounts or org details that fingerprint them).
11. **No internal vendor relationships.** Remove partnership status, contract / subscription / credit / discount terms, and anything about who we pay or who pays us. Public product facts are fine; the relationship is not.
12. **Keep internal evaluations and experiments.** Our own model / tool benchmarks, cost comparisons, and "what we tried this week" are the point of the digest - keep them, as long as they carry no personal information.
13. **Verify third-party claims.** Any factual claim about an outside company or product (`X shipped Y`, `vendor raised limits`, `rolled out in the US`) must be verified with web research before it stays. If it checks out, keep it and cite the source (inline or in a `.quiet` note). If it cannot be verified, ask the user for a source; if none is given, drop the claim. Never publish an unverified third-party claim as fact.
14. **Avoid internal gaps; keep tool capabilities.** Do not publish our own weaknesses, delivery slippage, roadmap, or insecure workarounds. Reframe such a story as the capability or the lesson (e.g. "drive a web tool with Chrome MCP") without exposing the gap or the workaround.

---

## 2. Filling the cover + chrome placeholders

| Placeholder | What goes in it | Example |
|---|---|---|
| `{{ISSUE_NO}}` | Zero-padded issue number (appears 3x, keep identical) | `02` |
| `{{COVER_KICKER}}` | Fixed-flavour kicker over the thesis | `This week's transmission` |
| `{{COVER_LINE}}` | The single-sentence thesis of the week, with ONE `<em>` for the turn | `The models moved again. So did <em>the way we work with them.</em>` |
| `{{TEASER_ITEMS}}` | 4-6 `<li>` cover lines (the most stealable takeaways) | see below |
| `{{DATELINE}}` | Mono facts row: filed date, duration, headcount, host, desk | see below |
| `{{EDITORS_HEADLINE}}` | A punchy editor's-note headline (NOT the same words as the cover line) | `The model is a commodity. The workflow is the edge.` |
| `{{EDITORS_NOTE}}` | 2 `<p>` paragraphs: the week's texture + "here's what's worth carrying forward" | |
| `{{BYLINE}}` | `Host: <name> &nbsp;·&nbsp; Desk: Leads Sync` | |
| `{{CONTENTS}}` | One `<li>` per department, numbered, with a one-line `<small>` gloss | see below |
| `{{SECTIONS}}` | The department blocks assembled from §5 components | |
| `{{COMPILED_FROM}}` | `Compiled from the <DD Mon YYYY> leads weekly sync` | `Compiled from the 22 Jul 2026 leads weekly sync` |

**Teaser items**
```html
          <li>Kimi's quiet value play</li>
          <li>The two-reviewer problem</li>
          <li>Don't grade your own homework</li>
          <li>Experience contracts</li>
          <li>The death of the site chatbot</li>
```

**Dateline**
```html
      <span>Filed <b>Wed 22 Jul 2026</b></span><span class="dot">/</span>
      <span>~50 min</span><span class="dot">/</span>
      <span>20+ leads</span><span class="dot">/</span>
      <span>Host <b>Dharmaraj M</b></span><span class="dot">/</span>
      <span>Desk <b>Leads Sync</b></span>
```

**Contents** (one per department; `n` = zero-padded number matching the `.dept-no`)
```html
        <li><span class="n">01</span><a href="#models">Model Watch<small>Fable vs Opus 4.8, Kimi savings, OpenAI credits</small></a></li>
```

---

## 3. Section recipe (how to structure an issue)

The middle of the page is a sequence of **departments**, numbered `§ 01`, `§ 02`, ... Each department is
one `.dept` divider followed by one or more grid rows of content. Departments flex to the week's transcript,
but follow this spine:

1. **§ 01 — Model Watch** (always first): the week's model/pricing landscape. Lead article + a `.panel-dark`
   scoreboard `.mtable` + a vendor-moves `.card`.
2. **§ 02 — Cover Story** (the single biggest theme of the week): mark the `.dept-kind` as `Cover Story`,
   give it a `Filed <date> · <reporter>` stamp, a `.drop` lead, a diagram `figure`, a `.pull`, and a
   follow-up `.g2` (the concrete example + the deeper worry, ending in a `.verdict`).
3. **Thematic departments** (pick what the transcript supports, in this rough order): a review/process
   department, a tooling department (`.g3` of tools + a `.card.coral` "gotchas"), a workflow/"Staying in
   Flow" department (wrap in a `.spread` for rhythm, add a diagram), a "Big Idea" department, an
   "On the Horizon" department (use the `.shift` old-world/new-world split), a "Field Notes" `.g3` of
   quick hits (wrap in a `.spread`).
4. **§ NN — Action Board** (always last): a `.board` of 2-4 `.todo` cards — who owns what next week.

Rhythm rules that keep issues feeling alike:
- Alternate plain sections with **at most two** `.spread` inset bands and **one or two** `.panel-dark` blocks.
  Do not make every section dark.
- Give the FIRST article of a major department a `.drop` (drop cap). Do not drop-cap every article.
- One `.pull` quote per major department at most.
- Number departments sequentially with no gaps; the `.dept-no`, the `#id`, and the `{{CONTENTS}}` number must agree.

---

## 4. Department divider (start every section with this)

```html
  <section id="models">
    <div class="dept">
      <div class="dept-rail"><span class="dept-no">§ 01</span><span class="dept-kind">Model Watch</span><span class="dept-line"></span><span class="dept-stamp">The landscape this week</span></div>
      <h3 class="dept-title">Model Watch</h3>
    </div>
    <!-- grid rows go here -->
  </section>
```
- `.dept-kind` = the department's category (e.g. `Cover Story`, `Big Idea`, `The Toolshed`).
- `.dept-stamp` = a short mono note: a reporter (`Filed 22.07 · Ragul Kachiappan`), attendees (`Rajesh · Aravind · Dharma`), or a gloss (`Quick hits from the room`).

---

## 5. Component library (assemble sections from these — exact class names)

### Grid rows
```html
<div class="g2"> ... two .art columns ... </div>          <!-- equal 1fr/1fr -->
<div class="g2 wide"> ... </div>                           <!-- 1.4fr / 1fr (text-heavy left) -->
<div class="g2 narrow"> ... </div>                         <!-- 1fr / 1.35fr (right-heavy) -->
<div class="g3"> ... three .art columns ... </div>
```

### Article block
```html
<div class="art">
  <div class="eyebrow">Benchmark // Kimi K3</div>            <!-- optional mono eyebrow (teal-ink) -->
  <h4 class="drop">Kimi is the quiet value play</h4>         <!-- add class "drop" for a drop cap; use sparingly -->
  <p>Body copy. Use <b>bold</b> for names/terms and <em>em</em> (renders coral) for the emphasis beat.</p>
</div>
```

### Pull quote
```html
<div class="pull">&ldquo;Quote text, paraphrased faithfully.&rdquo;<cite>- Speaker Name</cite></div>
```

### Card (white) — variants: add `coral` or `teal` for a top rule
```html
<div class="card coral">
  <div class="label">Vendor moves</div>
  <h5>Everyone's discounting</h5>
  <p>Body. Last <p> drops its bottom margin automatically.</p>
</div>
```

### Dark panel (navy) — for scoreboards / high-contrast data
```html
<div class="panel-dark">
  <div class="label">Scoreboard</div>
  <table class="mtable">
    <thead><tr><th>Model</th><th>Where it earns its keep</th><th>Note</th></tr></thead>
    <tbody>
      <tr><td class="m">Fable 5</td><td>Ambiguous / research problems</td><td><span class="chip">slow · pricey</span></td></tr>
    </tbody>
  </table>
</div>
```

### Clean list (checklist / findings)
```html
<ul class="clean">
  <li><b>Split the roles.</b> One model for code-correctness, one for product-fit.</li>
</ul>
```

### Verdict callout (the takeaway of a department; coral-bordered)
```html
<div class="verdict"><b>Dharma's framing:</b> the loop isn't done until the <b>definition of done</b> is real.</div>
```

### Tag row
```html
<div class="tags"><span class="tag">loop-engineering</span><span class="tag">review-gates</span></div>
```

### Inset spread (light rhythm band — wrap a whole department's content)
```html
<div class="spread">
  <div class="dept">...divider...</div>
  <div class="g3">...</div>
  <figure>...</figure>
</div>
```

### Old-world / new-world split (for "On the Horizon"; coral=old, navy=new)
```html
<div class="shift">
  <div class="col old"><div class="tag2">Old world</div><p>The status quo / the pain.</p></div>
  <div class="col new"><div class="tag2">New world</div><p>The shift / the fix.</p></div>
</div>
```
`.shift .new p` is white (`#EAF1F8`) by an explicit rule in the frozen CSS — do not restyle it.

### Figure + SVG diagram
```html
<figure>
  <svg viewBox="0 0 520 340" role="img" aria-label="Describe the diagram for screen readers">
    <!-- use the SVG palette from Golden Rule 7 -->
  </svg>
  <figcaption>Fig. 1 - One-line takeaway of the diagram.</figcaption>
</figure>
```
Diagrams are optional. Include one for the Cover Story and at most one or two more. Keep them simple
(boxes, arrows, a labelled panel). Reuse the shapes in `example-issue-01.html` as models.

### Quiet aside (mono footnote)
```html
<p class="quiet">// A short aside or meta-note.</p>
```

### Action board (always the last department)
```html
<div class="board">
  <div class="todo">
    <div class="owner">Owner: Aravind &middot; with Arjun</div>
    <h5>Ship a weekly leads digest</h5>
    <p>What they'll do next week.</p>
  </div>
</div>
```

---

## 6. Self-check before the issue is done (this is a gate, not a suggestion)

**Design**
- [ ] Zero `—` (em-dash) anywhere, including HTML comments.
- [ ] No "Claude vs Sonnet"-style family/model confusion; exact model names used.
- [ ] `{{ISSUE_NO}}` identical in all three spots; contents numbers match `.dept-no` and `#id`s.
- [ ] coral used only for problems/emphasis, teal only for fixes/forward.
- [ ] Cover line and editor's headline are different sentences.
- [ ] Ends with an Action Board; starts with Model Watch.
- [ ] Looks like `example-issue-01.html` at a glance (rhythm: mostly light, 1-2 spreads, 1-2 dark panels).

**Publication safety (rules 8-14)**
- [ ] No customer / client / project / product / codename anywhere. Customers appear only as `a customer` or by nature. (If any customer's nature was unknown, the user was asked - not guessed.)
- [ ] No PII and no company-identifying info in any customer story (no partner names, no headcounts that fingerprint them).
- [ ] No internal vendor-relationship details (partnerships, credits, subscriptions, discounts).
- [ ] Every third-party factual claim was web-verified and cited, or attributed to a user-supplied source, or dropped. None left asserted-but-unverified.
- [ ] No internal gaps / weaknesses / roadmap / insecure workarounds. Tool capabilities kept.
- [ ] Internal evaluations and experiments are present (that is the point) and carry no personal information.
- [ ] Employee/speaker names kept and correctly attributed.
