# Mobile Screen Runbook

**The brief, the traps, and the order of operations for getting an AI to design a full mobile app in Figma.**

Derived from a 118-screen build (18 flow groups, 3 personas, 367 prototype hotspots) that shipped with a clean structural audit and zero unreachable screens.

---

## 1. Skills to install first

Install these before handing over the brief. The first two carry mobile-specific pattern libraries; the rest ship with Claude Code and only need naming in the prompt so the agent actually reaches for them.

| Install | What it gives you |
|---|---|
| `npx skills add wshobson/agents --skill mobile-ios-design` | iOS conventions — safe areas, device chrome, native navigation patterns, touch-target sizing. |
| `npx skills add ceorkm/mobile-app-ui-design --skill mobile-app-ui-design` | Mobile UI pattern vocabulary: hubs, sheets, tab bars, empty and error states. |
| `ui-ux-pro-max` *(built in)* | Searchable styles, palettes, font pairings, product archetypes, UX guidelines. |
| `frontend-design` / `ui-design-system` *(built in)* | Token systems and visual direction that doesn't read as templated. |
| `dataviz` *(built in)* | Load before any chart. Enforces one scale, and keeps status colour out of data. |

---

## 2. The brief

Replace the bracketed values. Everything else is load-bearing — each clause exists because its absence produced a defect in a real build.

```text
Design the complete mobile app for [PRODUCT] in Figma.

Repo: [GIT URL, BRANCH]
Figma file: [URL] — the desktop designs are already on the canvas.
Reference/inspiration: [PATH OR URL]

Work to production quality, not concept quality. I will review at full size.

=== 1. GROUND TRUTH BEFORE YOU DESIGN ANYTHING ===
Clone the repo and read it. Then read the desktop frames on the canvas.
- Enumerate every route/page in the codebase.
- Extract REAL field names, enums and step counts from schemas, type
  definitions and endpoint constants. Never invent a status value, a form
  field or a wizard step. Every pill, label and input must trace to code.
- Screenshot the key desktop frames and match their visual language:
  colours, radii, spacing, component shapes.

Then produce a GAP ANALYSIS in BOTH directions and show me before designing:
  a) Designed on canvas but NOT built in code
  b) Built in code but NEVER designed
Do not silently design either side. Ask me what to do with each gap.

=== 2. FLOW INVENTORY BEFORE YOU BUILD ===
List every flow and its REAL step count from the code. A 5-step onboarding
is 5 screens, not 1. "One screen per feature" is the #1 failure mode.
State the total and get my confirmation before mass-producing.
Each module needs: hub + feature screens + at least one multi-step flow +
empty states + error states. Not just the happy path.

=== 3. BUILD A PRIMITIVE SYSTEM FIRST ===
Before any screen, build one token/primitive file: status bar, nav bar,
headers, buttons, inputs, cards, chips, list rows, stat tiles, empty state,
sheets. Compose every screen from those.
ENCODE THE RULES IN THE PRIMITIVES so screens cannot violate them:
- Status colours (green/amber/red) reachable ONLY through a statusPill()
  helper. Never decorative, never in charts.
- Category/chart colour comes from a separate monochrome brand ramp.
- Icons sit in soft-tinted boxes keyed by FUNCTION category (~6 tones),
  not by state.
- Repeating grids go through a chunk(items, perRow) helper that emits
  explicit rows. Never rely on flex-wrap.
- Spacers always carry explicit cross-axis dimensions.

=== 4. HARD LAYOUT RULES ===
- Design at 390x844. NO SCREEN MAY BE SMALLER THAN 390x844. Content may
  extend past 844; it may never fall short. Enforce with a post-pass, not
  by hand.
- Never hand-guess frame heights. Let frames hug real content, then floor
  them at 844. (Guessed heights once left every screen ~2x too tall with a
  huge dead-space tail.)
- Every screen gets real chrome: status bar, header, and bottom tab bar or
  home indicator — consistent across a flow.
- The bottom nav must be the LAST in-flow child, or a trailing spacer will
  push it into the middle of the screen.
- Decision UI (confirm, pick, quick-add) = bottom SHEET. Anything that
  scrolls = full SECTION. Never the other way round.
- Sheets must be REAL overlays: clone the underlying screen, place it
  behind a dim scrim, so live content shows through. A card floating on
  flat grey is not an overlay.
- Floating action buttons: position them in a post-pass pinned to the
  bottom-right above the nav. Do not position them inline.
- One primary add action per screen. A FAB plus an "Add" link on the same
  screen is a bug.

=== 5. PERSONAS ===
If the product has multiple user types, give each a genuinely distinct
visual system — recognisable from three metres. Different ground colour,
different accent, its own tab set matching that persona's real navigation
in the code. Keep the brand's primary for the main persona.

=== 6. CONTENT ===
- One explanation per screen, maximum. Facts and rules, no narrative.
- Extra prose budget only for consent, deletion, safety and irreversible
  actions.
- Real content throughout. No lorem, no "Item 1".
- Prefer tables over charts past ~7 categories.
- Use real assets. If a logo exists, embed it — never a text-initial badge.

=== 7. NAMING AND CANVAS ORGANISATION ===
- Name frames {Group}{n} - {description}, e.g. "H3 - Add Guest".
- One row per flow group, with a persona/function banner above each row.
- Keep hotspot names unique per DESTINATION, not per position, so
  prototype wiring survives reordering.
- Wrap everything in one named Section.

=== 8. PHASING ===
Build in phases of one flow group at a time. After each phase:
- Screenshot at FULL SIZE (scale 1), not thumbnails. Thumbnail review
  hides real defects.
- Screenshot at least one full-size example per distinct PATTERN (stat
  tiles, forms, lists, heroes, sheets), not just per flow.
- Show me before continuing.

=== 9. AUDIT — AUTOMATE IT, DON'T EYEBALL IT ===
Write a script that walks every screen and reports:
- any screen under 390x844
- any frame not clipping its content
- bottom nav not the last in-flow child
- text wider than its parent
- absolutely-positioned nodes outside their frame bounds
- children overflowing horizontally
- empty text nodes / zero-size nodes
Re-run it after every phase. It must return all zeros before you call the
work done. Report the numbers to me.

=== 10. PROTOTYPE — WIRE EVERY SCREEN ===
- Every nav tab to that persona's hub.
- Every back arrow to its real parent, not a generic home.
- Every CTA to the next step. Sheets slide up, sections push in, tabs fade.
- Then write a REACHABILITY CHECK: walk the graph from every flow starting
  point and report unreachable screens and dead ends. Both must be zero.
- Empty/error states cannot hang off the same control as their populated
  version — expose those as their own flow starting points.
- Strip prototype reactions from any cloned/decorative layers so dimmed
  backdrops aren't clickable.

=== 11. REPORTING ===
Tell me what you could not verify. If you spot-checked rather than checked
everything, say so and say which. Never report success you did not confirm.
```

---

## 3. Connecting an AI to Figma

*Skip this if your agent writes to Figma another way. If it drives Figma Desktop through `figma-cli`, these two locks will stop it — and the error messages don't explain either.*

### Lock 1 — the stripped switch

Figma removes `--remote-debugging-port` at startup, so the flag appears in the process command line but nothing ever binds to the port. `figma-cli` ships a byte-patcher for its `app.asar`:

```js
import('./src/figma-patch.js').then(m => {
  console.log(m.isPatched(), m.canPatchFigma());
  m.patchFigma();   // reversible: m.unpatchFigma()
});
```

Back up `app.asar` first. The patch only takes effect on the **next launch**, and a Figma auto-update ships a fresh unpatched folder — re-patch after every update.

> **Don't let the CLI restart Figma.** Its `connect` runs `taskkill /F`, a force-kill, which is the documented data-loss path. Close the window gracefully, let it flush, stop the leftover tray process, then relaunch with the flag yourself.

### Lock 2 — the plugin gate

Even with the port bound, `window.figma` stays `undefined` until a plugin is running. This is deliberate and **not bypassable** — direct access, `new Function`, calling the getter explicitly, deep stacks, `setTimeout`, promise chains and an injected page-origin `<script>` all return undefined. Someone must open the companion plugin once per session.

The payoff: once it's open, `figma` becomes live in the **main page context**, so you can drive it over CDP directly and skip the plugin's own socket. That buys two things the socket doesn't:

- **Large payloads survive.** A 65 KB image-embedding call died on the socket and succeeded first try over CDP.
- **Work continues with the Figma window minimised**, which kills the "I can't leave this screen" problem.

---

## 4. Traps

These fail **silently** — no error, no warning, just wrong output. Paste the relevant ones into your brief as explicit "do not do this" instructions.

| Signature | What happens | Fix |
|---|---|---|
| `<br/>` inside `<Text>` | **Deletes the entire text node.** No error — the heading simply vanishes. | Split line breaks into sibling Text nodes. |
| `bg="transparent"` | **Not a valid paint.** Becomes `NaN` and aborts the whole batch mid-render, leaving orphan partial frames. | Omit the fill attribute entirely. Then hunt the orphans it already made. |
| `items="center"` on the parent | **Does not centre text.** A Text child stretches to full width and stays left-aligned while fixed-width siblings (icons) centre correctly. | Full-width text needs `w="fill"` **and** `align="center"`. |
| `overflow="hidden"` | **Does not set `clipsContent`.** Overlay clones taller than the frame spill out and show the underlying screen's footer below the sheet. | Force `clipsContent = true` in a post-pass. |
| `position="absolute" y={-84}` | **Measured from the parent's TOP-left, not the bottom.** A negative Y meant as "up from the bottom" put a floating button in the header. | Emit floating elements unpositioned; pin in a post-pass: `y = h - navHeight - size - margin`. |
| Section child coordinates | **Section children are section-RELATIVE**, like frames — not page-absolute. Appending at x=30000 into a section at x=29880 lands it at 59880. | Lay out in section-local space, append first, then assign local x/y, resize, position the section last. |
| `opacity` on a frame | **Dims its icon children too.** A translucent tile made its own icon invisible. | Use a solid mid-tone fill instead of opacity. |
| `stroke-dasharray` for donut segments | **Renders as a repeating dash pattern** around the whole circle — a striped ring, not four arcs. | Draw real filled arc paths and import as SVG. |
| `"error": "Plugin disconnected"` | **The render may have SUCCEEDED.** Payloads ~150 KB break the response, not the operation. Blindly retrying produces a full set of duplicates. | Count frames and check duplicate names *before* re-rendering. Cap batches at ~10 screens. |
| Trailing spacer after the nav | **Pushes the bottom nav up the screen.** On one screen it sat 271px from the bottom. | Pass the nav so it lands last, or re-order it to the final in-flow child. |
| Self-navigation in a prototype | **Rejected by the API.** A hotspot whose destination is its own top-level frame throws on commit. | Skip edges where source screen === destination screen. |
| Cloned overlay backdrops | **Carry copies of the source screen's hotspots**, so tapping the dimmed area behind a sheet navigates. | Strip reactions inside clones after every re-clone. |
| `flex-wrap` | **Not reliably honoured**, and breaks silently at scale. | Chunk repeating items into explicit N-per-row rows. |
| `₦` and other struck glyphs | **The Naira sign is legitimately an N with a crossbar.** It looks like a strikethrough and isn't — a whole debugging cycle went into chasing this ghost. | Verify against an existing frame first. Separately: bold text like `48.2M` *can* hit a real glyph bug — split into a bold number plus a regular-weight unit. |

---

## 5. Build pipeline

Run this after every phase. Two orderings are load-bearing and produce wrong output if swapped.

1. **render** — in batches of ≤ 10 screens, then verify the count and check for duplicate names.
2. **assets** — swap logo/brand-mark placeholders for real embedded images and vectors.
3. **charts** — replace chart placeholders with real vector geometry.
4. **alignment** — centre any full-width text that needs it.
5. **floor to 844** — must run **before** cloning, or you clone the short pre-floor version.
6. **overlay clones** — clone each sheet's underlying screen behind its scrim.
7. **cleanup** — force `clipsContent`, remove markers, strip clone hotspots.
8. **fix pass** — nav to last in-flow child, pin FABs, fix overflowing text.
9. **layout** — rows, banners, section bounds.
10. **audit** — must return all zeros. If not, fix and re-run from the failing step.

> **Also fix it at source.** A post-pass repairs what exists; the generator will recreate the same defect on the next phase unless the primitive is fixed too. Do both, every time.

---

## 6. How to review it

### What actually catches defects

- **Insist on full-size screenshots.** Thumbnail review at ~195px wide hides everything. Both defects the client caught first were invisible at that scale.
- **Ask for the audit numbers**, not "it looks good". Numbers are checkable; adjectives aren't.
- **Ask what wasn't verified.** At 100+ screens nobody checks every pixel — the honest answer is "one per pattern", and you should know which.
- **Push back on anything that looks off.** Every single thing that looked wrong in this build *was* wrong, including the ones that looked like taste.

### Red flags in the agent's output

- Screens shorter than the device — heights aren't being floored.
- A confirmation card floating on flat grey — the sheet isn't a real overlay.
- Status green/amber/red inside a chart or used as a category colour.
- Field names or statuses that don't appear anywhere in your codebase.
- A flow with one screen where the code has five steps.
- Only happy paths — no empty, error or rejected states.
- "Fixed" with no screenshot and no re-run of the audit.

---

## 7. Pre-flight

- [ ] Every flow built at its real step count from the code, not flattened.
- [ ] Gap analysis delivered in both directions, and each gap explicitly resolved or deferred.
- [ ] Every field name, status and enum traces to real code.
- [ ] Each persona is distinguishable from three metres.
- [ ] Chrome present and consistent on every screen.
- [ ] No screen smaller than 390×844.
- [ ] Decision UI is sheets; scrolling content is sections.
- [ ] Sheets show real underlying content through the scrim.
- [ ] Status colour used only for status — never decoration, never in charts.
- [ ] Real logo and assets, not text-initial placeholders.
- [ ] Empty and error states designed, not just happy paths.
- [ ] Structural audit returns all zeros.
- [ ] Prototype wired; reachability check reports zero unreachable and zero dead ends.
- [ ] Full-size screenshot reviewed for every distinct screen pattern.

---

*Sections 3–5 are specific to driving Figma Desktop through `figma-cli`. If your next project uses a different bridge, sections 1, 2, 6 and 7 still transfer intact — the traps won't.*
