---
name: accessibility-audit
description: Audits a web page, component or its source code against WCAG 2.2 level AA - keyboard access, visible focus, colour contrast, text alternatives, form labels and errors, ARIA misuse, motion and timing, target size - plus right-to-left and Arabic-specific issues. Produces a findings table with the success criterion, severity, affected element and a concrete code-level fix. Use when reviewing UI before release, responding to an accessibility complaint, or checking a bilingual or RTL interface.
---

# Accessibility Audit

Audit what a user actually experiences: with a keyboard only, with a screen reader, zoomed to 200%, with reduced motion, and in Arabic right-to-left layout.

## When to use
- A page, component or design system piece is about to ship.
- Someone reported they cannot use a feature with assistive technology.
- An English interface has been localised into Arabic.

## Inputs
Rendered HTML or a URL you can inspect, component source, design tokens (colours), and the target languages. State what you could not test (for example, a live screen reader) rather than assuming it passes.

## Method
Work through each area; record a finding only with a specific element and evidence.

1. **Keyboard (2.1.1, 2.1.2, 2.4.3)**
   - Every interactive element reachable by Tab, operable by Enter/Space, arrow keys inside composite widgets (tabs, menus, listboxes).
   - No keyboard traps; modals trap focus while open and return it to the trigger on close; Escape closes.
   - Clickable `div`/`span` without role and key handling is a finding - prefer native `button`/`a`.
2. **Focus (2.4.7, 2.4.11)**
   - Visible focus indicator on every focusable element; `outline: none` with no replacement is a finding.
   - Focused element not hidden behind sticky headers, cookie bars or chat widgets.
3. **Contrast (1.4.3, 1.4.11)**
   - Text 4.5:1; large text (24px, or 18.66px bold) 3:1; icons, input borders and focus rings 3:1 against adjacent colours.
   - Check hover, disabled-looking-but-active, placeholder and dark-mode states, not just the default.
4. **Text alternatives (1.1.1)**
   - Informative images: `alt` describing purpose. Decorative: `alt=""`. Icon-only buttons need an accessible name.
   - Charts need a text summary or data table.
5. **Forms (1.3.1, 3.3.1, 3.3.2, 4.1.2)**
   - Every input has a programmatic label (not placeholder alone). Required state announced.
   - Errors: identified in text, linked with `aria-describedby`, focus or announce the error summary on submit.
   - `autocomplete` tokens on personal-data fields (1.3.5).
6. **ARIA misuse**
   - First rule: no ARIA when native HTML works. Flag `role="button"` on a link, `aria-label` that contradicts visible text (2.5.3), `aria-hidden="true"` on focusable content, missing required states (`aria-expanded`, `aria-selected`).
   - Live regions for toasts and async results; not for whole page sections.
7. **Structure (1.3.1, 2.4.2, 2.4.6)**: one `h1`, logical heading order, landmarks (`main`, `nav`), unique page titles, descriptive link text.
8. **Motion and timing (2.2.1, 2.2.2, 2.3.3)**: respect `prefers-reduced-motion`; auto-playing carousels have pause; session timeouts warn and allow extension.
9. **Target size and input (2.5.8, 2.5.7)**: targets at least 24x24 CSS px or spaced; drag actions have a single-pointer alternative.
10. **Reflow and resize (1.4.4, 1.4.10, 1.4.12)**: usable at 320px width and 200% zoom without horizontal scroll; user-adjusted text spacing does not clip content.
11. **Authentication and help (3.3.8, 3.2.6)**: no cognitive tests without an alternative; allow paste in password and code fields; help links in a consistent place.

## RTL and Arabic checks
- `<html lang="ar" dir="rtl">` set; mixed-language fragments use `lang` and `dir="auto"` or `<bdi>` so screen readers switch voice and numbers do not flip.
- Use logical CSS properties (`margin-inline-start`) so layout mirrors; directional icons (back/next arrows, progress) mirror, but media controls and logos do not.
- Arabic text not set in letter-spacing (breaks joining) or ALL-CAPS-style transforms; line height sufficient for diacritics.
- Arabic font size often needs to be one step larger than Latin for equal legibility - verify contrast at the actual weight.
- Accessible names and `alt` text translated, not left in English.
- Phone numbers, codes and Latin product names wrapped so they keep LTR order.

## Severity
- **Blocker** - a task cannot be completed (keyboard trap, unlabeled submit, captcha with no alternative).
- **Serious** - completable with major difficulty (low contrast body text, missing error text).
- **Moderate** - friction (missing landmarks, small targets).
- **Minor** - best practice.

## Output format
```
| # | WCAG SC | Severity | Element / location | Problem | Fix |
```
Then: a short summary by area, a list of what was not tested, and the top three fixes by user impact. Do not claim the product "is WCAG compliant"; report findings against the criteria checked and recommend testing with real assistive-technology users.

## Worked example
`<div class="btn" onclick="save()"><svg>...</svg></div>` in a toolbar.
- 2.1.1 / 4.1.2, Blocker: not focusable, no name. Fix: `<button type="button" aria-label="Save"><svg aria-hidden="true">...</svg></button>`; in Arabic UI `aria-label="حفظ"`.

## Pitfalls
- Relying only on automated scanners; they catch roughly a third of issues.
- Fixing contrast in light mode only.
- Adding ARIA to compensate for wrong elements instead of using the right element.
