<!--
~~ Licensed to the Apache Software Foundation (ASF) under one
~~ or more contributor license agreements.  See the NOTICE file
~~ distributed with this work for additional information
~~ regarding copyright ownership.  The ASF licenses this file
~~ to you under the Apache License, Version 2.0 (the
~~ "License"); you may not use this file except in compliance
~~ with the License.  You may obtain a copy of the License at
~~
~~   http://www.apache.org/licenses/LICENSE-2.0
~~
~~ Unless required by applicable law or agreed to in writing,
~~ software distributed under the License is distributed on an
~~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
~~ KIND, either express or implied.  See the License for the
~~ specific language governing permissions and limitations
~~ under the License.

~~ NOTE: For help with the syntax of this file, see:
~~ https://maven.apache.org/doxia/references/apt-format.html
-->

## Working with Github pull requests

In the past, patches would come in through JIRA in the form of a patch file.
Nowadays, many contributors start with offering pull requests through [Github](https://github.com/apache).
This short guide describes how to properly integrate those contributions.

1. Make sure your own copy of the repository is up-to-date with Gitbox.
Also make sure your working copy is "clean" (you have no uncommitted changes or unpushed commits).

1. Pull requests may multiple commits.
Fetch the whole pull request as a patch file from Github by appending `.patch` to the pull request URL.
Apply the pull request to your own copy using `git am whatever.patch`.

1. Squash all commits in the pull request into one commit.
[See below](#squashing-commits) for more details.

1. Make sure the result still fixes the problem at hand.

1. Finally, push the one squashed commit to the main branch on Gitbox.
It will automatically be pushed to the corresponding branch on Github.

1. Update administration:

    1. Close the Github pull request and thank the author for their contribution.

    1. Also, close the accompanying JIRA ticket, referencing the commit hash that contains the fix.

### Squashing commits
With `git status` you know how many commits to squash:

    On branch master
    Your branch is ahead of 'origin/master' by 6 commits.
      (use "git push" to publish your local commits)

Use that information to do an interactive rebase with `git rebase -i HEAD~6`.
Here, 6 is the number of commits in the pull request.

This opens up an editor (for example Vim) which a script.
Each commit in the pull request is one line starting with `pick`.
This means the commit should be used in the rebase.

Edit **all but the first** lines by replacing `pick` with `s` or `squash`.
A _squash_ uses the commit, but melds it into the previous commit.
Save and quit the editor.

Git will now execute the script you've written.
For squashing, the editor opens again, this time to build the squashed commit.
It will list all individual commits with their commit messages.
The content should be self-explanatory.
Edit the squashed commit, make sure the first line starts with a reference to the JIRA issue, save and quit.

