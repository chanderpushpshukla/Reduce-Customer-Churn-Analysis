<!-- .github/copilot-instructions.md -->
# Copilot instructions — Reduce-Customer-Churn-Analysis

Purpose: Quickly bring an AI coding/documentation agent up to speed so contributions stay aligned with this project's scope and conventions.

- **Big picture:** This repository is an analysis project that uses **Excel, Power Query and Power BI only** to perform descriptive and diagnostic churn analysis. See `idea.md` and `reduce_customer_churn_end_to_end_analytics_project_excel_power_bi.md` for the project objective, scope, and phased workflow. Do NOT introduce machine-learning pipelines, servers, or CI workflows.

- **Primary artifacts to edit or produce:** markdown docs, Excel transformation recipes (Power Query steps), Power BI report files (.pbix) and exported visuals (PNG/PDF). Keep content business-focused and non-technical in tone.

- **Scope constraints (must follow):**
  - No ML or predictive modeling in the repo.
  - Avoid complex DAX; prefer Power Query or simple calculated columns described in plain language.
  - Work is deliverable-driven: cleaned dataset, Power BI dashboard, insight summary, and recommendations.

- **Where to look first:**
  - `idea.md` — short project idea and high-level methodology.
  - `reduce_customer_churn_end_to_end_analytics_project_excel_power_bi.md` — detailed phases, EDA, cleaning and dashboard guidance.
  - `README.md` — repo entry point (currently minimal).

- **Edit/commit patterns for agents:**
  - When adding step-by-step procedures (e.g., Power Query transformations), add a new subsection under "Phase 3: Data Cleaning" in `reduce_customer_churn_end_to_end_analytics_project_excel_power_bi.md` with numbered steps and sample M snippets if requested.
  - When proposing visuals or mockups, include a brief rationale (business question being answered) and save assets in a new `artifacts/` directory (create it) with a descriptive filename.
  - When adding sample Excel templates, include one-sheet examples and document the cell locations used for key calculations.

- **Examples of acceptable edits:**
  - Expand the EDA section with a step list: "Create pivot table: rows=tenure bucket, cols=month, values=churn_count" and add expected pivot layout.
  - Add a Power Query recipe: list of applied steps (remove duplicates → standardize dates → fill missing values) either as plain steps or as a minimal M script block.

- **What not to add:**
  - New backend services, Dockerfiles, CI/CD configs, or test harnesses. There are no build/test commands discovered in this repo.
  - Heavy technical refactors or introducing new language runtimes — keep changes to documentation and dataset/report artifacts.

- **Communication style:**
  - Use clear business language first (what the stakeholder needs), then a short technical section (how to implement in Excel/Power Query/Power BI).
  - Keep suggestions concrete and reproducible: include explicit steps, example cell references or Power Query step names.

- **If asked to generate code or scripts:**
  - Prefer small, self-contained Power Query M snippets or Excel formulas. Only produce other code when the user explicitly requests it and acknowledges it changes project scope.

- **When adding files, follow these naming rules:**
  - Put report files or exported visuals in `artifacts/` with names like `artifacts/churn_dashboard_v1.pbix` or `artifacts/churn_kpi_card.png`.
  - Document new artifacts in `README.md` with a one-line description.

If anything above is unclear or you want additional examples (Power Query steps, sample pivot layouts, or a recommended `artifacts/` structure), tell me which section to expand.
