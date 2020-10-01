# Contributing

[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![Semantic Versioning](https://img.shields.io/SemVer/2.0.0.png)](http://semver.org/spec/v2.0.0.html)
[![Gitter](https://badges.gitter.im/Syncleus/aparapi.svg)](https://gitter.im/Syncleus/aparapi?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

When contributing to this repository, it is usually a good idea to first discuss the change you
wish to make via issue, email, or any other method with the owners of this repository before
making a change. This could potentially save a lot of wasted hours.

Please note we have a code of conduct, please follow it in all your interactions with the project.

## Development

### Commit Message Format

Starting version 1.3.3 and later all commits on the Syncleus Aparapi repository follow the
[Conventional Changelog standard](https://github.com/conventional-changelog/conventional-changelog-eslint/blob/master/convention.md).
It is a very simple format so you can still write commit messages by hand. However it is
highly recommended developers install [Commitizen](https://commitizen.github.io/cz-cli/),
it extends the git command and will make writing commit messages a breeze. All the Aparapi
repositories are configured with local Commitizen configuration scripts.

Getting Commitizen installed is usually trivial, just install it via npm. You will also
need to install the cz-customizable adapter which the Aparapi repository is configured
to use.

```bash

npm install -g commitizen@2.8.6 cz-customizable@4.0.0
```

Below is an example of Commitizen in action. It replaces your usual `git commit` command
with `git cz` instead. The new command takes all the same arguments however it leads you
through an interactive process to generate the commit message.

![Commitizen friendly](http://aparapi.com/images/commitizen.gif)

Commit messages are used to automatically generate our changelogs, and to ensure
commits are searchable in a useful way. So please use the Commitizen tool and adhere to
the commit message standard or else we cannot accept Pull Requests without editing
them first.

Below is an example of a properly formated commit message.

```
chore(Commitizen): Made repository Commitizen friendly.

Added standard Commitizen configuration files to the repo along with all the custom rules.

ISSUES CLOSED: #31
```

### Pull Request Process

1. Ensure that install or build dependencies do not appear in any commits in your code branch. 
2. Ensure all commit messages follow the [Conventional Changelog](https://github.com/conventional-changelog/conventional-changelog-eslint/blob/master/convention.md)
   standard explained earlier.
3. Update the CONTRIBUTORS.md file to add your name to it if it isn't already there (one entry
   per person).
4. Adjust the project version to the new version that this Pull Request would represent. The
   versioning scheme we use is [Semantic Versioning](http://semver.org/).
5. Your pull request will either be approved or feedback will be given on what needs to be
   fixed to get approval. We usually review and comment on Pull Requests within 48 hours.

### Making a Release

Only administrators with privilages to push to the Aparapi Maven Central account can deploy releases. If this isn't you
then you can just skip this section.

First ensure the package is prepared for the release process:

* Make sure any references to the version number or branch name in the readme is at the pre-release branch/version
  * The branch name in the build pipeline badge
* Ensure the Aparapi version set in .gitlab.yml correctly points to the correct version for the branch.
* Update the changelog file.
* When possible make all changes that are universal to all versions in the develop branch and merge to the others.
* Develop should always have the latest Aparapi version.

Next lets take a few steps to do the actual release:

1.  Update everything listed above. Any functional changes should be made in develop branch when possible. Make sure develop 
    points to latest Aparapi version.
2.  Checkout the branch for the Aparapi version you are working on, for example `git checkout 2.0.0` or create
    a new one if this is for a new version. For new branches make sure they are off of develop. Never add a `v` to the begining.
3.  Merge develop into the branch your working on, `git merge develop`.
4.  Update the README.md to ensure pipeline badge points to the correct branch.
5.  Update .gitlab-ci.yml to ensure the Aparapi version points to the correct version fpr new branches. If this is the latest version
    of aparapi you should not need to update this, it should have been done in the first step.
6.  Push the branch to the server, make sure pipeline passes, such as `git push origin 2.0.0:2.0.0`.
7.  If pipeline passed, create a generic new branch, do not push it, as follows `git checkout -b release`.
8.  Update the pipeline badge in the readme to point to the to-be-released repository tag/version.
9.  Commit the current changes using a generic commit message such as `build(release): version 1.2.3 revision 4`. But do not push.
10. Create a new tag for the current revision with the following command: `git tag -a 1.2.3-4 -m "Version 1.2.3 revision 4"`. Ensure 
    tags **always** have the aparapi version followed by revsion and never have the letter `v` prepending it.
11. Push the newly created tag to the server: `git push origin 1.2.3-4:1.2.3-4`.
12. Delete the release branch you created locally `git branch -D release`.
13. If the Aparapi version deployed is the latest then merge develop branch into master, develop branch should already contain latest
    Aparapi version from the first step.
14. Ensure the pipeline badge in the readme points to the master branch
15. Push master branch to server.
16. Go to Github and GitLab and go to the releases. Update the description with the changelog for the version.
