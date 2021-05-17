# Rails Foundry: Requesting code review

Please use the following procedure to request code review as you build out your project(s) during the Rails Foundry:

## Add instructors as collaborators

After you've created your repository, however you do that (perhaps by [using vanilla-rails as a template](https://github.com/appdev-projects/vanilla-rails/generate){:target="_blank"}), please [add the instructors to it as collaborators](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/inviting-collaborators-to-a-personal-repository){:target="_blank"}.

Our GitHub usernames are:

 - `jelaniwoods`
 - `pmckernin`
 - `raghubetina`

## Getting code review

### Asking for review

Whenever you're ready for initial code review, or re-review after you've made changes to address earlier reviews, do two things:

 1. Use GitHub's built-in functionality to [request code review in your PR](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/requesting-a-pull-request-review){:target="_blank"} from all three instructors.

    Note that you can (and should) request re-review after pushing further commits you've made to address a prior review.

    It's ultimately up to you, the author, when to merge your branch; we're not here to approve your PRs. We're simply providing feedback.
 2. Post a message in the `#review-requests` Slack channel ([`/join` it if you're not in it](https://slack.com/help/articles/205239967-Join-a-channel){:target="_blank"}) asking for review from your fellow students. Usually we say something like "Could use some review on this please 👀: [https://github.com/demostudent14/my-awesome-project/pull/1](https://github.com/demostudent14/my-awesome-project/pull/1)"

    Please keep an eye on this channel and if you see a request for review that you think looks interesting, pop in and participate in the PR discussion. Reading other people's code and asking questions about it is one of the very best ways to learn, and having to explain your own code to someone else is extremely valuable as well; so the more you all code review each other, the better off everyone will be.

### review-requests channel

We'll use the `#review-requests` channel in our Slack to ask each other for code review. We'll also use the GitHub+Slack app to get automated notifications of when instructors request changes, so that we can all learn from each other's feedback.

### Bot integration

#### #review-requests

In the #review-requests Slack channel ([`/join` it if you're not in it](https://slack.com/help/articles/205239967-Join-a-channel){:target="_blank"}), send the following messages, substituting your own repository name instead of `demostudent14/my-awesome-project`:

```
/github subscribe demostudent14/my-awesome-project
```

The very first time you use the `/github` Slack bot, it will ask you to authenticate. Go through the usual GitHub OAuth flow.

By default, when you subscribe a Slack channel to a repository using the GitHub integration, it adds listeners for four events: `issues`, `pulls`, `commits`, and `releases`.

When any of these events occur (i.e. a new issue is opened), a message is added to the channel. The integration allows you to use Slack as a centralized, customizable notification stream for certain types of GitHub events.

In the `#review-requests` channel, we don't want notifications for any of these four events, so let's unsubscribe from them:

```
/github unsubscribe demostudent14/my-awesome-project issues pulls commits releases
```

Then, let's subscribe to the `reviews` event:

```
/github subscribe demostudent14/my-awesome-project reviews
```

This will make it so that an automated notification is posted to the channel when instructor reviews are submitted, in case other students want to peruse them.

#### #repo-comments

[`/join` the #repo channel](https://slack.com/help/articles/205239967-Join-a-channel){:target="_blank"} and send the following messages, substituting your own repository name instead of `demostudent14/my-awesome-project`:

```
/github subscribe demostudent14/my-awesome-project
```

```
/github unsubscribe demostudent14/my-awesome-project issues pulls commits releases
```

```
/github subscribe demostudent14/my-awesome-project comments
```

This will make it so that an automated notification is posted to the channel whenever any comment is made on a PR or issue. This will be a place to go to see all the discussion that is happening across all of our projects.

There will likely be a lot of activity in this channel; so you may want to [tweak your notification settings](https://slack.com/help/articles/360056534254-Manage-notifications-for-specific-channels-and-direct-messages){:target="_blank"} for it if it becomes too much.
