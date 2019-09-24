# FRC Team 7170 Git Conventions/Usage Guide

## Naming

Repositories for robot projects should be named "Robot*YEAR*", where "*YEAR*" is replaced by the four digit
representation of the year (e.g. "2020").

## Commit Messages

Writing good commit messages with a consistent format that works well with Git tools is important. There is no sense
going into detail here when there are plenty of great online resources:
 - [Erlang's wiki page](https://github.com/erlang/otp/wiki/Writing-good-commit-messages) (concise)
 - [Tim Pope's post](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
 - [Chris Beam's post](https://chris.beams.io/posts/git-commit/) (detailed with lots of examples)

## Branching Model

A branching model is a specification of the (git-related aspects of the) workflow which must be adopted by all
developers on a project to ensure cooperation is organized.

Herein lies a git branching model for FRC development based on Vincent Driessen's highly popular branching model
covered in [this blog post](https://nvie.com/posts/a-successful-git-branching-model/).
Some aspects of Driessen's model are unnecessary for FRC development, and therefore omitted for simplicity; other
aspects are copied here in near verbatim.
For more detailed information, including the extra measures Driessen's branching model takes to make it robust enough
for real-world software development, and [pretty visuals](https://nvie.com/files/Git-branching-model.pdf), check out
Driessen's blog post.

This document assumes the reader is already familiar with basic Git ideas such as commits and branches.
There are [plenty](https://guides.github.com/introduction/git-handbook/) [of](https://git-scm.com/docs/gittutorial)
[online](https://git-scm.com/book/en/v1/Getting-Started) [resources](https://rogerdudler.github.io/git-guide/) for
learning Git.

The document also assumes that Git is being used in a centralized manner (e.g. a central repository exists on Github or
any other Git hosting service) and the central repository is referred to, as per convention, as "origin".

The examples given here use the Git command line tool. You should too.

### Primary Branches

In the central repository, there are two main branches with infinite lifetime (i.e. are never deleted): `master` and
`develop`.

(To be clear, none of the branch "types" described here are different from any other in a technical sense: in Git, a
branch is a branch. The distinctions described here are only in the way the various branch types should be used.)

#### The `master` Branch

The tip of the `master` branch should only ever contain code that is, to the best of your knowledge, correct/bug-free
(of course, it is impossible to guarantee this).

This rule must be strictly obeyed such that, if one needs a functional version of code, one need only look to the
`master` branch.

#### The `develop` Branch

The `develop` branch is based on the `master` branch and is a sort of "rendezvous point" for new
[features](#feature-branches) before they are collectively deemed stable and merged into `master`.
Individual features should be tested and functional before merging them into `develop`, but this is not a requirement.

##### Merging `develop` Into `master`

```shell script
# Assuming there are currently no uncommitted changes in working directory.
git checkout master

# "--no-ff" means no fast forward; refer to Driessen's post for the meaning.
git merge --no-ff develop

# Tag the commit? (see the tagging section bellow)

# Push the changes to the remote repository's copy of the master branch.
git push origin master
```

### Feature Branches

Feature branches are for the development of a single "feature". Feature branches branch from the `develop` branch, and
must be either merged back into the `develop` branch once the feature is complete (if one has decided that the feature
is to be kept), or deleted (if the feature is to be abandoned). Feature branches can be named anything that does not
interfere with the other branch types' naming schemes. Feature branches usually only exist in developer
repositories--not origin.

Feature branches are great for experimentation, since any potentially damaging changes are constrained to the feature
branch. Most real-world testing with a robot should also probably be done through a feature branch. For example, one
might create and checkout a branch called `pid-tuning`, tweak PID parameters, observe the effects on the robot, and, if
the effects are desirable, merge the parameter changes made in the `pid-tuning` back into `develop`.

#### What Constitutes a Feature?

The term "feature" would suggest that a feature branch must be used to add some sort of new functionality, but this is
not necessarily the case. Feature branches can be used for adding functionality, removing functionality, changing
functionality, or fixing functionality, can contain 3 commits or 100 commits, etc.

#### Creating a Feature Branch

The following creates a feature branch named "my-feature-branch".

```shell script
git checkout -b my-feature-branch develop
```

#### Merging a Feature Branch Into `develop`

```shell script
# Assuming there are currently no uncommitted changes in working directory.
git checkout develop

git merge --no-ff my-feature-branch

# Delete the feature branch.
git branch -d my-feature-branch

# Push the changes to the remote repository's copy of the develop branch.
git push origin develop
```

### Hotfix Branches

If, despite one's best efforts, a critical bug is discovered in `master`, then a hotfix branch is in order.

Ideally, bugs would be fixed in `develop` and then merged into `master`, but sometimes `develop` is in an unstable state
and the bug-fix cannot wait until `develop` stabilizes. (Note that *non-critical* bugs can usually just be fixed in a
feature branch and merged into `develop`, which is, in turn, eventually merged into `master`.)

Hotfix branches branch from the `master` branch and are merged back into both the `master` and `develop` branches once
the bug-fix is implemented.
Hotfix branch names should be prefixed with `hotfix-`.

#### Creating a Hotfix Branch

```shell script
git checkout -b hotfix-the-thing master
```

#### Integrating a Completed Hotfix Branch
```shell script
# Assuming there are currently no uncommited changes in working directory.
git checkout master

# Merge the changes into master.
git merge --no-ff hotfix-the-thing

# Tag the commit? (see the tagging section bellow)

# Push the changes to the remote repositoriy's copy of the master branch.
git push origin master

git checkout develop

# Merge the changes into develop.
git merge --no-ff hotfix-the-thing

# Push the changes to the remote repositoriy's copy of the develop branch.
git push origin develop

# Delete the hotfix branch.
git branch -d hotfix-the-thing
```

### Tagging

Tagging certain commits on the `master` branch is a great way to keep track of notable points/states during the
development of software.
(Tags work on commits of any branch, of course, but they are typically most useful for marking points on the `master`
branch.)

One might, for example, make a tag called `elevator-working` and then, if the elevator ever stops working in the future,
one could diff, or otherwise investigate and analyse the difference from the future code of, the commit tagged
`elevator-working`.

#### Versioning

It is worth noting that tagging is usually used for marking version numbers on commits on `master` but, seeing as
specifying and obeying a versioning scheme is usually unnecessary for FRC development, such tagging does not apply.

#### Tag the Tip of the `master` Branch

Both of the following commands create an annotated (`-a`) tag named "my-tag".
(Refer to Google for the difference between an annotated and non-annotated tag.)

```shell script
git tag my-tag master -a -m "<message>"
# Or, to prompt for a message in your default text editor, just do
git tag my-tag master -a
```

#### Pushing Tags to origin

By default, tags are not pushed to the remote repository when one does a `git push`. To push tags to the origin, one
must run the following:

```shell script
git push --follow-tags
```

Refer to [this StackOverflow answer](https://stackoverflow.com/a/26438076/5017282) for more information.

#### Working With Tagged Commits

To checkout a tagged commit (in detached HEAD mode), simply run the following:

```shell script
# Assuming there are currently no uncommited changes in working directory.
git checkout my-tag
# If you have a branch named the same thing as the tag, the above command is ambiguous and Git defaults
# to checking-out the branch; in that case, explicitly specify that you are looking for the tag:
git checkout tags/my-tag
```

To diff (display the difference between) one's current working directory and the tagged commit:

```shell script
git diff my-tag
# Or, for the same reason described above:
git diff tags/my-tag
```

Etc... Referring to a commit by a tag is essentially the same as referring to a commit by its hash, so anything you can
do with a commit's hash is possible.
