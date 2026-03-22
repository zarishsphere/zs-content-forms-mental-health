# PRD-MVP — `zs-content-forms-mental-health`

> **Document:** Product Requirements (MVP) | **Version:** 1.0.0-mvp
> **Repository:** [https://github.com/zarishsphere/zs-content-forms-mental-health](https://github.com/zarishsphere/zs-content-forms-mental-health)
> **Layer:** Layer 6A — Clinical Content | **Catalog #:** 144
> **Language:** JSON Schema 2020-12 | **License:** Apache 2.0

---

## Executive Summary

**PHQ-9, GAD-7, MHPSS intake, psychosocial assessment.**

This document defines the **Minimum Viable Product (MVP)** scope for `zs-content-forms-mental-health` within the ZarishSphere sovereign digital health platform. It covers what must be built first, acceptance criteria, user stories, and the complete repository file structure.


### Platform Non-Negotiables (apply to every repository)

| Constraint | Rule |
|-----------|------|
| **Zero Cost** | All tooling, hosting, and services must use genuinely free tiers |
| **Open Source** | Apache 2.0 license; all code public |
| **FHIR R5 Native** | All clinical data modelled as FHIR R5 resources |
| **Offline-First** | Must function without network connectivity |
| **No-Coder Friendly** | GUI-first, template-driven, automatable |
| **Documentation as Code** | All decisions in GitHub via RFC/ADR |
| **Multi-tenant** | tenant_id scoping on all data operations |
| **HIPAA/GDPR** | AuditEvent on all PHI access; field-level encryption |

---

## Problem Statement

Clinical forms without machine-readable FHIR mappings cannot be automatically processed or validated. Without coded fields, data cannot feed indicators or be shared across systems.

## MVP Goals

1. Every form validates against ZS Form Schema v1
2. Every clinical field has a fhirPath mapping
3. Coded fields carry LOINC / SNOMED / ICD-11 codes
4. EN and BN translations for all labels
5. CI validates every form on PR

## MVP Functional Requirements

| ID | Requirement | Acceptance Criteria | Priority |
|----|------------|---------------------|---------|
| M-01 | All forms validate against ZS Form Schema v1 | `python validate_forms.py` exits 0 | P0 |
| M-02 | Every field has fhirPath | Validator script catches missing mappings | P0 |
| M-03 | Coded fields have LOINC/SNOMED/ICD-11 code | Validator checks code presence | P0 |
| M-04 | EN + BN translations complete | All i18n keys present in both files | P1 |
| M-05 | Form renders in zs-ui-form-builder | Form loads and submits without error | P1 |

## Forms in this Repository

phq-9.json
gad-7.json
mhpss-intake.json
psychosocial-assessment.json

## MVP Complete Repository Tree

```
zs-content-forms-mental-health/
├── README.md
├── LICENSE
├── .gitignore
├── CHANGELOG.md
├── .github/
│   ├── CODEOWNERS
│   └── workflows/
│       └── validate.yml                   # JSON Schema validation CI
├── schemas/
│   └── zs-form-schema-v1.json             # ZS meta-schema
├── forms/
│   └── mental-health/
│   ├── phq-9.json
│   ├── gad-7.json
│   ├── mhpss-intake.json
│   ├── psychosocial-assessment.json
│       └── versions/
│           └── (historical form versions)
├── translations/
│   ├── en.json                            # English labels
│   ├── bn.json                            # Bengali labels
│   └── my.json                            # Burmese labels
├── tests/
│   └── validate_forms.py                  # Python Schema validator
└── docs/
    ├── FORM-INDEX.md
    └── ADDING-FORMS.md
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
