# getchange

## Overview

`getchange` will download a Gerrit change along with its unmerged ancestors
(i.e. dependencies) given only a "change-ish". A change along with its
dependencies is referred to here as a "change-chain."

A "change-ish" is any of the following:

* Gerrit Change-Id
    * Either 41-character ID, or
    * "Change-Id: xyz" (Copy to Clipboard button in Gerrit)
* Gerrit number
* URL (two styles)
    * https://review.openstack.org/#/c/123/
    * https://review.openstack.org/123

The rebase/cherry-pick will occur on a new branch updated with the commits of the
appropriate branch. Any changes will be stashed. Any branch with the same name
will be renamed.

`getchange` handles two use-cases:

1. Edit
2. Test/Review

### Edit

```
cd $repodir
getchange edit 123
```

`edit` will do the following:

* performs checks \*
* stashes any changes
* creates a branch matching the original topic
* fetches the latest revision of the parent change of the given change
* cherry-picks the given change on top of latest parent change (which could be master or a dependency)

## Test

```
cd $repodir
getchange test 123
```

`test` will do the following:

* performs checks \*
* stashes any changes
* creates a branch based on the change subject and patch set (saves trips to Gerrit to figure out what the change is)
* fetches the latest from the branch from which the change-chain originates
* rebases the change-chain on top of latest from branch

\* Checks performed:

* Confirm each change in change-chain is based on the latest available parent (excluding merged parent)

\* Checks to be added in the future:

* Confirm entire change-chain has passed all tests

## How is this different from git review -d or Gerrit UI shortcuts?

### git review -d

* `git review -d` does not preserve the original topic.
* `git review -d` does not rebase on top of the latest parent.

### Gerrit UI Shortcuts

* *Checkout*: Doesn't use latest master. There's no reason to test against an old
master.
* *Cherry-Pick*: Doesn't handle Gerrit changes with dependencies.
* *Format-Patch*: Doesn't handle Gerrit changes with dependencies.
* *Pull*: Creates a merge commit. Or, if rebase is configured when pulling, the
commits of the change and its dependencies are not at the tip. You're not
submitting this branch so it's OK but it's not intuitive to look at later to
see what's in the branch.
* *Patch-File*: Doesn't handle Gerrit changes with dependencies.

## Installation

1. Clone this repo
1. Optionally, add the clone directory to your `PATH`.
1. Run `git review -s` in the repo with which you want to work.

## Usage

Run `getchange` from a directory containing `.git`.