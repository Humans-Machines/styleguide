# Guidelines for working with GIT

These guidelines should help you and us to work together on sweet projects.

Some parts of these guidelines are inspired by two great articles: [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) and [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

## Table of contents

- [Projects](#projects)
- [Git Workflow](#git-workflow)
  * [General](#general)
  * [Feature](#feature)
  * [Release](#release)
  * [Hotfix](#hotfix)
  * [Further reading](#further-reading)
- [Git commits and messages](#git-commits-and-messages)
  * [1. Keep commits atomic](#1-keep-commits-atomic)
  * [2. Commit working code only](#2-commit-working-code-only)
  * [3. Limit the subject line to 50 characters](#3-limit-the-subject-line-to-50-characters)
  * [4. Capitalize the subject line](#4-capitalize-the-subject-line)
  * [5. Do not end the subject line with a period](#5-do-not-end-the-subject-line-with-a-period)
  * [6. Use the imperative mood in the subject line](#6-use-the-imperative-mood-in-the-subject-line)
  * [7. Use the body to explain what and why vs. how](#7-use-the-body-to-explain-what-and-why-vs-how)
  * [8. Meta commits (tbd.)](#8-meta-commits--tbd-)
  * [Further reading](#further-reading-1)

## Projects

We use Gitlab to manage our own projects. It can be accessed via [https://git.humans.am](https://git.humans.am)

Repositories are grouped by clients. If you would like to create a new repository, make sure that there is an existing group for the client. If not, create the group and tell the other Humans that created the clients group. Also make sure, that you assign yourself and all poeple who are involved in this project. New projects should always have the **Visibility Level** set to **Private**.

An example for a valid project path is `humans/website-prototype`.

Projects should have a `README.md` at the root of the project. The readme file should contain the following information:

* Framework/Technologies (e.g. Shop based on Shopware. Website based on Craftcms)
* Developer Setup (e.g. You need a Lamp stack with Elasticsearch)
* Build tasks (e.g. What Gulp/NPM tasks are relevant for development)
* Deployment process (e.g. Tags are automatically deployed to www.some-domain.com)

## Git Workflow

### General
We use gitflow to manage branches in our projects repository. Every project repo must at least have a `main` (Former: master) and a `develop` branch. `develop` must be set as default branch in Gitlab (*Settings > General > General project*).

### Feature
Single commits could be made directly to `develop` branch. Anything else, including new features or bigger changes, must be developed inside a `feature` branch. Feature branches are prefixed with `feature/` and must be named using kebap-case. A valid feature name is for example `feature/some-api-changes`.

### Release
Before creating a release branch, make an anouncement on Skype, Slack or Whatsapp that you're planning to make a release. Others, who are involved in this project, will have the chance to include their changes in the release. We use [Semver](https://semver.org/) for versioning releases and hotfixes. Most likely a release is a minor change to the project, so the minor version number should be increased by one.

Example: Develop is: `1.24.3`. Release should be: `1.25.0`

The new release branch, created through gitflow, must be named `release/x.y.z`. Therefore the first commit inside the branch must be the version bump. In most projects, the version is stored inside `package.json` and should be changed here. Further commits in the `release` branch should contain only comsetic changes. Merging features is therefore not allowed in the `release` branch.
Before finishing the release the current `release` branch should be compared with the `main`, in order to check and verify all the changes in the release. If the release contains changes of another contributors, talk to the persons to make sure the changes won't cause any regressions.

Dependant of the project, the release branch, in specific the tag, might be auto-deployed to a live environment. If this is not the case, checkout the fresh `tag` that was created with this release.

If something is not working as intended, rollback the release to the prior tag or create a hotfix.

### Hotfix
By default, we dont write bugs. But there might be some cases where a hotfix might be necessary. In this cases, start a new hotfix with gitflow from `main` branch and only from `main` branch. Like for releases, use Semver to bump the patch verison number by one.

Example: Main is: `1.25.0`. Hotfix should be: `1.25.1`

The hotfix branch must be named `hotfix/x.y.z`. Everything else mentioned in the [Release](#release) section also applies for a `hotfix` branch.

**CAUTION:** If there is an active `release` branch, make sure that the `hotfix` branch gets merged into the active `release` branch. This could be done by merging the `main` into the `hotfix` branch. There will be a conflict for the file, that contains the version number. Make sure to use the version number of the `release` branch.

### Further reading

* [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/ "A successful Git branching model")
* [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow "Gitflow Workflow | Atlassian Git Tutorial")


## Git commits and messages
Great commit message help others to understand changes without reviewing all code changes of a commit. Messages like *Stuff*, *Many changes* or *[no message]* are quite the opposite. Always keep in mind that other Humans (mostly not Machines) might participate in a project. Even if you started the project on your own, there might be a time when someone joins the project to assist you.

### 1. Keep commits atomic
Keep changes per commit atomic in order to provide a meaningful commit message. By keeping commits atomic, it's also quite simple to revert changes without breaking functionality.

```
// Bad
Add dependency and include some API method with small fixes

// Good
Add composer dependency for some API
Implement some API to fetch some information
Fix an issue where API could not be accessed
```

### 2. Commit working code only
Only working code should find his way into the `develop` branch. If you would like to try things, based on the projects codebase, create a `feature` branch. Feel free to test things in a `feature` branch. But before merging to `develop`, the code should be working and should not cause any regressions.

### 3. Limit the subject line to 50 characters
50 characters is not a hard limit, just a rule of thumb. Keeping subject lines at this length ensures that they are readable, and forces the author to think for a moment about the most concise way to explain what’s going on.

### 4. Capitalize the subject line
This is as simple as it sounds. Begin all subject lines with a capital letter.

```
// Bad
accelerate to 88 miles per hour

// Good
Accelerate to 88 miles per hour
```

### 5. Do not end the subject line with a period
Trailing punctuation is unnecessary in subject lines. Besides, space is precious when you’re trying to keep them to 50 chars or less.

```
// Bad
Open the pod bay doors.

// Good
Open the pod bay doors
```

### 6. Use the imperative mood in the subject line
Imperative mood just means “spoken or written as if giving a command or instruction”. A few examples:

* Clean your room
* Close the door
* Take out the trash

The imperative can sound a little rude; that’s why we don’t often use it. But it’s perfect for Git commit subject lines. One reason for this is that **Git itself uses the imperative whenever it creates a commit on your behalf**.

For example, the default message created when using `git merge` reads:

```
Merge branch 'myfeature'
```

And when using `git revert`:
```
Revert "Add the thing with the stuff"
```

So when you write your commit messages in the imperative, you’re following Git’s own built-in conventions. For example:

```
// Bad
Fixed bug with Y
Changing behavior of X

// Bad (written as description)
More fixes for broken stuff
Sweet new API methods

// Good
Refactor subsystem X for readability
Update getting started documentation
Remove deprecated methods
```

**A properly formed Git commit subject line should always be able to complete the following sentence:**

* If applied, this commit will *your subject line here*

For example:

* If applied, this commit will *refactor subsystem X for readability*
* If applied, this commit will *update getting started documentation*
* If applied, this commit will *remove deprecated methods*
* If applied, this commit will *release version 1.0.0*
* If applied, this commit will *merge pull request #123 from user/branch*

Notice how this doesn’t work for the other non-imperative forms:

* If applied, this commit will *fixed bug with Y*
* If applied, this commit will *changing behavior of X*
* If applied, this commit will *more fixes for broken stuff*
* If applied, this commit will *sweet new API methods*

### 7. Use the body to explain what and why vs. how

This [commit from Bitcoin Core](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) is a great example of explaining what changed and why:

```
commit eb0b56b19017ab5c16c745e6da39c53126924ed6
Author: Pieter Wuille <pieter.wuille@gmail.com>
Date:   Fri Aug 1 22:57:55 2014 +0200

   Simplify serialize.h's exception handling

   Remove the 'state' and 'exceptmask' from serialize.h's stream
   implementations, as well as related methods.

   As exceptmask always included 'failbit', and setstate was always
   called with bits = failbit, all it did was immediately raise an
   exception. Get rid of those variables, and replace the setstate
   with direct exception throwing (which also removes some dead
   code).

   As a result, good() is never reached after a failure (there are
   only 2 calls, one of which is in tests), and can just be replaced
   by !eof().

   fail(), clear(n) and exceptions() are just never called. Delete
   them.
```

Take a look at the [full diff](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) and just think how much time the author is saving fellow and future committers by taking the time to provide this context here and now. If he didn’t, it would probably be lost forever.

In most cases, you can leave out details about how a change has been made. Code is generally self-explanatory in this regard (and if the code is so complex that it needs to be explained in prose, that’s what source comments are for). Just focus on making clear the reasons why you made the change in the first place—the way things worked before the change (and what was wrong with that), the way they work now, and why you decided to solve it the way you did.

The future maintainer that thanks you may be yourself!

### 8. Meta commits (tbd.)
Sometimes you need to commit code changes, that are not actually functional changes. This could be a version bump, updated codestyle or a typo fix. Therefore these commits must not follow the rules above. Instead, they should be wrapped in braces.

Examples:
* [typo]
* [codestyle]
* [bump version to x.y.z]
* [docs]
* [skip ci]

**CAUTION:** Meta commits have to be atomic as well. For example, codestyle changes should never be mixed up with functional changes. Or a version bump should only change the related version file, nothing else.


### Further reading

* [https://www.freshconsulting.com/atomic-commits/](https://www.freshconsulting.com/atomic-commits/ "Developer Tip: Keep Your Commits 'Atomic'")
* [https://chris.beams.io/posts/git-commit/](https://chris.beams.io/posts/git-commit/ "How to Write a Git Commit Message") 
