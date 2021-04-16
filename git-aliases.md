# Git Aliases

In our workspaces, we've added some Git aliases to make our lives easier.

## git is now g

We've abbreviated `git` to just `g`.

In addition, if you don't supply a command, we've made the default `status`.

So now you should very frequently do:

```
g
```

And it will be the equivalent of:

```
git status
```

To see what's what.

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

> Technically, stashing doesn't discard changes; it stores them on a randomly named commit on a randomly named branch. You can get them back with `git stash pop`.

Instead of:

```
git add -A
git stash
```

You can do:

```
g as
```

### See a prettier log of the whole tree

Instead of:

```
g log --oneline --decorate --graph --all
```

You can do:

```
g sla
```
