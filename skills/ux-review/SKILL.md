---
name: ux-review
description: "Design or review a screen or flow of a data-dense internal tool (admin panel, back office, dashboard — forms, tables, bulk actions, irreversible or money-moving operations, multilingual text) for usability, using rulings distilled from Norman, Nielsen, Tognazzini, GOV.UK, Magic Ink, and WCAG. Use when the user asks for a UX review, whether a page, form, table, or flow is confusing or hard to use, whether an action needs a confirmation dialog or undo, when and where to validate a form, what belongs in a list versus a detail page, how dense a screen should be, or is about to build a new admin screen and wants the interaction right. Not for markup-level accessibility or checklist audits (web-design-guidelines), brand or visual style (frontend-design), or code structure (complexity-review)."
---

# UX Review

An interface has one job: let the user see **what state things are in** and **what they can do**.
Judge every finding by what it costs the **user** (a click per row, a remembered number, lost input, a wrong payment), not by taste.

Scope: internal tools used repeatedly by a few people. Mechanical checks (contrast ratios, focus rings, `label`, `tabular-nums`, `aria-live`, target sizes) belong to `web-design-guidelines`; don't repeat them here.

## Before building or reviewing

Answer these three first. They decide which rules apply.

1. **Who uses it, and how often?** Repeated use by practiced users → dense, comparable, bulk actions. A rare, high-risk flow → one step per page.
2. **Is this screen for looking or for changing?** A looking screen (list, report, overview) is judged by how many questions it answers with no click. A changing screen (form) is judged by feedback, error prevention, and recovery.
3. **Write down the 3–5 questions the user arrives with.** Then check that the screen answers each one without a click. If you can't write them, you don't understand the screen yet.

## Rulings

The sources disagree on these. Use this side.

- **Remove irrelevant information, never necessary controls.** A control needed to finish the task is always visible, not behind hover, right-click, or a "⋯" menu.
- **One visible main path.** Keyboard shortcuts accelerate that path; they are never the only way in and never mean something different.
- **Undo beats confirm.** Reversible action → no dialog, offer Undo. Irreversible and severe → confirm, naming the object, the count, and the amount; label buttons with the outcome ("Delete 3 records" / "Keep"); never make the dangerous choice the default.
- **Validate on submit, on the server.** On failure, redisplay the page with every answer kept, an error summary at the top, a message beside each failing field, and an error prefix in `<title>`. Use `novalidate` and write the messages yourself.
- **Opinionated flow, free presentation.** A process has one order of steps. Columns, sort, filters, and language belong to the user and are remembered.
- **Accept liberally, display strictly.** Take any unambiguous format (thousands separators, full-width digits, mixed date separators, stray whitespace). Don't block keystrokes or paste.

## Red flags

| Flag | Signal |
|---|---|
| Hand instead of eye | A common question needs a click per row to answer; data to be compared sits on different pages or tabs. |
| Hidden necessary control | A frequent action exists only on hover, in a context menu, or inside a collapsed menu. |
| Same look, different behavior | Links and buttons, or clickable and non-clickable rows, look alike; or one behavior has two looks. |
| Competing primaries | More than one top-weight action on a screen; a destructive action at the same weight beside the primary. |
| Label louder than data | Labels are larger, bolder, or darker than the values they describe. |
| Invisible state | Which period or record, what stage it is in, whether the view is filtered, whether you are acting as someone else — not persistently shown, or shown by color alone. |
| Hidden mode | The same action gives a different result depending on state the user can't see. |
| Cry-wolf confirm | A dialog on a routine or reversible action; "Are you sure?" with Yes / No. |
| Unreviewed commit | A legal, financial, or data-changing submission that is not reversible, not checked with a chance to correct, and has no review step (WCAG 3.3.4 requires one of the three). |
| Lost work | Fields cleared after a failed validation; leaving a page with unsaved changes gives no warning. |
| Empty error | "Invalid", an error code, "Something went wrong"; no fix suggested; not beside the failing field; wording doesn't match the field's label. |
| Misfiled error | No permission, not eligible, or a system failure shown as a field validation error instead of an explanation with a next step. |
| Strict input | Rejects an unambiguous format; blocks typing or paste. |
| Silent action | No visible change within ~100 ms of a click; no busy indicator past 1 s; no progress and cancel past 10 s; progress that isn't real. |
| Missing state | Only the well-populated view was designed: no empty, sparse, dense, error, or loading state. |
| Dead end | An empty, error, or no-permission screen with no next step; an empty state with no way to create the first item. |
| Fixed-width text | A button, tab, column header, or label with a fixed width. Short strings grow the most in translation (up to 2–3×). |
| Two words, one concept | One concept has two names within a language, or an invented name. If the project has a glossary, it is the source. |
| State outside the URL | Filter, sort, tab, or page is lost on refresh, back, or sharing the link. |

## Too far (don't flag, don't do)

- Removing a necessary capability to look simple.
- Citing "7±2" to cut options. It supports grouping, not limits.
- One question per page for a flow practiced users run every week or month.
- Confirming everything "to be safe".
- Validating on blur, interrupting the user mid-entry.
- Animating frequent actions. Animate only to show cause and effect.
- Making things that behave differently look the same for uniformity's sake.
- Inventing a new style or control on an existing interface. Use the partials, components, and classes already there.

## Verify

At most two rounds, then stop.

1. Each language with real copy. Look at the short strings: buttons, tabs, column headers, labels.
2. Worst-case data: the longest name, zero rows, very many rows, negative, zero, and empty values.
3. The main task by keyboard only.

## Output

One entry per finding, most costly first:

```
[flag] path:line — what the user sees
  cost: what the user pays — concrete consequence
  fix: smallest change that removes the flag
```

End with a one-line verdict: does this change make it easier or harder for the user to see the state and what they can do?
