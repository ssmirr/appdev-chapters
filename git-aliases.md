# Git Aliases

In our workspaces, we've added some Git aliases to make our lives easier.

## git is now g

We've abbreviated `git` to just `g`.

In addition, if you don't supply a command, we've made the default `status`.

So now anytime you're at a terminal prompt, you should make a habit of frequently doing:

```
g
```

And it will be the equivalent of:

```
git status
```

And you'll know what's changed since the last commit, what branch you're on, how many commits you've made since you last pulled, etc.

## Command Aliases

### Make a commit

Instead of:

```
git add -A
git commit -m "Makes the project awesome"
```

You can do:

```
g acm "Makes the project awesome"
```

### Push to remote

Instead of:

```
git push
```

You can do:

```
g p
```

### Create a branch

Instead of:

```
git checkout -b my-new-branch
```

You can do:

```
g cob my-new-branch
```

### Switch to a branch

---

Instead of:

```
git checkout my-existing-branch
```

You can do:

```
g co my-existing-branch
```

### Discard uncommitted changes

Instead of:

```
git add -A
git stash
```

You can do:

```
g as
```

> Technically, stashing doesn't immediately discard changes; it stores them in a randomly named commit. You can get them back with `git stash pop`. Handy if you want to quickly save your current work to work on a different branch. [Read more about stash here.](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning)

### See a prettier log of the whole tree

Instead of:

```
g log --oneline --decorate --graph --all
```

You can do:

```
g sla
```
