---
name: user-story-template
description: THE default user story template for all teams (HMIS, Mobile, Website). Format or create Azure DevOps PBI user stories using the structured, table-free format — styled story statement, numbered User/System Scenario steps, numbered Data Sections fields, Given/When/Then Acceptance Criteria cards, and Alerts & Messages cards. ALWAYS trigger this skill when the user wants to write, format, reformat, create, or rewrite a user story. No exceptions.
---

# User Story Template (Structured, Table-Free Format)

## Core Mandate

You are an Agile Business Analyst. Your job is to take raw, unstructured, or existing user story content and format it into the **structured, table-free template**. The Description and Acceptance Criteria fields use **numbered items and lightly styled blocks** (inline-CSS callouts, role badges, keyword colors) — **never HTML tables**.

**This is the default and only user story template.** It must be used whenever a user story is written or reformatted, regardless of which team (HMIS, Mobile, Website) the story belongs to. No exceptions.

## When to Use This Skill — ALWAYS

Trigger this skill whenever the user:
- Wants to **write** a new user story or PBI
- Wants to **format** or **reformat** a user story
- Says "rewrite this story", "reformat this PBI", "use the same style as story X"
- Says "create a new user story" or "write a PBI for..."
- References a story ID and wants another story formatted the same way
- Provides raw bullet points or notes and wants them turned into a proper user story
- Wants to apply "the template" or "the amgad template"

**There are no exceptions.** Any request involving writing, creating, or formatting a user story must use this skill.

## Workflow

1. **Read the source story** (if reformatting): use the ADO MCP get-work-item tool (`ado_get_work_item`) to fetch the current Description and Acceptance Criteria.
2. **Read the reference story** (if the user says "like story X"): fetch it to extract the content. Reference story: PBI 20872. Keep its content but always output it in this template's styling.
3. **Determine the module** (see Module Identification below).
4. **Suggest and ask for the custom fields**: Ticket Number, Business Value Category, Business Impact Value and Impact Level (see Custom Fields below). Suggest a value for each field except Ticket Number, then ask the user to use the suggestions or enter their own. Always ask, even when you are confident in the suggestions.
5. **Draft the Description** using the structure below.
6. **Draft the Acceptance Criteria** using the Given/When/Then cards below.
7. **Present the draft** to the user for review (unless they said "apply directly"). Show the custom-field values with the draft, and list every assumption you made under **Assumptions to confirm** (see Assumptions below). If the user has not answered the custom-field question yet, remind them once here.
8. **Apply to ADO** using the ADO MCP update tool (`ado_update_work_items`). Update `description`, `acceptance_criteria` and the confirmed custom fields (via `custom_fields`) in a single call. If the user said to skip, apply without the missing fields (see Skipping below).

---

## Module Identification

Before drafting the story, determine which **module** the story belongs to. The module identifies the system area and helps ensure the correct terminology, navigation paths, and domain context are used.

### How to identify the module:

1. **Check the existing work item** — if reformatting, read the `Custom.Module` field from ADO.
2. **Check the story content** — the user may mention a module name, screen name, or system area (e.g., "Lab and Rad", "Patient Registration", "Billing", "Appointments", "Pharmacy").
3. **Check the project/team** — if the story is in a Mobile or Website project, infer from the project name.
4. **If unsure, ASK the user.** Do not guess silently. Present the known modules and ask the user to confirm.

### Module determination rules:

- If the module is explicitly stated by the user → use it.
- If the module is in the ADO work item's `Custom.Module` field → use it.
- If the module can be inferred from the navigation path or screen names → use it, but mention your assumption to the user.
- If the module cannot be determined → **ask the user**: "Which module does this story belong to? (e.g., Lab and Rad, Patient Registration, Billing, Appointments, Pharmacy, etc.)"

---

## Custom Fields

Every story should have these four ADO custom fields. Suggest values for them, then ask the user to confirm.

| Field | ADO reference name | Type | Default suggestion | Example (PBI 49987) |
|-------|--------------------|------|--------------------|---------------------|
| Ticket Number | `Custom.Ticketnumber` | HTML / text | None. Always ask. | `123` |
| Business Value Category | `Custom.BusinessValueCategory` | Picklist | From the category rules below | `Compliance & Risk` |
| Business Impact Value | `Custom.BusinessImpactValue` | HTML / text | 1–2 lines written from the story | `Reduce rejection rate from diagnoses that are typically never covered by insurance` |
| Impact Level | `Custom.Impactlevel` | Picklist | `High` | `Critical` |

(This table is guidance for you only. These fields are separate ADO fields and never go inside the Description or Acceptance Criteria.)

**Reference story:** PBI 49987 has all four set. When you are unsure of a field's format, fetch PBI 49987 with `ado_get_work_item` and look at it.

### Suggesting the Business Value Category

The picklist values below are the exact ADO strings. Use them exactly, including case and `&`.

- **`Compliance & Risk`**: anything related to NPHIES, ZATCA, patient safety, or preventing users from doing something (blocking, validation, restrictions, disabling a button, mandatory checks).
- **`Customer & Strategic Value`**: anything patients will see or use (patient app, website, patient portal, SMS or notifications to patients, printouts handed to patients).
- **`Cost & Efficiency`**: anything about doing things in bulk, adding buttons or shortcuts, saving clicks or time, or otherwise making staff work faster.

If a story matches more than one rule, pick in this order: `Compliance & Risk`, then `Customer & Strategic Value`, then `Cost & Efficiency`. If none of the rules fits, suggest the closest one and say so in a few words.

### Suggesting the Business Impact Value

Write 1–2 plain lines that say what the business gains from the story. Base it on what the story actually does, and state the outcome, not the feature. Example: "Reduce claim rejections from diagnoses that insurance never covers."

### Suggesting the Impact Level

Suggest `High` by default. Still ask every time, because the user may want `Critical` or a lower level.

### How to ask

1. **Check the work item first.** If the PBI already has values for any of the four fields, show those as the current values instead of suggestions. Keep them unless the user changes them.
2. **Ask once, before drafting, in a single question.** Show the suggested (or current) value for each field and ask for the ticket number. Offer three answers:
   - **Use suggested values**: the user only needs to give the ticket number.
   - **Enter my own values**: the user gives any values they want to change, plus the ticket number.
   - **Skip and apply**: go ahead without the missing values (see Skipping below).
3. **Always ask, even when you are confident in the suggestions.** A suggestion is not written to ADO until the user accepts it.
4. **Remind once.** If the user has not answered when you present the draft, mention the open fields again in one line. Do not keep asking after that.
5. **Never invent a ticket number.**

Example question:

> Before I draft this, please confirm the custom fields:
> - Ticket Number: *(please provide)*
> - Business Value Category: **Compliance & Risk** (the story blocks saving a diagnosis)
> - Business Impact Value: **Reduce claim rejections from diagnoses that insurance never covers.**
> - Impact Level: **High**
>
> Use the suggested values, enter your own, or skip and apply?

### Skipping

If the user says to skip (for example "skip", "just apply", "apply anyway"), apply the story right away without asking about the fields again.

- Write only the values the user confirmed or that the PBI already had. Do not write suggestions the user did not accept.
- ADO's own required-field rules are the only control. If ADO rejects the update because a required field is empty, show the user the error with the field name and ask only for that field.
- After applying, say in one line which of the four fields are still empty on the PBI.

### How to write them

Pass them in the same `ado_update_work_items` call as the Description and Acceptance Criteria. Include only fields that have a confirmed value:

```json
"custom_fields": {
  "Custom.Ticketnumber": "123",
  "Custom.BusinessValueCategory": "Compliance & Risk",
  "Custom.BusinessImpactValue": "Reduce claim rejections from diagnoses that insurance never covers.",
  "Custom.Impactlevel": "High"
}
```

Impact Level and Business Value Category are picklists, so the value must match an allowed option exactly. If ADO rejects a value, show the user the error and ask for a valid value.

---

## Assumptions

Any behavior you put in the story that the user did not state and you did not verify is an assumption. Assumptions must be highlighted in the confirmation so the user can correct them before the story reaches ADO.

**Example of what went wrong:** in PBI 49987 (cash-only diagnoses), the draft included an AC for "several diagnoses, one cash-only" because it assumed the physician can add multiple diagnoses at once. In the real screen, diagnoses are added one at a time, so that AC was wrong. It should have been listed as an assumption for the user to confirm.

### What counts as an assumption

- **How the current screen works**: one item or many at a time, which buttons exist, what happens on edit, how the user reaches the screen.
- **Who and when**: which roles use it, which patients or visits it applies to (e.g. how "insured" is determined).
- **Scope**: what you included or left out (e.g. old records, other screens with the same function, a setup screen).
- **Data**: which table or column holds a value, defaults, how a new flag is maintained.
- **Behavior you added**: extra validation, a server-side check, a message, or an edge case the user did not ask for.

### How to handle them

1. **Verify first when you can.** Check the database, the code or a similar existing story before assuming. Anything you verified is not an assumption, but say where you verified it if it matters.
2. **List every remaining assumption in the confirmation**, under a heading **Assumptions to confirm**, numbered A-1, A-2, ... Each one is one line: the assumption, then which parts of the story depend on it (scenario steps, ACs, fields). Example:
   > **A-1** The physician can add several diagnoses at once before saving. *Affects AC-06 and scenario step 6.*
3. **Put the assumptions before the custom fields and before the apply question**, so they are not missed.
4. **Ask the user to confirm or correct them** in the same question that asks to apply. If the user corrects an assumption, update the story (remove or rewrite the affected steps and ACs), then show the changed parts.
5. **Do not write assumptions into the Description or Acceptance Criteria.** The story in ADO describes the agreed behavior only.
6. **If the user said "apply directly" or "skip"**, apply as asked, then list the assumptions in your reply after applying so the user can still check them.

---

## Styling Rules for ADO (read before writing any HTML)

ADO's rich-text sanitizer keeps **inline `style` attributes** but strips `<style>` blocks, `class` attributes, and scripts. So:

- **Inline CSS only.** Every style goes in a `style="..."` attribute.
- **Safe properties only:** `color`, `background-color`, `border`, `border-left`, `padding`, `margin`, `font-size`, `font-weight`, `display:inline-block`, `min-width`. No flexbox, grid, positioning, or shadows.
- **Every block that has a `background-color` must also set `color:#323130`.** ADO's dark theme turns default text white, which becomes unreadable on a light background.
- **No tables** anywhere — not for layout, not for data.
- **Palette** (Fluent / ADO colors, so the story looks native):

| Use | Color |
|-----|-------|
| Accent / story callout / WHEN | `#0078D4` border, `#F3F9FD` background |
| GIVEN | `#605E5C` |
| THEN | `#107C10` |
| USER scenario card | `#0078D4` left border and label, `#C7E0F4` border, `#F3F9FD` background |
| SYSTEM scenario card | `#605E5C` left border and label, `#E1DFDD` border, `#FAF9F8` background |
| Flow arrow, alternate-path dashes | `#A19F9D` |
| Muted text (types, notes, Arabic label) | `#605E5C` |
| Neutral border (field blocks, AC cards) | `#E1DFDD` / `#C8C6C4` |
| Error message | `#A4262C` border, `#FDE7E9` background |
| Warning message | `#C19C00` border, `#FFF4CE` background |
| Info message | `#0078D4` border, `#DEECF9` background |
| Success message | `#107C10` border, `#DFF6DD` background |

(This palette table is guidance for you only — never put a table in the story.)

---

## Description Format

### Structure

```
Story statement callout   (As a / I want to / so that — accent left border)
Context paragraph         (1–2 sentences of scope)
Navigation line           (Module › Screen › Action)
User/System Scenario      (a two-lane flow of step cards joined by ↓ arrows)
Data Sections             (one block per INPUT field, numbered F-01, F-02, ...)
```

### Rules for the Description

- **Story statement**: Always first, in the accent callout. Each of *As a / I want to / so that* on its own line with the keyword bold.
- **Context paragraph**: 1–2 sentences explaining scope.
- **Navigation**: Always include. Separate levels with `&rsaquo;`.
- **User/System Scenario**: A flow, not a list. Each step is a card; cards are joined by a centered `&darr;` arrow. **USER** cards (blue) sit in the left lane (`margin-right:80px`) and **SYSTEM** cards (gray) sit in the right lane (`margin-left:80px`), so the hand-offs between actor and system read as a zigzag. Each card's label carries the step number (`3 · USER`) so ACs and discussions can reference steps. Steps do not have to alternate — the system may respond with several consecutive cards. Alternate or exception paths go inside the card they branch from, as `&#8627;` lines in a dashed-left-border block. Never use `<ol>`/`<ul>` for the scenario.
- **Data Sections**: One block per **input field** (fields the user interacts with or enters data into). The field code `F-XX` is the item number. Each block shows: code + English name, Arabic name, field type · input type (Mandatory/Optional), then notes. Do NOT include display-only elements (labels, tooltips, read-only text, banners, grids, popups, sections, states) — only actual user inputs (text boxes, dropdowns, checkboxes, toggles, buttons, number inputs, date pickers, etc.).
- **No Channels, Is Data Changed, or Is Database Impacted**: These were removed from the template. Do not include them.

### HTML Template — Story Statement, Context, Navigation

```html
<div style="border-left:4px solid #0078D4;background-color:#F3F9FD;color:#323130;padding:10px 14px;margin:0 0 12px 0;">
<b>As a</b> [specific role],<br>
<b>I want to</b> [action],<br>
<b>so that</b> [value].
</div>
<p>[Context paragraph: what this story covers.]</p>
<p><b>Navigation:</b> [Module] &rsaquo; [Screen] &rsaquo; [Action]</p>
```

### HTML Template — User/System Scenario

Three building blocks — repeat USER / SYSTEM cards as needed, with one arrow between every pair of cards (none before the first or after the last).

```html
<h3>User/System Scenario</h3>

<!-- USER card (left lane) -->
<div style="border:1px solid #C7E0F4;border-left:4px solid #0078D4;background-color:#F3F9FD;color:#323130;padding:6px 12px;margin:0 80px 0 0;"><span style="font-size:11px;font-weight:600;color:#0078D4;">1 &nbsp;·&nbsp; USER</span><br>[user action]</div>

<!-- Arrow between cards -->
<div style="text-align:center;color:#A19F9D;font-size:14px;line-height:18px;margin:0;">&darr;</div>

<!-- SYSTEM card (right lane), with optional alternate paths -->
<div style="border:1px solid #E1DFDD;border-left:4px solid #605E5C;background-color:#FAF9F8;color:#323130;padding:6px 12px;margin:0 0 0 80px;"><span style="font-size:11px;font-weight:600;color:#605E5C;">2 &nbsp;·&nbsp; SYSTEM</span><br>[system response]<div style="border-left:2px dashed #A19F9D;margin:6px 0 0 2px;padding:0 0 0 10px;color:#605E5C;">&#8627; [alternate path: if X, the system ...]<br>&#8627; [another alternate path]</div></div>
```

Omit the inner dashed block when the step has no alternate paths. Write each card on one line with no whitespace between tags — ADO adds stray spacing otherwise.

### HTML Template — Data Sections (one block per input field)

```html
<h3>Data Sections</h3>
<div style="border-left:3px solid #C8C6C4;padding:4px 12px;margin:0 0 10px 0;">
<b>F-01&nbsp;&nbsp;[English name]</b>&nbsp;&nbsp;<span dir="rtl" style="color:#605E5C;">[Arabic name]</span><br>
<span style="color:#605E5C;font-size:12px;">[Text / Dropdown / Checkbox / Toggle / Button / Number / Date] · [Mandatory / Optional / -]</span><br>
[Notes: behavior, defaults, validation, source of values.]
</div>
```

---

## Acceptance Criteria Format

The Acceptance Criteria field uses **Given / When / Then** scenarios, one styled card per AC.

### Rules for the Acceptance Criteria

- **Numbering**: Each AC gets a code `AC-01`, `AC-02`, ... followed by a short scenario title (3–8 words).
- **Given / When / Then**: Every AC has at least one GIVEN, exactly one WHEN, and at least one THEN. Use **AND** for additional lines; an AND takes the color of the keyword it continues.
- **One behavior per AC**: If you need two WHENs, write two ACs.
- **Observable and testable**: Every THEN describes something a tester can verify on screen or in data.
- **Specific personas**: The GIVEN names the role (pharmacist, nurse, doctor, ...), never "the user".
- **Cross-reference messages**: If a THEN shows a message, append the message code in italics: `<i>(MSG-01)</i>`.
- **Keyword alignment**: Keywords use `display:inline-block;min-width:52px;` so the clauses line up. If ADO drops it, the card still reads correctly.

### HTML Template — Acceptance Criterion Card

Do not add an "Acceptance Criteria" heading — ADO already labels the field. The field starts directly with the AC-01 card.

```html
<div style="border:1px solid #E1DFDD;border-left:4px solid #0078D4;padding:8px 12px;margin:0 0 10px 0;">
<b>AC-01&nbsp;&nbsp;·&nbsp;&nbsp;[Short scenario title]</b><br>
<span style="display:inline-block;min-width:52px;color:#605E5C;font-weight:600;">GIVEN</span> [precondition, naming the role]<br>
<span style="display:inline-block;min-width:52px;color:#605E5C;font-weight:600;">AND</span> [additional precondition]<br>
<span style="display:inline-block;min-width:52px;color:#0078D4;font-weight:600;">WHEN</span> [single action or event]<br>
<span style="display:inline-block;min-width:52px;color:#107C10;font-weight:600;">THEN</span> [observable outcome] <i>(MSG-01)</i><br>
<span style="display:inline-block;min-width:52px;color:#107C10;font-weight:600;">AND</span> [additional outcome]
</div>
```

---

## Alerts and Messages Format

Alerts and Messages live **inside the Acceptance Criteria field**, after the AC cards. One card per message, colored by type. Only include this section if the story has messages.

### Rules

- **Code**: `MSG-XX` format, followed by a type label: `ERROR`, `WARNING`, `INFO`, `SUCCESS`.
- **Triggered by**: The AC code(s) that show this message.
- **Text**: English line, then Arabic line (`dir="rtl"`).
- **Colors**: Use the message palette above for the left border and background, and always set `color:#323130`.

### HTML Template — Message Card (Warning shown)

```html
<h3>Alerts and Messages</h3>
<div style="border-left:4px solid #C19C00;background-color:#FFF4CE;color:#323130;padding:8px 12px;margin:0 0 8px 0;">
<b>MSG-01</b>&nbsp;&nbsp;<span style="font-size:11px;font-weight:600;">WARNING</span>&nbsp;&nbsp;<span style="color:#605E5C;font-size:12px;">Triggered by AC-03, AC-04</span><br>
[English text]<br>
<span dir="rtl">[Arabic text]</span>
</div>
```

For the other types, swap the border and background: Error `#A4262C` / `#FDE7E9`, Info `#0078D4` / `#DEECF9`, Success `#107C10` / `#DFF6DD`.

---

## Complete Field Mapping

| ADO Field | Content |
|-----------|---------|
| `description` (System.Description) | Story statement callout + Context + Navigation + User/System Scenario + Data Sections |
| `acceptance_criteria` (Microsoft.VSTS.Common.AcceptanceCriteria) | Given/When/Then AC cards (no heading) + Alerts and Messages cards (if any) |
| `custom_fields` → `Custom.Ticketnumber` | Ticket number given by the user |
| `custom_fields` → `Custom.BusinessValueCategory` | Confirmed category (Compliance & Risk / Customer & Strategic Value / Cost & Efficiency) |
| `custom_fields` → `Custom.BusinessImpactValue` | Confirmed 1–2 line business impact |
| `custom_fields` → `Custom.Impactlevel` | Confirmed impact level (default suggestion: High) |

Description and Acceptance Criteria accept HTML. Set them and every confirmed custom field in the same update call.

---

## Anti-Patterns

- **Do NOT** use `<table>` anywhere in the story — use styled blocks.
- **Do NOT** write the User/System Scenario as a numbered or bulleted list — use the two-lane card flow.
- **Do NOT** add an "Acceptance Criteria" heading inside the Acceptance Criteria field.
- **Do NOT** write single-sentence "The system must..." ACs — every AC is Given/When/Then.
- **Do NOT** put more than one WHEN in an AC — split it.
- **Do NOT** use `<style>` blocks, `class` attributes, flexbox, or grid — ADO strips them.
- **Do NOT** set a `background-color` without also setting `color:#323130`.
- **Do NOT** include Channels, Is Data Changed?, or Is Database Impacted? — these were removed from the template.
- **Do NOT** use "User" as the persona — be specific (doctor, pharmacist, lab user, nurse, receptionist, patient, admin).
- **Do NOT** apply changes without user confirmation unless they said "apply directly".
- **Do NOT** skip the module identification step — always determine the module before drafting.
- **Do NOT** draft without first suggesting the custom-field values and asking the user to confirm them, even when you are confident.
- **Do NOT** write a suggested custom-field value to ADO unless the user accepted it, and **do NOT** invent a Ticket Number.
- **Do NOT** keep asking for custom fields after the user says to skip. Apply, and let ADO's required-field rules decide.
- **Do NOT** present or apply a draft without an **Assumptions to confirm** list. If you truly made no assumptions, say so in one line.
- **Do NOT** turn an unverified guess about how the screen works (e.g. "multiple items at once") into a scenario step or AC without listing it as an assumption.
- **Do NOT** include display-only elements (labels, tooltips, read-only text, banners, grids, popups, sections, states) in Data Sections — only actual user input fields.
