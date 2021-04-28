# Continuous Delivery

Now that we've leveled up our applications' code, let's level up our deployment workflow and infrastructure.

## Heroku Pipelines

We're still going to deploy to Heroku, but in a more robust way than we [first learned how to](https://chapters.firstdraft.com/chapters/775).

This time, we're going to take advantage of [Heroku Pipelines](https://devcenter.heroku.com/articles/pipelines) to easily manage _multiple_ deployment targets.

We'll have an app for our customers just like before, which we'll now call `production`; but we'll also have other apps: for beta testers, for demonstrating unreleased features, for quality assurance, for code review, etc

This will unlock powerful workflows wherein you can give and get continuous feedback on in-progress features every time you push a commit, even from non-technical stakeholders. Let's get started!

If you want to follow along, you can set up a workspace with any application that's ready to be deployed, even if it's a brand new one. I'll be using a repository that I created using the [minimal-heroku template](https://github.com/appdev-projects/minimal-heroku/generate).

## A deeper dive into Git remotes

First, let's dive deeper into Git "remotes".

Whenever we've been pushing code, we've been doing something like:

```
git push origin main
```

or just `git push` for short; which by default uses `origin` for the third part, and whichever branch you have checked out for the fourth part.

The third part is **the location that we want to send the code to**, known as **the "remote"**. You can list out all of the remotes that you've got with the `git remote` command:

```
gitpod /workspace/minimal-heroku:(main) $ git remote

origin
```

I happen to have one; you might have the same, or you might have more. `origin` is just a nickname; we can see the actual URL of the location with `git remote -v`:

```
gitpod /workspace/minimal-heroku:(main) $ git remote -v

origin  https://github.com/raghubetina/minimal-heroku.git (fetch)
origin  https://github.com/raghubetina/minimal-heroku.git (push)
```

This lets us know for sure where our code is going to and coming from when we `push` and `pull`. Typically, `origin` is our primary repository on GitHub.com.

Importantly, we can add new remotes with the `git remote add` command.

You don't need to do this part, but for demonstration purposes, I am going to add a remote. Let's say I want to make a redundant copy of the repository on [Gitlab.com](https://gitlab.com/), a GitHub alternative, and `push` to it from time to time for safekeeping.

First, I need to go to my Gitlab dashboard and create a repository. Then, they will assign the repository a Git URL, such as the following:

```
https://gitlab.com/raghubetina/minimal-heroku.git
```

Now I'm ready to add my remote and nickname it `gitlab`:


```
gitpod /workspace/minimal-heroku:(main) $ git remote add gitlab https://gitlab.com/raghubetina/minimal-heroku.git

gitpod /workspace/minimal-heroku:(main) $ git remote -v

gitlab  https://gitlab.com/raghubetina/minimal-heroku.git (fetch)
gitlab  https://gitlab.com/raghubetina/minimal-heroku.git (push)
origin  https://github.com/raghubetina-appdev/minimal-heroku.git (fetch)
origin  https://github.com/raghubetina-appdev/minimal-heroku.git (push)
```

Now I've got two remotes. I can `git push gitlab` whenever I choose, and my branch will be sent to Gitlab for safekeeping (provided [I set up authentication](https://docs.gitlab.com/ee/integration/gitpod.html#enable-gitpod-in-your-user-settings)).

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

 1. Through Heroku's API, it creates an app and retrieves the assigned Git URL. If the app name we chose is available the Git URL would be `https://git.heroku.com/my-app-name.git`.

    We could do the same thing by going to our Heroku dashboard in the browser and creating a new app there.
 1. It adds a Git remote called `heroku`:
 
    ```
    git remote add heroku https://git.heroku.com/my-app-name.git
    ```

    We could do the same thing ourselves at the command line.

The presence of this remote is what enables us to deploy using:

```
git push heroku main
```

As we know, once the deployment completes, Heroku will assign the domain:

```
https://my-app-name.herokuapp.com
```

We will, for any real application, never allow our users to see that underlying `.herokuapp.com` subdomain. We will purchase our own domain, e.g. `www.my-app-name.com`, and [configure it to point to our Heroku app](https://devcenter.heroku.com/articles/custom-domains); so that our users never know the difference.

## Set up production app

From now on, within our remotes, we're going to stop using the default nickname of `heroku`. For our primary app, the one that our customers interact with, lets use the nickname `production`.

As far as the app's name within Heroku, my convention is end the name in `-production` or `-prod` — e.g., `my-app-name-production`. Sometimes, the exact name I want will already be taken in Heroku; but that's okay, because I'm going to mostly be referring to them by my local Git remote nicknames when I'm running commands. So just pick a name that's close and available.

When initially creating an app using the `heroku` command-line tool, you can choose a remote name using the `-r` option. 
    
```
gitpod /workspace/minimal-heroku:(main) $ heroku create minimal-heroku-production -r production

Creating ⬢ minimal-heroku-production... done
https://minimal-heroku-production.herokuapp.com/ | https://git.heroku.com/minimal-heroku-production.git
```

If you already have a remote named `heroku`, you can rename it:

```
git remote rename heroku production
```

If you don't already have a `production` app, create one now and deploy your `main` branch to it:

```
git push production main
```

Visit your application with `heroku open`. Is it working? If so, yay!

If not, read the server log with `heroku logs --tail` and debug. You probably need to `heroku run rails db:migrate`, for one thing. Possibly also `heroku run rails sample_data` if that makes sense for your application.

Woohoo! Even 10+ years later, it still brings a tear to my eye how much Heroku has simplified deployment 😢

## Set up staging app

Let's say you want feedback on a feature that you're working on from a client, a co-founder, or a designer who isn't familiar with GitHub, GitPod, etc. It would be a huge amount of friction to ask them to sign up for accounts, `rails db:create`, `rails db:migrate`, `rails sample_data`, `bin/server`, etc. 

Instead, let's create a second Heroku app whose purpose will be for deploying experimental code to. We'll call this one `staging`:

```
heroku create minimal-heroku-staging -r staging
```

Let's deploy to this one, too:

```
git push staging main
```

If we look at our list of remotes now, we should see `origin`, `production`, and `staging`:

```
gitpod /workspace/minimal-heroku:(main) $ git remote

origin
production
staging
```

If you try to open your staging application with `heroku open`, you should see an error:

```
gitpod /workspace/minimal-heroku:(main) $ heroku open

 ›   Error: Multiple apps in git remotes
 ›     Usage: --remote staging
 ›        or: --app minimal-heroku-staging
 ›     Your local git repository has more than 1 app referenced in git remotes.
 ›     Because of this, we can't determine which app you want to run this command against.
 ›     Specify the app you want with --app or --remote.
 ›     Heroku remotes in repo:
 ›     minimal-heroku-production (production)
 ›   minimal-heroku-staging (staging)
 ›
 ›     https://devcenter.heroku.com/articles/multiple-environments
```

The issue is: now that we have more than one Heroku app, we have to be more specific when we run our `heroku` commands about which location we want them performed on.

```
heroku open -r staging
```

And if you have errors like before, to run the same commands on `staging`, add the `-r staging` flag:

```
heroku logs --tail -r staging
heroku run rails db:migrate -r staging
heroku run sample_data -r staging
```

Adding the `-r production` or `-r staging` flag to every `heroku ...` command is a pain, but I'll show you some shortcuts soon.

I now have two applications:

 - `production` is for customers, and will be reachable at (for example) `www.my-app-name.com` (once I [add a custom domain](https://devcenter.heroku.com/articles/custom-domains)).
 - `staging` is for testing or demonstration purposes (different teams use `staging` differently), and will be reachable at (for example) `staging.my-app-name.com`.

Much better than only having a single deployment target! Now I can merge to `main`, deploy to `staging`, kick the tires in a real production environment, and then finally ship it to customers.

## Create pipeline

Now that we have our two apps up and running, let's create a Heroku Pipeline to group them together.

Head over to your Heroku dashboard and, first, confirm that your two new apps appear in the list there. Create a new pipeline from the dropdown in the top-right:

![](/assets/continuous-delivery-1-create-pipeline.png)

Choose a name for your pipeline:

![](/assets/continuous-delivery-2-choose-name.png)

On the next screen, you will see Stages, Staging and Production. Add your apps to their respective pipeline. (If you hadn't already created your apps from the command-line with `heroku create`, you could have created them from here, added them as remotes with `git remote add`, and then done your initial deploys with `git push`). 

![](/assets/continuous-delivery-3-stages.png)

One immediate benefit of grouping the apps together in a Pipeline is that, once you're fully confident in a change, you can [promote your staging app's slug](https://devcenter.heroku.com/articles/pipelines#promoting) directly to the production app. This results in a faster deployment, in some cases with less downtime, than `git push production main`.

## Review Apps

Imagine there are 10 developers on the team with 2-3 branches each that they think are ready to merge into `main` and deploy to `production`. They are just waiting for approval from Quality Assurance and the product owners.

If you only have a single `staging` server, you're going to have a big traffic jam. Should each developer create their own staging server? Since we're using Heroku, with it's unbelievably good deployment ergonomics, that is actually within the realm of possibility; if we were working directly with e.g. Amazon AWS for hosting, it would be out of the question. However, there's a better way: Review Apps.

To enable Review Apps in our Pipeline, click the "Connect to GitHub" button, authorize Heroku to access your GitHub account (don't forget to Grant access to any organizations you want) and locate the repository that your `origin` remote is pointing at. Finally, click the "Enable" button next to "Enable Review Apps":

![](/assets/continuous-delivery-5-connect-github.png)

In the pane that opens, check off "Create new review apps for new pull requests automatically" and "Destroy stale review apps automatically":

![](/assets/continuous-delivery-6-configure-review.png)

Now, try the following:

 - Create a new feature branch. E.g.,

    ```
    git checkout -b a-new-feature
    ```
 - Commit a change. For me, in `minimal-heroku`, I made a change to `public/index.html` and committed it.
 - Push your change to GitHub and open a Pull Request.
 - Look at your Heroku Pipeline:

![](/assets/continuous-delivery-7-review-app-provisioning.png)

Voilá! Heroku automatically detected the new pull request, immediately provisioned a new app, and deployed the feature branch (not `main`) to it.

 - Once its ready, you can share the URL of the **Review App** with clients, designers, etc.
 - It's common to put a link in your task management/ticketing system, which aids tremendously in getting feedback and approval from all stakeholders before merging to `main`.
 - A link to the review app will automatically be added to the Pull Request thread. That means that code reviewers don't have to go through the trouble of stopping their work, `git pull`ing your branch, switching to it, possibly running database migrations, etc, in order to interact with your feature while providing feedback.
 - Every time you push a new commit, the Review App will automatically re-deploy.
 - Awesome!

In my experience, Review Apps _dramatically_ tighten feedback loops between product owners, developers, clients, designers, usability testers, and stakeholders all throughout the development cycle. This is one of the most important Continuous Delivery techniques that we'll add to our arsenal.

There's just one problem: when the Review App is done building and you visit it, it's quite likely that you probably see the familiar "Something went wrong" error, due to the familiar "pending migrations" issue. Ugh.

Fix it with:

```
heroku run rails db:migrate -a the-assigned-heroku-app-name
```

You can find the assigned Heroku app name in the pipeline, or in the pull request on GitHub.

## Procfile

Let's continue to make our deployment workflow even smoother.

Here's something that used to happen to me every almost every single time I deployed, and probably just happened to you a minute ago:

 - Deploy: `git push main production`
 - Visit application: `heroku open`
 - See the "We're sorry, but something went wrong. If you are the application owner check the logs for more information."
 - `heroku logs --tail`
 - See the "pending migrations" error.
 - `heroku run rails db:migrate -r production`
 - Visit application for real.

Argh! I _always_ forget to `rails db:migrate`. Can't we just tell Heroku to always `rails db:migrate` whenever we deploy, just in case there are any new migrations?

Why yes, we can! Heroku allows you to include a file called `Procfile` in the root folder of your application, in which you can specify commands that you want to be executed upon startup.

[There's a lot of things that you can include in a `Procfile`](https://devcenter.heroku.com/articles/procfile), but here's a good starting point that you can use for your applications:

```
# /Procfile

web: bundle exec puma -p $PORT -C ./config/puma.rb
release: bundle exec rails db:migrate
```

 - The first line, `web:`, is how you tell Heroku what commands to run when each Web dyno starts. Heroku does a pretty good job with Rails apps by default, but here you can fine tune it if you want. In the example above, we're telling Heroku to launch the Puma web server on the default port using the configuration we specified in our `config/puma.rb` file.
 - Very commonly, you'll add another line to tell Heroku commands to run when each [Worker dyno](https://devcenter.heroku.com/articles/background-jobs-queueing) starts.
 - The second line, `release:`, is how we tell Heroku any commands we want to run every time we deploy a new version of the app. Here's our chance to automatically `rails db:migrate` — phew!

Happily, the `Procfile` runs for Review Apps just like any other apps, which takes care of the issue we ran into above.

---

This deployment technique integrated into our already powerful Git branch-PR-merge workflow is a very powerful baseline _continuous delivery_ methodology.

