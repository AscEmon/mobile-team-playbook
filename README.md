# Mobile Team Playbook 📱

> AI-ready Flutter playbook for mobile team. Clean Architecture patterns, Riverpod state management, ssl_cli automation, and coding rules for humans and AI agents alike.

---

## 📖 What is This?

This repository is the **single source of truth** for the mobile development team. It contains coding standards, architecture patterns, AI agent instructions, and workflow guidelines that every developer and AI tool must follow when working on Flutter projects.

---

## 📁 Repository Structure

```
mobile-team-playbook/
├── CLAUDE.md                    ← AI agent entry point (read this first)
├── AI_CODING_RULES.md           ← Strict coding rules & patterns
└── docs/
    ├── 01-brd-guidelines.md     ← How to handle client BRD
    ├── 02-figma-guidelines.md   ← Figma to code rules
    ├── 03-coding-pattern.md     ← Coding standards (human readable)
    └── 04-testing-guidelines.md ← Testing rules
```

---

## 🚀 Getting Started

### For New Developers

1. **Read `CLAUDE.md`** — understand the project context and pipeline
2. **Read `AI_CODING_RULES.md`** — study the coding rules and patterns
3. **Install ssl_cli** — our custom Flutter CLI tool:
   ```bash
   dart pub global activate ssl_cli
   ```
4. **Verify installation:**
   ```bash
   ssl_cli help --all
   ```
5. **Add to PATH** (first time only):
   - **macOS/Linux:** add to `~/.zshrc` or `~/.bashrc`:
     ```bash
     export PATH="$PATH":"$HOME/.pub-cache/bin"
     ```
   - **Windows:** Add Dart pub cache to System Environment Variables

---

### For AI Agents (Claude, Cursor, Copilot, etc.)

1. Read `CLAUDE.md` first — it is your entry point
2. Read `AI_CODING_RULES.md` for strict coding rules
3. Always run `ssl_cli help --all` before any scaffolding
4. Never manually create folder structures — always use ssl_cli
5. Follow the pipeline: **BRD → Figma → Coding → Testing**

---

## 🔄 Development Pipeline

```
Stage 1: Client BRD       → Analyze requirements, extract user stories
         ↓
Stage 2: Figma Design     → Review design, map to global widgets
         ↓
Stage 3: Coding           → Scaffold with ssl_cli, follow Clean Architecture
         ↓
Stage 4: Testing          → Unit & widget tests, minimum 80% coverage
```

---

## 🛠️ ssl_cli

All scaffolding MUST be done via ssl_cli. Never create folder structures manually.

| Command | Description |
|---------|-------------|
| `ssl_cli help --all` | Check installation & view all commands |
| `ssl_cli create <project_name>` | Create new Flutter project (select pattern 4 + Riverpod) |
| `ssl_cli module <module_name>` | Add new feature module (select pattern 3 + Riverpod) |
| `ssl_cli generate k_assets.dart` | Regenerate assets enum after adding images or SVGs |
| `ssl_cli build apk --DEV` | Build DEV flavor APK |
| `ssl_cli build apk --LIVE` | Build LIVE flavor APK |
| `ssl_cli build apk --LIVE --t` | Build LIVE APK + share to Telegram |

---

## 🏗️ Architecture

This project follows **Clean Architecture** with three strict layers:

```
┌──────────────────────────┐
│ Presentation Layer       │  Riverpod, Pages, Widgets
├──────────────────────────┤
│ Domain Layer             │  UseCases, Entities, Repository Contracts
├──────────────────────────┤
│ Data Layer               │  Models, Repository Impl, DataSources
└──────────────────────────┘
```

- **Domain** has zero dependencies on outer layers
- **Data** depends only on Domain
- **Presentation** depends only on Domain (via UseCases)

---

## 🎨 Design System

- All UI uses **Global Widgets** — never use raw Flutter widgets
- Responsive sizing via **flutter_screenutil** (`.w`, `.h`, `.sp`, `.r`)
- Colors via `AppColors` enum — never hardcode HEX values
- Assets via `k_assets.dart` enum — never hardcode asset paths
- After adding any image or SVG → always run `ssl_cli generate k_assets.dart`

---

## 📋 Feature Creation Checklist

Use this every time you build a new feature:

- [ ] ssl_cli installed and verified (`ssl_cli help --all`)
- [ ] Module created via `ssl_cli module <name>`
- [ ] Figma frame reviewed before coding UI
- [ ] Domain layer created (Entity → Repository contract → UseCases)
- [ ] Data layer created (Model → DataSources → Repository impl)
- [ ] Dependencies registered in `service_locator.dart`
- [ ] Presentation layer created (State → Provider → Page → Widgets)
- [ ] Global widgets used throughout (no raw Flutter widgets)
- [ ] ScreenUtil used for all sizing
- [ ] Error handling with `Either<Failure, Data>`
- [ ] `ssl_cli generate k_assets.dart` run if assets were added

---

## 📚 Key Documents

| Document | Description |
|----------|-------------|
| [`CLAUDE.md`](./CLAUDE.md) | AI agent entry point & project context |
| [`AI_CODING_RULES.md`](./AI_CODING_RULES.md) | Strict coding rules, templates & patterns |
| [`docs/01-brd-guidelines.md`](./docs/01-brd-guidelines.md) | BRD analysis guide |
| [`docs/02-figma-guidelines.md`](./docs/02-figma-guidelines.md) | Figma to code workflow |
| [`docs/03-coding-pattern.md`](./docs/03-coding-pattern.md) | Coding standards (human readable) |
| [`docs/04-testing-guidelines.md`](./docs/04-testing-guidelines.md) | Testing rules & coverage |

---

## 🔌 Team Tools

| Tool | Purpose |
|------|---------|
| Figma | UI Design & Design System |
| Jira | Task & Project Management |
| Confluence | BRD & Documentation |
| Notion | Team Docs & Guides |
| Telegram | Build Distribution |

---

## 🤝 Contributing

1. Clone this repo
2. Create a new branch for your changes
3. Follow all rules in `AI_CODING_RULES.md`
4. Submit a pull request for team review


