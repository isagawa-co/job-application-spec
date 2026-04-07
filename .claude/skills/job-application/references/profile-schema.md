# Profile Schema

The user profile is a single JSON file that all applications pull from.

---

## Location

`profile.json` at project root.

## Schema

```json
{
  "personal": {
    "first_name": "",
    "last_name": "",
    "email": "",
    "phone": "",
    "headline": ""
  },
  "location": {
    "address": "",
    "city": "",
    "state": "",
    "zip": "",
    "country": ""
  },
  "experience": [
    {
      "company": "",
      "title": "",
      "start_date": "",
      "end_date": "",
      "current": false,
      "description": ""
    }
  ],
  "education": [
    {
      "school": "",
      "degree": "",
      "field": "",
      "graduation_year": ""
    }
  ],
  "links": {
    "linkedin": "",
    "github": "",
    "portfolio": "",
    "other": []
  },
  "resume_path": "",
  "cover_letter_path": "",
  "preferences": {
    "salary_min": null,
    "salary_max": null,
    "remote_preference": "",
    "start_date": ""
  },
  "authorization": {
    "work_authorized": true,
    "sponsorship_required": false,
    "visa_status": ""
  },
  "question_bank": {
    "why_interested": "",
    "strengths": "",
    "salary_expectations": "",
    "start_date": "",
    "custom": {}
  }
}
```

## Validation Rules

| Rule | Check |
|------|-------|
| Required fields | `personal.first_name`, `personal.last_name`, `personal.email` must be non-empty |
| Resume exists | `resume_path` must point to an existing file |
| Email format | Basic email validation |
| Phone format | Digits + optional formatting |

## Field Mapping

During form discovery, the agent maps form field labels to profile keys:

| Common Labels | Profile Key |
|---------------|-------------|
| First Name, Given Name | `personal.first_name` |
| Last Name, Surname, Family Name | `personal.last_name` |
| Email, Email Address | `personal.email` |
| Phone, Phone Number, Mobile | `personal.phone` |
| LinkedIn, LinkedIn URL | `links.linkedin` |
| Resume, CV, Upload Resume | `resume_path` |
| Cover Letter | `cover_letter_path` |
| Current Company, Employer | `experience[0].company` |
| Current Title, Job Title | `experience[0].title` |
| Salary, Compensation, Desired Salary | `preferences.salary_min` / `preferences.salary_max` |
| Authorized to work, Work Authorization | `authorization.work_authorized` |
| Sponsorship, Require Sponsorship | `authorization.sponsorship_required` |

This table is a starting point. The agent extends it via `/kernel/learn` as new label patterns are encountered.
