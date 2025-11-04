#  Centsible Project Roadmap & Learning Plan

> _From the graveyard of unfinished projects rises one to rule them all._

Centsible is a Rust-powered budgeting app that combines a modern terminal interface with real-time financial data.  
It’s built to help me learn Rust, data persistence, and API integration — all while building a project that feels genuinely useful.

---

##  Weekly Progress Tracker

| Week | Focus Area | Learning Goals | Deliverables |
|------|-------------|----------------|---------------|
| **1** | Rust Setup & Toolchain | Install Rust, Cargo, Rust Analyzer; learn project structure. | ✅ Cargo project initialized, prints “Hello, Centsible!” |
| **2** | Rust Fundamentals I | Ownership, borrowing, variables, functions, control flow. | Write small CLI tests using user input + simple structs. |
| **3** | Rust Fundamentals II | Structs, enums, `Result` types, error handling. | Basic module layout; can simulate adding mock transactions. |
| **4** | SQLite Intro | Learn `rusqlite`, open DB connection, create tables. | Database schema (accounts, categories, transactions). |
| **5** | CRUD Operations | Write queries (INSERT, SELECT, DELETE). Test manually. | CLI commands: `add`, `list`, and `delete`. |
| **6** | Plaid API Basics | Create sandbox keys, set up `.env`, learn Reqwest + Serde. | Environment loads API keys successfully. |
| **7** | Plaid Integration I | Fetch accounts from API and print to terminal. | `cargo run sync-accounts` outputs sample data. |
| **8** | Plaid Integration II | Fetch transactions and insert into SQLite. | `cargo run sync` populates DB with Plaid data. |
| **9** | Async & Error Handling | Handle errors with `thiserror` and async requests. | Clean logs, robust sync command. |
| **10** | TUI Setup | Initialize Ratatui; render static layout. | TUI displays placeholder sections (Accounts / Budget). |
| **11** | TUI Event Loop | Handle keyboard input, navigation between screens. | Pressing `S` triggers sync status update. |
| **12** | Dynamic Data | Connect TUI with live SQLite queries. | Transactions table displays synced data. |
| **13** | Budget Logic I | Calculate totals by category + date using Chrono. | Budget summary section in TUI. |
| **14** | Budget Logic II | Config file (TOML) for preferences. | Reads theme + default account from config. |
| **15** | Testing & Validation | Write integration tests for sync and summary flow. | Automated tests pass with mock Plaid data. |
| **16** | UI Polish | Apply Catppuccin Mocha theme; smooth transitions. | TUI color palette matches theme preview. |
| **17** | Packaging | Create release build, README polish, badges. | First working build (`v0.1.0`). |
| **18** | Demo & Wrap-Up | Create GIF demo + reflect on learnings. | Published release + roadmap review. |
| **19+** | Stretch Goals | Auto-sync, CSV import/export, encryption, multi-profile. | Optional future upgrades. |

---

##  Phase 1 – Foundations & Rust Fundamentals

** Goal:** Learn Rust basics and set up development environment.

###  Learning Focus
- The Rust toolchain: `rustup`, `cargo`, and crates.
- Ownership, borrowing, structs, enums, pattern matching, and error handling.
- Organizing modules and using `Result` types.

###  Tools & Resources
- [The Rust Book](https://doc.rust-lang.org/book/)
- [Rustlings Exercises](https://github.com/rust-lang/rustlings)
- [Let’s Get Rusty (YouTube)](https://www.youtube.com/c/LetsGetRusty)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rust Analyzer for VS Code](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

###  Tasks
- [ ] Install Rust (`rustup`) and SQLite.
- [ ] Initialize Cargo project and set up `.gitignore`, LICENSE, and README.
- [ ] Write a simple “Hello, Centsible” CLI program.
- [ ] Practice Rust fundamentals with small command-line snippets.

---

##  Phase 2 – Local Database & Data Model

** Goal:** Design the core SQLite schema and CRUD logic.

###  Learning Focus
- Using the `rusqlite` crate for data persistence.
- Defining relationships between accounts, categories, and transactions.
- Handling currency safely with integer storage (cents, not floats).
- Querying and filtering data.

###  Tools & Resources
- [Rusqlite Crate](https://docs.rs/rusqlite/latest/rusqlite/)
- [SQLite Tutorial](https://www.sqlitetutorial.net/)
- [DB Browser for SQLite](https://sqlitebrowser.org/)
- [Rust Error Handling Patterns](https://blog.burntsushi.net/rust-error-handling/)

###  Tasks
- [ ] Create tables:
  - `accounts (id, name, type, external_id)`
  - `categories (id, name, group)`
  - `transactions (id, account_id, category_id, amount_cents, date, memo, external_id, source)`
  - `budgets (id, month, category_id, amount_cents)`
- [ ] Implement CRUD operations for transactions and categories.
- [ ] Write functions to filter transactions by category and date.
- [ ] Create a mock CLI for manual transaction entry:

bash
cargo run add "Coffee" -350
cargo run list

---

##  Phase 2.5 – Plaid API Integration (New)

** Goal:** Connect Centsible to the Plaid API to automatically fetch bank transactions.

###  Learning Focus
- REST APIs and HTTP requests in Rust.
- Authentication with API keys and access tokens.
- JSON serialization/deserialization with Serde.
- Mapping API data into your SQLite schema.

###  Tools & Resources
- [Plaid API Documentation](https://plaid.com/docs/api/)
- [Reqwest Crate](https://docs.rs/reqwest/latest/reqwest/)
- [Serde JSON](https://serde.rs/)
- [Dotenvy Crate](https://docs.rs/dotenvy/latest/dotenvy/)
- [Tokio (Async Runtime)](https://docs.rs/tokio/latest/tokio/)

###  Tasks
- [ ] Create a Plaid developer account and use sandbox credentials.
- [ ] Add `.env` file for `PLAID_CLIENT_ID`, `PLAID_SECRET`, and `PLAID_ENV`.
- [ ] Build a new `plaid_client.rs` module with:
  - `get_access_token()`
  - `fetch_accounts()`
  - `fetch_transactions()`
- [ ] Parse Plaid’s JSON and map it into your SQLite tables.
- [ ] Add commands for syncing data:

bash
cargo run sync
cargo run sync --account checking

---

##  Phase 3 – Terminal UI (TUI) Foundations

** Goal:** Build the app’s interactive terminal interface using Ratatui.

###  Learning Focus
- Ratatui layouts, widgets, and event loops.  
- Handling user input and screen updates.  
- Displaying dynamic data from SQLite.  
- Managing UI state cleanly with structs and enums.

###  Tools & Resources
- [Ratatui Docs](https://ratatui.rs)  
- [Crossterm Crate](https://docs.rs/crossterm/latest/crossterm/)  
- [Ratatui Example Gallery](https://github.com/ratatui-org/ratatui/tree/main/examples)  
- [No Boilerplate – Build a TUI in Rust (YouTube)](https://www.youtube.com/watch?v=2kP4Smw4j8U)

###  Tasks
- [ ] Create a TUI that shows a main dashboard and current balance.  
- [ ] Add a “Sync” keybinding (`S`) to fetch new Plaid data.  
- [ ] Display a status bar showing `Last Synced: <timestamp>`.  
- [ ] Build a scrollable transactions table with category colors.

---

##  Phase 4 – Core Features & Budget Logic

** Goal:** Implement budgeting, analytics, and account insights.

###  Learning Focus
- Calculating balances and category totals.  
- Building monthly budget summaries.  
- Persisting user preferences in a config file.  
- Writing integration tests for business logic.

###  Tools & Resources
- [Serde + TOML Config](https://serde.rs/)  
- [Chrono Crate (Dates)](https://docs.rs/chrono/latest/chrono/)  
- [Thiserror / Anyhow](https://github.com/dtolnay/thiserror)  
- [Clap Crate (for CLI args)](https://docs.rs/clap/latest/clap/)

###  Tasks
- [ ] Display account balances from Plaid transactions.  
- [ ] Add `config.toml` for preferences (theme, default account, sync interval).  
- [ ] Track monthly budgets and spending by category.  
- [ ] Write tests covering the full “sync → compute → render” workflow.

---

---

##  Phase 5 – Polish & User Experience

** Goal:** Make Centsible beautiful, stable, and ready to showcase.

###  Learning Focus
- Theming and color design with Catppuccin Mocha.  
- Graceful error handling and UI feedback.  
- Packaging and documenting for GitHub.  

###  Tools & Resources
- [Catppuccin Palette](https://catppuccin.com/palette)  
- [Cargo Packaging Guide](https://doc.rust-lang.org/cargo/reference/publishing.html)  
- [Shields.io Badges](https://shields.io/)  
- [Cargo Make / Justfile Automation](https://github.com/sagiegurari/cargo-make)

###  Tasks
- [ ] Apply Catppuccin Mocha color scheme across the TUI.  
- [ ] Add toast-style notifications for sync status and errors.  
- [ ] Create demo GIFs or screenshots for the README.  
- [ ] Publish `v0.1.0` with clear setup and usage instructions.  

---


##  Phase 6 – Future Ideas

These are optional enhancements for later development — ideas to extend Centsible beyond the MVP stage.

###  Potential Features
- [ ] **CSV import/export** of transactions for offline backups.  
- [ ] **Scheduled background sync** (e.g., auto-sync every 6 hours).  
- [ ] **Multiple budget profiles** (personal, church, business).  
- [ ] **Encrypted storage** for Plaid access tokens using `aes-gcm` or similar.  
- [ ] **Cloud sync / web dashboard** version (v2.0+).  
- [ ] **Notification system** (CLI alerts or email for large expenses).  
- [ ] **Statistics view**: charts for monthly spending and trends.  
- [ ] **Plugin hooks** to add custom features later.

###  Long-Term Vision
Turn Centsible into a **privacy-first finance tool** — a cross-platform hybrid app (TUI + Web) that syncs encrypted data across devices but keeps ownership in your hands.

> “The best apps aren’t built overnight — they evolve with every sync.”

---

---

##  Continuous Learning Resources

| Topic | Resource |
|-------|-----------|
| **Rust Fundamentals** | [The Rust Book](https://doc.rust-lang.org/book/) |
| **Async & APIs** | [Reqwest Docs](https://docs.rs/reqwest/latest/reqwest/) |
| **TUI Development** | [Ratatui Docs](https://ratatui.rs) |
| **Databases** | [Rusqlite Crate](https://docs.rs/rusqlite/latest/rusqlite/) |
| **Error Handling** | [Thiserror / Anyhow](https://github.com/dtolnay/thiserror) |
| **CLI Arguments** | [Clap Crate](https://docs.rs/clap/latest/clap/) |
| **Styling** | [Catppuccin Mocha Palette](https://catppuccin.com/palette) |
| **Videos** | [Let’s Get Rusty](https://www.youtube.com/c/LetsGetRusty), [No Boilerplate](https://www.youtube.com/@noboilerplate) |

---

##  Progress Tracking Template


| Date | Milestone | Status | Notes |
|------|------------|--------|-------|
| YYYY-MM-DD | Initialize Cargo project | ✅ Done | Created repo and README |
| YYYY-MM-DD | SQLite schema completed | ⏳ In Progress | Working on DB layer |
| YYYY-MM-DD | Plaid API integration working | ⬜ Planned | Sandbox setup next |
| YYYY-MM-DD | First TUI dashboard | ⬜ Planned |  |
| YYYY-MM-DD | Budget logic complete | ⬜ Planned |  |
| YYYY-MM-DD | v0.1.0 Released | ⬜ Planned |  |


---

## Final Note

Centsible is more than just a coding project — it’s my personal Rust apprenticeship.
The goal isn’t to finish fast — it’s to finish **well**.  
Each phase builds real skills i can talk about in interviews and showcase on my resume

---


