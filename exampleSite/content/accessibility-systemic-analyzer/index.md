---
title: "Accessibility Systemic Analyzer"
summary: "Project overview covering the architecture, supported tools, workflow, workbook, and dashboard notes."
description: "A refreshed project overview for the Accessibility Systemic Analyzer, covering the current architecture, supported adapters, WCAG handling, systemic analysis workflow, workbook model, and dashboard support."
categories: ["Accessibility", "Testing"]
tags: ["accessibility", "testing", "analysis", "wcag", "dashboard"]
date: 2026-05-28
draft: false
showauthor: false
showTableOfContents: true
showReadingTime: true
---

The Accessibility Systemic Analyzer is a multi-tool analysis and reporting layer for accessibility scan output. It is designed for the point where a team has more findings than it can triage comfortably page by page, and needs a way to understand what is repeated, what is systemic, what is corroborated across tools, and which fixes are most likely to remove broad classes of issues across a site or estate.

Instead of treating accessibility results only as isolated page-level defects, the analyzer normalizes data from multiple engines, deduplicates likely overlaps, maps findings to WCAG where possible, groups repeated issue patterns, and produces both a dashboard and workbook that are easier to use for remediation planning.

> **Current state:** the analyzer now supports a wider adapter set, richer WCAG handling, friendlier rule labels, stronger workbook and dashboard alignment, static dashboard builds, and drilldown search / pagination.

---

## Why this exists

Most accessibility tools report results per page and per node. That is useful for debugging, but hard to use for planning. A single underlying component problem can create hundreds of repeated findings across many pages.

| Page         | Finding               |
|--------------|-----------------------|
| Home         | Button contrast issue |
| Product page | Button contrast issue |
| Cart         | Button contrast issue |
| Checkout     | Button contrast issue |

Traditional reports treat these as separate rows.

> The underlying issue may really be “the shared button implementation has a contrast problem”.

The analyzer exists to help move from raw issue volume to reusable remediation targets.

---

## What the analyzer does

- Detects and loads supported report formats.
- Normalizes source-specific findings into a shared row model.
- Maps rules to WCAG criteria, levels, and titles where possible.
- Normalizes page keys, selectors, severity, rule ids, and display labels.
- Infers likely component and component-group ownership.
- Deduplicates likely matching findings across tools on the same page.
- Builds clusters and pattern rollups.
- Computes metrics for overlap, confidence, systemicity, page spread, and prioritization.
- Exports analyst-friendly spreadsheets and static dashboard assets.

---

## Current supported tool families and adapters

| Tool / Source             | Current notes                                                                                                    |
|---------------------------|---------------------------------------------|
| axe-core                  | Supports confirmed violations and incomplete/manual-review style findings, including WCAG extraction from tags.  |
| axe-scan                  | Handles axe-derived report output and incomplete/manual-review style contrast findings.                          |
| IBM Accessibility Checker | Handles both `violation` and `potentialviolation` output and maps IBM rule ids to WCAG where possible.           |
| Lighthouse                | Reads failed accessibility audits and extracts WCAG tags from audit debug metadata.                              |
| Oobee                     | Crawler-style source with broader estate/page coverage.                                                          |
| UUV                       | Flow/run-based source with hardened metadata handling, severity normalization, and conformance metadata support. |
| Pa11y                     | Now treated as a single `pa11y` source with per-issue runner detection for mixed axe + HTMLCS runs.              |
| HTMLCS / html-sniffer     | Supported through HTMLCS-aware parsing and WCAG technique-style label handling.                                  |

Some adapters preserve “needs review” style outputs rather than only confirmed failures. This is intentional.

---

## Current WCAG handling

- rule-to-WCAG mapping tables
- message / heuristic fallback mapping
- WCAG title and level enrichment
- cleanup of WCAG-only charts and exports so that only real criterion codes appear in WCAG-specific views
- friendlier rule labels so bare WCAG codes are not reused unnecessarily as human rule names

In practice, the analyzer tries to separate:

- **Rule**: a human-readable issue label
- **Rule Id**: source-specific technical identifier
- **WCAG**: criterion code such as `1.4.3`
- **WCAG Name**: criterion title such as `Contrast (Minimum)`

---

## Important recent model changes

### Pa11y mixed-run support

Pa11y is no longer treated as two fake tools when axe and HTMLCS findings come from the same Pa11y run. The analyzer now models one `pa11y` source and keeps the per-issue `runner` such as `axe` or `htmlcs`.

### Friendlier rule labels

Rule formatting has been tightened so that the analyzer no longer prefers a bare WCAG code or raw HTMLCS identifier when a friendlier label can be derived from `rule_name`, `ruleId`, WCAG title metadata, or technique-aware enrichment such as `1.4.6 [G17]`.

### WCAG chart cleanup

WCAG-specific dashboard and export views now accept only real WCAG codes. This prevents rule ids, messages, or fallback text from leaking into WCAG-only charts.

### Needs-review preservation

The analyzer now more intentionally preserves incomplete/manual-review style findings from sources like axe-core, IBM, and HTMLCS-derived reports.

### Drilldown UX upgrade

The dashboard drilldown now supports search, page-size selection, and pagination.

---

## How the pipeline works

### 1. Adapters

Source-specific adapters detect report shapes and emit normalized rows with fields such as page/url, rule id, message, severity, selector/dom, source, and any tool-specific metadata worth keeping.

### 2. Report loading and enrichment

Rows are enriched with WCAG fields, page keys, rule display fields, and related metadata. This is where much of the rule/WCAG cleanup happens.

### 3. Processing

The processing layer infers component, component group, design-system or custom origin, ownership hints, issue scope, and ranking-related fields.

### 4. Deduplication

Likely matching findings on the same page are merged into deduplicated rows. This is where tool overlap becomes visible and where consensus / confidence become meaningful.

### 5. Clustering and metrics

Repeated issue families are rolled up into clusters and pattern-level summaries. Metrics such as overlap, confidence, page concentration, shared source rate, and fix prioritization are computed here.

### 6. Exports

The final output can be written to analyst-friendly workbook tabs, Power BI-friendly sheets, dimension/fact sheets, and static dashboard payload files.

---

## What “systemic” means in this project

A systemic issue is any finding pattern that appears repeated enough that fixing it once may help many pages or instances. In practice that can mean a design-system component issue, a repeated template issue, a repeated content structure issue, or a crawler/flow-discovered issue affecting many pages.

The analyzer uses several related but distinct concepts:

- **design_system_issue**: stronger hint of a reusable shared source
- **issue_scope**: shared component, shared template/pattern, or page-specific
- **systemic**: repeated enough to matter as a shared remediation target

---

## Dashboard and static site support

The project now supports a static-build path suitable for documentation-style hosting. The static site includes dashboard cards and charts, a WCAG support coverage summary table, page inventory validation, fix prioritization, drilldown search and pagination, and a downloadable workbook file.

---

## Workbook export model

The workbook supports both analyst review and BI workflows. It includes human summary tabs, issue-detail tabs, systemic cluster tabs, fix prioritization tabs, flat Power BI-friendly tables, and fact/dimension tabs for relational reporting.

---

## Interpreting overlap and confidence

The analyzer includes fields such as `sources`, `tool_count`, `tool_families`, `tool_family_count`, `consensus`, and `confidence`. These help answer whether a finding was seen by multiple engines and how much evidence sits behind the pattern.

---

## Interpretation guidance and limits

> **Warning:** **Automated coverage remains partial.** A wider tool stack improves breadth, overlap, and confidence, but it does not mean the project is capturing the full accessibility picture. Manual testing and human review are still necessary.

Examples of areas that often still need human judgement include alt-text quality, meaningful link text in context, logical focus order, error recovery, semantic clarity, and cognitive accessibility.

---

## Suggested workflow

1.  Run the scanners and collect report files.
2.  Build the normalized dashboard / workbook output.
3.  Check the page inventory panel first.
4.  Use the dashboard to identify top problem types, hot components, WCAG concentration, and fix-once opportunities.
5.  Use the workbook for filtered triage, handoff, BI, and reporting.
6.  Re-run after remediation to compare concentration, overlap, and systemic impact.

---

## Quick start

    python -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

    uvicorn app.main:app --reload

---

## Related guides

- **Dashboard Guide** explains the cards, panels, and drilldowns.
- **Workbook Guide** explains each spreadsheet tab and the main column families.
