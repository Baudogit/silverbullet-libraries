   _Updated: 2026-03-01_
   _Edition pdf:_ [[Library/baudogit/TCreference.pdf]]
   
# Task Commander — Syntax Reference

## Overview

Task Commander provides 6 input modes, each with its own syntax.

**IMPORTANT**
==**Right-click** or focus **on the command bar** and choose a mode. Then use **`/` to open the attribute popup** in any mode, **`//` for itags** (Filter and Query only) and **`///` for ref of task** (Filter only).==

See [[#Common Features]] for more details.
    
---

## Mode Summary

| Mode | Action | Trigger | Scope |
|------|--------|---------|-------|
| 1. **Filter** | Real-time text filtering | As you type | Visible tasks |
| 2. **Sort** | Reorder tasks by attributes | Enter | Visible tasks without filter|
| 3. **Calc** | Compute and write new attributes | Enter + confirm | Visible tasks |
| 4. **Update** | Add, delete or rename attributes | Enter + confirm | Visible tasks |
| 5. **Stats** | Edit and export statistics | Enter + confirm | Visible tasks |
| 6. **Query** | Custom `where` clause on indexed tasks | Enter | All tasks |

### ❗Scenarios

- `Query` + `Filter` to target tasks
- `Query` + `Filter` + `[Calc or Update]` to apply operations on target tasks
- `Sort` by attributes (with default option) then `Filter` precisely

### Workflow

- `Clear button` on the command bar to reset `Filter` or `Sort` mode.
- `Sort` cancels the current Filter. Then, you can `Filter` again.
- If a `default where clause` is configured, it is executed when open or reset.
  
---

## 1. Filter mode

Filters visible tasks in real time (no Enter needed).

### ❗Syntax

| Pattern | Meaning | Example |
|---------|---------|---------|
| 1. `text` | Contains "text" (case-insensitive) | `urgent` |
| 2. `term1 term2` | One of the terms (OR) | `issue new ` |
| 3. `term1+term2` | Both terms | `bug+urgent` |
| 4. `-term` | Must NOT contain term | `-done` |
| 5. `"..."` | The expression exactly | `"Lorem ipsum ? dol"` |
| 6. `[attr:]` | Attribute's name equals name | `[priority:]` |
| 7. `[attr: value]` | Attribute’s equals value or part | `[priority: hi]` |
| 8. `#xyz` | Task’s itags contains this tag | `#meta/library` |
| 9. `{#xyz #abc}` | Task’s itags contains these tags | `{#meta/library #PROJET1}` |
| 10. `[[ ]]` | Task’s ref (example: collected it in a modal)| `[[TECH/observer_tests@847]]` |
| 11. Combined | Mix freely | `{#meta/library}+"norm" [attrib0:]` |

**Recognized groups:** `“ “` (string), `[ :]` (attribute’s name ; value is facultative), `{#, #}` (itags), `[[ ]]` (ref)
**Popup operators:** `space` (OR), `+` (AND), `-` (exclude)

---

## 2. Sort mode

Reorders displayed tasks by one or more attributes. Press **Enter** to apply.

### ❗Syntax

| Pattern | Meaning | Example |
|---------|---------|---------|
| `attr+` | Sort ascending by attr | `priority+` |
| `attr-` | Sort descending by attr | `date-` |
| `attr1+ attr2-` | Multi-level sort | `status+ priority-` |
| `attr+ !` | Sort but keep ALL tasks (no filter) | `date+ !` |

Dates are auto-detected (ISO, EU, US formats, with or without `" "`). Numeric values sort numerically.

**Trigger:** `!` tasks missing the sort attribute are shown. By default they are hidden.
**Popup operators:** `+` (ascending), `-` (descending)

---

## 3. Calc mode

For each visible task, computes a value from existing attributes and writes the result as a new attribute. Press **Enter** to preview, then confirm in the modal.

### ❗Syntax

```
term1 [+|-] term2 [+|-] term3 ... -> resultAttr
```

Each **term** is either an attribute name or a literal number.

| Expression | Example | Result |
|------------|---------|--------|
| 1. `date2 - date1 -> attr` | `end-start -> duration (+-) ` | Number of days between dates |
| 2. `date1 + number -> attr` | `deadline + 30 -> extended` | Date shifted forward |
| 3. `date1 - number -> attr` | `deadline - 7 -> reminder` | Date shifted backward |
| 4. `num1 + num2 -> attr` | `price + tax -> total` | Sum (integer) |
| 5. `num1 - num2 -> attr` | `budget-spent -> remaining` | Difference (integer) |
| 6. `num1 + literal -> attr` | `price + 30 -> majoration` | 30 is a literal number |
| 7. Chained | `a + b - c + 10 -> result` | Evaluated left to right |

### Combinations

| val1 type | operator | val2 type | → result type |
|-----------|----------|-----------|---------------|
| 1. attrib_date | − | attrib_date | number (days) |
| 2. attrib_date | + | number | date |
| 3. attrib_date | − | number | date |
| 4. attrib_number | + | attrib_number | number |
| 5. attrib_number | − | attrib_number | number |
| 6. attrib_date | + | attrib_date | ❌ invalid combination |

### Rules

- A task is **skipped** if any referenced attribute is missing
- A task is **skipped** if the result’s name already exists
- A task is **skipped** if the types are incompatible (e.g. date + date)
- Chained operations are evaluated **left to right**, the intermediate result type carries through
- **Dates** can be in ISO (`2026-02-10`, `2026-02-10T14:41:57`), french (`21/02/2026`, `21-02-2026`, `21.02.2026`), US (`02/21/2026`), and timestamp formats, with or without quotation marks. Time is always ignored.

**Popup operators:** `+` (add), `-` (subtract), `->` (assign to result)

---

## 4. Update mode

Adds, deletes or renames attributes on all visible tasks. Press **Enter** to preview, then confirm.

### ❗Syntax

| Pattern | Meaning | Example |
|---------|---------|---------|
| `add:name` | Add attribute with placeholder `..` | `add:priority` |
| `add:name=value` | Add attribute with value | `add:priority=high` |
| `delete:name` | Remove attribute | `delete:obsolete` |
| `rename:old->new` | Rename attribute | `rename:prio->priority` |

Only one operation per command. Tasks where the attribute already exists (for add) or doesn't exist (for delete/rename) are silently skipped.

**Popup operators:** `add:`, `delete:`, `rename:`

---

## 5. Statistics mode

Adds values of one or more attributes on all visible tasks. Press **Enter** to view the result. Then, print or export the list or cancel.

### ❗Syntax

| Pattern | Meaning | Example |
|---------|---------|---------|
| `name [name]` | Add values of each attribute in the list | `duration` |

**No popup operators**

---
## 6. Query mode

Executes a `where` clause against the SilverBullet task index with Lua Integrated Query (LIQ). Press **Enter** to query.

### ❗Syntax

Standard `where` expressions:

| Pattern | Meaning | Example |
|---------|---------|---------|
| `_.attr == 'value'` | Attribute equals value | `_.priority == 'high'` |
| `_.attr > value` | Comparison | `_.score > 80` |
| `expr and expr` | Both conditions | `_.status = 'open' and _.priority = 'high'` |
| `expr or expr` | Either condition | `_.tag = 'bug' or _.tag = 'feature'` |
| `not expr` | Negation | `not _.done` |

In this mode, submenus provided examples of **complex Where clauses**, a **Where clause history** and a list of **standard Properties** of task.

**Popup operators:** `and`, `or`, `not`

---

## Common Features

| Feature | How | Available in |
|---------|-----|--------------|
| 1. Attribute popup | Type `/` | All modes |
| 2. Itag popup | Type `//` | Filter, Query |
| 3. Task’ref format | Type `///` | Filter |
| 4. Right-click menu | Right-click on input | All modes |
| 5. Clear input | Click `X` on command line or press Esc twice | All modes |
| 6. Clear mode | Click `X` on command line | Filter, Sort |
| 7. Status bar | Click info button `ⓘ` next to the number of tasks | All modes |
| 8. Instructions | Click bulb icon `💡` in mode bar | All modes |
