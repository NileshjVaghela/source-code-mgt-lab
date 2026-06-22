# GitHub Source Code Management - Hands-on Workshop

## Overview

This workshop teaches a team of 3 developers how to manage source code collaboratively using GitHub. The team will build a static website (HTML, CSS, images) and deploy it to AWS S3 using AWS CodePipeline.

## Workshop Structure

| Level | Duration | File | Topics |
|-------|----------|------|--------|
| 1. Introductory | 30-40 min | [LEVEL1-INTRODUCTORY.md](LEVEL1-INTRODUCTORY.md) | Repo setup, cloning, branches, basic commits, push, pull |
| 2. Intermediate | 30-40 min | [LEVEL2-INTERMEDIATE.md](LEVEL2-INTERMEDIATE.md) | Pull Requests, code review, merge strategies, conflict resolution |
| 3. Expert | 30-40 min | [LEVEL3-EXPERT.md](LEVEL3-EXPERT.md) | Tags, Releases, branch protection, CodePipeline deployment to S3 |

## Team Setup

| Role | Branch | Responsibility |
|------|--------|---------------|
| Developer 1 (Lead) | `feature/homepage` | Homepage (index.html), repo setup, pipeline |
| Developer 2 | `feature/about` | About page (about.html) |
| Developer 3 | `feature/contact` | Contact page (contact.html) |

## Prerequisites

- GitHub account for each developer
- Git installed locally
- AWS account with S3 and CodePipeline access
- Basic HTML/CSS knowledge

## Project Structure

```
website/
├── index.html
├── about.html
├── contact.html
├── css/
│   └── style.css
├── images/
│   └── logo.png
└── README.md
```

## Pipeline Architecture

```
GitHub (main branch)
    │
    ↓ (webhook trigger)
AWS CodePipeline
    │
    ├── Source Stage → Pull from GitHub
    │
    └── Deploy Stage → Deploy to S3 (Static Website Hosting)
    │
    ↓
S3 Bucket (Public Static Website)
```

## Success Criteria

- All 3 developers contribute via separate branches
- All changes merged to main through Pull Requests
- At least one merge conflict resolved
- Repository tagged with version numbers
- GitHub Release created
- Static website deployed to S3 via CodePipeline
- Website accessible via S3 website endpoint

## Getting Started

Begin with [LEVEL1-INTRODUCTORY.md](LEVEL1-INTRODUCTORY.md)
