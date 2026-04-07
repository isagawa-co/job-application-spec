# Question Handling

Hybrid approach: template bank first, Claude Code generation as fallback.

---

## Priority Order

```
1. Exact match in profile.question_bank        → use stored answer
2. Fuzzy match in profile.question_bank.custom  → use closest match
3. No match                                     → Claude Code generates answer
```

## Template Bank

The `question_bank` in `profile.json` stores pre-written answers:

| Key | Common Questions |
|-----|-----------------|
| `why_interested` | "Why are you interested in this role/company?" |
| `strengths` | "What are your strengths?", "What makes you a good fit?" |
| `salary_expectations` | "What are your salary expectations?" |
| `start_date` | "When can you start?", "What is your availability?" |
| `custom` | Object of key-value pairs for domain-specific questions |

## Fuzzy Matching

Before generating, attempt fuzzy match:
1. Normalize question text (lowercase, strip punctuation)
2. Check if any `question_bank` key or `custom` key appears as substring
3. If match confidence is high, use stored answer
4. If ambiguous, flag for HITL with suggestion

## Claude Code Generation

When no template match exists:

1. Read the job posting title and description (from the page or URL)
2. Read the user's profile summary (`personal.headline` + `experience[0]`)
3. Generate a concise, professional answer that:
   - Connects the user's background to the role
   - Is specific to the job, not generic
   - Stays under any character/word limit shown on the form
4. Tag as `"source": "generated"` in the field map
5. All generated answers appear in HITL review — human always approves

## Anti-Patterns

- Do NOT generate answers for factual fields (salary, dates, authorization) — use profile data
- Do NOT submit generated answers without HITL review
- Do NOT use generic/template language ("I am passionate about...")
- Do NOT exceed field character limits
