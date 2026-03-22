# PRD — `zs-content-forms-mental-health`

> **Document Class:** PRD | **Version:** 1.0.0 | **Status:** Bootstrapping
> **Repository:** [https://github.com/zarishsphere/zs-content-forms-mental-health](https://github.com/zarishsphere/zs-content-forms-mental-health)
> **Layer:** Layer 6A — Clinical Content | **Catalog #:** 144
> **License:** Apache 2.0 | **Governance:** RFC-0001

---

## 1. Overview

Mental health forms — PHQ-9, GAD-7, MHPSS intake, psychosocial assessment, AUDIT.

---

## 2. Repository Metadata

- **Name:** `zs-content-forms-mental-health`
- **Organization:** [https://github.com/zarishsphere](https://github.com/zarishsphere)
- **Language / Runtime:** JSON Schema 2020-12 / Markdown
- **Visibility:** Public
- **License:** Apache 2.0
- **Default Branch:** `main`
- **Branch Protection:** Required (2-owner review for critical paths)

---

## 3. Platform Context

This repository is part of the **ZarishSphere** sovereign digital health operating platform — a free, open-source, FHIR R5-native system for South and Southeast Asia.

**Non-negotiable constraints:**
- Zero cost — all tooling must use genuinely free tiers
- FHIR R5 native — all clinical data modelled as FHIR R5 resources
- Offline-first — must work without network connectivity
- No-coder friendly — GUI-first, template-driven
- Documentation as Code — all decisions in GitHub

---

## 4. Goals & Objectives

- Provide validated JSON Schema clinical forms for mental-health workflows
- Every field must have a FHIR R5 mapping (fhirPath)
- Coded fields must carry LOINC, SNOMED CT, or ICD-11 codes
- Every label must have translations in EN, BN, MY, UR, HI, TH

## 5. Functional Requirements

| ID | Requirement | Priority |
|----|------------|---------|
| F-01 | All forms validate against ZS Form Schema v1 | P0 |
| F-02 | Every clinical field has fhirPath mapping | P0 |
| F-03 | Coded fields include LOINC/SNOMED/ICD-11 code | P0 |
| F-04 | i18n keys exist in zs-data-translations | P0 |
| F-05 | Automated validation CI on every PR | P0 |
| F-06 | Form versioning (semver: major for breaking field changes) | P1 |
| F-07 | Form preview in zs-ui-form-builder | P2 |

## 6. Repository Tree

```
zs-content-forms-mental-health/
├── README.md
├── LICENSE
├── .gitignore
├── .github/
│   ├── CODEOWNERS
│   └── workflows/
│       └── validate.yml               # JSON Schema validation + FHIR mapping check
├── schemas/
│   └── zs-form-schema-v1.json         # ZS Form Schema v1 meta-schema
├── forms/
│   └── mental-health/
│       ├── (form JSON files per workflow)
│       └── versions/
│           └── (historical versions)
├── translations/
│   ├── en.json                        # English labels
│   ├── bn.json                        # Bengali labels
│   └── my.json                        # Burmese labels
├── tests/
│   └── validate_forms.py              # Python JSON Schema validator
└── docs/
    ├── FORM-INDEX.md                  # All forms with version + status
    └── ADDING-FORMS.md                # How to add a new form
```

## 7. Form Schema Structure

```json
{
  "$schema": "https://zarishsphere.com/schema/form/v1",
  "id": "zs-form-mental-health-01",
  "title": "Mental Health Form",
  "version": "1.0.0",
  "fhirResource": "Observation",
  "sections": [{
    "id": "section-1",
    "title": "{{i18n:forms.mental-health.section1}}",
    "fields": [{
      "id": "field-1",
      "type": "number",
      "label": "{{i18n:forms.mental-health.field1}}",
      "fhirPath": "Observation.valueQuantity.value",
      "loincCode": "12345-6",
      "required": true
    }]
  }]
}
```

## 9. Ownership & Governance

| Role | GitHub User |
|------|-------------|
| Platform Lead | `@arwa-zarish` |
| Technical Lead | `@code-and-brain` |
| DevOps Lead | `@DevOps-Ariful-Islam` |
| Health Programs | `@BGD-Health-Program` |

All changes go through Pull Request → 1+ owner review → CI pass → merge.
Breaking changes require RFC + ADR.

---

## 10. Definition of Done

- [ ] All listed files exist with content
- [ ] CI pipeline passes (test + lint + security)
- [ ] README.md reflects current state
- [ ] OpenAPI / AsyncAPI spec present (services only)
- [ ] At least 1 integration test using testcontainers-go (Go) or Playwright (UI)
- [ ] No secrets committed (GitGuardian verified)
- [ ] CODEOWNERS file present
- [ ] Linked to CATALOGS.md and TODO.md

---

*This PRD is the canonical source of truth for this repository's purpose, structure, and requirements.*
*Changes require a PR against this file with owner approval.*
