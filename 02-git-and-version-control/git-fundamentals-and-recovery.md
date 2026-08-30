# Git Fundamentals, Branching & Recovery Playbook

A practical guide covering standard Git operations, branch management, conflict resolution, and history recovery.

---

### 1. The Core Lifecycle (The Shipping Box Analogy)

* **`git init`**: Initializes a new local Git tracking repository.
* **`git status`**: Inspects the state of the working directory and staging area.
* **`git add <file>`**: Moves changes from the working directory into the **staging area** (placing the item in the box).
* **`git restore --staged <file>`**: Unstages a file back to the working directory without losing code edits (taking the item out of the box).
* **`git commit -m "message"`**: Takes a snapshot of staged changes with a descriptive label (taping and labeling the box).
* **`git push origin <branch>`**: Uploads committed local snapshots to the remote repository (GitHub).

---

### 2. Branching & Merging

* **`git branch`**: Lists all existing local branches.
* **`git switch -c <branch-name>`** *(or `git checkout -b`)*: Creates a new branch from current `HEAD` and switches to it.
* **`git switch <branch-name>`**: Swaps between existing branches cleanly.
* **`git merge <branch-name>`**: Integrates history and commits from the target branch into your current active branch.
* **Resolving Merge Conflicts**:
  1. Open conflicting files showing `<<<<<<<`, `=======`, and `>>>>>>>` markers.
  2. Edit files to keep the desired code and remove the conflict markers.
  3. Run `git add <file>` to mark conflicts as resolved.
  4. Run `git commit` to complete the merge.

---

### 3. Precision Tools & Recovery

| Command | Purpose | Real-World Scenario |
| :--- | :--- | :--- |
| **`git stash`** | Shelves all dirty, uncommitted changes temporarily. | You need to switch branches immediately to fix an emergency bug without making a messy commit. |
| **`git stash pop`** | Restores shelved changes and clears them from the stash list. | Returning to your feature branch after fixing the emergency. |
| **`git cherry-pick <hash>`** | Copies a specific commit from one branch and applies it directly to the current branch. | Applying an isolated production hotfix without merging unready feature code. |
| **`git log --oneline`** | Displays commit history in a compact, readable format. | Tracking recent verified milestones. |
| **`git reflog`** | Git's flight recorder tracking every local movement of `HEAD`. | Recovering deleted branches, undoing accidental hard resets, or finding lost commit hashes. |

---

### 4. Branch Recovery Recipe with Reflog

1. View the flight recorder: `git reflog`
2. Locate the commit hash or `HEAD@{n}` where the lost branch existed.
3. Recreate the branch at that exact point:
   ```bash
   git switch -c <recovered-branch-name> <commit-hash>
