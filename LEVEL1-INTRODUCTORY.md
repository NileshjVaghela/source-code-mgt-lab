# Level 1: Introductory - Git & GitHub Basics

**Duration:** 30-40 minutes  
**Objective:** Set up the repository, understand branching, and make first commits as a team.

---

## Step 1: Repository Creation (Developer 1 - Lead)

Developer 1 creates the repository on GitHub.

1. Go to https://github.com → Click **New Repository**
2. Configure:
   - Repository name: `team-website`
   - Visibility: **Public**
   - Check: **Add a README file**
   - Add `.gitignore`: None
   - License: None
3. Click **Create repository**

---

## Step 2: Add Team Members as Collaborators (Developer 1)

1. Go to repository → **Settings** → **Collaborators**
2. Click **Add people**
3. Add Developer 2 and Developer 3 by their GitHub usernames
4. Both developers accept the invitation from their email or GitHub notifications

---

## Step 3: Generate a Personal Access Token (All Developers)

GitHub does not allow password authentication over HTTPS. Each developer must create a **Personal Access Token (PAT)** to push code.

📖 **Official Guide:** https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic

**Steps to create your token:**

1. Go to GitHub → Click your **profile picture** (top-right) → **Settings**
2. Scroll down the left sidebar → Click **Developer settings**
3. Click **Personal access tokens** → **Tokens (classic)**
4. Click **Generate new token** → **Generate new token (classic)**
5. Configure:
   - **Note:** `team-website-workshop`
   - **Expiration:** 30 days
   - **Scopes:** Check **repo** (full control of private repositories)
6. Click **Generate token**
7. **Copy the token immediately** — you cannot see it again!

> ⚠️ **Important:** When Git prompts for a password during `git push` or `git clone`, paste this **token** instead of your GitHub password.

**Cache your token (optional, avoids re-entering every time):**

```bash
git config --global credential.helper 'cache --timeout=3600'
```

This stores your credentials in memory for 1 hour.

---

## Step 4: Clone the Repository (All Developers)

Each developer runs on their local machine:

```bash
git clone https://github.com/<developer1-username>/team-website.git
cd team-website
```

When prompted:
- **Username:** Your GitHub username
- **Password:** Paste your **Personal Access Token** (not your GitHub password)

Verify the remote:

```bash
git remote -v
```

Expected output:
```
origin  https://github.com/<developer1-username>/team-website.git (fetch)
origin  https://github.com/<developer1-username>/team-website.git (push)
```

---

## Step 5: Set Up Git Identity (All Developers)

Each developer configures their identity:

```bash
git config user.name "Your Name"
git config user.email "your-email@example.com"
```

---

## Step 6: Create Project Structure on Main (Developer 1)

Developer 1 creates the base project structure:

```bash
mkdir css images
```

Create `css/style.css`:

```css
/* Team Website - Common Styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    color: #333;
}

header {
    background: #2c3e50;
    color: #fff;
    padding: 20px;
    text-align: center;
}

nav {
    background: #34495e;
    padding: 10px;
    text-align: center;
}

nav a {
    color: #fff;
    text-decoration: none;
    margin: 0 15px;
    font-size: 16px;
}

nav a:hover {
    text-decoration: underline;
}

.container {
    max-width: 960px;
    margin: 20px auto;
    padding: 20px;
}

footer {
    background: #2c3e50;
    color: #fff;
    text-align: center;
    padding: 15px;
    position: fixed;
    bottom: 0;
    width: 100%;
}
```

Create a placeholder `images/.gitkeep`:

```bash
touch images/.gitkeep
```

Update `README.md`:

```markdown
# Team Website

A collaborative static website built by a team of 3 developers.

## Team Members
- Developer 1: Homepage
- Developer 2: About Page
- Developer 3: Contact Page

## Tech Stack
- HTML5
- CSS3
- Hosted on AWS S3
```

Commit and push:

```bash
git add .
git commit -m "Initial project structure with CSS and README"
git push origin main
```

---

## Step 7: Pull Latest Changes (Developer 2 and Developer 3)

Before creating branches, pull the latest:

```bash
git pull origin main
```

---

## Step 8: Create Feature Branches (Each Developer)

**Developer 1:**
```bash
git checkout -b feature/homepage
```

**Developer 2:**
```bash
git checkout -b feature/about
```

**Developer 3:**
```bash
git checkout -b feature/contact
```

Verify you are on the correct branch:

```bash
git branch
```

The asterisk (*) shows your current branch.

---

## Step 9: Developer 1 - Create Homepage

Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Website - Home</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Team Website</h1>
        <p>Built collaboratively using GitHub</p>
    </header>
    <nav>
        <a href="index.html">Home</a>
        <a href="about.html">About</a>
        <a href="contact.html">Contact</a>
    </nav>
    <div class="container">
        <h2>Welcome to Our Website</h2>
        <p>This website was built by a team of 3 developers using Git and GitHub for source code management.</p>
        <p>Version: 1.0</p>
    </div>
    <footer>
        <p>&copy; 2026 Team Website. All rights reserved.</p>
    </footer>
</body>
</html>
```

Commit and push:

```bash
git add index.html
git commit -m "Add homepage with navigation and content"
git push origin feature/homepage
```

---

## Step 10: Developer 2 - Create About Page

Create `about.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Website - About</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Team Website</h1>
        <p>Built collaboratively using GitHub</p>
    </header>
    <nav>
        <a href="index.html">Home</a>
        <a href="about.html">About</a>
        <a href="contact.html">Contact</a>
    </nav>
    <div class="container">
        <h2>About Us</h2>
        <p>We are a team of 3 developers learning Git and GitHub workflows.</p>
        <h3>Our Mission</h3>
        <p>To master source code management and collaborative development practices.</p>
        <h3>Team Members</h3>
        <ul>
            <li>Developer 1 - Lead Developer</li>
            <li>Developer 2 - Frontend Developer</li>
            <li>Developer 3 - Frontend Developer</li>
        </ul>
    </div>
    <footer>
        <p>&copy; 2026 Team Website. All rights reserved.</p>
    </footer>
</body>
</html>
```

Commit and push:

```bash
git add about.html
git commit -m "Add about page with team information"
git push origin feature/about
```

---

## Step 11: Developer 3 - Create Contact Page

Create `contact.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Website - Contact</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Team Website</h1>
        <p>Built collaboratively using GitHub</p>
    </header>
    <nav>
        <a href="index.html">Home</a>
        <a href="about.html">About</a>
        <a href="contact.html">Contact</a>
    </nav>
    <div class="container">
        <h2>Contact Us</h2>
        <p>Get in touch with our team.</p>
        <h3>Email</h3>
        <p>team@example.com</p>
        <h3>Location</h3>
        <p>Mumbai, India</p>
        <h3>Working Hours</h3>
        <p>Monday - Friday: 9:00 AM - 6:00 PM IST</p>
    </div>
    <footer>
        <p>&copy; 2026 Team Website. All rights reserved.</p>
    </footer>
</body>
</html>
```

Commit and push:

```bash
git add contact.html
git commit -m "Add contact page with team details"
git push origin feature/contact
```

---

## Step 12: Verify Branches on GitHub (All Developers)

1. Go to the repository on GitHub
2. Click the branch dropdown (shows "main")
3. Verify all 3 feature branches are listed:
   - `feature/homepage`
   - `feature/about`
   - `feature/contact`

---

## Step 13: View Commit History

Each developer views their own branch commit history:

```bash
git log --oneline
```

Expected output (example for Developer 1):
```
a1b2c3d Add homepage with navigation and content
f4e5d6a Initial project structure with CSS and README
```

View all branches with a visual graph (shows the full team's work):

```bash
git log --oneline --graph --all
```

Expected output:
```
* a1b2c3d (feature/homepage) Add homepage with navigation and content
| * b2c3d4e (origin/feature/about) Add about page with team information
|/
| * c3d4e5f (origin/feature/contact) Add contact page with team details
|/
* f4e5d6a (origin/main, main) Initial project structure with CSS and README
```

> 💡 The first command shows only your current branch. The second command shows **all branches** with a tree-like graph showing how they diverge from main.

---

## Key Concepts Learned

| Concept | Command | Purpose |
|---------|---------|---------|
| Clone | `git clone <url>` | Copy remote repo locally |
| Branch | `git checkout -b <name>` | Create and switch to new branch |
| Stage | `git add <file>` | Stage changes for commit |
| Commit | `git commit -m "msg"` | Save changes locally |
| Push | `git push origin <branch>` | Upload branch to GitHub |
| Pull | `git pull origin <branch>` | Download latest changes |
| Log | `git log --oneline` | View commit history |

---

## Checkpoint

Before moving to Level 2, verify:

- [ ] Repository created with all 3 collaborators
- [ ] Base project structure exists on main (CSS, README)
- [ ] Each developer has their own feature branch
- [ ] Each developer has pushed at least one commit to their branch
- [ ] All 3 feature branches visible on GitHub

---

**Next:** [LEVEL2-INTERMEDIATE.md](LEVEL2-INTERMEDIATE.md) - Pull Requests, Merging & Conflict Resolution
