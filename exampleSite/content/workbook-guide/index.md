---
title: "Spreadsheet Workbook Guide"
summary: "Reference guide for the exported spreadsheet tabs, columns, and analysis model."
description: "A guide to the Accessibility Systemic Analyzer workbook export, including analyst tabs, BI-friendly sheets, fact/dimension tabs, field concepts, and suggested workflows."
categories: ["Accessibility", "Testing"]
tags: ["accessibility", "workbook", "reporting", "wcag", "power-bi"]
date: 2026-05-28
draft: false
showauthor: false
showTableOfContents: true
showReadingTime: true
---

This workbook is the main analysis export of the accessibility framework. It combines processed findings, deduplicated and clustered views, pattern prioritization, and BI-friendly model tabs in one place.

> **Recent workbook changes:** friendlier rule labels, stricter separation of Rule vs WCAG fields, improved WCAG-only handling, preserved needs-review findings, and closer alignment with the current dashboard behavior.

---

## How to read the workbook overall

The workbook has three broad layers:

### 1. Human summary / analyst tabs

- Summary
- Systemic Clusters
- Issue Details
- Fix Once Benefit Many
- Top Fixes

### 2. Flat reporting / BI tabs

- Power BI Findings
- Power BI Patterns

### 3. Star-schema tabs

- Fact Findings
- Dim Page
- Dim Rule
- Dim Component
- Dim Pattern

---

## Core field concepts used across tabs

| Field family              | Meaning                                          |
|---------------------------|--------------------------------------------------|
| Rule                      | Friendly display label for the issue             |
| Rule Id                   | Underlying tool-specific identifier              |
| WCAG                      | Criterion code such as `1.4.3`                   |
| WCAG Name                 | Criterion title such as `Contrast (Minimum)`     |
| Severity                  | Normalized severity used for sorting and triage  |
| Consensus                 | Overlap signal across merged tools               |
| Confidence                | Broader evidence judgement                       |
| Issue Rank Score          | Numeric prioritization signal                    |
| Pattern / Display Pattern | Repeated issue family identifier and human label |

---

## Sheet-by-sheet guide

### 1) Summary

The executive overview sheet with values such as Violations, Pages Affected, Design System Impact, Accessibility Debt Index, and Accessibility Opportunity Score.

### 2) Systemic Clusters

One of the most important analyst tabs because it moves from raw findings to repeated issue families. Typical columns include Rule, Rule Id, WCAG, WCAG Name, Level, Severity, Component, Display Pattern, Systemic, Pages, Count, Issue Rank Score, Owner Team, Root Cause, Message, and Sources.

Use it when you want to answer which issues repeat across the estate, which look systemic, and which clusters should be triaged first.

### 3) Issue Details

The detailed analyst tab: one row per processed finding with readable enriched fields such as Page, Rule, Rule Id, WCAG, WCAG Name, Severity, Component, Pattern, Consensus, Confidence, DOM context, Message, and Sources.

The workbook is now stricter about **not** using a bare WCAG code as the Rule label when that code already appears in the WCAG column.

### 4) Power BI Findings

A flatter reporting table with stable-ish keys and display fields for downstream BI.

### 5) Power BI Patterns

Pattern-level rollup for repeated issue families, page spread, ownership, severity, and systemicity.

### 6) Fact Findings

The central fact-like table with one row per processed finding.

### 7) Dim Page

Normalized page keys and display fields.

### 8) Dim Rule

Rule display label, rule id, WCAG code, WCAG name, level, and sort fields.

### 9) Dim Component

Component and ownership metadata used for grouping and reporting.

### 10) Dim Pattern

Pattern-level dimension for repeated issue-family reporting.

### 11) Fix Once Benefit Many

The pre-ranked shortlist behind the main remediation panel, including pattern, component, severity, findings count, affected pages count, owner team, and priority score.

**Priority score note:** the numeric score is a ranking aid. Many top rows can tie if they share very similar severity and spread inputs.

### 12) Top Fixes

A simpler root-cause summary used for concise management-style reporting.

---

## Needs-review findings in the workbook

Some tools emit rows that are not hard failures but still matter operationally. Depending on the adapter, these may appear as `incomplete`, `potentialviolation`, or `warning`. These rows are preserved because they are useful for investigation, but they should not always be interpreted exactly the same way as confirmed failures.

---

## When the workbook and dashboard differ

The workbook and dashboard should usually align, but the workbook is often the better place to inspect the exact fields behind a chart. If something looks odd in the dashboard, check **Issue Details**, **Dim Rule**, and **Systemic Clusters**.

---

## Typical analyst workflow

1.  Start with **Summary** and **Systemic Clusters**.
2.  Use **Issue Details** for filtered triage and handoff.
3.  Use **Fix Once Benefit Many** to build a remediation queue.
4.  Use **Power BI Findings** or the fact/dimension tabs for downstream reporting and modeling.
