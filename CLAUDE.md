# SSLWireless Mobile Team — AI Context

> **Purpose:** This is the entry point for any AI agent working on SSLWireless mobile projects. Read this first before doing anything.

---

## 🏢 About the Team

- **Company:** SSLWireless
- **Team:** Mobile Development
- **Stack:** Flutter (Dart)
- **Architecture:** Clean Architecture (Feature Based)
- **State Management:** Riverpod
- **CLI Tool:** ssl_cli (SSLWireless custom Flutter CLI)

---

## 🤖 AI Agent — Start Here

Before writing any code, follow this order:

1. **Read this file** — understand the project context
2. **Read `AI_CODING_RULES.md`** — follow strict coding rules and patterns
3. **Check ssl_cli** — run `ssl_cli help --all` before any scaffolding
4. **Identify the pipeline stage** — see pipeline section below

> Full coding rules, templates, folder structure, naming conventions, and ssl_cli commands are in [`AI_CODING_RULES.md`](./AI_CODING_RULES.md)

---

## 🔄 Development Pipeline

Every feature follows this exact pipeline. Always identify which stage you are in before starting work:

```
Stage 1: Client BRD
        ↓
Stage 2: Figma Design
        ↓
Stage 3: Coding
        ↓
Stage 4: Testing
```

### Stage 1 — Client BRD
- Analyze the BRD document
- Extract user stories, screens needed, API requirements
- Identify edge cases and missing information
- List clarification questions for the client
- Output as structured markdown

### Stage 2 — Figma Design
- Always ask for Figma frame URL before coding any UI
- Read design tokens (spacing, colors, typography)
- Check existing component library before creating new components
- Map Figma components to global widgets in the codebase
- Never hardcode colors or sizes — use design tokens

### Stage 3 — Coding
- Always use `ssl_cli` for scaffolding (never manually create structure)
- Follow Clean Architecture strictly: Domain → Data → Presentation
- Use global widgets, ScreenUtil, Riverpod
- Follow all rules in `AI_CODING_RULES.md`

### Stage 4 — Testing
- Unit test all UseCases and ViewModels
- Widget test all reusable components
- Mock all external dependencies
- Minimum coverage: 80%

---

## 🔌 Connected Tools & Resources

| Tool | Purpose | Link |
|------|---------|-------|
| **Figma** | UI Design & Design System | _add your figma link_ |
| **Jira** | Task & Project Management | _add your jira link_ |
| **Confluence** | BRD & Documentation | _add your confluence link_ |
| **Notion** | Team Docs & Guides | _add your notion link_ |
| **GitHub** | Source Code | _add your github link_ |

---

## 📁 Repo Structure

```
sslwireless-mobile-playbook/
├── CLAUDE.md                  ← You are here (AI entry point)
├── AI_CODING_RULES.md         ← Strict coding rules & patterns
└── docs/
    ├── 01-brd-guidelines.md   ← How to handle client BRD
    ├── 02-figma-guidelines.md ← Figma to code rules
    ├── 03-coding-pattern.md   ← Coding standards (human readable)
    └── 04-testing-guidelines.md ← Testing rules
```

---

## 📐 Architecture Overview

```
┌──────────────────────────┐
│ Presentation Layer       │  Riverpod, Pages, Widgets
├──────────────────────────┤
│ Domain Layer             │  UseCases, Entities, Repository Contracts
├──────────────────────────┤
│ Data Layer               │  Models, Repository Impl, Remote/Local DataSources
└──────────────────────────┘
```

**Dependency Rule:** Dependencies only point inward. Domain layer has zero dependencies on outer layers.

---

## 🎨 Design System

- All UI components use **Global Widgets** (GlobalText, GlobalButton, GlobalLoader, etc.)
- Responsive sizing via **flutter_screenutil** (.w, .h, .sp, .r)
- Colors defined in `AppColors` enum — never hardcode HEX values
- Assets registered in `k_assets.dart` enum — never hardcode asset paths
- After adding any image or SVG → run `ssl_cli generate k_assets.dart`

---

## 🚀 ssl_cli — Team CLI Tool

SSLWireless uses a custom CLI to scaffold projects and modules. **Always use it — never manually create folder structures.**

```bash
# Check installation
ssl_cli help --all

# Install if not found
dart pub global activate ssl_cli

# New project
ssl_cli create <project_name>     # Select pattern 4 + Riverpod

# New feature module
ssl_cli module <module_name>      # Select pattern 3 + Riverpod

# After adding images or SVGs
ssl_cli generate k_assets.dart

# Build
ssl_cli build apk --flavorType    # --DEV / --LIVE / --LOCAL / --STAGE
```

> Full ssl_cli command reference and agent decision flow is in [`AI_CODING_RULES.md`](./AI_CODING_RULES.md)

---

## ✅ Quick Checklist for Every Feature

- [ ] Identified pipeline stage (BRD / Figma / Coding / Testing)
- [ ] ssl_cli is installed and verified
- [ ] Module scaffolded via `ssl_cli module <name>`
- [ ] Figma frame reviewed before UI coding
- [ ] Clean Architecture layers followed (Domain → Data → Presentation)
- [ ] Global widgets used (no raw Flutter widgets)
- [ ] Dependencies registered in `service_locator.dart`
- [ ] Error handling with `Either<Failure, Data>`
- [ ] `ssl_cli generate k_assets.dart` run if assets were added

---

*Maintained by SSLWireless Mobile Team*
