---
id: sw_essentials
aliases:
  - Version Control
---

**Table of Contents**
```table-of-contents
```

---
# Git Configuration

## Author Configuration

```bash
# Set user info 
git config --global user.name "Your Name" 
git config --global user.email "your.email@company.com"
```

## Connecting Git and GitHub

1. Checking for existing SSH keys:    
2. Generating a new SSH key and adding it to the ssh-agent:
3. Adding a new SSH key to your GitHub account:
4. Testing your SSH connection:
5. Clone the 10xCode repo for trial
```bash
git clone <repo_url>
```

---
# Git Commit Message Formats

The Conventional Commits specification is a lightweight convention on top of commit messages. It provides an easy set of rules for creating an explicit commit history; which makes it easier to write automated tools on top of. This convention dovetails with [SemVer](http://semver.org "http://semver.org"), by describing the features, fixes, and breaking changes made in commit messages.

The commit message should be structured as follows:

```bash
<type>(mandatory scope): <description>

[optional body]

[optional footer(s)]
```

## Why Use Conventional Commits

- Automatically generating CHANGELOGs.
- Automatically determining a semantic version bump (based on the types of commits landed).
- Communicating the nature of changes to teammates, the public, and other stakeholders.
- Triggering build and publish processes.
- Making it easier for people to contribute to your projects, by allowing them to explore a more structured commit history.

## Specifications
- Everything should be lowercase. Do not use Upper case for type and scope.
## Structure

### Types

- API relevant changes
    - `feat` Commits, which adds or removes a new feature    
    - `fix` Commits, that fixes a bug
- `refactor` Commits, which rewrite/restructure your code, however, do not change any API behaviour
- `perf` Commits are special `refactor` commits, that improve performance
- `style` Commits, that do not affect the meaning (white space, formatting, missing semi-colons, etc)    
- `test` Commits, which add missing tests or correct existing tests
- `docs` Commits, that affect documentation only
- `build` Commits, that affect build components like build tool, ci pipeline, dependencies, project version
- `ops` Commit, which affects operational components like infrastructure, deployment, backup, recovery
- `chore` Miscellaneous commits e.g. modifying `.gitignore`
- `wip` Commits that are works in progress. These commits should be squashed into a proper commit before being merged into the `main` branch.

### Scopes
The `scope` provides additional contextual information.
- Allowed Scopes depends on the specific project
- Don't use issue identifiers as scopes
- Is a **mandatory** part of the format

### Breaking Changes Indicator
Breaking changes should be indicated by an `!` before the `:` in the subject line e.g. `feat(api)!: remove status endpoint.`This is an **optional** part of the format

### Description
The `description` contains a concise description of the change.

- Is a **mandatory** part of the format
- Use the imperative, present tense: "change" not "changed" nor "changes"
- Think of `This commit will...` or `This commit should...`        
- Don't capitalize the first letter
- No dot (`.`) at the end

### Body
The `body` should include the motivation for the change and contrast this with previous behaviour.
- Is an **optional** part of the format
- Use the imperative, present tense: "change" not "changed" nor "changes"
- This is the place to mention issue identifiers and their relations

### Footer

The `footer` should contain any information about **Breaking Changes** and is also the place to **reference Issues** this commit refers to.
- An **optional** part of the format
- **optionally** references an issue by its ID.
- **Breaking Changes** should start with the word `BREAKING CHANGES:` followed by a space or two newlines. The rest of the commit message is then used for this.

---
# Commit Types
## Merge Commit

```bash
Merge branch '<branch name>'
```

Follows default git merge message

## Revert Commit

```bash
Revert "<reverted commit subject line>"
```

Follows default git revert message

## Initial Commit

```bash
chore: init
```

## Examples
```
feat: add email notifications on new direct messages
```

```
feat(shopping cart): add the amazing button
```

```
feat!: remove ticket list endpoint

refers to JIRA-1337

BREAKING CHANGES: ticket enpoints no longer supports list all entites.
```

```
fix(shopping-cart): prevent order an empty shopping cart
```

```
fix(api): fix wrong calculation of request body checksum
```

```
fix: add missing parameter to service call

The error occurred because of <reasons>.
```

```
perf: decrease memory footprint for determine uniqe visitors by using HyperLogLog
```

```
build: update dependencies
```

```
build(release): bump version to 1.0.0
```

```
refactor: implement fibonacci number calculation as recursion
```

```
style: remove empty line
```

## References

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [qoomon/conventional-commits-cheatsheet.md](https://gist.github.com/qoomon/5dfcdf8eec66a051ecd85625518cfd13)

---
# Branch Management

## Protected Branches
- `master` (or `main`) branch is protected
- Direct pushes to master are **strictly prohibited**
- All changes must go through Pull Requests
- Branch protection rules enforce:
	- Required code review approvals (minimum 1)
    - Passing CI/CD checks
## Feature Development
1. Always create a new branch for features/fixes
2. Keep branches focused on single features/issues
3. Delete branches after successful merge

## Branch Naming Convention
Format: `<username>/<type>/<description>`
Example: `johndoe/feature/add-login-page`
Types:
- `feature/` - New features
- `fix/` - Bug fixes
- `refactor/` - Code refactoring
- `test/` - Test additions/modifications
---
# Pull Request Process
1. Create Branch
    `git checkout -b username/feature/description`
2. Make Changes
```bash
git add . git commit -m "feat: description"
git push origin username/feature/description
```
3. Create Pull Request
	1. Use PR template
	2. Fill in all required sections
	3. Link related issues
	4. Add appropriate labels
---
# Code Review Guidelines

## For Authors
1. Keep PRs focused and reasonably sized 
2. Self-review before requesting reviews
3. Respond to all comments
4. Update PR based on feedback
5. Resolve conversations once addressed

## For Reviewers
1. Check for:
	1. Code quality and standards
	2. Test coverage
	3. Documentation
	4. Security concerns
2. Provide constructive feedback
3. Use GitHub's suggestion feature when applicable
---
# Git Commands Reference

## Daily Usage

```bash
# Update local master 
git checkout master git pull origin master 

# Create new branch 
git checkout -b username/feature/description 

# Stage changes 
git add . 

# or specific files 
git add file1 file2 

# Commit 
git commit -m "type: description" 

# Push 
git push origin username/feature/description 

# Update branch with master 
git checkout username/feature/description
git rebase master
```

## Useful Commands

```bash
# View status 
git status 

# View branch history 
git log --oneline --graph 

# Discard local changes 
git checkout -- file 
git reset --hard 

# Create/switch branches 
git checkout -b branch-name 
git switch branch-name 

# Stash changes 
git stash 
git stash pop 

# View remote info 
git remote -v
```

## Best Practices

1. Pull latest master before creating new branch
2. Regularly sync branch with master
3. Keep commits atomic and focused
4. Write meaningful commit messages    
5. Squash commits before merging if necessary
6. Delete branches after merging
7. Never force push to shared branches
8. Document significant changes
