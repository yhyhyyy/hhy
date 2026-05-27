# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This is a **documentation/design-only workspace** containing a single enterprise IT architecture design document for  (Huahong Pharmaceutical). There is no build system, test suite, or runtime — it's a standalone HTML file viewed directly in a browser.

## Key file

- `V3-企业信息化顶层架构设计-ERPCRMSRMMOM.html` — self-contained HTML app that renders Mermaid.js flowcharts for the enterprise architecture.

## How to view

Open the HTML file directly in a browser (no server needed). The Mermaid library is loaded from CDN, so an internet connection is required on first load.

## HTML document architecture

The file is a single-page app with three structural layers:

1. **CSS (lines 9-141)** — dark theme (`#0f172a` base), responsive sidebar layout, print styles, custom scrollbar. Uses CSS custom properties for theming.

2. **Diagram sources (lines 250-647)** — `diagramSources` object with 8 Mermaid flowchart definitions:
   - `d0` — 总体架构全景图 (overall architecture with ERP hub, SRM/CRM/MOM triad, legend)
   - `d1` — 计划与采购管理 (planning & procurement, SRM+ERP)
   - `d2` — 仓储与物料管理 (warehouse & material management, WMS)
   - `d3` — 生产制造全流程 (full production process, MES)
   - `d4` — 质量检验与放行 (quality inspection & release, QMS+LIMS)
   - `d5` — 销售物流与追溯 (sales logistics & traceability, CRM)
   - `d6` — 支撑保障体系 (support systems: EAM, EMS, HR, validation)
   - `d7` — 系统数据流与集成 (data flow & system integration, dark background)

3. **JavaScript (lines 649-982)** — three subsystems:
   - `panZoom` object — mouse/touch drag pan, scroll-wheel zoom, pinch-to-zoom, fit-to-screen, reset. Bounded scale range 0.1x–6x.
   - Diagram management — `renderDiagram()` (calls `mermaid.render()` on the container), `switchDiagram()` (updates active tab and re-renders), edit mode with live Mermaid source editing.
   - Utilities — SVG download, `.mmd` source export, print, keyboard shortcuts.

## Keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+E` | Toggle edit mode |
| `Ctrl+S` | Apply edit (in edit mode) or download source (in preview mode) |
| `Ctrl+0` | Fit diagram to screen |
| `Esc` | Exit edit mode |
| Double-click diagram | Fit to screen |
| Right-click | (suppressed during drag) |

## Editing diagrams

When editing Mermaid source through the in-app editor, diagrams use `flowchart LR` or `flowchart TB` layout with `classDef` for color-coded node categories. Nodes use HTML line breaks (`<br/>`) for multi-line labels. The color scheme follows a consistent convention: ERP = dark blue/gold, SRM = red, CRM = teal, WMS = deep orange, MES = amber, QMS = green, support = purple, external systems = dark gray.

## Architecture domain model

The design follows an **ERP-centric hub model** with three operational spokes:
- **SRM** (supplier-facing): demand planning → supplier qualification → procurement → receiving
- **CRM** (customer-facing): sales orders → outbound → logistics → traceability/recall
- **MOM** (manufacturing): WMS (warehouse) + MES (production) + QMS/LIMS (quality) + support systems

All four systems connect to a GMP compliance layer and external systems (SCADA, TMS). Each sub-diagram (d1-d6) expands one spoke into a node-by-node process flow with inputs, outputs, intelligent features, and compliance checkpoints.

## Modifying this document

- To add a new diagram: add a new key to `diagramSources` and a corresponding sidebar tab in the HTML `<nav>`.
- To change the theme: edit the CSS `:root` variables and `mermaid.initialize()` `themeVariables`.
- The file uses Mermaid 10.x CDN; do not upgrade to Mermaid 11+ without testing — the `mermaid.render()` API signature changed.
- This file is versioned as v2.2 (2026-05).
