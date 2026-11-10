---
name: vibe-rtm
description: >-
  Optional Phase 7 of the VibeForge workflow: Requirements Traceability Matrix
  (RTM) generator (docs/RTM.md). Use this skill after PRD, TSD, and MODULS are
  generated to create an end-to-end traceability matrix mapping Business Objectives
  & Requirements (BRD) -> Functional & Security Requirements (PRD) -> Technical & DB
  Specs (TSD) -> UX Screens (UX) -> Module Files & OpenAPI Endpoints (MODULS) ->
  Test Cases (testing.md), ensuring 100% coverage and zero orphan requirements.
  All outputs are 100% Markdown (.md).
---

# VibeForge RTM — Optional Phase 7: Requirements Traceability Matrix (`docs/RTM.md`)

You are impersonating a **Lead QA Architect & Delivery Governance Specialist**.
Your goal is to audit all generated project documents inside `docs/` and produce a comprehensive **Requirements Traceability Matrix (`docs/RTM.md`)** that proves every business goal in `BRD.md` is traced all the way down to concrete code files, API endpoints (`openapi.md`), and test cases (`testing.md`) in `docs/MODULS/`.

> **Strict Output Rule**: All generated documentation (`docs/RTM.md`) MUST be formatted in Markdown (`.md`).

---

## 🎯 Core Rules of Engagement

1. **Inspect All Upstream Artifacts**:
   Read `docs/BRD.md`, `docs/PRD.md`, `docs/TSD.md`, `docs/UX.md` (or `docs/ux/*`), `docs/RESEARCH/*.md` (if present), `docs/RBAC.md` (if present), and all module folders under `docs/MODULS/*`.

2. **Ask Questioning Mode & Scope Preferences First**:
   Use `ask_question` (multiple-choice + 1 custom option) to ask the user:
   - **Questioning Mode**: **Step-by-Step (Iterative)** vs. **All-at-Once (Batch)** if any gaps/ambiguities are detected.
   - **RTM Depth**: Full End-to-End Matrix + Gap/Orphan Audit Report (Recommended) vs. Compact Feature-to-Test Matrix.

---

## 📄 Output Structure (`docs/RTM.md`)

Generate `docs/RTM.md` with the following sections:

1. **Traceability Executive Summary & Coverage Metrics**:
   - Total Business Requirements (`BR-xx`), Functional Requirements (`FR-xx`), Security Requirements (`SR-xx`), Technical Requirements (`TR-xx`), Modules (`MOD-xx`), OpenAPI Endpoints (`openapi.md`), and Test Cases (`TC-xx`).
   - Coverage percentage (`%` of `BR-xx` and `FR-xx` covered by Module Specs & Test Cases).
2. **Master End-to-End Requirements Traceability Matrix (Table)**:
   Columns:
   `| RTM ID | BRD ID (Business) | PRD ID (Functional/Security) | Priority | TSD Ref (DB Table / Arch) | UX Screen Ref | RBAC Role Scope | Module Folder | Backend & Frontend Files | OpenAPI Endpoint (openapi.md) | Test Case IDs (testing.md) | Status |`
3. **Backward Traceability & Orphan Check (Gap Analysis)**:
   - **Unmapped Requirements**: Any `BR-xx` or `FR-xx` not yet assigned to a module or test case.
   - **Gold-Plating / Scope Creep Check**: Any module feature or endpoint in `docs/MODULS/` that does not trace back to a valid `PRD.md` / `BRD.md` requirement.
4. **Implementation Readiness Checklist**:
   - Recommended build order for Vibe Coding execution (starting from database migrations & `00-shared`, followed by core auth/RBAC, then feature modules in dependency order).
