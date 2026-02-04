# Git Efficiency Protocol: The Full Manual

This document is not just a set of rules; it is a **Master Class** on how to manage this project's code. Following this ensures that our production site never breaks and our development moves fast.

---

## 🏗️ Part 1: The Architecture (The "Why")

We use **Gitflow**. Imagine a restaurant kitchen:
*   **Dining Room (`master`)**: This is what the customers see. It must be perfect, clean, and ready to serve. You never chop onions on the customer's table.
*   **Kitchen (`develop`)**: This is where the cooking happens. It's busy, messy, and constantly changing.
*   **Prep Station (`feature/*`)**: This is where you work on *one specific dish*. You don't cook the steak and the soup in the same pan.

### The Golden Rules
1.  **Never push to `master`.** (Unless you are God, and even then, double-check).
2.  **`master` is Sacred.** If it's on master, it is being deployed to real users.
3.  **`develop` is Safe.** It shouldn't crash, but it might have half-finished ideas.

---

## 🌿 Part 2: The Branches

### 1. `master` (Production)
*   **Role**: The live version of the website.
*   **updates**: ONLY when releasing a new version (e.g., v1.0 -> v1.1).

### 2. `develop` (Integration)
*   **Role**: The main collaboration zone.
*   **Updates**: When a feature is finished and reviewed.

### 3. `feature/name-of-feature` (Work)
*   **Role**: A temporary sandbox for one specific task.
*   **Source**: Branched FROM `develop`.
*   **Destination**: Merged back INTO `develop`.
*   **Examples**: `feature/cart-system`, `feature/login-page-redesign`

### 4. `fix/name-of-bug` (Repair)
*   **Role**: A temporary sandbox to fix a bug found in development.
*   **Source**: Branched FROM `develop`.
*   **Destination**: Merged back INTO `develop`.
*   **Examples**: `fix/header-typo`, `fix/login-crash`

---

## 🎓 Part 3: The Workflows (Tutorial)

### Tutorial A: Creating a New Feature (The Daily Routine)
*Objective: You want to create a new "About Us" page.*

**Step 1: Prepare the Kitchen (Update Develop)**
Before staring, make sure you have everyone else's latest code.
```bash
git checkout develop
git pull origin develop
```

**Step 2: Go to your Station (Create Branch)**
Create a safe space to work.
```bash
git checkout -b feature/about-page
```

**Step 3: Cook (Code)**
*   Create `About.jsx`.
*   Add CSS.
*   *Tip: Commit often!*

**Step 4: Save your Progress (Commit)**
Wrap your work in a committed package.
```bash
git add .
git commit -m "feat(pages): creating initial layout for about page"
```

**Step 5: Publish your Draft (Push)**
Send your branch to GitHub so others can see it.
```bash
git push -u origin feature/about-page
```

**Step 6: Peer Review (Pull Request)**
*   Go to GitHub.
*   Create a "Pull Request" (PR) from `feature/about-page` -> `develop`.
*   Wait for approval, then click "Merge".

---

### Tutorial B: Fixing a Bug (The Repair Job)
*Objective: The "Submit" button color is wrong.*

**Step 1: Start from the latest Code**
```bash
git checkout develop
git pull origin develop
```

**Step 2: Create a Fix Branch**
```bash
git checkout -b fix/submit-button-color
```

**Step 3: Fix it & Commit**
```bash
# ... change the CSS ...
git add .
git commit -m "fix(ui): correct submit button hex code"
```

**Step 4: Push & Merge**
```bash
git push -u origin fix/submit-button-color
# Go to GitHub and Merge into 'develop'
```

---

### Tutorial C: The Release (Shipping to Production)
*Objective: The features in `develop` are ready for the world. Let's release Version 1.1.0.*

**Step 1: Finalize Version Number (In Code)**
You are still on `develop`. Open `package.json` and change the version.
```json
// package.json
{
  "name": "colestia",
  "version": "1.1.0",  // Changing from 1.0.0
  ...
}
```
Commit this change:
```bash
git commit -am "chore(release): bump version to 1.1.0"
git push origin develop
```

**Step 2: Switch to Production**
```bash
git checkout master
git pull origin master
```

**Step 3: Merge the New Code**
Bring the tested code from `develop` into `master`.
```bash
git merge develop
```

**Step 4: Tag the Release**
This creates a permanent bookmark in history.
```bash
git tag -a v1.1.0 -m "Release Version 1.1.0"
```

**Step 5: Go Live**
Push the code and the tag.
```bash
git push origin master
git push origin v1.1.0
```

---

## 📝 Part 4: Writing Professional Commits

We use **Conventional Commits**. This makes our history readable.

**Format**: `type(scope): description`

| Type | Description | Example |
| :--- | :--- | :--- |
| **feat** | A new feature | `feat(auth): add google login` |
| **fix** | A bug fix | `fix(nav): fix broken link in footer` |
| **docs** | Documentation only | `docs(readme): add setup instructions` |
| **style** | Formatting/White-space | `style(home): format code with prettier` |
| **refactor** | Code restructuring (no behavior change) | `refactor(api): simplified user fetch logic` |
| **chore** | Maintenance/Config | `chore(deps): update react version` |

---

## ❓ Part 5: FAQ & Troubleshooting

**Q: I accidentally committed to `develop`!**
A: Don't panic.
1.  Make a branch right now: `git checkout -b feature/my-saved-work`
2.  Your commit is now safe on that new branch.
3.  Go back to develop and reset it (carefully): `git checkout develop`, `git reset --hard origin/develop`.

**Q: I have "Merge Conflicts"!**
A: This means two people changed the exact same line of code.
1.  Open the file.
2.  Look for `<<<<<<< HEAD` and `>>>>>>> feature`.
3.  Decide which code is correct.
4.  Delete the markers.
5.  `git add .`, `git commit`.

**Q: Why "Pull Request"? Why not just merge myself?**
A: The PR is the "Quality Gate". It forces you to verify your changes and allows automated tests (if we have them) to run. It prevents "Sunday Night  mistakes".
