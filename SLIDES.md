---
author: Ayoub Eddaoudi
date: 2026-01-12
paging: Slide %d / %d
---

# Git Good: HEAD first

<br>

<br>

Welcome

---

## whoami

<br>

<br>

<br>

Ayoub Eddaoudi, 1337 School Student

<br>

- [Github](https://github.com/ayoubedd)
- [Website](https://ayoubedd.com)
- [Email](mailto:contact@ayoubedd.me)

---

## Goal

<br>

My goal is to teach you enough git to be `dangerous`.

- Eliminate Manual Backups
- Experiment Without Fear
- Collaborate Seamlessly
- Traceability
- Release Management

---

## Short History

<br>

Git was born out of a moment of necessity and frustration in 2005. Created by `Linus Torvalds`, the mastermind behind Linux, it was developed after the Linux community lost free access to their previous version control tool, `BitKeeper`.

Some of the goals of git were as follows:

- Speed
- Simple design
- Fully distributed
- Allows huge teams to work on different parts of a project at the same time without getting in each other's way.
- Able to handle large projects like the Linux kernel efficiently (speed and data size)

<br>

> An idiot admires complexity, a genius admires simplicity

---

## Resources

<br>

<br>

- RTFM (read the friendly manual) `man git-*`
- Googlit
- Pro Git (book)

<br>

<br>

<br>

### Slides

- [Slides](https://github.com/ayoubedd/um6p_cs_git_workshop)

---

## KeyTerms

<br>

<br>

- `Repo/Repository`: A project tracked with git
- `Hash`: A hash function is any function that can be used to map data of arbitrary size to fixed-size values
- `Remote/Server`: Someones else computer
- `Worktree`: basically the files on your disk (but in a git manager project)

<br>

## Note

When ever you encounter some value wrapped between `<replace_me>` it has meant to be replaced with its appropriate value. (also remove the wrapping characters `<>`)

---

## Configssss

Git exposes us a way tell it how to behave in certain scenarios or tell it our name to use in the history tracking.

Configs can be local to a single repository or global (meaning it applies on all repositories in your system).
Git honors local over global configurations when duplicate configurations where encountered.

### Usage

- Note: You can set `--global` or `--local` flags as you which for your appropriate case.

```sh
git config --global --get <section>.<keyname> # gets a value of a key
git config --global --unset <section>.<keyname> # unsets a value of a key
```

<br>

### Important

We need to tell git our name and email, to use them in history manifestation. Because git needs it to keep track of who did what.

```sh
git config --global --add user.name '<first_name> <last_name>'
git config --global --add user.email '<email>'
```

---

## Create Your First Repository

<br>

<br>

I am sure this is not your first time to create a git repository, but lets just pretend so

<br>

```zsh
git init 42
Reinitialized existing Git repository in /path/where/git/command/executed/42/.git/
```

<br>

Change directory to the newly created repository.

```sh
cd 42
```

---

## Making History

<br>

```sh
## Adding first file

echo 'A' > A # <- creates a file named A containing A as value
git add 'A' # <- tells git to add this to our next snapshot (staging area or index)
git commit -m 'A' # <- take a snapshot (commit)

## Adding a second file

echo 'B' > 'B'
git add 'B'
git commit -m 'B'

```

---

## But Whats a Commit?

<br>

A commit is a snapshot in time, representing the state of the project.

<br>

### Inspecting Commits

```sh
git cat-file -p '<commitish>'
```

<br>

### Inspecting Trees

```sh
git cat-file -p '<tree_hash>'
```

---

## Inspecting History

<br>

Listing commits

```sh
git log
# my fav
git log --oneline --graph
```

Show the differences a commit or a series of commits have made.

```sh
git diff
```

Search for a commit by its commit message

```sh
git log -S'<commit_message>'
# or even better
git log --grep="<term>"
```

---

## Branches

Branches allow you to develop features, fix bugs, or safely experiment with new ideas in a contained area of your repository. But in practice branches is just pointers to commits with friendly names. And when we commit they get updated automatically to point to the newly created commit.

<br>

### List

```sh
git branch
```

### Create

```sh
git branch '<new_branch_name>'
```

### Switch

```sh
git swtich '<branch_to_switch_to>'
## or
git checkout '<branch_to_switch_to>'
```

### Create & Switch

```sh
git checkout -b '<new_branch_name>'
## or
git switch -c '<new_branch_name>'
```

## Delete

```sh
git branch -d '<branch_name_to_delete>'
```

---

## Branches All the Way

Work outside main branch.

### Feature Branch

```sh
git chekcout -b 'feat'
```

### Status

```
~~~graph-easy --as=boxart
[B] -> [A]

[master] -> [B]
[feat] -> [B]
~~~
```

### Committing Work

```sh
echo 'C' > 'C'
git add 'C'
git commit -m 'C'
```

### State

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]

[master] -> [B]
[feat] -> [C]
~~~
```

---

## Merging

New features and bug fixes done in any branch other than the main branch, at some point in the future, we will need those changes in the main branch. Merging is one way of retrieving changes in a branch into another.

### Example

Continuing from our last branch example, we can try to merge the `feat` into master.

First we must first switchback to the `master` branch.

### Before (Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]

[master] -- ref --> [B]
[feat] -- ref --> [C]
~~~
```

### Merging

```sh
git merge 'feat'
```

### After (Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]

[master] -- ref --> [C]
[feat] -- ref --> [C]
~~~
```

---

## Fear Not the Conflicts

Life Is Not All Sunshine and Roses.

When merging a branch into another sometimes (a lie, most of the time) conflicts occur.

### Example

Update file `A` in a new branch `feat2`.

```sh
git checkout -b 'feat2'
echo 'AA' > 'A'
git add 'A'
git commit -m 'D'
```

Then switchback `master` branch and edit same file with `different` changes.

```sh
git checkout 'master'
echo 'AAA' > 'A'
git add 'A'
git commit -m 'E'
```

### Status

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]

[feat2] -- ref --> [D]
[master] -- ref --> [E]
~~~
```

---

<br>

Now try to merge `feat2` into `master`.

```sh
# while in master branch
git merge 'feat2'
```

<br>

Once all our conflicts are resolved, we add out resolved conflicted files to staging area and we commit.

```sh
git commit -m 'F'
```

<br>

### Status (After Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]

[feat2] -- ref --> [D]
[master] -- ref --> [E]
~~~
```

### Status (After Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]
[F(merge commit)] -> [E]
[F(merge commit)] -> [D]

[feat2] -- ref --> [D]
[master] -- ref --> [F(merge commit)]
~~~
```

---

## But How It Actually Merges Commits? (Situation 1)

Git by default uses a merge strategy called 3-way merge when pulling or merging one branch.

<br>

### Situation

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]

[master] -- ref --> [C]
[dev] -- ref --> [C]
~~~
```

Times goes by...

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]
[F] -> [E]
[G] -> [D]

[master] -- ref --> [F]
[dev] -- ref --> [G]
~~~
```

---

## But How It Actually Merges Commits? (Situation 2 Result)

Since the branches has diverged in history since they common ancestor, git will merge the commits resulting in a merge commit combining the differences in a single commit.

### Status (Before Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]
[F] -> [E]
[G] -> [D]

[master] -- ref --> [F]
[dev] -- ref --> [G]
~~~
```

### Result

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [C]
[F] -> [E]
[G] -> [D]
[K] -> [G]
[K] -> [F]

[master] -- ref --> [K]
[dev] -- ref --> [G]
~~~
```

---

## But How It Actually Merges Commits? (Situation 2)

### Situation

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]

[master] -- ref --> [C]
[dev] -- ref --> [C]
~~~
```

Time goes by...

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [D]

[master] -- ref --> [C]
[dev] -- ref --> [E]
~~~
```

---

## But How It Actually Merges Commits? (Situation 2 Result)

### Status (Before Merge)

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [D]

[master] -- ref --> [C]
[dev] -- ref --> [E]
~~~
```

### Result

Because the master branch have seen no changes since we create `dev` branch, which means `git` can easily use the `E` commit without creating a merge commit, because there is nothing really to merge.

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [D]

[master] -- ref --> [E]
[dev] -- ref --> [E]
~~~
```

---

## Rebasing (Rewriting History)

Git Allows us to rewrite history, for example to undo some work, reorder commits, update their commit messages... etc.

Rebasing is like saying: I want to base my changes on what everybody has recently done.

### Situation

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E] -> [D]
[F] -> [E]
[G] -> [D]
[K] -> [G]

[master] -- ref --> [K]
[dev] -- ref --> [F]
~~~
```

### Result

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[D] -> [C]
[E'] -> [K]
[F'] -> [E']
[G] -> [D]
[K] -> [G]

[master] -- ref --> [K]
[dev] -- ref --> [F']
~~~
```

---

## Rebase Vs Merge

Well its complicated, no one is better than the other, as everything in computer science it's a game of tradeoffs.

<br>

| Feature       | Git Merge                                 | Git Rebase                                          |
| ------------- | ----------------------------------------- | --------------------------------------------------- |
| History Style | Preserves original structure (Non-linear) | Flattens history (Linear)                           |
| Commit Hashes | Keeps original hashes + 1 new commit      | Rewrites hashes for moved commits                   |
| Ease of Use   | High (Beginner friendly)                  | Medium (Requires caution)                           |
| Traceability  | Excellent (Shows exactly what happened)   | Moderate (Clean, but hides original branch context) |


<br>

### When to Use Which?

#### Use Git Merge when:

- You are working on a shared or public branch (like main or develop).
- You want to maintain a complete, audited record of every event in the repository.
- You are finishing a feature and want to merge it back into the main codebase for good.

#### Use Git Rebase when:

- You are working on a local/private branch and want to pull in the latest changes from main.
- You want to clean up your commit history before submitting a Pull Request.
- You prefer a "linear" history where every commit follows the previous one in a straight line.

#### Pro tip
A common workflow is to rebase your local feature branch frequently to stay up to date with main, but use merge when you are finally ready to push your completed feature into the team's shared repository.

---

## Cherry Pick

Git `cherry-pick` allow us to copy over the changes introduced by one commit (or more), creating a new commit () on top of `HEAD`.

### Basic Usage (Single Commit)

```sh
git cherry-pick '<commitish>'
```

### Range

We can cherry-pick a range of commits

```sh
git cherry-pick '<commitish_start>..<commitish_end>'
```

### Multiple Commits

```sh
git cherry-pick '<commitish> <commitish>'
```

### Edit Commit Message Before Committing

```sh
git cherry-pick -e '<commitish>'
```

### Bring the changes but dont commit

```sh
git cherry-pick --no-commit '<commitish>'
```

---

## Sync

<br>

More often than not, you will find yourself wanting to share code with collaborators or for backup. Git allows us to synchronize our local repositories with a remote server, so our peers can see our changes and build on top of them.

<br>

### Model

```
~~~graph-easy --as=boxart
[user1] -> [github]
[user2] -> [github]
[user3] -> [github]
~~~
```

### Preps

In order to retrieve changes made by others first we need to setup a few things.

The first thing we need to do is tell git about our server (`origin`).

```sh
git remote add '<remote_name>' '<remote_url>'
```

<br>

Then we need to assign our local branch to what branch on the server to sync to.

```sh
git branch --set-upstream-to=<remote_name>/<branch_name> '<local_branch_name>'
```

<br>

Check branches and their corresponding remotes with:

```sh
git branch -vvv
```

---

## Sync: Push/Pull

Now that we have configured our upstream, we can push our commits to the server.

### Pushing

```sh
git push '<origin>' '<local_branch_name>'
# or
git push # <- git will automatically deduce the origin and from your current branch
```

### Pulling

In fact `git pull` is a compound command, composed of `git fetch` and `git merge` (at least by default).

While pulling git first performs a `fetch` from the upstream. to retrieve the latest changes of the branch we are fetching or all branches if no flags where provided.

Then it performs a `git merge`, trying to merge our local with latest changes from the remote.
If conflicts encounters git will prompt us with those conflicts and asks us to resolve them and it will create a merge commit for us.

To pull changes from the upstream we do so using the following command

```sh
git pull
```

---

## Stash

While performing branch switching or pulling, your local changes might be conflicting with the commit you are checking out to. You will quickly find your self in a situation where your local changes is not ready yet to be committed but also you want to perform your git pull or checkout.

`git stash` record the current state of the working directory and the staging area and saves them. Allowing us to retrieve them later.

### Create

```sh
git stash
# or
git stash push
# or (to include untracked files)
git stash --include-untracked
```

### List

```sh
git stash list
```

### Apply

```sh
git stash apply '<specifier>'
```

### Drop

```sh
git drop '<specifier>'
```

---

## Revert

<br>

Sometimes we want to undo work but we don't want to delete it from history. `git revert` to the rescue.

### Usage

```sh
git revert '<commit_we_want_revert>'
# or
git revert '<start_commit>..<end_commit>'
```

Perform the revert without committing.

```sh
git revert -n '<commit_we_want_revert>'
# or
git revert -n '<start_commit>..<end_commit>'
```

## Example

```sh
echo 'hello world' >> 'A'
git add 'A'
git commit -m 'E'
```

Now we perform a revert of the last commit.

```sh
git revert HEAD
```

### Status

```
~~~graph-easy --as=boxart
[B] -> [A]
[C] -> [B]
[E] -> [C]

[master] -- ref --> [E]
~~~
```

---

## Reset

Sometimes we want to throw off work we no longer need. `git revert` moves our branch to a different commit.

Resetting but retaining the changes in work tree and staging area.

```sh
git revert --soft '<commitish>'
```

Hard Resetting: Drop all local changes and reset staging area while resetting.

```sh
git revert --soft '<commitish>'
```

<br>

### Note

Only perform reset on local or personal branches, otherwise you might be deleting somebody else work.

---

## Worktrees (Beyond Stashing)

Git worktrees allows us to access multiple branches at the same time without having to drop or stash our local changes in order to switch to the other.

### List

```sh
git worktree list
```

### Create

```sh
git worktree add '/path/to/place/the/wotktree/of/add/branch'
```

Note: Make sure to place the new worktree outside your current repository to avoid git trying to track it self (inception).

### Delete

```sh
git worktree remove '<worktree_name>'
```

---

## Reflog

<br>

Using `git revert` can be frightening because you are basically deleting commits. git not only keeps track of files history but also keeps track of changes to refs (HEAD, branches).

Every time we `checkout`, `revert` or `commit` git track that change.

`git reflog` (reference log).

To inspect the `reflog`:

```sh
git reflog
```

to print more information:

```sh
git reflog --pretty=raw
```

---

## Tags

Tags allow us to mark specific points in a repository’s history with friendly names. Very much like branches all they are just pointers to commit hashes with the distinction of being immutable.

<br>

### Listing

```sh
git tag
```

### Creation

```sh
git tag '<tag_name>' # <- will use the current HEAD value
```

### Deletion

```sh
git tag -d '<tag_name>'
```

### Note

Tags are not pushed to remotes (server) automatically, so we have to push them manually.

```sh
git push '<origin>' '<tag_name>'
```

As with creation deletion also doesn't happen automatically in the remotes, we have to delete them manually.

```sh
git push -d '<origin>' '<tag_name>'
```

---

## What About GitHub?

<br>

<br>

<br>

What about it? Isn't it obvious at this point?

---

### Q&A

<br>

<br>

<br>

Questions?

---

### Thank You
