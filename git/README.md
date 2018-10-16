# Guidelines for working with GIT

These guidelines should help you and us to work together on certain projects.

Some parts of these guidelines are inspired by two great articles: [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/) and [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

## Projects

We use Gitlab to manage our own projects. It can be accessed via [https://git.humans.am](https://git.humans.am)

Repositories are grouped by clients. If you would like to create a new repository, make sure that there is an existing group for the client. If not, create the group and tell the other Humans that there is a new group for the client. Also make sure, that you assign yourself and all the poeple who are involved in this project. New projects should always have the **Visibility Level** set to **Private**.

An example for a valid project path is `humans/website-prototype`.

Projects should have a `README.md` at the root of the project. The readme file should contain the following information:

* Framework/Technologies (e.g. Shop based on Shopware. Website based on Craftcms)
* Developer Setup (e.g. You need a Lamp stack with Elasticsearch)
* Build tasks (e.g. What Gulp/NPM tasks are relevant for development)
* Deployment process (e.g. Tags are automatically deployed to www.some-domain.com)

## Git Workflow

### General
We use gitflow to manage branches in our project repository. Every project repository must at least have a `master` and a `develop` branch. `develop` must be set as default branch in Gitlab (*Settings > General > General project*).

### Feature
Single commits could be made directly to `develop` branch. Anything else, including new features or bigger changes ,must be developed inside a feature branch. Feature branches are prefixed with `feature/` and must be named using kebap-case. A valid feature name is for example `feature/my-cool-feature`.

### Release
Before creating a release branch, make an anouncement on Skype, Slack or Whatsapp that you're planning to make a release. Others, who are involved in this project, will have the chance to also include changes in the release. We use [Semver](https://semver.org/) for versioning releases and hotfixes. Most likely a release is a minor change to the project, so the minor version number should be increased by one.

Example: Develop is: `1.24.3`. Release should be: `1.25.0`

The new release branch, created through gitflow, must be named `release/x.y.z`. Therefore the first commit inside the branch must be the version bump. In most projects, the version is stored inside `package.json` and should be changed here. Further commits in the release branch should contain only comsetic changes. Merging features is therefore not allowed in the release branch.
Before finishing the release the `release` branch should be compared with the `master`, in order to check and verify all the changes in the release. If the release contains changes of another contributor, talk to the person to make sure the changes won't cause any regressions.

Dependant of the project, the release branch, in specific the tag, might be auto-deployed to the live system. If this is not the case, checkout the fresh `tag` that was created through this release.

### Hotfix
By default, we dont write bugs. But there might be some cases where a hotfix is necessary. In this cases, start a new hotfix with gitflow from `master` branch and only from `master` branch. Like for releases, use Semver to bump the patch verison number by one.

Example: Master is: `1.25.0`. Hotfix should be: `1.25.1`

The hotfix branch must be named `hotfix/x.y.z`. Everything else mentioned in the [Release](#release) section also applies for a `hotfix` branch.

**CAUTION:** If there is an active `release` branch, make sure that the `hotfix` branch gets merged into the active `release` branch. This could be done by merging the `master` into the `hotfix` branch. There will be a conflict for the file, that contains the version number. Make sure to the version number of the `release` branch.

### Futher reading
Further information about gitflow and branching could be found here:

* [https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/ "A successful Git branching model")
* [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow "Gitflow Workflow | Atlassian Git Tutorial")

## Git commit messages
