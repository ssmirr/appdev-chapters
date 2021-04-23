# Continuous Delivery

Now that we've leveled up our applications' code, let's level up our deployment workflow and infrastructure.

## Heroku Pipelines

We're still going to deploy to Heroku, but in a more robust way than we [first learned how to](https://chapters.firstdraft.com/chapters/775).

This time, we're going to take advantage of [Heroku Pipelines](https://devcenter.heroku.com/articles/pipelines) to easily manage _multiple_ deployment targets.

We'll have an app for our customers just like before, which we'll now call `production`; but we'll also have other apps: for beta testers, for demonstrating unreleased features, for quality assurance, for code review, etc

This will unlock powerful workflows wherein you can give and get continuous feedback on in-progress features every time you push a commit, even from non-technical stakeholders. Let's get started!

If you want to follow along, you can set up a workspace with any application that's ready to be deployed, even if it's a brand new one. I'll be using [industrial-auth-1](https://github.com/appdev-projects/industrial-auth-1).

## A deeper dive into Git remotes

First, let's dive deeper into Git "remotes".

Whenever we've been pushing code, we've been doing something like:

```
git push origin main
```

or just `git push` for short; which by default uses `origin` for the third part, and whichever branch you have checked out for the fourth part.

The third part is **the location that we want to send the code to**, known as **the "remote"**. You can list out all of the remotes that you've got with the `git remote` command:

```
gitpod /workspace/industrial-auth-1:(main) $ git remote

origin
upstream
```

I happen to have two; you might have the same, or you might only have one, or you might have more. `origin` and `upstream` are just nicknames; we can see the actual URLs of the locations with `git remote -v`:

```
gitpod /workspace/industrial-auth-1:(main) $ git remote -v

origin  https://github.com/raghubetina/industrial-auth-1.git (fetch)
origin  https://github.com/raghubetina/industrial-auth-1.git (push)
upstream        https://github.com/appdev-projects/industrial-auth-1.git (fetch)
upstream        https://github.com/appdev-projects/industrial-auth-1.git (push)
```

This lets us know for sure where our code is going to and coming from when we `push` and `pull`. Typically, `origin` is our primary repository on GitHub.com; and if it's a fork of another repository, then that one is called `upstream`.

Most importantly, we can add new remotes with the `git remote add` command.

You don't need to do this part, but for demonstration purposes, I am going to add a remote. Let's say I want to make a redundant copy of the repository on [Gitlab.com](https://gitlab.com/), a GitHub alternative, and `push` to it from time to time for safekeeping. I went there, signed in to my account, and created a repository. They assigned the repository the following URL:

```
https://gitlab.com/raghubetina/industrial-auth-1.git
```

Now I'm ready to add my remote and nickname it `gitlab`:


```
gitpod /workspace/industrial-auth-1:(main) $ git remote add gitlab https://gitlab.com/raghubetina/industrial-auth-1.git

gitpod /workspace/industrial-auth-1:(main) $ git remote -v

gitlab  https://gitlab.com/raghubetina/industrial-auth-1.git (fetch)
gitlab  https://gitlab.com/raghubetina/industrial-auth-1.git (push)
origin  https://github.com/raghubetina-appdev/industrial-auth-1.git (fetch)
origin  https://github.com/raghubetina-appdev/industrial-auth-1.git (push)
upstream        https://github.com/appdev-projects/industrial-auth-1.git (fetch)
upstream        https://github.com/appdev-projects/industrial-auth-1.git (push)
```

Now I've got three remotes. I can `git push gitlab` whenever I choose, and my branch will be sent to Gitlab for safekeeping (provided [I set up authentication](https://docs.gitlab.com/ee/integration/gitpod.html#enable-gitpod-in-your-user-settings)).

The point is: we're not limited to just one storage location for our repositories; we can `add` as many `remote`s as we like, and sending our code to them is as simple as `git push`.

One last thing: you can see an overview of all of your remotes as well as some other Git configuration by opening the config file within the hidden `.git` folder:

```
open .git/config
```

Take a look at it, but be careful with this file; you don't want to make any errant keystrokes in here by mistake. I will sometimes edit the URLs of remotes directly in this file, but only very cautiously.

## Heroku is just another git remote

Now, back to Pipelines.

The thing that made Heroku revolutionary when [they stormed the scene in 2009](https://www.infoq.com/news/2009/05/heroku-provisionless-revolution/) was that they said "Okay, as long as you're sending code around with `git push`, send it to us that way too — and we'll provision a server for you, put your code on it, spin up a database, set up a connection pool, and do the 1001 other things needed to get your app up and running. You just need to add us as an additional `remote` and `git push` a commit whenever it's ready to ship."

So, when we run the `heroku create my-app-name` command, it actually does two things:

 1. Through Heroku's API, it creates a Git repository called `https://git.heroku.com/my-app-name.git`
 2. It adds a Git remote called `heroku`:
 
    ```
    git remote add heroku https://git.heroku.com/my-app-name.git
    ```

That is what enables us to deploy using:

```
git push heroku main
```

From now on, we're going to stop using the default remote nickname of `heroku`. We'll call our first Heroku app, the one that our customers interact with, `production`.

 - When initially creating an app using the `heroku` command-line tool, you can choose a remote name using the `-r` option:
    
    ```
    heroku create my-app-name -r production
    ```
 - If you already have a remote named `heroku`, you can rename it:

    ```
    git remote rename heroku production
    ```

## Create a new pipeline

Assuming we have an application that we're ready to deploy  in this article, but it can be any app at any stage of completion — even a brand new one), the first step is to create a Pipeline:


Within, you'll see Staging and Production stages. Let's add an app to the Production stage — if you've already deployed to Heroku, find your existing app, otherwise 
