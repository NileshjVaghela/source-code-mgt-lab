# Level 3: Expert - Tags, Releases, Branch Protection & S3 Deployment

**Duration:** 30-40 minutes  
**Objective:** Tag releases, create GitHub Releases, configure branch protection rules, and deploy the website to S3 using AWS CodePipeline.

---

## Part A: Tags and Releases

### Step 1: Create a Tag (Developer 1)

Tags mark specific points in history, typically used for release versions.

Create an annotated tag for version 1.0.0:

```bash
git checkout main
git pull origin main
git tag -a v1.0.0 -m "Version 1.0.0 - Initial release with all pages"
```

View the tag:

```bash
git tag
git show v1.0.0
```

Push the tag to GitHub:

```bash
git push origin v1.0.0
```

---

### Step 2: Verify Tag on GitHub (All Developers)

1. Go to repository → Click **Tags** (near the branch dropdown)
2. Verify `v1.0.0` is listed
3. Click on it to see the tag details and associated commit

---

### Step 3: Create a GitHub Release (Developer 1)

1. Go to repository → **Releases** (right sidebar) → **Create a new release**
2. Configure:
   - **Choose a tag:** Select `v1.0.0`
   - **Release title:** `v1.0.0 - Initial Release`
   - **Description:**
     ```
     ## What's Included
     - Homepage with navigation
     - About page with team information
     - Contact page with details
     - Common CSS stylesheet
     
     ## Contributors
     - Developer 1: Homepage
     - Developer 2: About page
     - Developer 3: Contact page
     
     ## Deployment
     Deployed to AWS S3 static website hosting.
     ```
   - **Pre-release:** Leave unchecked
3. Click **Publish release**

---

### Step 4: Make a Small Change for v1.1.0 (Developer 2)

Developer 2 adds a small enhancement:

```bash
git checkout -b feature/styling-update
```

Edit `css/style.css` — add at the end:

```css
/* v1.1.0 - Enhanced styling */
h2 {
    color: #2c3e50;
    border-bottom: 2px solid #3498db;
    padding-bottom: 10px;
    margin-bottom: 15px;
}

ul {
    padding-left: 20px;
}

ul li {
    margin: 8px 0;
}
```

Commit, push, create PR, and merge:

```bash
git add css/style.css
git commit -m "Enhance heading and list styles"
git push origin feature/styling-update
```

Create PR on GitHub → Get review from Developer 1 → Merge → Delete branch.

---

### Step 5: Tag v1.1.0 and Create Release (Developer 1)

```bash
git checkout main
git pull origin main
git tag -a v1.1.0 -m "Version 1.1.0 - Enhanced styling"
git push origin v1.1.0
```

Create a GitHub Release for `v1.1.0`:
- Title: `v1.1.0 - Enhanced Styling`
- Description: `Improved heading styles and list formatting.`
- Publish

---

### Step 6: Compare Releases (All Developers)

1. Go to **Releases** page
2. Notice both v1.0.0 and v1.1.0 are listed
3. Click on a release → Click **Compare** to see changes between tags

From command line:

```bash
git log v1.0.0..v1.1.0 --oneline
```

---

## Part B: Branch Protection Rules

### Step 7: Configure Branch Protection (Developer 1 - Repo Owner)

Protect the main branch so no one can push directly.

1. Go to **Settings** → **Branches**
2. Click **Add branch protection rule**
3. Configure:
   - **Branch name pattern:** `main`
   - Check: **Require a pull request before merging**
     - Check: **Require approvals** → Set to **1**
   - Check: **Require status checks to pass before merging** (optional for now)
   - Check: **Do not allow bypassing the above settings**
4. Click **Create**

---

### Step 8: Test Branch Protection (Developer 3)

Developer 3 tries to push directly to main:

```bash
git checkout main
```

Edit `index.html` — change the version text to `Version: 2.0`:

```bash
git add index.html
git commit -m "Direct push test"
git push origin main
```

**Expected result:** Push is rejected with an error message indicating branch protection rules.

Undo the local commit:

```bash
git reset --soft HEAD~1
git checkout -- index.html
```

This confirms that all changes must go through Pull Requests.

---

## Part C: Deploy to S3 Using AWS CodePipeline

### Step 9: Create S3 Bucket for Website Hosting (Developer 1 - AWS Console)

1. Go to **AWS Console** → **S3**
2. Click **Create bucket**
3. Configure:
   - Bucket name: `team-website-<unique-id>` (e.g., `team-website-2026-workshop`)
   - Region: Choose your preferred region
   - **Uncheck** Block all public access
   - Acknowledge the warning
4. Click **Create bucket**

---

### Step 10: Enable Static Website Hosting

1. Click on the created bucket
2. Go to **Properties** tab
3. Scroll to **Static website hosting** → Click **Edit**
4. Configure:
   - Static website hosting: **Enable**
   - Hosting type: **Host a static website**
   - Index document: `index.html`
5. Click **Save changes**

---

### Step 11: Add Bucket Policy for Public Access

1. Go to **Permissions** tab
2. Click **Bucket policy** → **Edit**
3. Add this policy (replace `<bucket-name>` with your actual bucket name):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<bucket-name>/*"
        }
    ]
}
```

4. Click **Save changes**

---

### Step 12: Create GitHub Connection in AWS (Developer 1)

1. Go to **AWS Console** → **CodePipeline** → **Settings** → **Connections**
2. Click **Create connection**
3. Select **GitHub** as provider
4. Connection name: `github-connection`
5. Click **Connect to GitHub**
6. Authorize AWS Connector for GitHub in the popup
7. Click **Install a new app** → Select your GitHub account → Select the `team-website` repository
8. Click **Connect**

---

### Step 13: Create CodePipeline (Developer 1)

1. Go to **AWS Console** → **CodePipeline** → **Create pipeline**
2. **Step 1 - Pipeline settings:**
   - Pipeline name: `team-website-pipeline`
   - Pipeline type: **V2**
   - Execution mode: **Queued**
   - Service role: **New service role**
   - Role name: `AWSCodePipelineServiceRole-team-website`
   - Click **Next**

3. **Step 2 - Source stage:**
   - Source provider: **GitHub (Version 2)**
   - Connection: Select `github-connection`
   - Repository name: Select `team-website`
   - Branch name: `main`
   - Output artifact format: **CodePipeline default**
   - Trigger: **Push in a branch**
   - Click **Next**

4. **Step 3 - Build stage:**
   - Click **Skip build stage** → Confirm skip
   - (No build needed for static HTML/CSS)

5. **Step 4 - Deploy stage:**
   - Deploy provider: **Amazon S3**
   - Region: Same as your bucket
   - Bucket: Select your bucket (e.g., `team-website-2026-workshop`)
   - Check: **Extract file before deploy**
   - Leave deployment path empty
   - Click **Next**

6. **Step 5 - Review:**
   - Review all settings
   - Click **Create pipeline**

---

### Step 14: Verify Initial Deployment

The pipeline runs automatically after creation.

1. Watch the pipeline stages turn green (Source → Deploy)
2. Once complete, access your website:
   - Go to **S3** → Your bucket → **Properties** → **Static website hosting**
   - Copy the **Bucket website endpoint** URL
   - Open in browser

**Expected:** You see the homepage with navigation links. All pages should work.

---

### Step 15: Test Automatic Deployment (Developer 3)

Developer 3 makes a change to trigger the pipeline:

```bash
git checkout main
git pull origin main
git checkout -b feature/homepage-update
```

Edit `index.html` — change the welcome text:

```html
    <div class="container">
        <h2>Welcome to Our Team Website</h2>
        <p>This website demonstrates a complete GitHub + AWS CI/CD workflow.</p>
        <p>Automatically deployed via AWS CodePipeline.</p>
        <p>Version: 1.2</p>
    </div>
```

Commit and push:

```bash
git add index.html
git commit -m "Update homepage content for v1.2"
git push origin feature/homepage-update
```

Create PR on GitHub → Get approval from Developer 1 → Merge.

---

### Step 16: Watch Pipeline Auto-Trigger

1. Go to **AWS Console** → **CodePipeline**
2. Observe `team-website-pipeline` starts automatically after the merge
3. Wait for all stages to complete (1-2 minutes)
4. Refresh the website URL — new content should appear

---

### Step 17: Tag and Release v1.2.0 (Developer 1)

```bash
git checkout main
git pull origin main
git tag -a v1.2.0 -m "Version 1.2.0 - Pipeline deployed release"
git push origin v1.2.0
```

Create GitHub Release:
- Tag: `v1.2.0`
- Title: `v1.2.0 - Pipeline Deployed`
- Description: `First release deployed automatically via AWS CodePipeline to S3.`
- Publish

---

## Part D: Git Best Practices Review

### Step 18: Review Repository State (All Developers)

Check the final state of the repository:

```bash
git checkout main
git pull origin main

# View all tags
git tag -l

# View recent history
git log --oneline -10

# View branch graph
git log --oneline --graph --all -15
```

---

### Step 19: View Network Graph on GitHub

1. Go to repository → **Insights** → **Network**
2. This shows the visual branch and merge history
3. Identify the merge commits, feature branches, and tags

---

## Summary of Git Workflow

```
Developer creates feature branch
         │
         ↓
Developer commits and pushes to branch
         │
         ↓
Developer creates Pull Request
         │
         ↓
Reviewer reviews code → Approves or Requests changes
         │
         ↓
Developer merges PR to main (branch deleted)
         │
         ↓
CodePipeline detects change → Deploys to S3
         │
         ↓
Lead tags the release → Creates GitHub Release
```

---

## Key Concepts Learned

| Concept | Command / Location | Purpose |
|---------|-------------------|---------|
| Annotated Tag | `git tag -a v1.0.0 -m "msg"` | Mark release points |
| Push Tags | `git push origin v1.0.0` | Share tags with team |
| List Tags | `git tag -l` | View all tags |
| Compare Tags | `git log v1.0.0..v1.1.0` | See changes between versions |
| GitHub Release | Releases → New release | Distribution point with notes |
| Branch Protection | Settings → Branches | Enforce PR workflow |
| CodePipeline | AWS Console | Automate deployment on merge |
| S3 Static Hosting | S3 → Properties | Host static website |

---

## Final Checkpoint

Verify the complete workshop is successful:

- [ ] Repository has 3+ tags (v1.0.0, v1.1.0, v1.2.0)
- [ ] GitHub Releases created with descriptions
- [ ] Branch protection prevents direct push to main
- [ ] S3 bucket hosts the website publicly
- [ ] CodePipeline deploys automatically on merge to main
- [ ] Website is accessible via S3 endpoint URL
- [ ] All team members contributed via PRs

---

## Cleanup

When the workshop is complete:

**AWS Resources:**
1. Delete CodePipeline: `team-website-pipeline`
2. Empty and delete S3 bucket: `team-website-<unique-id>`
3. Delete CodePipeline service role from IAM
4. Delete GitHub connection from CodePipeline settings

**GitHub:**
1. Optionally archive or delete the repository

---

## Congratulations!

You have completed the full GitHub SCM workshop covering:

- Repository setup and collaboration
- Branching and committing
- Pull Requests and code reviews
- Merge conflict resolution
- Tags and releases
- Branch protection rules
- Automated deployment with AWS CodePipeline to S3

This workflow represents a real-world DevOps source code management practice used in production environments.
