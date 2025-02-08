
# Branching Workflow & Commit Guidelines

This document explains our branching strategy, commit rules, and how to handle common scenarios.  
**All team members must follow this workflow to maintain consistency.**

---

## Table of Contents
1. [Branch Descriptions](#branch-descriptions)
2. [Commit Message Rules](#commit-message-rules)
3. [Feature Branch Workflow](#feature-branch-workflow)
4. [Handling Special Cases](#handling-special-cases)
5. [Best Practices](#best-practices)

---

## Branch Descriptions

### 1. `main` Branch
- **Purpose**: Production-ready code. Always stable and deployable.
- **Rules**:
  - No direct commits allowed.
  - Changes only via Pull Requests (PRs) from `testing`.
  - Code must be fully tested before merging.
### 2. `dev` Branch
- **Purpose**: Latest development work. Used for integrating features.
- **Rules**:
  - No direct commits allowed.
  - Features are merged via PRs from `feat/*` branches.
  - PRs require **1 approval**.

### 3. `testing` Branch
- **Purpose**: Pre-release testing environment.
- **Rules**:
  - Changes come from `dev` via PRs.
  - Used for final testing before merging to `main`.

### 4. `feat/*` Branches
- **Purpose**: Develop new features or bug fixes.
- **Naming Convention**:  
  `feat/<short-description>` (e.g., `feat/user-login`).  
  `fix/<short-description>` for bug fixes (e.g., `fix/login-error`).
- **Rules**:
  - Created from `dev`.
  - Deleted after merging into `dev`.

---

## Commit Message Rules
Follow the **[Conventional Commits](https://www.conventionalcommits.org/)** standard:

### Format:
```
<type>: <short description>

[optional body]

[optional footer]
```

### Allowed Types:
| Type       | Use Case                                                                 |
|------------|-------------------------------------------------------------------------|
| `feat`     | New feature (e.g., `feat: add user registration form`).                |
| `fix`      | Bug fix (e.g., `fix: resolve login button alignment`).                 |
| `docs`     | Documentation changes (e.g., `docs: update README`).                   |
| `chore`    | Maintenance tasks (e.g., `chore: update dependencies`).                |
| `rework`   | Major refactoring (e.g., `rework: simplify authentication flow`).      |
| `test`     | Test-related changes (e.g., `test: add login component tests`).        |
| `style`    | Code style/formatting (e.g., `style: format indentation`).             |

### Examples:
```bash
# Good
feat: add shopping cart functionality
fix: prevent null pointer exception in user API
docs: update installation instructions

# Bad
"added cart" → Missing type and description
"fixed stuff" → Too vague
```

---

## Feature Branch Workflow

### Step 1: Create a Feature Branch
```bash
# Start from the latest dev branch
git checkout dev
git pull origin dev

# Create a new feature branch
git checkout -b feat/user-profile
```

### Step 2: Make Commits
```bash
# Follow commit message rules
git add .
git commit -m "feat: add profile picture upload"
git push origin feat/user-profile
```

### Step 3: Keep Your Branch Updated
If `dev` has new changes while you’re working:
```bash
# Fetch latest changes
git fetch origin

# Rebase your feature branch onto dev (preferred)
git checkout feat/user-profile
git rebase dev

# OR merge dev into your branch (if rebasing is too complex)
git merge dev
```

### Step 4: Resolve Conflicts
1. Open conflicting files and look for `<<<<<<<` markers.
2. Edit the code to keep the correct changes.
3. Stage the resolved files:
   ```bash
   git add <file-name>
   ```
4. Continue the rebase/merge:
   ```bash
   git rebase --continue  # For rebase
   git commit             # For merge
   ```

### Step 5: Create a Pull Request (PR)
1. Go to GitHub and create a PR from `feat/user-profile` to `dev`.
2. Add a description explaining your changes.
3. Assign reviewers and wait for approvals.

---

## Handling Special Cases

### Case 1: `dev` Has New Changes and Your Feature is Outdated
**Solution**: Rebase your feature branch onto `dev` (see [Step 3](#step-3-keep-your-branch-updated)).

### Case 2: Hotfix for `main`
1. Create a hotfix branch from `main`:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b hotfix/login-error
   ```
2. Fix the issue, test, and create a PR to `main`.
3. Merge the hotfix back into `dev` afterward.

### Case 3: Reverting a Bad Commit
Use `git revert` to undo changes without rewriting history:
```bash
git revert <commit-hash>
git push origin dev
```

---

## Best Practices
1. **Small Commits**: Break changes into small, logical commits.
2. **Pull Frequently**: Regularly pull updates from `dev` to avoid conflicts.
3. **PR Templates**: Use GitHub PR templates to standardize descriptions.
4. **Delete Old Branches**: Clean up merged branches to reduce clutter.
5. **Communicate**: Discuss workflow issues with the team early.

---

## Glossary
- **PR**: Pull Request (request to merge changes between branches).
- **Rebase**: Update a branch by rewriting its history (use carefully!).
- **Conflict**: When two changes modify the same code and Git can’t auto-merge.