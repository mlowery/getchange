# getchange

## Overview

`getchange` will cherry-pick a Gerrit change along with its unmerged ancestors
given only a "change-ish".

A "change-ish" is any of the following:

* Commit hash
* Gerrit Change-Id
    * Either 41-character ID, or
    * "Change-Id: xyz" (Copy to Clipboard button in Gerrit)
* Gerrit number
* URL (two styles)
    * https://review.openstack.org/#/c/123/
    * https://review.openstack.org/123

The cherry-picks will occur on a new branch updated with the commits of the
appropriate branch. Any changes will be stashed. Any branch with the same name
will be renamed.

## Installation

1. Clone this repo
1. Optionally, add the clone directory to your `PATH`.
2. Add this block to your `~/.ssh/config`:

```
Host review
    HostName review.openstack.org
    Port 29418
    User <gerrit-username>
```