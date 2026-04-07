# Form Discovery

How the agent discovers and maps form fields on any application page.

---

## Process

### 1. Snapshot

```
browser_snapshot → accessibility tree of current page
```

The snapshot returns the page's accessible elements: labels, inputs, buttons, selectors.

### 2. Extract Fields

From the snapshot, identify all interactive form elements:

| Element Type | What to Capture |
|-------------|-----------------|
| Text input | Label, placeholder, name attribute, required status |
| Textarea | Label, placeholder, max length |
| Select/dropdown | Label, options list |
| Checkbox | Label, checked state |
| Radio group | Group label, option labels |
| File upload | Label, accepted file types |
| Hidden fields | Skip — do not fill |

### 3. Classify

Map each field to a profile category using label matching:

```
Field label → normalize (lowercase, strip punctuation)
           → match against profile-schema.md field mapping table
           → if match: tag as "mapped"
           → if no match: tag as "unmapped" + classify type
```

Unmapped field types:

| Type | Handling |
|------|----------|
| Yes/No question | Check `question_bank`, else flag HITL |
| Freeform text | Check `question_bank`, else → `references/question-handling.md` |
| Dropdown with options | Attempt best-match from profile, else flag HITL |
| Unknown | Flag HITL |

### 4. Build Field Map

Output a structured field map:

```json
{
  "fields": [
    {
      "selector": "input#first-name",
      "label": "First Name",
      "type": "text",
      "required": true,
      "profile_key": "personal.first_name",
      "status": "mapped"
    },
    {
      "selector": "textarea#cover-letter",
      "label": "Why are you interested in this role?",
      "type": "textarea",
      "required": false,
      "profile_key": null,
      "status": "unmapped",
      "handling": "question-handling"
    }
  ]
}
```

## Multi-Page Detection

After filling a page, check for:
- Buttons with text: "Next", "Continue", "Next Step", "Save and Continue"
- Progress indicators (step 1 of 3, progress bar)
- URL changes after form section submission

If detected: click to advance, run discovery again on the new page.

## Anti-Patterns

- Do NOT fill hidden fields
- Do NOT click "Submit" during discovery — only "Next" / "Continue"
- Do NOT guess values for unmapped required fields — flag HITL
- Do NOT skip fields silently — every field appears in the HITL review
