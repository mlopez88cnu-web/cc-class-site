# PRD: ROI Calculator

## Overview

An interactive, ungated ROI calculator embedded as a section on `index.html` that lets VP/Director-level B2B SaaS marketing leaders quantify the financial impact of hiring The Lopez Group. The output is a shareable summary they can copy and send to their CFO or CEO to build a business case.

---

## Why This Matters

Our persona's #1 blocker isn't interest — it's internal buy-in. They need to justify the spend to a CFO or CEO with hard numbers. Right now that burden falls entirely on them. This calculator does the math for them and produces a ready-made business case they can share in 60 seconds.

From the persona doc:
- "Needs internal buy-in — has to justify the spend to a CFO or CEO; needs clear ROI projections and a concise business case."
- "I can't afford to add another vendor right now" is a top objection.

---

## Placement

Embedded section on `index.html`, positioned between the "First 90 Days" timeline and the final CTA. Anchor: `#roi-calculator`.

---

## User Inputs

The calculator asks for four numbers the persona already knows off the top of their head:

| Input | Type | Default | Why |
|---|---|---|---|
| Monthly marketing spend | Currency ($) | $50,000 | Their existing budget — frames the retainer as a % of current spend |
| Average deal size (ACV) | Currency ($) | $40,000 | Needed to calculate revenue impact |
| Current deals closed per quarter | Number | 10 | Baseline to project improvement against |
| Monthly agency retainer | Currency ($) | $15,000 | The Lopez Group's flat monthly fee — what they're evaluating |

### Input Constraints
- All fields required, numeric only, minimum value of $0 / 0
- Currency fields formatted with commas and dollar sign as user types
- Defaults pre-filled so the calculator shows results immediately on load

---

## Calculation Logic

The calculator models a conservative (3x) and strong (5x) ROI scenario based on the agency's track record.

```
quarterly_retainer = monthly_retainer * 3

# Conservative scenario (3x ROI)
conservative_revenue = quarterly_retainer * 3
conservative_new_deals = conservative_revenue / acv
conservative_total_deals = current_deals + conservative_new_deals
conservative_net = conservative_revenue - quarterly_retainer
conservative_roi_pct = (conservative_net / quarterly_retainer) * 100

# Strong scenario (5x ROI)
strong_revenue = quarterly_retainer * 5
strong_new_deals = strong_revenue / acv
strong_total_deals = current_deals + strong_new_deals
strong_net = strong_revenue - quarterly_retainer
strong_roi_pct = (strong_net / quarterly_retainer) * 100
```

### Displayed Outputs

| Metric | Conservative (3x) | Strong (5x) |
|---|---|---|
| New pipeline revenue per quarter | `conservative_revenue` | `strong_revenue` |
| Additional deals closed per quarter | `conservative_new_deals` | `strong_new_deals` |
| Net ROI (revenue minus retainer) | `conservative_net` | `strong_net` |
| ROI percentage | `conservative_roi_pct` | `strong_roi_pct` |
| Retainer as % of new revenue | `quarterly_retainer / revenue * 100` | same |

### Output Presentation
- Two side-by-side columns: "Conservative (3x)" and "Strong (5x)"
- All currency values formatted with $ and commas
- Deal counts rounded to one decimal place
- ROI percentage displayed as "200%" / "400%" format
- Results update in real time as inputs change (no submit button)

---

## Shareable Summary

Below the results, a "Copy Business Case" button generates a plain-text summary to the clipboard:

```
ROI Projection — The Lopez Group
──────────────────────────────

Current state:
• Monthly marketing spend: $50,000
• Average deal size: $40,000
• Deals closed per quarter: 10

Proposed investment:
• Monthly retainer: $15,000 ($45,000/quarter)

Projected quarterly impact:
              Conservative    Strong
New revenue:  $135,000        $225,000
New deals:    3.4             5.6
Net ROI:      $90,000         $180,000
ROI:          200%            400%

Based on 3–5x ROI observed across B2B SaaS clients
in the $10M–$150M ARR range.

→ thelopezgroup.com
```

### Share Behavior
- Single "Copy Business Case" button
- On click: copies the text above to clipboard
- Button text changes to "Copied!" for 2 seconds, then reverts
- No email gate, no form, no friction

---

## Design & UX

### Visual Style
- Matches existing site design system (indigo accent, same fonts, border-radius, card styles)
- Section background: `#f8f8ff` (consistent with alternating pattern)
- Input fields: clean, large, with labels above and $ prefix inside the field
- Output cards: two side-by-side cards with a subtle indigo left border on the "Strong" card to draw the eye

### Interaction
- All defaults pre-filled so results are visible immediately without any interaction
- Results recalculate live on every keystroke (debounced)
- No submit button — feels like a tool, not a form
- Inputs and outputs visible on screen at the same time (no scroll between them on desktop)

### Layout (Desktop)
```
┌─────────────────────────────────────────────────┐
│  Section Label: "Calculate Your ROI"            │
│  Heading: "Run the numbers before the call."    │
│  Subhead: ...                                   │
│                                                 │
│  ┌─── Inputs ──────────┐ ┌─── Results ───────┐ │
│  │ Marketing spend      │ │ Conservative  Strong│
│  │ Deal size            │ │ Revenue:   $X   $X │
│  │ Deals/quarter        │ │ Deals:     X    X  │
│  │ Monthly retainer     │ │ Net ROI:   $X   $X │
│  └─────────────────────┘ │ ROI %:     X%   X% │
│                          └────────────────────┘ │
│                                                 │
│          [ Copy Business Case ]                 │
└─────────────────────────────────────────────────┘
```

### Layout (Mobile)
- Inputs stack full-width above results
- Results stack vertically: conservative card, then strong card
- Copy button full-width at bottom

---

## Scope & Constraints

### In scope
- HTML, inline CSS, and vanilla JavaScript (no dependencies)
- Embedded as a section in the existing `index.html`
- Real-time calculation, no server calls
- Clipboard copy for shareable summary

### Out of scope
- Email capture or any gating
- PDF export
- Saving or persisting calculator state
- Custom scenario sliders beyond the four inputs
- Backend or database

---

## Success Criteria

This ships well if:
1. A VP of Marketing can land on the page, see pre-filled results immediately, adjust 1–2 numbers, and copy a business case to send to their CFO in under 60 seconds
2. The shareable summary is clean enough to paste into a Slack message or email without editing
3. The section feels like a natural part of the existing page, not a bolted-on widget
