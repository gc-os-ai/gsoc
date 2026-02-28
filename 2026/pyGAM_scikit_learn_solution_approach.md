# README: Solution Approach for pyGAM Scikit-Learn Interface

## Context
This document proposes a practical implementation plan for **Idea #2: Scikit-Learn Interface** from `2026/pyGAM.md`.

The goal is to make pyGAM models behave like native scikit-learn estimators so they can be used reliably in workflows such as:
- `Pipeline`
- `GridSearchCV` / `RandomizedSearchCV`
- model cloning (`sklearn.base.clone`)
- standard estimator checks and metadata access

---

## Problem Summary
pyGAM currently provides strong modeling functionality, but compatibility gaps with modern scikit-learn APIs can reduce usability in broader ML workflows.

Typical issues in this category include:
- incomplete or inconsistent `get_params` / `set_params` behavior
- constructor arguments that are not fully clone-safe
- missing estimator tags / conventions expected by scikit-learn utilities
- inconsistent behavior when composed in pipelines and CV utilities

---

## Proposed Solution
Implement incremental API alignment with scikit-learn estimator contracts while preserving pyGAM’s current modeling behavior.

### 1) Estimator API Compliance Audit
- Review core pyGAM estimators against current scikit-learn requirements.
- Identify mismatches in constructor signatures, fit/predict method expectations, and parameter round-tripping.
- Define a compatibility checklist to drive implementation and tests.

### 2) Base Class and Parameter Handling Updates
- Refactor base estimator classes to ensure constructor arguments are explicit and serializable.
- Ensure `get_params(deep=True)` and `set_params(**params)` behave as expected.
- Confirm cloning works across estimator variants.

### 3) Pipeline/SearchCV Compatibility Hardening
- Validate behavior inside `Pipeline` and with `GridSearchCV`.
- Ensure hyperparameter exposure uses scikit-learn naming conventions.
- Verify no hidden state leaks across folds or cloned instances.

### 4) Unit and Integration Tests
- Add targeted tests for:
  - parameter introspection and mutation
  - cloning and reproducibility
  - fit/predict compatibility in a pipeline
  - search CV with small synthetic datasets
- Keep tests deterministic and lightweight.

### 5) Documentation and Examples
- Add concise usage examples for pyGAM + scikit-learn composition.
- Include known limitations (if any) and version notes.

---

## Execution Plan (12 Weeks)

### Phase 1 (Weeks 1-2): Discovery and Design
- Study existing pyGAM estimator architecture.
- Build API-compatibility checklist.
- Propose minimal refactor scope and acceptance criteria.

### Phase 2 (Weeks 3-6): Core Refactor
- Update estimator constructors and parameter handling.
- Implement API fixes in base classes and derived models.
- Add first set of compliance tests.

### Phase 3 (Weeks 7-9): Workflow Integration
- Validate with `Pipeline` and `GridSearchCV` examples.
- Add integration tests for common ML workflows.
- Resolve edge cases and regression risks.

### Phase 4 (Weeks 10-12): Stabilization and Docs
- Improve reliability, finalize tests, and clean APIs.
- Add/update developer and user docs.
- Prepare final benchmark and compatibility summary.

---

## Deliverables
1. Updated pyGAM estimator interfaces with scikit-learn-compatible behavior.
2. Comprehensive unit and integration test suite for API compatibility.
3. Example notebooks/scripts demonstrating pipeline and search workflows.
4. Documentation updates with migration/usage notes.

---

## Validation Strategy
- Run project test suite before/after changes to prevent regressions.
- Add compatibility tests that mimic real user workflows.
- Verify clone safety and deterministic behavior under CV.
- Compare behavior across supported Python and scikit-learn versions (as defined by pyGAM support policy).

---

## Risks and Mitigations
- **Risk:** API changes could impact existing user code.
  - **Mitigation:** keep backward compatibility where possible, document deprecations early.

- **Risk:** scikit-learn API evolution during development.
  - **Mitigation:** pin tested versions and track upstream compatibility notes.

- **Risk:** hidden estimator state causing CV leakage.
  - **Mitigation:** enforce clone-based tests and fold-isolation checks.

---

## First PR Scope (Beginner-Friendly)
A practical first pull request from this plan can be:
- add a compatibility checklist document
- add/adjust a small set of tests for `get_params` and `set_params`
- make one minimal estimator/base-class fix that enables a failing test to pass

This keeps the PR reviewable and provides immediate value while setting up larger improvements.

---

## Why This Is a Good GSoC Proposal Direction
- Strong user impact through ecosystem interoperability.
- Clear, testable milestones.
- Balanced scope for medium project length.
- High mentorship value through API design + engineering rigor.
