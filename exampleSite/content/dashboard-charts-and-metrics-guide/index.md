---
title: "Dashboard Charts and Metrics Guide"
summary: "Guide to the current dashboard cards, charts, panels, and drilldown behavior."
description: "A guide to the Accessibility Systemic Analyzer dashboard, covering top cards, WCAG support coverage, problem type charts, component heatmaps, drilldowns, and interpretation guidance."
categories: ["Accessibility", "Testing"]
tags: ["accessibility", "dashboard", "metrics", "wcag", "testing"]
date: 2026-05-28
draft: false
showauthor: false
showTableOfContents: true
showReadingTime: true
---

This guide explains the current dashboard cards, charts, wide panels, and drilldown behavior. It reflects the newer support table, stricter WCAG-only charting, friendlier rule labels, and drilldown search/paging.

> **Recent dashboard changes:** WCAG Support Coverage panel, WCAG chart cleanup, improved rule labels, better component label display, and drilldowns with search, page-size controls, and pagination.

---

## How to read the dashboard overall

1.  How much issue volume exists?
2.  Where is it concentrated?
3.  How much of it looks systemic or reusable?
4.  Which fixes could remove the most repeated issues first?

The top cards provide a snapshot. The chart panels show pattern shape. The wide panels validate the dataset and help turn findings into action. The drilldown lets you inspect the findings behind a chart bar or fix row.

---

## Top metric cards

- **Violations**: total processed findings.
- **Pages Affected**: count of unique normalized pages with at least one finding.
- **WCAG Criteria Affected**: count of distinct WCAG success criteria represented.
- **Shared Pattern Impact**: percentage of issue volume tied to shared patterns or reusable sources.
- **Opportunity Score**: estimated percentage of findings concentrated in the top remediation opportunities.
- **Accessibility Debt Index**: weighted burden score based on repetition, spread, and systemicity.
- **Cross-Tool Overlap**: counts of `verified`, `likely`, and `single` findings.
- **Evidence Confidence**: evidence strength summary.
- **WCAG Levels**: A / AA / AAA counts.
- **Legacy Frames**: frame/iframe-related counts and pages.
- **Shared Source Rate**: percentage tied to shared components, templates, or patterns.
- **Top 5 Page Concentration**: share of issue volume in the five most affected pages.

---

## Main chart and table panels

### Tool Agreement Profile

Shows how often each tool’s findings agree across families, overlap only within the same family, or remain unique. This is a corroboration view, not a truth meter.

### WCAG Support Coverage

A summary table of high-level WCAG support breadth by tool framework. Treat it as a capability overview rather than a guarantee that every configured run is exercising every supported rule.

### Top Problem Types

Rolls component findings into broad issue areas such as Forms, Interactive, Navigation, Content, Structure, Media, ARIA, and Other.

### Component Heatmap

Shows which inferred UI components generate the most findings. Labels are humanized for display, while drilldowns still use the raw component keys underneath.

### WCAG Rule Breakdown

Shows only valid WCAG criteria now. Non-WCAG rule ids and messages are intentionally excluded.

### Issues per Page

Highlights the most affected pages so you can spot concentration or template noise quickly.

### Page Inventory Check

Validates cross-tool page coverage before you trust the rest of the dashboard. This is one of the most important trust checks in the dashboard.

### Fix Once, Benefit Many

A ranked shortlist of repeated patterns most worth fixing first. It is the most action-oriented panel in the dashboard.

---

## Drilldown behavior

Clicking a chart bar or fix row opens the drilldown panel for the current subset.

- **Search** across the current drilldown result set
- **Page size** selection (10 / 25 / 50 / 100)
- **Pagination** for long result sets

Typical fields shown include rule label, page, component, issue scope, pattern, source, DOM path / selector, fingerprint, and message.

---

## Interpreting overlap and coverage carefully

- More overlap across tool families often suggests stronger corroboration.
- Same-family overlap can be sensitive to deduplication logic and selector/fingerprint strictness.
- A wider tool stack improves breadth and confidence, but automated coverage remains partial.

> **Warning:** Dashboard totals and overlap views should be interpreted as **evidence organization**, not as the final truth about accessibility quality.

---

## Suggested dashboard reading order

1.  Check **Page Inventory Check** first.
2.  Use **Top Problem Types** and **Component Heatmap** to understand the shape of the issue space.
3.  Use **WCAG Rule Breakdown** for standards-focused reporting.
4.  Use **Tool Agreement Profile** to understand corroboration patterns.
5.  Use **Fix Once, Benefit Many** to build a remediation queue.
6.  Use drilldown search and paging to inspect the exact findings behind any bar or row.
