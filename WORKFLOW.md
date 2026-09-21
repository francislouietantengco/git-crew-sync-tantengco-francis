## WORKFLOW.md Answers

### 1. What did the rejected push error message tell you, and why did it happen?
The error message (`! [rejected] - non-fast-forward`) indicated that the remote branch contained commits that Clone B did not have locally. GitHub rejected the push to prevent overwriting history on the remote repository. 

This happened because Clone A had already pushed new commits to the shared `feature/overtime-pay` branch. Since Clone B made local commits without fetching those new changes first, the commit histories diverged, making a simple fast-forward update impossible.

---

### 2. What's the actual difference between how you resolved Task 3 (merge) vs Task 4 (rebase)?
* **Task 3 (Merge):** `git merge` preserved both divergent commit histories as they were and combined them using a new **merge commit**. This left a non-linear, branching path in the Git history graph showing where Clone A and Clone B worked in parallel.
* **Task 4 (Rebase):** `git rebase` temporarily stashed Clone A's local commits, updated the branch to match the remote state, and then re-applied Clone A's local commits **on top** of the newly fetched remote history. This created a completely linear commit history as if Clone A's changes were written after the remote changes.

---

### 3. What one habit would have avoided both rejected pushes in this lab?
Running `git pull` (or `git fetch` followed by checking status) **immediately before starting any new work or attempting to push**. 

By routinely syncing with the remote repository before making local changes, you ensure your local branch is up to date, avoiding non-fast-forward rejections and minimizing merge conflicts with teammates.

---

### 4. Which approach — merge or rebase — would you default to on a shared team branch, and why?
On a shared team branch, the default approach should be **Merge** (or `git pull --no-rebase`). 

**Reasoning:** Merging is safer and non-destructive. It preserves the exact historical timeline of when commits occurred across different contributors without rewriting commit hashes that others may have already pulled. 

While `rebase` produces a cleaner, linear history, rewriting shared history on a public or collaborative branch can cause serious sync issues for teammates who have based their work on the original commits. Rebase is best reserved for personal feature branches prior to integrating into a main branch.