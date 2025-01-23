# GitLab Flow

## Table of Contents

1. [Overview of GitLab Flow](#overview-of-gitlab-flow)
2. [Structure and Tree Visualization](#structure-and-tree-visualization)
3. [Basic Commands and Workflow](#basic-commands-and-workflow)
4. [Medium Commands and Features](#medium-commands-and-features)
5. [Advanced Commands and Workflows](#advanced-commands-and-workflows)
6. [Combined Command Examples](#combined-command-examples)
7. [Best Practices and Limitations](#best-practices-and-limitations)
8. [Example Workflow](#example-workflow)
9. [Key Takeaways](#key-takeaways)

---

## Overview of GitLab Flow

GitLab Flow is a flexible Git workflow that builds on **GitHub Flow** while introducing environment-specific branches, issue-driven development, and integration with CI/CD pipelines. It bridges the gap between **continuous deployment workflows** and **versioned release workflows**. GitLab Flow is well-suited for projects requiring multiple environments like `staging` and `production`.

### Key Concepts
1. **Single Source of Truth**:
   - The `main` branch serves as the central branch for production-ready code.
2. **Environment-Specific Branches**:
   - Additional branches (`staging`, `production`) are used to manage code in different environments.
3. **Feature/Issue-Driven Development**:
   - Developers work on short-lived branches for individual features or fixes.
4. **Pull Requests and CI/CD**:
   - Merge Requests (MRs) ensure collaboration and automated testing before merging.

---

## Structure and Tree Visualization

GitLab Flow supports multiple environments. Here's a typical structure:

```
(production)      *---------* (deployed to production)
                   \
(staging)           *---*---* (tested and stable)
                     \
(main)                *---*---* (latest changes ready for staging)
                       \
(feature/abc)            *---* (merged)
(feature/xyz)                  *--*---* (merged)
```

### Key Branches
1. **`main`**: Production-ready code.
2. **Environment branches**: `staging`, `production`, etc.
3. **Feature branches**: Short-lived branches for specific features or bug fixes.

---

## Basic Commands and Workflow

### Step 1: Create a Feature or Issue Branch
```bash
git checkout main
git pull origin main       # Ensure main is up-to-date
git checkout -b feature/new-feature
```

### Step 2: Work and Commit Changes
```bash
# Make changes, stage them, and commit
git add .
git commit -m "Implement new feature logic"
```

### Step 3: Push and Open a Merge Request
```bash
# Push the feature branch to the remote repository
git push -u origin feature/new-feature
```
- Open a **Merge Request (MR)** in GitLab from `feature/new-feature` to `main`.

### Step 4: Merge the Branch
After approval:
```bash
git checkout main
git pull origin main
git merge feature/new-feature
git push origin main
```

### Step 5: Deploy to Staging or Production
If CI/CD is configured, merging into `main` triggers deployment pipelines.

---

## Medium Commands and Features

### Rebasing Feature Branches
Keep your branch up-to-date with `main` to avoid conflicts:
```bash
git checkout feature/new-feature
git pull --rebase origin main
# Resolve any conflicts
git add .
git rebase --continue
```

### Squashing Commits
Before merging, squash multiple commits into one for a cleaner history:
```bash
git rebase -i origin/main
# Mark commits as "squash" or "s" in the editor
```

### Cherry-Picking Commits
Apply a specific commit to another branch:
```bash
git cherry-pick <commit-hash>
```

### Stashing Changes
Temporarily save uncommitted changes:
```bash
git stash
# Switch branches or pull updates
git checkout main
git pull origin main
git checkout feature/new-feature
git stash pop
```

---

## Advanced Commands and Workflows

### Deploying to Specific Environments

1. Push changes to an environment-specific branch (`staging`, `production`):
```bash
git checkout staging
git pull origin staging
git merge main
git push origin staging
```

2. CI/CD triggers deployment to the `staging` environment.

### Merge Request Approvals
Use **GitLab’s built-in approval system**:
- Set **approval rules** to ensure specific team members review the code before merging.

### GitLab-Specific Tools
1. **Issue Boards**:
   - Integrate GitLab Flow with issue boards to track development progress.
   - Example: Automatically close issues by mentioning `Closes #<issue-number>` in the commit message.

2. **GitLab CI/CD Integration**:
   - Add a `.gitlab-ci.yml` file for automated testing and deployment:
   ```yaml
   stages:
     - test
     - deploy

   test:
     script:
       - npm test

   deploy_staging:
     script:
       - ./deploy-to-staging.sh
     only:
       - staging
   ```

---

## Combined Command Examples

### Automate Pull and Rebase
```bash
git pull --rebase origin main && git push
```

### Fast-Forward Merge to Staging
```bash
git checkout staging
git pull origin staging
git merge --ff-only main
git push origin staging
```

### Resolve Conflicts with Mergetool
```bash
git checkout main
git merge feature/conflict-fix
git mergetool
git commit
```

---

## Best Practices and Limitations

### Best Practices
1. **Environment-Specific Branches**:
   - Use `staging`, `production`, etc., for deployment-ready code.
2. **Short-Lived Feature Branches**:
   - Create branches per feature or issue and merge them quickly.
3. **Continuous Integration**:
   - Automate testing and deployments using `.gitlab-ci.yml`.

### Limitations
1. **Complexity with Many Environments**:
   - Requires careful management of environment branches.
2. **Merge Delays**:
   - Can delay deployments if MRs require lengthy reviews or approvals.

---

## Example Workflow

1. **Feature Development**:
   ```bash
   git checkout -b feature/add-user-auth
   # Make changes and commit
   git add .
   git commit -m "Add user authentication"
   git push -u origin feature/add-user-auth
   ```

2. **Merge and Deploy to Staging**:
   ```bash
   git checkout main
   git pull origin main
   git merge feature/add-user-auth
   git push origin main

   # Merge into staging
   git checkout staging
   git pull origin staging
   git merge main
   git push origin staging
   ```

3. **Deploy to Production**:
   ```bash
   git checkout production
   git pull origin production
   git merge staging
   git push origin production
   ```

---

## Key Takeaways

1. GitLab Flow is ideal for projects with **multiple environments** (staging, production, etc.).
2. **Merge Requests** ensure code quality through peer reviews.
3. CI/CD pipelines automate testing and deployment.
4. Keep branches **short-lived** to maintain a clean repository.
5. Use environment branches to separate development, testing, and production-ready code.

By following GitLab Flow, you’ll achieve better collaboration, streamlined development processes, and seamless deployments.
