# Level 2: Intermediate - Pull Requests, Merging & Conflict Resolution

**Duration:** 30-40 minutes  
**Objective:** Merge feature branches into main using Pull Requests, perform code reviews, and resolve merge conflicts.

---

## Step 1: Create Pull Request (Developer 1)

Developer 1 creates the first PR to merge the homepage.

1. Go to repository on GitHub
2. Click **Pull requests** tab → **New pull request**
3. Set:
   - Base: `main`
   - Compare: `feature/homepage`
4. Fill in details:
   - Title: `Add homepage with navigation`
   - Description:
     ```
     ## Changes
     - Added index.html with navigation bar
     - Includes links to About and Contact pages
     
     ## Testing
     - Opened index.html in browser locally
     - All navigation links present
     ```
5. Assign **Developer 2** as reviewer
6. Click **Create pull request**

---

## Step 2: Code Review (Developer 2 reviews Developer 1's PR)

Developer 2 reviews the PR:

1. Go to **Pull requests** → Click on Developer 1's PR
2. Click **Files changed** tab
3. Review the code:
   - Click the **+** icon next to any line to add a comment
   - Add a comment on the `<p>Version: 1.0</p>` line: "Consider making this dynamic in future versions"
4. Click **Review changes** (top right)
5. Select **Approve**
6. Click **Submit review**

---

## Step 3: Merge First PR (Developer 1)

After receiving approval:

1. Go to the PR page
2. Click **Merge pull request**
3. Select merge method: **Create a merge commit**
4. Click **Confirm merge**
5. Click **Delete branch** to clean up the remote feature branch

---

## Step 4: Create Pull Request (Developer 2)

Developer 2 creates a PR for the about page:

1. Go to **Pull requests** → **New pull request**
2. Set:
   - Base: `main`
   - Compare: `feature/about`
3. Fill in:
   - Title: `Add about page with team information`
   - Description:
     ```
     ## Changes
     - Added about.html with team details
     - Includes mission statement and team member list
     ```
4. Assign **Developer 3** as reviewer
5. Click **Create pull request**

---

## Step 5: Code Review and Merge (Developer 3 reviews, Developer 2 merges)

**Developer 3:**
1. Review the PR → **Files changed**
2. Approve the PR

**Developer 2:**
1. Click **Merge pull request** → **Confirm merge**
2. Click **Delete branch**

---

## Step 6: Create Pull Request (Developer 3)

Developer 3 creates a PR for the contact page:

1. **Pull requests** → **New pull request**
2. Set:
   - Base: `main`
   - Compare: `feature/contact`
3. Fill in:
   - Title: `Add contact page`
   - Description:
     ```
     ## Changes
     - Added contact.html with email, location, working hours
     ```
4. Assign **Developer 1** as reviewer
5. Click **Create pull request**

---

## Step 7: Code Review and Merge (Developer 1 reviews, Developer 3 merges)

**Developer 1:**
1. Review and approve the PR

**Developer 3:**
1. Merge and delete the branch

---

## Step 8: Sync Local Repositories (All Developers)

All developers update their local main branch:

```bash
git checkout main
git pull origin main
```

Verify all files are present:

```bash
ls
```

Expected output:
```
README.md  about.html  contact.html  css  images  index.html
```

---

## Step 9: Create a Merge Conflict (Deliberate Exercise)

Now we simulate a real-world conflict where two developers edit the same file.

**Developer 1** creates a branch and modifies the footer in `index.html`:

```bash
git checkout -b bugfix/footer-dev1
```

Edit `index.html` — change the footer line to:

```html
    <footer>
        <p>&copy; 2026 Team Website | Designed by Developer 1</p>
    </footer>
```

Commit and push:

```bash
git add index.html
git commit -m "Update footer with designer credit"
git push origin bugfix/footer-dev1
```

**Developer 2** creates a branch from main and modifies the same footer:

```bash
git checkout main
git checkout -b bugfix/footer-dev2
```

Edit `index.html` — change the footer line to:

```html
    <footer>
        <p>&copy; 2026 Team Website | Built with love by the team</p>
    </footer>
```

Commit and push:

```bash
git add index.html
git commit -m "Update footer with team message"
git push origin bugfix/footer-dev2
```

---

## Step 10: Merge First Footer Change (Developer 1)

Developer 1 creates a PR and merges `bugfix/footer-dev1` → `main`:

1. Create PR: `Update footer - Designer credit`
2. Get review from Developer 3
3. Merge and delete branch

---

## Step 11: Observe the Conflict (Developer 2)

Developer 2's PR (`bugfix/footer-dev2`) now shows a merge conflict on GitHub.

1. Go to Developer 2's PR
2. GitHub displays: **This branch has conflicts that must be resolved**

---

## Step 12: Resolve the Merge Conflict (Developer 2)

**Option A: Resolve on GitHub (Simple conflicts)**

1. Click **Resolve conflicts** button on the PR page
2. GitHub shows the conflicting file with conflict markers:
   ```
   <<<<<<< bugfix/footer-dev2
       <p>&copy; 2026 Team Website | Built with love by the team</p>
   =======
       <p>&copy; 2026 Team Website | Designed by Developer 1</p>
   >>>>>>> main
   ```
3. Edit to keep the desired version (combine both):
   ```html
       <p>&copy; 2026 Team Website | Designed by Developer 1 | Built with love</p>
   ```
4. Remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
5. Click **Mark as resolved**
6. Click **Commit merge**

**Option B: Resolve Locally (Complex conflicts)**

```bash
git checkout bugfix/footer-dev2
git pull origin main
```

Git shows the conflict. Open `index.html` in an editor, find the conflict markers, resolve them, then:

```bash
git add index.html
git commit -m "Resolve merge conflict in footer"
git push origin bugfix/footer-dev2
```

---

## Step 13: Complete the Merge (Developer 2)

After resolving conflicts:

1. The PR now shows no conflicts
2. Get review approval from Developer 3
3. Click **Merge pull request** → **Confirm merge**
4. Delete the branch

---

## Step 14: Sync Again (All Developers)

```bash
git checkout main
git pull origin main
```

Verify the footer in `index.html` has the resolved content.

---

## Step 15: Clean Up Local Branches (All Developers)

Delete merged local branches:

```bash
git branch -d feature/homepage
git branch -d feature/about
git branch -d feature/contact
git branch -d bugfix/footer-dev1
git branch -d bugfix/footer-dev2
```

View remaining branches:

```bash
git branch -a
```

Prune stale remote references:

```bash
git fetch --prune
```

---

## Key Concepts Learned

| Concept | Where | Purpose |
|---------|-------|---------|
| Pull Request | GitHub UI | Request to merge branch into main |
| Code Review | PR → Files changed | Team reviews code before merge |
| Merge Commit | PR merge button | Combines branch history into main |
| Merge Conflict | Same file edited | Two branches modify same lines |
| Conflict Resolution | Editor / GitHub | Manually decide which changes to keep |
| Branch Cleanup | `git branch -d` | Remove merged branches |
| Prune | `git fetch --prune` | Remove stale remote references |

---

## Merge Strategies Reference

| Strategy | When to Use | Result |
|----------|-------------|--------|
| Merge commit | Default, preserves full history | Creates a merge commit |
| Squash and merge | Many small commits on feature branch | Single commit on main |
| Rebase and merge | Clean linear history preferred | No merge commit, linear history |

---

## Checkpoint

Before moving to Level 3, verify:

- [ ] All 3 feature branches merged to main via PRs
- [ ] At least one PR was reviewed before merging
- [ ] Merge conflict was created and resolved
- [ ] All developers have latest main locally
- [ ] All merged branches are deleted (local and remote)

---

**Next:** [LEVEL3-EXPERT.md](LEVEL3-EXPERT.md) - Tags, Releases, Branch Protection & Deployment
