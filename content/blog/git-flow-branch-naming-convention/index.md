---
title: Git Flow Branch Naming Convention
date: "2024-10-10T22:40:32.169Z"
description: How To Name Branches In Git Flow
---

When using Git Flow, naming branches consistently is essential for maintaining a clear and organized workflow. Git Flow has specific conventions for branch naming based on the type of work being done. Here’s how to name the branches in a Git Flow workflow:

### 1. **Main Branches**
- **`main` or `master`**: This is the stable branch where the source code of HEAD always reflects a production-ready state.

### 2. **Develop Branch**
- **`develop`**: This is the integration branch for features. It contains the latest development changes that will eventually be merged into the main branch.

### 3. **Feature Branches**
- **Naming Convention**: `feature/<feature-name>`
- **Example**: `feature/user-authentication`
- **Description**: Used for developing new features. The `<feature-name>` should be descriptive and reflect the work being done.

### 4. **Release Branches**
- **Naming Convention**: `release/<version>`
- **Example**: `release/1.0.0`
- **Description**: Used to prepare a new production release. This branch allows for final tweaks and bug fixes before the release.

### 5. **Hotfix Branches**
- **Naming Convention**: `hotfix/<issue-name>`
- **Example**: `hotfix/fix-login-bug`
- **Description**: Used for urgent fixes that need to be applied directly to production. The name should describe the issue being addressed.

### 6. **Bugfix Branches (optional)**
- **Naming Convention**: `bugfix/<issue-name>`
- **Example**: `bugfix/correct-typo`
- **Description**: Similar to feature branches but specifically for fixing bugs. This is not always used in Git Flow, as hotfixes often cover urgent issues.

### Summary of Naming Conventions
- **Feature**: `feature/<feature-name>`
- **Release**: `release/<version>`
- **Hotfix**: `hotfix/<issue-name>`
- **Bugfix**: `bugfix/<issue-name>` (if used)

Using these conventions helps team members understand the purpose of each branch at a glance, improving collaboration and project management.