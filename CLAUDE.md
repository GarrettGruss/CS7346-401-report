# CLAUDE.md — CS7346-401 Architectural Katas Report

## Project Overview

This repo contains the deliverables for the CS7346-401 (Cloud Computing) Architectural Katas project.

- **Team**: Blaine Harris and Garrett Gruss — team name **LightsPlease**
- **Presentation date**: Thursday, April 23, 2026; 8:00 – 8:15 PM
- **Kata**: Home automation system — a home electronics giant building a cloud-connected platform for controlling lights, locks, cameras, thermostats, and other modular smart-home units sold to consumer households

## Deliverables

### 1. Written Report (PDF via Canvas)
- **Length**: 15–20 pages of body content (cover page, abstract, TOC, figures/tables lists, and references do not count toward the page limit)
- **Font**: Calibri (Body), 12pt body text, 14pt section headers
- **Spacing**: 1.5 line spacing throughout
- **Margins**: ~1 inch on all sides
- **Figures/tables**: clearly labeled and referenced in text
- **File format**: PDF only

**Writing style**: cohesive explanatory paragraphs — not bullet lists. Each section must explain *why* decisions were made, *how* the design satisfies requirements, and *what tradeoffs* were considered. Bullet points may be used sparingly to summarize but cannot replace narrative.

**Required sections** (in order):
1. Introduction and Solution Overview
2. Assumptions and Constraints
3. Use Cases and Actors
4. System Architecture Diagram
5. Cloud Architecture Quality Attributes ("-ilities")
6. Cloud-Native Application Design (Twelve-Factor App)
7. Well-Architected Framework Alignment
8. Cloud Services Selection and Justification
9. Scalability, Availability, and Fault Tolerance
10. Cost Considerations
11. Risks and Mitigation Strategies
12. Conclusion
13. References

### 2. Presentation Deck (PowerPoint via Canvas)
- **Time limit**: 15 minutes total including Q&A
- **Length**: ~10–12 slides (title + conclusion included)
- Slides support spoken explanation — concise prompts, diagrams, visuals; not dense text
- Presenters may use speaker notes

## Kata Requirements Summary

The kata ("LightsPlease" / home automation):
- **Users**: individual consumers (small families); thousands of units expected in first 3 years
- **Requirements**: turnkey but modular (camera, lock, thermostat, etc.); accessible over the Internet for remote monitoring and control; assumes existing home WiFi; customers can program rules/automation
- **Constraint**: the electrical engineering and low-level module firmware are handled by other teams — the software protocol for controlling modules is flexible and must be specified as part of this architecture

## Tooling

- **Report**: LaTeX (LaTeX Workshop VSCode extension)
- **Diagrams**: draw.io (hediet.vscode-drawio VSCode extension)
- **Environment**: Dev Containers (ms-vscode-remote.remote-containers)
- **Build artifacts**: LaTeX auxiliary files and build outputs are gitignored (standard LaTeX .gitignore)

## Report Conventions

- **Risk identifiers**: Risks in Section 11 are formally numbered as R-01, R-02, … in subsection headings for traceability
- **Writing style enforcement**: Sections should use cohesive narrative paragraphs; "residual risk remains" trailing sentences have been trimmed in favor of inline acknowledgment of limitations
- **Cost section cross-references**: Section 11 risks referencing cost sensitivity should cite `\ref{sec:cost}` for traceability

## Key Evaluation Criteria

- Architectural reasoning and defensible tradeoffs — not a single "correct" solution
- Explicit, justified assumptions
- Internal consistency of the design
- Clear alignment with cloud-native principles (Twelve-Factor App) and a well-architected framework (e.g., AWS WAF)
- Quality of written narrative (not bullet-list depth)
