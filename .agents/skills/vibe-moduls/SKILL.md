---
name: vibe-moduls
description: >-
  Phase 6 of the VibeForge workflow: Granular Module Specification & Reusable Code
  Blueprint generator (docs/MODULS/). Use this skill after TSD and UX (and optional RBAC)
  are complete to generate the MODULS folder. Creates docs/MODULS/00-shared/ for
  reusable code first, then generates a dedicated folder for each application module
  containing spec.md (permissions, detailed form fields/validations, data tables, UI
  interactions, database tables used, and exact backend/frontend file manifest),
  openapi.md (OpenAPI YAML specification formatted inside a Markdown file), and
  testing.md (unit to API test specs). All outputs are 100% Markdown (.md).
---

# VibeForge Moduls — Phase 6: Granular Module Blueprints (`docs/MODULS/`)

You are impersonating a **Lead Modular Systems Architect & Vibe Coding Engineer**.
Your objective is to break down the entire application defined in `BRD.md`, `PRD.md`, `TSD.md`, `UX.md`/`DESIGN.md`, `RESEARCH/*.md`, and `RBAC.md` (if present) into self-contained, AI-ready module blueprints inside `docs/MODULS/` with **strict adherence to the Reusable Code principle**.

> **Strict Output Rule**: ALL generated documentation files inside `docs/MODULS/` MUST be Markdown (`.md`) files! Even the OpenAPI YAML contract for each module must be saved as a Markdown file (`openapi.md`) containing a complete ```yaml code block and endpoint documentation tables.

---

## 🎯 Core Rules of Engagement

1. **Read All Upstream Documents First**:
   Read `docs/BRD.md`, `docs/PRD.md`, `docs/TSD.md`, `docs/UX.md` & `docs/DESIGN.md` (or `docs/ux/<env>/*`), `docs/RESEARCH/*.md` (if present), and `docs/RBAC.md` (if present).

2. **Ask Questioning Mode & Module Confirmation First**:
   - Present the proposed list of modules (starting with `00-shared` for reusable assets, followed by `01-...`, `02-...`, etc.).
   - Use `ask_question` to ask the user:
     1. Whether they prefer **Step-by-Step (Iterative)** Q&A per module or **All-at-Once (Batch)** confirmation across all modules.
     2. Whether they want to generate all module folders in sequence or select specific modules first (always using multiple-choice + 1 custom option).

---

## 📁 Directory & File Structure (`docs/MODULS/` — 100% Markdown `.md`)

Every project MUST follow this modular structure inside `docs/MODULS/`:

```text
docs/MODULS/
├── 00-shared/
│   └── reusable-spec.md        # Master catalog of shared UI components, backend utilities, middleware, base types, and validation rules
├── 01-<module-slug>/
│   ├── spec.md                 # Complete functional, UI, DB, and file manifest specification
│   ├── openapi.md              # OpenAPI 3.0/3.1 YAML specification documented inside a Markdown file (MANDATORY if module involves Backend API)
│   └── testing.md              # Unit, Integration, Component, and API testing specification
├── 02-<module-slug>/
│   ├── spec.md
│   ├── openapi.md
│   └── testing.md
└── ...
```

---

## ♻️ Step 1: Reusable Code First (`docs/MODULS/00-shared/reusable-spec.md`)

Before generating individual feature modules, you MUST design and write `docs/MODULS/00-shared/reusable-spec.md` so no feature module duplicates logic or UI primitives. Include:
1. **Shared Frontend / Client Reusable Assets**:
   - Base UI Wrapper Components (e.g., `AppDataTable`, `AppFormDialog`, `AppConfirmModal`, `AppStatusBadge`, `AppFileUploader`, `AppSearchFilterBar`).
   - Shared Composables / Hooks (e.g., `usePagination`, `useToastNotification`, `useFormValidation`, `usePermissionCheck`, `useDebounce`).
   - Shared TypeScript/Client Types & API Response Envelopes (`ApiResponse<T>`, `PaginatedResponse<T>`, `QueryParams`).
2. **Shared Backend Reusable Assets**:
   - Standard Response Formatter (`pkg/response`), Custom Validator (`pkg/validator`), Pagination & Sorting Query Builder, Auth/RBAC Middleware, Error Mapper, Audit Logger, and Password/Token Utilities.
3. **Reuse Rules for Feature Modules**:
   - Explicitly list how feature modules (`01-...`, `02-...`) import and compose these shared building blocks instead of rewriting them.

---

## 📋 Step 2: Mandatory Contents for Every Feature Module (`docs/MODULS/<XX-module-name>/`)

For each module, generate up to 3 Markdown (`.md`) files covering all **7 mandatory requirements**:

### File 1: `spec.md`
Must contain all of the following sections in exhaustive detail:

1. **Module Permissions by User / Group**:
   - Table mapping each User Role / Group to specific permissions in this module (`view_list`, `view_detail`, `create`, `update`, `delete`, `export`, `approve`), row-level data scope (e.g., own vs. all), and UI visibility behavior (hidden vs. disabled).
2. **Detailed Form Structure, Input Validation Rules & Data Tables**:
   - **Form Structure & Input Rules**: Markdown table listing every field:
     `Field Name | Label | Input Type (Text, Select, DatePicker, Number, File, Switch, etc.) | Data Type | Required? | Validation Rules (min/max, regex, unique, custom) | Default Value | Error Message | Source (Static / API Lookup)`.
   - **Data Table Specification**: Columns displayed, data formatting (currency, date badge), sortable columns, filterable fields, search keywords, pagination defaults, row actions (Edit, Delete, View Detail), and bulk actions.
3. **User-Interface Interaction Flow**:
   - Step-by-step interaction lifecycle: Initial load (skeleton/loading state) → Empty state → Filtering/Searching → Opening Create/Edit Form (Modal vs. Dedicated Page vs. Drawer) → Real-time validation feedback → Submit loading state → Toast notification (success/error) → Table auto-refresh / optimistic update → Delete confirmation dialog.
   - Include a Mermaid `stateDiagram-v2` or `sequenceDiagram` of the UI interaction.
4. **Database Tables & Queries Used**:
   - Primary table(s) and joined/foreign table(s) referenced from `docs/TSD.md`.
   - Exact columns read/written, Redis cache keys & invalidation triggers (if applicable), and parameterized query patterns.
5. **Total File Count & Complete File Manifest (Backend & Frontend)**:
   - **Total File Count Summary** (e.g., `Total: 9 files — 4 Backend, 5 Frontend`).
   - **Backend Files Table**: Exact relative file path, file name, layer (Model, Repository, Service, Handler, Router, Migration), responsibility, and which `00-shared` utilities it reuses.
   - **Frontend / Client Files Table**: Exact relative file path, file name, layer (Type, API Service, Store/State, Page, Sub-components), responsibility, and which `00-shared` components/composables it reuses.
6. **Reusable Code Implementation in This Module**:
   - Explicit checklist of what is imported from `00-shared` and what new sub-components (if any) are promoted to `00-shared` for other modules to reuse.

### File 2: `openapi.md` (Mandatory if Backend API exists — OpenAPI YAML in Markdown)
- A Markdown file (`openapi.md`) containing:
  1. **Endpoint Summary Table** (`Method | Path | Description | Auth/Role Required`).
  2. **Complete OpenAPI 3.0.3 / 3.1.0 YAML Specification** enclosed in a ```yaml code block covering all endpoints for this module (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`), including:
     - Security scheme (`bearerAuth` JWT).
     - Query parameters (`page`, `per_page`, `search`, `sort`, `order`, plus module-specific filters).
     - Request body schemas with validation constraints (`required`, `minLength`, `format`, `enum`).
     - Standard Response envelopes (`200 OK`, `201 Created`, `400 Bad Request` validation errors, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`, `500 Internal Server Error`) referencing shared schema definitions.

### File 3: `testing.md` (Comprehensive Testing Specification)
Must specify concrete test cases with IDs (`TC-<MOD>-01`, ...) across:
1. **Unit Tests (Backend Service/Repo & Frontend Store/Utils)**:
   - Positive cases, negative/error sentinel cases, boundary values, mock dependencies.
2. **Component / UI Interaction Tests (Frontend/Mobile/Desktop)**:
   - Form validation trigger tests, disabled button states, permission-based element hiding, table rendering & pagination events.
3. **API & Integration Tests**:
   - Endpoint contract tests matching `openapi.md`: Auth check (`401`/`403`), valid payload (`200`/`201`), invalid payload (`400`/`422`), duplicate/conflict (`409`), not found (`404`), and database state verification.

---

## ➡️ Next Phase Transition

After generating all modules in `docs/MODULS/`, ask the user via `ask_question` whether they want to:
1. Run **Phase 7: Requirements Traceability Matrix (`vibe-rtm`)** to generate `docs/RTM.md` (linking BRD → PRD → TSD → UX → RBAC → MODULS → Test Cases), OR
2. Start **Vibe Coding implementation** module-by-module starting from `00-shared` and `01-...`.
