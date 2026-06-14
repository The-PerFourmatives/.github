# 📚 Contributing to the Readar Library System

Welcome to the Readar repository! To ensure our codebase stays clean as our architecture evolves and we hit our July deadlines without merge conflicts, everyone must strictly follow these Git standards.

---

## 1. Branching Strategy

We use a strict Feature Branch workflow. **Never push directly to `main`.**

### The Core Branches
* `main` : The stable, production-ready version of our app. Code only gets here via Pull Requests.
* `develop` : The active integration branch. All features merge here first to ensure the front-end and back-end play nicely together.

### Feature Branches (Your Daily Workspace)
When you start a new task, always branch off `develop`. Name your branch using standard Git flow prefixes, followed by a descriptive name of the task:

* **`feat/`** : For new features across any layer (e.g., `feat/login-page`, `feat/book-models`, `feat/borrowing-service`)
* **`fix/`** : For bug fixes (e.g., `fix/fines-calculation`, `fix/ui-alignment`)
* **`hotfix/`** : For urgent, critical fixes that need to be fast-tracked (e.g., `hotfix/database-connection-crash`)
* **`chore/`** : For project maintenance, package updates, or configuration (e.g., `chore/update-dotnet-packages`, `chore/gitignore-setup`)
* **`docs/`** : For updating documentation (e.g., `docs/update-readme`)

### Example Workflow:
If you are building the database models for the catalog, your branch should be `feat/catalog-database-models`. If you are building the frontend UI for that catalog, it should be `feat/catalog-ui`.

---

## 2. Commit Message Convention

We strictly follow [Conventional Commits](https://www.conventionalcommits.org/). This makes our project history readable and professional.

**Format:** `type(scope): brief description`

### Allowed Types:
* `feat`: A new feature (e.g., adding a new page or endpoint).
* `fix`: A bug fix.
* `style`: UI styling, Tailwind adjustments, or code formatting (no logic changes).
* `refactor`: Rewriting code without changing its behavior.
* `docs`: Updating documentation or markdown files.
* `chore`: Project setup, installing packages, or updating `.gitignore`.

### Examples for Our Team:
* **Backend:** `feat(api): implement JWT token generation in AuthService`
* **Backend:** `fix(db): resolve 1-to-1 foreign key error on Fines table`
* **Frontend:** `feat(ui): build responsive signup form with Tailwind`
* **Frontend:** `style(nav): update active tab colors on student dashboard`
* **Global:** `chore(setup): add UserSecrets configuration to .gitignore`

---

## 3. The Daily Workflow

Follow these exact steps every time you sit down to code:

**1. Get the latest code:**
```bash
git checkout develop
git pull origin develop

```

**2. Create your workspace:**

```bash
git checkout -b your-branch-type/your-feature-name

```

**3. Write code, test locally, and stage your changes:**

```bash
git add .

```

**4. Commit using the convention:**

```bash
git commit -m "feat(dashboard): add active session data display"

```

**5. Push your branch to GitHub:**

```bash
git push -u origin your-branch-type/your-feature-name

```

**6. Open a Pull Request (PR):**
Go to GitHub, open a PR from your branch into `develop`.

---

## 4. Pull Request (PR) Conventions

Our PR process ensures that broken code never reaches our users. When opening any PR, GitHub will automatically provide our standard checklist, but you can also manually refer to our **[Pull Request Template](pull_request_template.md)** for the exact formatting requirements.

### Phase 1: Feature Branch ➔ `develop` (The Daily Merge)

When you finish a feature, open a PR to merge your branch into `develop`.

* **Title Format:** Must strictly follow the commit convention (e.g., `feat(auth): add JWT login endpoint`).
* **Description:** Briefly explain what you built and how to test it locally using the provided template sections.
* **Review Requirement:** You must get at least **one approval** from a teammate (e.g., the PM or the lead of the other stack) before merging.

### Phase 2: `develop` ➔ `main` (The Module Release)

Merging into `main` is a major event. We do not release single features to `main`. Instead, we release completed **Modules** (e.g., the entire Auth System, the Book Catalog).

* **Who does this:** The Project Manager (PM) and the Tech Lead share the responsibility for initiating and merging release PRs.
* **Approval Requirement:** Both the PM and the Tech Lead must review and approve the module release before it is merged. This ensures both architectural integrity and feature completeness.
* **Title Format:** `Release: Module Name (RD.XX.YY.ZZ)`
* **Testing:** The entire team must verify that the frontend and backend communicate flawlessly on the `develop` branch before this PR is opened.
---

## 5. Versioning and Releases

Every time code is merged from `develop` into `main`, it is considered a **Stable Release**. We use the custom Readar versioning scheme: **`RD.XX.YY.ZZ`**

* **`RD`**: Readar Library System
* **`XX` (Major)**: Significant system milestones. We will remain at `00` while building the core system. The official launch will be `RD.01.00.00`.
* **`YY` (Minor)**: Increments when a large, new architectural module is completed and merged (e.g., Auth Module, Book Catalog).
* **`ZZ` (Patch)**: Increments for urgent bug fixes or minor UI tweaks to an existing, previously released module.

**Example Progression:**
* `RD.00.01.00` - Initial Authentication Module completed.
* `RD.00.01.01` - Fixed a bug in the login UI.
* `RD.00.02.00` - Book Catalog Module completed.
* `RD.01.00.00` - Official Readar Launch.

Immediately after merging a PR into `main`, the Tech Lead will tag the release in Git:

```bash
git tag -a RD.00.01.00 -m "Release RD.00.01.00: Initial Authentication Module"
git push origin RD.00.01.00
```

---

## 6. The Changelog Standard (Release-Only)

We maintain a `CHANGELOG.md` file in the root directory to track our progress. To protect the development team's coding velocity, we strictly use a **Release-Only** strategy. 

Developers **do not** update the changelog when merging daily features into `develop`. Instead, the Project Manager (PM) owns this document and updates it only when a module is completely finished and ready for a production release.

### The PM Release Workflow:
1. **Prepare the Summary:** Once all module features are merged and tested in `develop`, the PM writes a high-level summary of the completed module in `CHANGELOG.md`. *(You can copy the required boilerplate from our **[Changelog Template](CHANGELOG_TEMPLATE.md)**).*
2. **The Direct Commit:** To avoid creating unnecessary Pull Requests just for documentation, the PM is authorized to commit and push this file update **directly to the `develop` branch**.
   * *Commit Example:* `docs(release): update changelog for RD.00.01.00`
3. **The Final Merge:** The PR from `develop` to `main` is then opened by the Tech Lead or PM, carrying the finalized changelog with it.

### Format:
We do not use `[Unreleased]` tags. Always group your updates cleanly under the new release version (using our `RD.XX.YY.ZZ` standard) and categorize them using the following headers:

* `### Added` for new features or modules.
* `### Changed` for changes in existing functionality.
* `### Removed` for removed features.
* `### Fixed` for any bug fixes.
* `### Security` for patching vulnerabilities.

**Example of a Release Changelog update:**

```markdown
## [RD.00.01.00] - 2026-06-20
### Added
**Authentication Module**
- `feat(db)`: Built the user database architecture and roles.
- `feat(ui)`: Created the student login and registration Razor Pages.

### Fixed
- `fix(db)`: Resolved 1-to-1 relationship error on Fines table.
```

## 7. The Golden Security Rule

**Never commit your API Keys, Database Passwords, or Secrets to GitHub.**

Because we use a unified N-Tier architecture, all local secrets are managed in one place.

*  All developers (whether you are writing Razor Pages or configuring the database) must use `.NET User Secrets` attached strictly to the `Readar.WebApp` project.

> **CRITICAL:** If you accidentally push a secret, API key, or password, notify the entire team immediately so we can perform key rotation before the repository is compromised.
