# TECH-DESIGN-MVP — `zs-content-forms-mental-health`

> **Document:** Technical Design (MVP) | **Version:** 1.0.0-mvp
> **Repository:** [https://github.com/zarishsphere/zs-content-forms-mental-health](https://github.com/zarishsphere/zs-content-forms-mental-health)
> **Layer:** Layer 6A | **Catalog #:** 144
> **Language:** JSON Schema 2020-12 | **License:** Apache 2.0

---

## Technical Summary

**PHQ-9, GAD-7, MHPSS intake, psychosocial assessment.**

This document defines the **technical architecture, implementation design, complete repository tree, and acceptance criteria** for the MVP of `zs-content-forms-mental-health`.

---

## ZS Form Schema v1 — Minimal Valid Form

```json
{
  "$schema": "https://zarishsphere.com/schema/form/v1",
  "id": "zs-form-mental-health-01",
  "title": "{{i18n:forms.mental-health.title}}",
  "version": "1.0.0",
  "fhirResource": "Observation",
  "sections": [
    {
      "id": "section-main",
      "title": "{{i18n:forms.mental-health.section_main}}",
      "fields": [
        {
          "id": "field-001",
          "type": "number",
          "label": "{{i18n:forms.mental-health.field_001}}",
          "fhirPath": "Observation.valueQuantity.value",
          "loincCode": "55284-4",
          "required": true,
          "validation": {
            "min": 0,
            "max": 999
          }
        }
      ]
    }
  ]
}
```

## Validation Script

```python
# tests/validate_forms.py
import json, sys
from jsonschema import validate, ValidationError
from pathlib import Path

meta_schema = json.loads(Path("schemas/zs-form-schema-v1.json").read_text())
errors = []

for form_file in Path("forms").rglob("*.json"):
    if "versions" in str(form_file):
        continue
    form = json.loads(form_file.read_text())
    try:
        validate(instance=form, schema=meta_schema)
        # Check every field has fhirPath
        for section in form.get("sections", []):
            for field in section.get("fields", []):
                if "fhirPath" not in field:
                    errors.append(f"{form_file}: field {field['id']} missing fhirPath")
    except ValidationError as e:
        errors.append(f"{form_file}: {e.message}")

if errors:
    print("\n".join(errors))
    sys.exit(1)
print(f"All forms valid.")
```

## CI Validation Workflow

```yaml
name: Validate Forms
on: [push, pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: {python-version: '3.12'}
      - run: pip install jsonschema
      - run: python tests/validate_forms.py
```

---


## Owners & Governance

| Role | GitHub Handle | Responsibility |
|------|--------------|----------------|
| Platform Lead | `@arwa-zarish` | Final approval, RFC votes |
| Technical Lead | `@code-and-brain` | Architecture, Go/TS review |
| DevOps Lead | `@DevOps-Ariful-Islam` | CI/CD, infra, deployment |
| Health Programs | `@BGD-Health-Program` | Clinical content, country programs |

**PR Policy:** All changes via Pull Request. Minimum 1 owner review. CI must pass. No direct commits to `main`.


---

## MVP Acceptance Checklist

- [ ] All MVP files exist in repository with real content (not placeholders)
- [ ] CI pipeline passes on `main` branch
- [ ] No secrets, credentials, or PHI committed
- [ ] README.md reflects current state with setup instructions
- [ ] CODEOWNERS file present
- [ ] All MVP functional requirements verified manually or via automated tests
- [ ] Linked to `CATALOGS.md` and `TODO.md` in `zs-docs-platform`

---

*This document is the authoritative MVP specification for `zs-content-forms-mental-health`.*
*Changes require a Pull Request with at least 1 owner approval.*
